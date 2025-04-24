---
title: 弹出预览
---

像维基百科一样，当你在 Quartz 中将鼠标悬停在链接上时，会弹出一个页面预览窗口，你可以滚动查看整个内容。链接到标题的情况也会自动滚动弹窗，使该标题显示在视图中。

默认情况下，由于 [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) 限制，Quartz 只会为你的知识库内的页面获取预览。它通过选择所有带有 `popover-hint` 类的 HTML 元素来实现。对于大多数页面，这包括页面标题、页面元数据（如字数和阅读时间）、标签以及实际页面内容。

在 [[创建组件|自定义组件]] 时，你也可以添加 `popover-hint` 类，使其也能在弹窗中显示。

类似于 Obsidian，[[quartz layout.png|通过维基链接引用的图片]] 也可以以弹窗形式预览。

## 配置

- 移除弹窗预览：在 `quartz.config.ts` 文件中将 `enablePopovers` 字段设置为 `false`。
- 样式文件：`quartz/components/styles/popover.scss`
- 脚本文件：`quartz/components/scripts/popover.inline.ts`

