---
title: 全文搜索
tags:
  - component
---

Quartz 的全文搜索由 [Flexsearch](https://github.com/nextapps-de/flexsearch) 提供支持。即使是包含多达 50 万字的 Quartz，也能在 10 毫秒内返回搜索结果。

你可以通过点击搜索栏或按下 `⌘`/`ctrl` + `K` 打开搜索。每次查询会显示前 5 个搜索结果。匹配的子词会被高亮显示，并摘录最相关的 30 个单词。点击搜索结果会跳转到对应页面。

要通过标签搜索内容，可以按 `⌘`/`ctrl` + `shift` + `K`，或在查询前加上 `#`（例如 `#components`）。

该组件也支持键盘操作：Tab 和 Shift+Tab 可前后切换搜索结果，Enter 会跳转到高亮结果（默认是第一个结果）。你也可以用 `ArrowUp` 和 `ArrowDown` 导航搜索结果。

> [!info]
> 搜索功能需要在 [[configuration]] 中启用 `ContentIndex` emitter 插件。

### 索引行为

默认情况下，它会索引站点上的每个页面，并**移除 Markdown 语法**。例如，链接的 URL 不会被索引。

它能正确地对中文、韩文和日文字符进行分词，并为标题、内容和标签分别建立索引，标题匹配的权重高于内容匹配。

## 自定义

- 移除搜索：从 `quartz.layout.ts` 中删除所有 `Component.Search()` 的用法。
- 组件：`quartz/components/Search.tsx`
- 样式：`quartz/components/styles/search.scss`
- 脚本：`quartz/components/scripts/search.inline.ts`
  - 你可以根据需要编辑 `contextWindowWords`、`numSearchResults` 或 `numTagResults`

