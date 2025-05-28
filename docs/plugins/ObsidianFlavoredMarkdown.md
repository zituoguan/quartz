---
title: ObsidianFlavoredMarkdown
tags:
  - plugin/transformer
---

该插件为[[Obsidian兼容性]]提供支持。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见[[configuration#Plugins|配置]]页面。

该插件接受以下配置选项：

- `comments`: 如果为 `true`（默认值），则启用对 `%%` 样式 Obsidian 注释块的解析。
- `highlight`: 如果为 `true`（默认值），则启用对内容中 `==` 样式高亮标记的解析。
- `wikilinks`: 如果为 `true`（默认值），则将 [[wikilinks]] 转换为常规链接。
- `callouts`: 如果为 `true`（默认值），则添加对 [[callouts|callout]] 块的支持，用于强调内容。
- `mermaid`: 如果为 `true`（默认值），则启用在 Markdown 文件中渲染 [[Mermaid diagrams|Mermaid 图表]]。
- `parseTags`: 如果为 `true`（默认值），则解析内容中的标签并为其创建链接。
- `parseArrows`: 如果为 `true`（默认值），则将箭头符号转换为其 HTML 字符等效项。
- `parseBlockReferences`: 如果为 `true`（默认值），则处理块引用，链接到特定的内容块。
- `enableInHtmlEmbed`: 如果为 `true`，则允许直接在 HTML 中嵌入内容。默认为 `false`。
- `enableYouTubeEmbed`: 如果为 `true`（默认值），则启用使用外部图像 Markdown 语法嵌入 YouTube 视频和播放列表。
- `enableVideoEmbed`: 如果为 `true`（默认值），则启用视频文件的嵌入。
- `enableCheckbox`: 如果为 `true`，则添加对内容中交互式复选框的支持。默认为 `false`。
- `disableBrokenWikilinks`: 如果为 `true`，则将指向不存在笔记的链接替换为变暗的禁用链接。默认为 `false`。

> [!warning]
> 如果你使用[[Obsidian兼容性|Obsidian]]创作内容，请勿移除此插件！

## API

- 分类：Transformer
- 函数名：`Plugin.ObsidianFlavoredMarkdown()`
- 源码：[quartz/plugins/transformers/ofm.ts](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/ofm.ts)
