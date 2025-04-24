---
title: 文件夹和标签列表
tags:
  - feature/emitter
---

Quartz 会为你拥有的任何文件夹和标签生成列表页面。

## 文件夹列表

Quartz 会为该文件夹下的所有页面生成一个索引页面。这包括多级嵌套的内容。

此外，Quartz 还会为子文件夹生成页面。例如，你在嵌套文件夹 `content/abc/def/note.md` 中有一条笔记，那么 Quartz 会为 `abc` 下的所有笔记生成一个页面，也会为 `abc/def` 下的所有笔记生成一个页面。

你可以通过引用文件夹名称并加上斜杠来链接到文件夹列表，例如：`[[advanced/]]`（结果为 [[advanced/]]）。

默认情况下，Quartz 会将页面标题设为 `Folder: <文件夹名称>`，且没有描述。你可以通过在该文件夹下创建一个带有 `title` [[authoring content#Syntax|frontmatter]] 字段的 `index.md` 文件来覆盖默认设置。你在该文件中编写的任何内容也会被用作文件夹描述。

例如，对于文件夹 `content/posts`，你可以添加一个 `content/posts/index.md` 文件，为其添加特定描述。

## 标签列表

Quartz 也会为你的库中每个唯一标签创建一个索引页面，并显示所有带有该标签的笔记列表。

Quartz 还支持标签层级（如 `plugin/emitter`），并会为标签层级的每一级生成单独的标签页面。同时会在 `/tags` 生成一个默认的全局标签索引页面，显示你 Quartz 中所有标签的列表。

你可以通过在标签名前加上 `tag/` 前缀来链接到标签列表，例如：`[[tags/plugin]]`（结果为 [[tags/plugin]]）。

与文件夹列表类似，你也可以为标签页面提供描述和标题，只需为每个标签创建一个文件即可。例如，如果你想为 #component 标签创建自定义描述，可以在 `content/tags/component.md` 文件中添加标题和描述。

## 自定义

Quartz 允许你为这两种页面类型的内容定义自定义排序顺序。文件夹列表功能由 [[FolderPage]] 插件提供，标签列表由 [[TagPage]] 插件提供。自定义选项请参见插件页面。
