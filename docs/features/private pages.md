---
title: 私有页面
tags:
  - feature/filter
---

有些笔记你可能不希望发布到网站上。Quartz 通过两种机制支持这一需求，并且可以结合使用：

## 过滤插件

[[making plugins#Filters|过滤插件]] 是根据特定条件过滤内容的插件。默认情况下，Quartz 使用 [[RemoveDrafts]] 插件，该插件会过滤掉 frontmatter 中包含 `draft: true` 的笔记。

如果你只想发布特定的笔记，可以改用 [[ExplicitPublish]] 插件，它会过滤掉所有笔记，只有 frontmatter 中包含 `publish: true` 的笔记会被发布。

> [!warning]
> 无论使用哪种过滤插件，**所有非 Markdown 文件都会被输出，并在最终构建中公开可用。** 这包括图片、语音录音、PDF 等文件。防止这种情况并仍然能够嵌入本地图片的一种方法是专门创建一个公共媒体文件夹，并在 ignorePatterns 数组中添加以下两个模式。
>
> `"!(PublicMedia)**/!(*.md)", "!(*.md)"`

## `ignorePatterns`

这是 `quartz.config.ts` 主 [[configuration]] 下的一个字段，允许你指定一组模式来彻底排除解析。任何有效的 [fast-glob](https://github.com/mrmlnc/fast-glob#pattern-syntax) 模式都可以使用。

> [!note]
> Bash 的 glob 语法与 fast-glob 略有不同，使用 bash 语法可能会导致意外结果。

常见示例包括：

- `some/folder`：排除整个 `some/folder` 文件夹
- `*.md`：排除所有 `.md` 扩展名的文件
- `!*.md`：排除所有 _不是_ `.md` 扩展名的文件
- `**/private`：排除任意层级下名为 `private` 的文件或文件夹

> [!warning]
> 无论通过插件还是 `ignorePatterns` 标记为私有，只会阻止页面被包含在最终构建的网站中。如果你的 GitHub 仓库是公开的，也请确保在 Quartz 的 `.gitignore` 文件中忽略这些内容。更多信息请参阅 `git` [文档](https://git-scm.com/docs/gitignore#_pattern_format)。

