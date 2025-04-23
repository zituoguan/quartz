---
title: RemoveDrafts
tags:
  - plugin/filter
---

该插件会过滤掉您的库中的内容，仅保留已定稿的内容。这可以防止[[私有页面]]被发布。默认情况下，它会过滤掉所有 frontmatter 中包含 `draft: true` 的页面，其他页面则保持不变。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见[[configuration#Plugins|配置]]页面。

该插件没有配置选项。

## API

- 分类：过滤器
- 函数名：`Plugin.RemoveDrafts()`
- 源码：[quartz/plugins/filters/draft.ts](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/filters/draft.ts)
