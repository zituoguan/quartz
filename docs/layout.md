---
title: 布局
---

某些发射器（emitters）也可以输出 [HTML](https://developer.mozilla.org/zh-CN/docs/Web/HTML) 文件。为了便于自定义，这些发射器允许你完全重新排列页面布局。默认的页面布局可以在 `quartz.layout.ts` 中找到。

每个页面由多个不同的部分组成，这些部分包含 `QuartzComponents`。以下代码片段列出了你可以添加组件的所有有效部分：

```typescript title="quartz/cfg.ts"
export interface FullPageLayout {
  head: QuartzComponent // 单个组件
  header: QuartzComponent[] // 水平排列
  beforeBody: QuartzComponent[] // 垂直排列
  pageBody: QuartzComponent // 单个组件
  afterBody: QuartzComponent[] // 垂直排列
  left: QuartzComponent[] // 桌面和平板为垂直，移动端为水平
  right: QuartzComponent[] // 桌面为垂直，平板和移动端为水平
  footer: QuartzComponent // 单个组件
}
```

这些对应于页面的以下部分：

| 布局                             | 预览                                 |
| -------------------------------- | ------------------------------------ |
| 桌面端（宽度 > 1200px）          | ![[quartz-layout-desktop.png\|800]]  |
| 平板端（800px < 宽度 < 1200px）  | ![[quartz-layout-tablet.png\|800]]   |
| 移动端（宽度 < 800px）           | ![[quartz-layout-mobile.png\|800]]   |

> [!note]
> 上述图表中未显示两个额外的布局字段。
>
> 1. `head` 是一个单独的组件，用于渲染 HTML 中的 `<head>` [标签](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/head)。它不会在页面上可视化显示，仅负责文档的元数据，如标签标题、脚本和样式。
> 2. `header` 是一组水平排列的组件，显示在 `beforeBody` 部分之前。这使你可以复刻旧版 Quartz 3 的头部栏，其中包含标题、搜索栏和暗色模式切换。默认情况下，Quartz 4 不会在 `header` 中放置任何组件。

Quartz **组件**（components），类似于插件，可以通过配置选项接收额外属性。如果你熟悉 React 术语，可以将它们视为高阶组件（Higher-order Components）。

参见[所有组件列表](component.md)，了解所有可用组件及其配置选项。此外，Quartz 提供了若干内置的高阶组件用于布局组合——详见 [[layout-components]]。

如果你有兴趣进一步自定义 Quartz 的行为，也可以查看 [[creating components]] 指南。

### 布局断点

Quartz 会根据访问网站的屏幕宽度采用不同的布局。

布局断点可在 `variables.scss` 中配置。

- `mobile`：屏幕宽度低于该值时使用移动端布局。
- `desktop`：屏幕宽度高于该值时使用桌面端布局。
- 屏幕宽度介于 `mobile` 和 `desktop` 之间时使用平板端布局。

```scss
$breakpoints: (
  mobile: 800px,
  desktop: 1200px,
);
```

### 样式

大多数有意义的样式更改（如配色方案和字体）都可以通过 [[configuration#General Configuration|通用配置]] 选项轻松完成。不过，如果你想进行更深入的样式更改，可以编写自己的样式。Quartz 4 与 Quartz 3 一样，使用 [Sass](https://sass-lang.com/guide/) 进行样式编写。

你可以在 `quartz/styles/base.scss` 查看基础样式表，并在 `quartz/styles/custom.scss` 编写自己的样式。

> [!note]
> 某些组件也可能提供自己的样式！例如，`quartz/components/Darkmode.tsx` 从 `quartz/components/styles/darkmode.scss` 导入样式。如果你想自定义某个特定组件的样式，请检查该组件定义，了解其样式的定义方式。

