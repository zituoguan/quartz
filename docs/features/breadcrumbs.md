---
title: "面包屑导航"
tags:
  - component
---

面包屑导航提供了一种通过其父文件夹列表在网站内导航页面层次结构的方法。

默认情况下，页面最顶部的元素是面包屑导航栏（在此页面的顶部也可以看到！）。

## 自定义

大多数配置可以通过向 `Component.Breadcrumbs()` 传递选项来完成。

例如，以下是默认配置的样子：

```typescript title="quartz.layout.ts"
Component.Breadcrumbs({
  spacerSymbol: "❯", // 面包屑之间的符号
  rootName: "Home", // 第一个/根元素的名称
  resolveFrontmatterTitle: true, // 是否通过 frontmatter 标题解析文件夹名称
  showCurrentPage: true, // 是否在面包屑中显示当前页面
})
```

在传递您自己的选项时，如果您想保留某个字段的默认值，可以省略该字段或所有这些字段。

您还可以通过调整 [[layout]]（上移或下移 `Component.Breadcrumbs()`）来调整面包屑的显示位置。

想要进行更多自定义？

-   移除面包屑：从 `quartz.layout.ts` 中删除所有 `Component.Breadcrumbs()` 的使用。
-   组件：`quartz/components/Breadcrumbs.tsx`
-   样式：`quartz/components/styles/breadcrumbs.scss`
-   脚本：内联于 `quartz/components/Breadcrumbs.tsx`

