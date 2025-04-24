---
title: "Obsidian 兼容性"
tags:
  - feature/transformer
---

Quartz 最初被设计为一个将 Obsidian 笔记库发布为网站的工具。即使随着 Quartz 的功能范围不断扩大，它依然能够与 Obsidian 无缝协作。

默认情况下，Quartz 内置了 [[ObsidianFlavoredMarkdown]] 插件，这是一个转换器插件，为 [Obsidian Flavored Markdown](https://help.obsidian.md/Editing+and+formatting/Obsidian+Flavored+Markdown) 提供支持。这包括对 [[wikilinks]] 和 [[Mermaid diagrams]] 等功能的支持。

它还内置了对 [frontmatter 解析](https://help.obsidian.md/Editing+and+formatting/Properties) 的支持，通过 [[Frontmatter]] 转换器插件，支持与 Obsidian 相同的字段。

最后，Quartz 还提供了 [[CrawlLinks]] 插件，可以自定义 Quartz 的链接解析行为，使其与 Obsidian 保持一致。

## 配置

这些功能由 [[ObsidianFlavoredMarkdown]]、[[Frontmatter]] 和 [[CrawlLinks]] 插件提供。有关自定义选项，请参阅各插件页面。

