---
title: GitHubFlavoredMarkdown
tags:
  - plugin/transformer
---

该插件增强了 Markdown 的处理能力，支持 GitHub Flavored Markdown（GFM），增加了自动链接文本、脚注、删除线、表格和任务列表等功能。

此外，该插件还可选提供排版优化（如将直引号转换为弯引号、破折号转换为短破折号/长破折号、省略号等）以及为标题自动添加链接（在悬停时显示一个符号）。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参阅 [[configuration#Plugins|配置]] 页面。

该插件支持以下配置选项：

- `enableSmartyPants`：为 true 时启用排版增强。默认值为 true。
- `linkHeadings`：为 true 时自动为标题添加链接。默认值为 true。

## API

- 分类：Transformer
- 函数名：`Plugin.GitHubFlavoredMarkdown()`
- 源码：[quartz/plugins/transformers/gfm.ts](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/gfm.ts)
