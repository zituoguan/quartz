---
title: "Roam Research 兼容性"
tags:
  - feature/transformer
---

[Roam Research](https://roamresearch.com) 是一款以独特且互联方式组织知识图谱的笔记工具。

Quartz 支持将 Roam Research 的特殊 Markdown 语法（如 `{{[[components]]}}` 及其他格式）通过 [[RoamFlavoredMarkdown]] 插件转换为常规 Markdown。

```typescript title="quartz.config.ts"
plugins: {
  transformers: [
    // ...
    Plugin.RoamFlavoredMarkdown(),
    Plugin.ObsidianFlavoredMarkdown(),
    // ...
  ],
},
```

> [!warning]
> 如上所示，在 `quartz.config.ts` 中放置 `Plugin.RoamFlavoredMarkdown()` 的位置非常重要。它必须在 `Plugin.ObsidianFlavoredMarkdown()` 之前。

## 自定义

此功能由 [[RoamFlavoredMarkdown]] 插件提供。请参阅插件页面了解自定义选项。
