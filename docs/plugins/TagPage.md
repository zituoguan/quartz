---
title: TagPage
tags:
  - plugin/emitter
---

该插件会为内容中使用的每个标签生成专属页面。更多信息请参见 [[folder and tag listings]]。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参阅 [[configuration#Plugins|配置]] 页面。

这些页面使用 `quartz.layouts.ts` 中的 `defaultListPageLayout` 进行展示。内容部分使用 `TagContent` 组件。如果你想修改布局，需要直接编辑该文件（`quartz/components/pages/TagContent.tsx`）。

该插件支持以下配置选项：

- `sort`：类型为 `(f1: QuartzPluginData, f2: QuartzPluginData) => number{:ts}` 的函数，用于排序条目。默认按日期排序，若日期相同则按字典序排序。

## API

- 分类：Emitter
- 函数名：`Plugin.TagPage()`。
- 源码：[`quartz/plugins/emitters/tagPage.tsx`](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/emitters/tagPage.tsx)。
