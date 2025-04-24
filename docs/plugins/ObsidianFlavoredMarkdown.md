---
title: ObsidianFlavoredMarkdown
tags:
  - plugin/transformer
---

该插件为[[Obsidian兼容性]]提供支持。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见[[configuration#Plugins|配置]]页面。

该插件接受以下配置选项：

- `comments`：若为`true`（默认），启用对`%%`风格Obsidian注释块的解析。
- `highlight`：若为`true`（默认），启用对内容中`==`高亮语法的解析。
- `wikilinks`：若为`true`（默认），将[[wikilinks]]转换为常规链接。
- `callouts`：若为`true`（默认），为强调内容添加对[[callouts|标注]]块的支持。
- `mermaid`：若为`true`（默认），在Markdown文件中启用[[Mermaid diagrams|Mermaid图表]]渲染。
- `parseTags`：若为`true`（默认），解析并链接内容中的标签。
- `parseArrows`：若为`true`（默认），将箭头符号转换为其HTML字符等价物。
- `parseBlockReferences`：若为`true`（默认），处理块引用，链接到特定内容块。
- `enableInHtmlEmbed`：若为`true`，允许在HTML中直接嵌入内容。默认为`false`。
- `enableYouTubeEmbed`：若为`true`（默认），允许使用外部图片Markdown语法嵌入YouTube视频和播放列表。
- `enableVideoEmbed`：若为`true`（默认），允许嵌入视频文件。
- `enableCheckbox`：若为`true`，为内容添加交互式复选框支持。默认为`false`。

> [!warning]
> 如果你使用[[Obsidian兼容性|Obsidian]]创作内容，请勿移除此插件！

## API

- 分类：Transformer
- 函数名：`Plugin.ObsidianFlavoredMarkdown()`
- 源码：[quartz/plugins/transformers/ofm.ts](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/ofm.ts)
