---
title: Frontmatter
tags:
  - plugin/transformer
---

该插件使用 [gray-matter](https://github.com/jonschlinkert/gray-matter) 库解析页面的 frontmatter（前言信息）。更多信息请参见 [[authoring content#Syntax]]、[[Obsidian compatibility]] 和 [[OxHugo compatibility]]。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|配置]] 页面。

该插件接受以下配置选项：

- `delimiters`：用于 frontmatter 的分隔符。可以是一个值（如 `"---"`），也可以为起始和结束分隔符分别指定不同的值（如 `["---", "~~~"]`）。默认为 `"---"`。
- `language`：用于解析 frontmatter 的语言。可选 `yaml`（默认）或 `toml`。

> [!warning]
> 不可移除此插件，否则 Quartz 将无法正常工作。

## 支持的 Frontmatter 列表

Quartz 支持以下 frontmatter 字段：

- 标题
  - `title`
- 描述
  - `description`
- 固定链接
  - `permalink`
- 评论
  - `comments`
- 语言
  - `lang`
- 发布
  - `publish`
- 草稿
  - `draft`
- 启用目录
  - `enableToc`
- 标签
  - `tags`
  - `tag`
- 别名
  - `aliases`
  - `alias`
- CSS 类
  - `cssclasses`
  - `cssclass`
- 社交描述
  - `socialDescription`
- 社交图片
  - `socialImage`
  - `image`
  - `cover`
- 创建时间
  - `created`
  - `date`
- 修改时间
  - `modified`
  - `lastmod`
  - `updated`
  - `last-modified`
- 发布时间
  - `published`
  - `publishDate`
  - `date`

## API

- 分类：Transformer
- 函数名：`Plugin.Frontmatter()`
- 源码：[quartz/plugins/transformers/frontmatter.ts](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/frontmatter.ts)
