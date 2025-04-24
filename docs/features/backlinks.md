---
title: 反向链接
tags:
  - component
---

一个笔记的反向链接是指从另一个笔记指向该笔记的链接。如果启用了该功能，反向链接窗格中的链接还具有丰富的 [[弹出预览]] 功能。

## 自定义

- 移除反向链接：从 `quartz.layout.ts` 中删除所有 `Component.Backlinks()` 的用法。
- 为空时隐藏：如果给定页面不包含任何反向链接，则隐藏 `Backlinks`（默认为 `true`）。要禁用此功能，请使用 `Component.Backlinks({ hideWhenEmpty: false })`。
- 组件：`quartz/components/Backlinks.tsx`
- 样式：`quartz/components/styles/backlinks.scss`
- 脚本：`quartz/components/scripts/search.inline.ts`
