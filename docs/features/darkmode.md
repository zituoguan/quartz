---
title: "暗黑模式"
tags:
  - component
---

Quartz 原生支持暗黑模式，并会自动遵循用户的主题偏好。用户手动切换暗黑模式后，设置会被保存在浏览器的本地存储中，以便在后续页面加载时持续生效。

## 自定义

- 移除暗黑模式：从 `quartz.layout.ts` 中删除所有 `Component.Darkmode()` 的用法。
- 组件：`quartz/components/Darkmode.tsx`
- 样式：`quartz/components/styles/darkmode.scss`
- 脚本：`quartz/components/scripts/darkmode.inline.ts`

你也可以监听 `themechange` 事件，在主题切换时执行自定义逻辑。

```js
document.addEventListener("themechange", (e) => {
  console.log("主题已切换为 " + e.detail.theme) // "light" 或 "dark"
  // 在这里编写你的逻辑
})
```
