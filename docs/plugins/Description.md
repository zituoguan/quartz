---
title: Description
tags:
  - plugin/transformer
---

该插件会生成用于 HTML `head` 元数据、[[RSS Feed]] 以及 [[文件夹和标签列表]] 的描述。如果没有正文内容，描述将作为标题和列表之间的文本显示。

如果 frontmatter 中包含 `description` 属性，则会优先使用该属性（参见 [[authoring content#Syntax]]）。否则，插件会尽量使用内容的前几句话来达到目标描述长度。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参阅 [[configuration#Plugins|配置]] 页面。

该插件支持以下配置选项：

- `descriptionLength`：生成描述的最大长度，默认为 150 个字符。截断会在第一个超过该长度的句子后进行。
- `replaceExternalLinks`：如果为 `true`（默认），则会将描述中的外部链接替换为其域名和路径（例如 `https://domain.tld/some_page/another_page?query=hello&target=world` 会被替换为 `domain.tld/some_page/another_page`）。

## API

- 分类：Transformer
- 函数名：`Plugin.Description()`。
- 源码：[quartz/plugins/transformers/description.ts](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/description.ts)。
