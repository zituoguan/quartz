---
title: "Citations"
tags:
  - plugin/transformer
---

该插件为 Quartz 增加了引用（Citation）支持。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|配置]] 页面。

该插件接受以下配置选项：

- `bibliographyFile`：参考文献文件的路径。默认为 `./bibliography.bib`。该路径相对于您的仓库根目录。
- `suppressBibliography`：是否在文档末尾隐藏参考文献。默认为 `false`。
- `linkCitations`：是否将引用链接到参考文献。默认为 `false`。
- `csl`：使用的引用格式。默认为 `apa`。更多选项请参考 [rehype-citation](https://rehype-citation.netlify.app/custom-csl)。
- `prettyLink`：是否为引用使用美化链接。默认为 `true`。

## API

- 分类：Transformer
- 函数名：`Plugin.Citations()`。
- 源码：[`quartz/plugins/transformers/citations.ts`](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/citations.ts)。
