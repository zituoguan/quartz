---
title: FolderPage
tags:
  - plugin/emitter
---

该插件为文件夹生成索引页面，为每个包含多个内容文件的文件夹创建一个列表页面。更多信息请参见 [[folder and tag listings]]。

示例：[[advanced/|Advanced]]

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|Configuration]] 页面。

页面使用 `quartz.layouts.ts` 中的 `defaultListPageLayout` 进行展示。内容部分使用 `FolderContent` 组件。如果需要修改布局，必须直接编辑该组件（`quartz/components/pages/FolderContent.tsx`）。

该插件接受以下配置选项：

- `sort`：类型为 `(f1: QuartzPluginData, f2: QuartzPluginData) => number{:ts}` 的函数，用于排序条目。默认按日期排序，相同日期时按字典序排序。

## API

- 分类：Emitter
- 函数名：`Plugin.FolderPage()`。
- 源码：[`quartz/plugins/emitters/folderPage.tsx`](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/emitters/folderPage.tsx)。
