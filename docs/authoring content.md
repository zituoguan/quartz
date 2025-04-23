---
title: 内容创作
---

你所有的 Quartz 内容都应放在 `/content` 文件夹中。Quartz 首页的内容位于 `content/index.md`。如果你已经[[index#🪴 Get Started|完成了 Quartz 的设置]]，这个文件夹应该已经初始化。该文件夹中的任何 Markdown 文件都会被 Quartz 处理。

推荐使用 [Obsidian](https://obsidian.md/) 来编辑和维护你的 Quartz。它自带优秀的编辑器和图形界面，方便你预览、编辑和链接本地文件及附件。

一切都设置好了吗？让我们[[build|构建]]并在本地预览你的 Quartz 吧！

## 语法

Quartz 以 Markdown 文件作为主要的内容创作方式，因此完全支持 Markdown 语法。默认情况下，Quartz 还内置了一些语法扩展，比如 [Github Flavored Markdown](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)（脚注、删除线、表格、任务列表）和 [Obsidian Flavored Markdown](https://help.obsidian.md/Editing+and+formatting/Obsidian+Flavored+Markdown)（[[callouts]]、[[wikilinks]]）。

此外，Quartz 还允许你在笔记中指定额外的元数据，称为 **frontmatter**。

```md title="content/note.md"
---
title: 示例标题
draft: false
tags:
  - 示例标签
---

你的内容正文写在这里。你可以在这里使用 **Markdown** :)
```

Quartz 原生支持的一些常用 frontmatter 字段：

- `title`：页面标题。如果未提供，Quartz 会使用文件名作为标题。
- `description`：用于链接预览的页面描述。
- `permalink`：页面的自定义 URL，即使文件路径更改也保持不变。
- `aliases`：该笔记的其他名称。为字符串列表。
- `tags`：该笔记的标签。
- `draft`：是否发布该页面。这也是让[[private pages|页面私有]]的一种方式。
- `date`：笔记发布的日期字符串，通常使用 `YYYY-MM-DD` 格式。

完整的 frontmatter 字段列表请参见 [[Frontmatter]]。

## 内容同步

当你对 Quartz 的内容满意后，可以将更改保存到 GitHub。
首先，确保你已经[[setting up your GitHub repository|设置好了 GitHub 仓库]]，然后执行 `npx quartz sync`。

## 个性化定制

`title`、`tags`、`aliases` 和 `cssclasses` 的 frontmatter 解析由 [[Frontmatter]] 插件实现，`date` 字段由 [[CreatedModifiedDate]] 插件处理，`description` 字段由 [[Description]] 插件处理。更多定制选项请参见各插件页面。

