---
title: 标注框（Callouts）
tags:
  - feature/transformer
---

Quartz 支持与 Obsidian 相同的 Admonition 标注框语法。

包括：

- 12 种不同类型的标注框（每种有多个别名）
- 可折叠的标注框

```
> [!info] 标题
> 这是一个标注框！
```

参见[这里的官方文档，了解支持的类型和语法](https://help.obsidian.md/Editing+and+formatting/Callouts)。

> [!warning]
> 标注框没有显示？即使已启用，也可能需要调整插件顺序，让 [[ObsidianFlavoredMarkdown]] 位于 [[SyntaxHighlighting]] 之后。

## 自定义

标注框功能由 [[ObsidianFlavoredMarkdown]] 插件提供。请参阅插件页面了解如何启用或禁用。

你可以通过自定义 `quartz/styles/callouts.scss` 文件来修改图标。

### 添加自定义标注框

默认情况下，自定义标注框会应用 `note` 样式。要实现更炫酷的效果，需要在 `custom.scss` 中添加如下内容：

```scss title="quartz/styles/custom.scss"
.callout {
  &[data-callout="custom"] {
    --color: #customcolor;
    --border: #custombordercolor;
    --bg: #custombg;
    --callout-icon: url("data:image/svg+xml; utf8, <custom formatted svg>"); //SVG 图标代码
  }
}
```

> [!warning]
> 请确保 SVG 在放入 CSS 前已进行 URL 编码。你可以使用[这个工具](https://yoksel.github.io/url-encoder/)来帮助编码。

## 示例展示

> [!info]
> 默认标题

> [!question]+ 标注框可以 _嵌套_ 吗？
>
> > [!todo]- 可以！而且还能折叠！
> >
> > > [!example] 甚至可以多层嵌套。

> [!note]
> 别名："note"

> [!abstract]
> 别名："abstract", "summary", "tldr"

> [!info]
> 别名："info"

> [!todo]
> 别名："todo"

> [!tip]
> 别名："tip", "hint", "important"

> [!success]
> 别名："success", "check", "done"

> [!question]
> 别名："question", "help", "faq"

> [!warning]
> 别名："warning", "attention", "caution"

> [!failure]
> 别名："failure", "missing", "fail"

> [!danger]
> 别名："danger", "error"

> [!bug]
> 别名："bug"

> [!example]
> 别名："example"

> [!quote]
> 别名："quote", "cite"

