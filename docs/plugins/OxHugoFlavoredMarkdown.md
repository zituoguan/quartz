---
title: OxHugoFlavoredMarkdown
tags:
  - plugin/transformer
---

该插件为 [ox-hugo](https://github.com/kaushalmodi/ox-hugo) 提供兼容性支持。更多信息请参见 [[OxHugo compatibility]]。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|Configuration]] 页面。

该插件支持以下配置选项：

- `wikilinks`：若为 `true`（默认），将 Hugo 的 `{{ relref }}` 短代码转换为 Quartz 的 [[wikilinks]]。
- `removePredefinedAnchor`：若为 `true`（默认），会从标题中移除预定义锚点。
- `removeHugoShortcode`：若为 `true`（默认），会从内容中移除 Hugo 短代码语法（`{{}}`）。
- `replaceFigureWithMdImg`：若为 `true`（默认），会将 `<figure/>` 替换为 `![]()`。
- `replaceOrgLatex`：若为 `true`（默认），会将 Org-mode 的 [[features/Latex|Latex]] 片段转换为 Quartz 兼容的 LaTeX（行内用 `$` 包裹，块级用 `$$` 包裹）。

> [!warning]
> 虽然可以与 [[ObsidianFlavoredMarkdown]] 一起使用，但不推荐这样做，因为可能会导致文件发生意外更改。请谨慎使用。
>
> 如果你使用 `toml` frontmatter，请确保相应配置 [[Frontmatter]] 插件。示例请参见 [[OxHugo compatibility]]。

## API

- 分类：Transformer
- 函数名：`Plugin.OxHugoFlavoredMarkdown()`。
- 源码：[`quartz/plugins/transformers/oxhugofm.ts`](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/oxhugofm.ts)。
