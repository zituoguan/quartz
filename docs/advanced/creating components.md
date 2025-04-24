---
title: 创建你自己的 Quartz 组件
---

> [!warning]
> 本指南假设你已经具备 JavaScript 编写经验，并且熟悉 TypeScript。

通常在 Web 开发中，我们使用 HTML 编写布局代码，大致如下：

```html
<article>
  <h1>An article header</h1>
  <p>Some content</p>
</article>
```

这段 HTML 表示一个带有标题和段落的文章。它通常与 CSS 结合用于页面样式，并通过 JavaScript 增加交互性。

然而，HTML 本身并不支持创建可复用的模板。如果你想新建一个页面，需要复制粘贴上述代码片段并手动修改内容。如果网站有大量结构相似的内容，这种方式就很低效。React 的创造者也遇到了类似问题，于是发明了“组件”——返回 JSX 的 JavaScript 函数——来解决代码重复问题。

实际上，组件允许你编写一个 JavaScript 函数，接收一些数据并输出 HTML。**虽然 Quartz 并不使用 React，但它采用了类似的组件理念，让你可以在 Quartz 站点中轻松表达布局模板。**

## 组件示例

### 构造器

组件文件以 `.tsx` 结尾，存放在 `quartz/components` 文件夹下。它们会在 `quartz/components/index.ts` 中重新导出，方便在布局和其他组件中引用。

每个组件文件都应有一个默认导出，满足 `QuartzComponentConstructor` 函数签名。它是一个接收单个可选参数 `opts` 并返回 Quartz 组件的函数。参数 `opts` 的类型由你自己通过 `Options` 接口定义。

你可以在组件中使用配置选项的值来改变渲染行为。例如，下面代码中的组件如果 `favouriteNumber` 小于 0 就不会渲染。

```tsx {11-17}
interface Options {
  favouriteNumber: number
}

const defaultOptions: Options = {
  favouriteNumber: 42,
}

export default ((userOpts?: Options) => {
  const opts = { ...userOpts, ...defaultOpts }
  function YourComponent(props: QuartzComponentProps) {
    if (opts.favouriteNumber < 0) {
      return null
    }

    return <p>My favourite number is {opts.favouriteNumber}</p>
  }

  return YourComponent
}) satisfies QuartzComponentConstructor
```

### Props

Quartz 组件本身（上面高亮的 11-17 行）看起来像一个 React 组件。它接收属性（有时称为 [props](https://react.dev/learn/passing-props-to-a-component)）并返回 JSX。

所有 Quartz 组件都接受以下 props：

```tsx title="quartz/components/types.ts"
// 为演示简化
export type QuartzComponentProps = {
  fileData: QuartzPluginData
  cfg: GlobalConfiguration
  tree: Node<QuartzPluginData>
  allFiles: QuartzPluginData[]
  displayClass?: "mobile-only" | "desktop-only"
}
```

- `fileData`：当前页面的元数据，可能由 [[making plugins|插件]] 添加。
  - `fileData.slug`：当前页面的 slug。
  - `fileData.frontmatter`：解析到的 frontmatter。
- `cfg`：`quartz.config.ts` 中的 `configuration` 字段。
- `tree`：处理和转换文件后得到的 [HTML AST](https://github.com/syntax-tree/hast)。如果你想用 [hast-util-to-jsx-runtime](https://github.com/syntax-tree/hast-util-to-jsx-runtime) 渲染内容，可以参考 `quartz/components/pages/Content.tsx`。
- `allFiles`：所有已解析文件的元数据。适合做页面列表或分析站点结构。
- `displayClass`：一个工具类，指示用户希望在移动端或桌面端如何渲染。可用于根据设备类型有选择地隐藏组件。

### 样式

Quartz 组件还可以在实际函数组件上定义 `.css` 属性，Quartz 会自动识别。它应为 CSS 字符串，可以直接内联，也可以从 `.scss` 文件导入。

注意，内联样式 **必须** 是标准 CSS：

```tsx {6-10} title="quartz/components/YourComponent.tsx"
export default (() => {
  function YourComponent() {
    return <p class="red-text">Example Component</p>
  }

  YourComponent.css = `
  p.red-text {
    color: red;
  }
  `

  return YourComponent
}) satisfies QuartzComponentConstructor
```

导入样式时，可以使用 SCSS 文件：

```tsx {1-2,9} title="quartz/components/YourComponent.tsx"
// 假设样式表在 quartz/components/styles/YourComponent.scss
import styles from "./styles/YourComponent.scss"

export default (() => {
  function YourComponent() {
    return <p>Example Component</p>
  }

  YourComponent.css = styles
  return YourComponent
}) satisfies QuartzComponentConstructor
```

> [!warning]
> Quartz 不使用 CSS modules，因此你声明的样式会全局生效。如果只想作用于当前组件，请使用特定的类名和选择器。

### 脚本与交互

那交互性怎么办？比如你想添加点击事件。和 `.css` 属性类似，你还可以声明 `.beforeDOMLoaded` 和 `.afterDOMLoaded` 属性，它们是包含脚本的字符串。

```tsx title="quartz/components/YourComponent.tsx"
export default (() => {
  function YourComponent() {
    return <button id="btn">Click me</button>
  }

  YourComponent.beforeDOMLoaded = `
  console.log("hello from before the page loads!")
  `

  YourComponent.afterDOMLoaded = `
  document.getElementById('btn').onclick = () => {
    alert('button clicked!')
  }
  `
  return YourComponent
}) satisfies QuartzComponentConstructor
```

> [!hint]
> 对于来自 React 的开发者，Quartz 组件与 React 组件不同，仅用 JSX 做模板和布局。像 `useEffect`、`useState` 等 Hook 不会被渲染，`onClick` 这类属性也不会生效。请用常规 JS 脚本直接操作 DOM 元素。

如其名，`.beforeDOMLoaded` 脚本会在页面加载前执行，此时无法访问页面元素，通常用于预取关键数据。

`.afterDOMLoaded` 脚本会在页面完全加载后执行，适合设置需要持续整个站点访问周期的内容（如从 localStorage 读取数据）。

如果你需要创建依赖于 _页面特定_ 元素的 `afterDOMLoaded` 脚本（比如页面导航后元素会变化），可以监听 `"nav"` 事件（如果启用了 [[SPA Routing]]，页面导航时会触发）。

```ts
document.addEventListener("nav", () => {
  // 在这里处理页面特定逻辑
  // 例如添加事件监听器
  const toggleSwitch = document.querySelector("#switch") as HTMLInputElement
  toggleSwitch.addEventListener("change", switchTheme)
  window.addCleanup(() => toggleSwitch.removeEventListener("change", switchTheme))
})
```

你还可以通过 `prenav` 事件为 [[SPA Routing]] 添加类似 `beforeunload` 的事件。

```ts
document.addEventListener("prenav", () => {
  // 在 SPA 导航触发后、页面替换前执行
  // 常见用法是在 prenav 存储 sessionStorage
  // 然后在随后的 nav 中有条件地加载
})
```

建议通过 `window.addCleanup` 跟踪事件处理器，以防止内存泄漏。
该方法会在页面导航时调用。

#### 导入代码

当然，把代码写成字符串并不总是实际或理想的做法。

Quartz 支持通过 `.inline.ts` 文件导入组件代码。

```tsx title="quartz/components/YourComponent.tsx"
// @ts-ignore: typescript 不识别我们的 inline 打包系统
// 所以需要屏蔽类型错误
import script from "./scripts/graph.inline"

export default (() => {
  function YourComponent() {
    return <button id="btn">Click me</button>
  }

  YourComponent.afterDOMLoaded = script
  return YourComponent
}) satisfies QuartzComponentConstructor
```

```ts title="quartz/components/scripts/graph.inline.ts"
// 此处的任何 import 都会被打包到浏览器端
import * as d3 from "d3"

document.getElementById("btn").onclick = () => {
  alert("button clicked!")
}
```

如上例所示，你还可以在 `.inline.ts` 文件中导入第三方包。Quartz 会自动打包并插入实际脚本。

### 使用组件

创建自定义组件后，在 `quartz/components/index.ts` 中重新导出：

```ts title="quartz/components/index.ts" {4,10}
import ArticleTitle from "./ArticleTitle"
import Content from "./pages/Content"
import Darkmode from "./Darkmode"
import YourComponent from "./YourComponent"

export { ArticleTitle, Content, Darkmode, YourComponent }
```

然后，你可以像其他组件一样在 `quartz.layout.ts` 通过 `Component.YourComponent()` 使用它。详情见 [[configuration#Layout|布局]] 章节。

由于 Quartz 组件本质上是返回 React 组件的函数，你可以在其他 Quartz 组件中组合使用它们。

```tsx title="quartz/components/AnotherComponent.tsx"
import YourComponent from "./YourComponent"

export default (() => {
  function AnotherComponent(props: QuartzComponentProps) {
    return (
      <div>
        <p>It's nested!</p>
        <YourComponent {...props} />
      </div>
    )
  }

  return AnotherComponent
}) satisfies QuartzComponentConstructor
```

> [!hint]
> 可以参考 `quartz/components` 目录下的更多组件示例，作为你自定义组件的参考！

