---
title: "目录"
tags:
  - component
  - feature/transformer
---

Quartz 可以自动从每页的标题列表生成目录（TOC）。它还会通过用不同颜色高亮显示已滚动过的标题，来显示你当前在页面中的滚动位置。

你可以通过在该页面的 frontmatter 中添加 `enableToc: false` 来隐藏目录。

默认情况下，目录会显示从 H1（`# 标题`）到 H3（`### 标题`）的所有标题，并且仅在页面上有多个标题时才会显示。

## 自定义

目录功能由 [[TableOfContents]] 插件提供。更多自定义选项请参见插件页面。

它还需要 `TableOfContents` 组件，默认显示在右侧边栏。你可以通过自定义 [[layout]] 来更改这一点。TOC 组件可以通过 `layout` 参数进行配置，该参数可以是 `modern`（默认）或 `legacy`。

