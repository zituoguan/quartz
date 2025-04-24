---
title: 最近笔记
tags: component
---

Quartz 可以根据一些筛选和排序条件生成最近笔记的列表。虽然该组件默认未包含在任何 [[layout]] 中，但你可以在 `quartz.layout.ts` 中通过使用 `Component.RecentNotes` 添加它。

## 自定义

- 更改标题（默认为“最近笔记”）：传递额外参数 `Component.RecentNotes({ title: "最近写作" })`
- 更改最近笔记数量：传递额外参数 `Component.RecentNotes({ limit: 5 })`
- 是否显示笔记标签（默认为 true）：`Component.RecentNotes({ showTags: false })`
- 显示“查看更多”链接：传递额外参数 `Component.RecentNotes({ linkToMore: "tags/components" })`。该字段应为已存在页面的完整 slug。
- 自定义筛选：传递额外参数 `Component.RecentNotes({ filter: someFilterFunction })`。筛选函数应具有签名 `(f: QuartzPluginData) => boolean`。
- 自定义排序：传递额外参数 `Component.RecentNotes({ sort: someSortFunction })`。默认情况下，Quartz 会按日期排序并在有相同日期时按字母顺序排序。排序函数应具有签名 `(f1: QuartzPluginData, f2: QuartzPluginData) => number`。可参考 `quartz/components/PageList.tsx` 中的 `byDateAndAlphabetical` 示例。
- 组件文件：`quartz/components/RecentNotes.tsx`
- 样式文件：`quartz/components/styles/recentNotes.scss`

