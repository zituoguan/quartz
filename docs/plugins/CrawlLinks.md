---
title: CrawlLinks
tags:
  - plugin/transformer
---

该插件解析链接并处理它们以指向正确的位置。它对于嵌入式链接（如图片）也是必需的。更多信息请参见 [[Obsidian compatibility]]。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|Configuration]] 页面。

该插件接受以下配置选项：

- `markdownLinkResolution`：设置解析 Markdown 路径的策略，可选值为 `"absolute"`（默认）、`"relative"` 或 `"shortest"`。建议与 [[Obsidian compatibility|Obsidian]] 中的设置保持一致。
  - `absolute`：相对于内容文件夹根目录的路径。
  - `relative`：相对于当前链接文件的路径。
  - `shortest`：文件名。如果文件名不足以唯一标识文件，则使用完整的绝对路径。
- `prettyLinks`：如果为 `true`（默认），则通过移除文件夹路径简化链接，使其更易读（例如 `folder/deeply/nested/note` 变为 `note`）。
- `openLinksInNewTab`：如果为 `true`，则将外部链接配置为在新标签页中打开。默认为 `false`。
- `lazyLoad`：如果为 `true`，则为资源元素（如 `img`、`video` 等）添加懒加载，以提升页面加载性能。默认为 `false`。
- `externalLinkIcon`：为外部链接添加图标（默认为 `true`），以便与内部链接区分。

> [!warning]
> 不建议移除此插件，否则页面可能无法正常工作。

## API

- 分类：Transformer
- 函数名：`Plugin.CrawlLinks()`。
- 源码：[quartz/plugins/transformers/links.ts](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/links.ts)。
