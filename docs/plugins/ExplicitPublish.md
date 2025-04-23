---
title: ExplicitPublish
tags:
  - plugin/filter
---

该插件根据 frontmatter 中的显式 `publish` 标志过滤内容，仅允许明确标记为发布的内容通过。这是 [[RemoveDrafts]] 的“选择加入”版本。更多信息请参见 [[private pages]]。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|配置]] 页面。

该插件没有配置选项。

## API

- 分类：过滤器
- 函数名：`Plugin.ExplicitPublish()`
- 源码：[quartz/plugins/filters/explicit.ts](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/filters/explicit.ts)

