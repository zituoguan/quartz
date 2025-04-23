---
title: "语法高亮"
tags:
  - plugin/transformer
---

该插件用于为 Quartz 中的代码块添加语法高亮。更多信息请参见 [[syntax highlighting]]。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|配置]] 页面。

该插件接受以下配置选项：

- `theme`：Shikiji 内置主题之一的 id，可分别为浅色模式和深色模式设置。默认为 `theme: { light: "github-light", dark: "github-dark" }`。可在 [Shikiji 主题列表](https://shikiji.netlify.app/themes) 查看所有主题。
- `keepBackground`：若设置为 `true`，将使用 Shikiji 主题的背景色。默认为 `false`，此时将使用 Quartz 主题的背景色。

此外，你还可以在 `quartz/styles/syntax.scss` 文件中进一步自定义颜色。

## API

- 分类：Transformer
- 函数名：`Plugin.SyntaxHighlighting()`。
- 源码：[`quartz/plugins/transformers/syntax.ts`](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/syntax.ts)。
