---
title: "CreatedModifiedDate"
tags:
  - plugin/transformer
---

该插件通过三种潜在数据源（frontmatter 元数据、Git 历史和文件系统）确定文档的创建、修改和发布日期。更多信息请参见 [[authoring content#Syntax]]。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|配置]] 页面。

该插件接受以下配置选项：

- `priority`：用于获取日期信息的数据源，优先级从高到低。可选值为 `"frontmatter"`、`"git"` 和 `"filesystem"`。默认值为 `["frontmatter", "git", "filesystem"]`。

加载 frontmatter 时，使用 [[Frontmatter#List]] 的值。

> [!warning]
> 如果你依赖 `git` 获取日期，请确保在 `quartz.config.ts` 中将 `defaultDateType` 设置为 `modified`。
>
> 根据你 [[hosting|部署]] Quartz 的方式，本地文件的 `filesystem` 日期可能与最终日期不一致。在这种情况下，建议使用 `git` 或 `frontmatter` 以确保日期的准确性。

## API

- 分类：Transformer
- 函数名：`Plugin.CreatedModifiedDate()`。
- 源码：[`quartz/plugins/transformers/lastmod.ts`](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/lastmod.ts)。
