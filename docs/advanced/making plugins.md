---
title: 制作你自己的插件
---

> [!warning]
> 本部分文档假设你已经具备 TypeScript 的使用经验，并将包含描述 Quartz 插件接口的代码片段。

Quartz 的插件是一系列对内容进行转换的操作。下图展示了处理流程管道：

![[quartz transform pipeline.png]]

所有插件都定义为一个函数，接收一个选项参数 `type OptionType = object | undefined`，并返回一个与插件类型对应的对象。

```ts
type OptionType = object | undefined
type QuartzPlugin<Options extends OptionType = undefined> = (opts?: Options) => QuartzPluginInstance
type QuartzPluginInstance =
  | QuartzTransformerPluginInstance
  | QuartzFilterPluginInstance
  | QuartzEmitterPluginInstance
```

接下来的章节将详细介绍每种插件类型可以实现的方法。在此之前，先澄清几个类型：

- `BuildCtx` 定义在 `quartz/ctx.ts`，包含：
  - `argv`：传递给 Quartz [[build]] 命令的命令行参数
  - `cfg`：完整的 Quartz [[configuration]]
  - `allSlugs`：所有有效内容 slug 的列表（关于 slug 详见 [[paths]]）
- `StaticResources` 定义在 `quartz/resources.tsx`，包含：
  - `css`：需要加载的 CSS 样式定义列表。CSS 样式由 `CSSResource` 类型描述，也定义在 `quartz/resources.tsx`，可接受源 URL 或样式表的内联内容。
  - `js`：需要加载的脚本列表。脚本由 `JSResource` 类型描述，也定义在 `quartz/resources.tsx`，可定义加载时机（DOM 加载前或后）、是否为模块，以及源 URL 或脚本的内联内容。
  - `additionalHead`：要添加到页面 `<head>` 标签的 JSX 元素或返回 JSX 元素的函数列表。函数接收页面数据作为参数，可有条件地渲染元素。

## 转换器（Transformers）

转换器对内容进行**映射**，接收 Markdown 文件并输出修改后的内容或为文件添加元数据。

```ts
export type QuartzTransformerPluginInstance = {
  name: string
  textTransform?: (ctx: BuildCtx, src: string) => string
  markdownPlugins?: (ctx: BuildCtx) => PluggableList
  htmlPlugins?: (ctx: BuildCtx) => PluggableList
  externalResources?: (ctx: BuildCtx) => Partial<StaticResources>
}
```

所有转换器插件必须至少定义一个 `name` 字段用于注册插件，还可以实现一些可选函数，用于在转换单个 Markdown 文件的不同阶段进行操作。

- `textTransform` 在文件被解析为 [Markdown AST](https://github.com/syntax-tree/mdast) 之前执行文本到文本的转换。
- `markdownPlugins` 定义 [remark 插件](https://github.com/remarkjs/remark/blob/main/doc/plugins.md) 列表。`remark` 是一个以结构化方式将 Markdown 转换为 Markdown 的工具。
- `htmlPlugins` 定义 [rehype 插件](https://github.com/rehypejs/rehype/blob/main/doc/plugins.md) 列表。`rehype` 以结构化方式将 HTML 转换为 HTML。
- `externalResources` 定义插件在客户端正常工作所需加载的外部资源。

通常对于 `remark` 和 `rehype`，你可以找到现成的插件。如果你想自己创建 `remark` 或 `rehype` 插件，请参考 [创建插件指南](https://unifiedjs.com/learn/guide/create-a-plugin/)（基于 unified AST 解析和转换库）。

一个结合了 `remark` 和 `rehype` 生态的转换器插件示例是 [[plugins/Latex|Latex]] 插件：

```ts title="quartz/plugins/transformers/latex.ts"
import remarkMath from "remark-math"
import rehypeKatex from "rehype-katex"
import rehypeMathjax from "rehype-mathjax/svg"
import { QuartzTransformerPlugin } from "../types"

interface Options {
  renderEngine: "katex" | "mathjax"
}

export const Latex: QuartzTransformerPlugin<Options> = (opts?: Options) => {
  const engine = opts?.renderEngine ?? "katex"
  return {
    name: "Latex",
    markdownPlugins() {
      return [remarkMath]
    },
    htmlPlugins() {
      if (engine === "katex") {
        // 如果需要向插件传递参数，可以使用 [plugin, options] 元组
        return [[rehypeKatex, { output: "html" }]]
      } else {
        return [rehypeMathjax]
      }
    },
    externalResources() {
      if (engine === "katex") {
        return {
          css: [
            {
              // 基础 css
              content: "https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/katex.min.css",
            },
          ],
          js: [
            {
              // 修复复制行为：https://github.com/KaTeX/KaTeX/blob/main/contrib/copy-tex/README.md
              src: "https://cdnjs.cloudflare.com/ajax/libs/KaTeX/0.16.9/contrib/copy-tex.min.js",
              loadTime: "afterDOMReady",
              contentType: "external",
            },
          ],
        }
      }
    },
  }
}
```

另一个常见的转换器插件功能是解析文件并为其添加额外数据：

```ts
export const AddWordCount: QuartzTransformerPlugin = () => {
  return {
    name: "AddWordCount",
    markdownPlugins() {
      return [
        () => {
          return (tree, file) => {
            // tree 是 mdast 根元素
            // file 是 vfile
            const text = file.value
            const words = text.split(" ").length
            file.data.wordcount = words
          }
        },
      ]
    },
  }
}

// 告诉 typescript 我们添加了自定义数据字段
// 其他插件也会识别这个数据字段
declare module "vfile" {
  interface DataMap {
    wordcount: number
  }
}
```

你还可以使用 `unist-util-visit` 包的 `visit` 函数或 `mdast-util-find-and-replace` 包的 `findAndReplace` 函数对 Markdown 或 HTML AST 进行转换。

```ts
export const TextTransforms: QuartzTransformerPlugin = () => {
  return {
    name: "TextTransforms",
    markdownPlugins() {
      return [() => {
        return (tree, file) => {
          // 用斜体替换 _text_
          findAndReplace(tree, /_(.+)_/, (_value: string, ...capture: string[]) => {
            // inner 是正则 () 内的文本
            const [inner] = capture
            // 返回 mdast 节点
            // https://github.com/syntax-tree/mdast
            return {
              type: "emphasis",
              children: [{ type: 'text', value: inner }]
            }
          })

         // 移除所有链接（仅保留链接内容）
         // 通过 mdast 节点的 'type' 字段匹配
         // https://github.com/syntax-tree/mdast#link
          visit(tree, "link", (link: Link) => {
            return {
              type: "paragraph"
              children: [{ type: 'text', value: link.title }]
            }
          })
        }
      }]
    }
  }
}
```

所有转换器插件都位于 `quartz/plugins/transformers`。如果你编写了自己的转换器插件，别忘了在 `quartz/plugins/transformers/index.ts` 重新导出它。

最后提醒一句：转换器插件较为复杂，如果一时没弄明白也不用担心。可以参考内置转换器，看看它们是如何处理内容的，从而更好地实现你的需求。

## 过滤器（Filters）

过滤器对内容进行**筛选**，接收所有转换器的输出，决定实际保留哪些文件、丢弃哪些文件。

```ts
export type QuartzFilterPlugin<Options extends OptionType = undefined> = (
  opts?: Options,
) => QuartzFilterPluginInstance

export type QuartzFilterPluginInstance = {
  name: string
  shouldPublish(ctx: BuildCtx, content: ProcessedContent): boolean
}
```

过滤器插件必须定义 `name` 字段和 `shouldPublish` 函数，后者接收经过所有转换器处理的内容，根据是否应传递给发射器插件返回 `true` 或 `false`。

例如，以下是内置的移除草稿插件：

```ts title="quartz/plugins/filters/draft.ts"
import { QuartzFilterPlugin } from "../types"

export const RemoveDrafts: QuartzFilterPlugin<{}> = () => ({
  name: "RemoveDrafts",
  shouldPublish(_ctx, [_tree, vfile]) {
    // 使用转换器解析的 frontmatter
    const draftFlag: boolean = vfile.data?.frontmatter?.draft ?? false
    return !draftFlag
  },
})
```

## 发射器（Emitters）

发射器对内容进行**归约**，接收所有转换和筛选后的内容，生成输出文件。

```ts
export type QuartzEmitterPlugin<Options extends OptionType = undefined> = (
  opts?: Options,
) => QuartzEmitterPluginInstance

export type QuartzEmitterPluginInstance = {
  name: string
  emit(
    ctx: BuildCtx,
    content: ProcessedContent[],
    resources: StaticResources,
  ): Promise<FilePath[]> | AsyncGenerator<FilePath>
  partialEmit?(
    ctx: BuildCtx,
    content: ProcessedContent[],
    resources: StaticResources,
    changeEvents: ChangeEvent[],
  ): Promise<FilePath[]> | AsyncGenerator<FilePath> | null
  getQuartzComponents(ctx: BuildCtx): QuartzComponent[]
}
```

发射器插件必须定义 `name` 字段、`emit` 函数和 `getQuartzComponents` 函数。可选实现 `partialEmit`，用于增量构建。

- `emit` 负责处理所有解析和筛选后的内容，生成文件并返回创建的文件路径列表。
- `partialEmit` 是可选函数，支持增量构建。它接收变更文件信息（`changeEvents`），可选择性地只重建必要文件。若未定义，则默认使用 `emit`。
- `getQuartzComponents` 声明发射器用于构建页面的 Quartz 组件。

创建新文件可以使用 Node 的 [fs 模块](https://nodejs.org/api/fs.html)（如 `fs.cp` 或 `fs.writeFile`），也可以用 `quartz/plugins/emitters/helpers.ts` 中的 `write` 函数。`write` 的签名如下：

```ts
export type WriteOptions = (data: {
  // 构建上下文
  ctx: BuildCtx
  // 要生成的文件名（不含扩展名）
  slug: FullSlug
  // 文件扩展名
  ext: `.${string}` | ""
  // 要写入的文件内容
  content: string
}) => Promise<FilePath>
```

这是对写入输出文件夹的简单封装，并确保中间目录存在。如果你选择使用原生 Node fs API，也要确保输出到 `argv.output` 文件夹。

如果你创建的发射器插件需要渲染组件，还需注意三点：

- 组件应通过 `getQuartzComponents` 声明所用的 `QuartzComponents`。详见 [[creating components]]。
- 可用 `quartz/components/renderPage.tsx` 的 `renderPage` 函数将 Quartz 组件渲染为 HTML。
- 若需将 HTML AST 渲染为 JSX，可用 `quartz/util/jsx.ts` 的 `htmlToJsx` 函数，示例见 `quartz/components/pages/Content.tsx`。

例如，以下是简化版的内容页面插件，会渲染每个页面：

```tsx title="quartz/plugins/emitters/contentPage.tsx"
export const ContentPage: QuartzEmitterPlugin = () => {
  // 构建布局
  const layout: FullPageLayout = {
    ...sharedPageComponents,
    ...defaultContentPageLayout,
    pageBody: Content(),
  }
  const { head, header, beforeBody, pageBody, afterBody, left, right, footer } = layout
  return {
    name: "ContentPage",
    getQuartzComponents() {
      return [head, ...header, ...beforeBody, pageBody, ...afterBody, ...left, ...right, footer]
    },
    async emit(ctx, content, resources, emit): Promise<FilePath[]> {
      const cfg = ctx.cfg.configuration
      const fps: FilePath[] = []
      const allFiles = content.map((c) => c[1].data)
      for (const [tree, file] of content) {
        const slug = canonicalizeServer(file.data.slug!)
        const externalResources = pageResources(slug, file.data, resources)
        const componentData: QuartzComponentProps = {
          fileData: file.data,
          externalResources,
          cfg,
          children: [],
          tree,
          allFiles,
        }

        const content = renderPage(cfg, slug, componentData, opts, externalResources)
        const fp = await emit({
          content,
          slug: file.data.slug!,
          ext: ".html",
        })

        fps.push(fp)
      }
      return fps
    },
  }
}
```

注意它接收 `FullPageLayout` 作为选项。它由 `SharedLayout` 和 `PageLayout` 组合而成，二者都在 `quartz.layout.ts` 文件中提供。

> [!hint]
> 更多插件示例可参考 `quartz/plugins`，以便为你自己的插件提供参考！

