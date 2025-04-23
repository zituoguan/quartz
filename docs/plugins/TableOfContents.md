---
title: TableOfContents
tags:
  - plugin/transformer
---

该插件为 Markdown 文档生成目录（TOC）。更多信息请参见 [[table of contents]]。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|配置]] 页面。

该插件支持以下配置选项：

- `maxDepth`：限制 TOC 包含的标题深度，范围为 `1`（仅顶级标题）到 `6`（所有标题级别）。默认值为 `3`。
- `minEntries`：显示 TOC 所需的最小标题数。默认值为 `1`。
- `showByDefault`：如果为 `true`（默认），TOC 默认显示。可通过 frontmatter 设置覆盖。
- `collapseByDefault`：如果为 `true`，TOC 初始为折叠状态。默认值为 `false`。

> [!warning]
> 此插件需要在 `quartz.layout.ts` 中包含 `Component.TableOfContents` 组件，以确定 TOC 的显示位置。否则不会显示任何内容。两者应始终一起添加或移除。

## API

- 分类：Transformer
- 函数名：`Plugin.TableOfContents()`。
- 源码：[`quartz/plugins/transformers/toc.ts`](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/toc.ts)。
