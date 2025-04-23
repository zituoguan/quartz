---
title: "Latex"
tags:
  - plugin/transformer
---

该插件为 Quartz 增加了 LaTeX 支持。更多信息请参见 [[features/Latex|Latex]]。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|配置]] 页面。

该插件接受以下配置选项：

- `renderEngine`：用于渲染 LaTeX 公式的引擎。可选值为 `"katex"`（[KaTeX](https://katex.org/)）、`"mathjax"`（[MathJax](https://www.mathjax.org/) [SVG 渲染](https://docs.mathjax.org/en/latest/output/svg.html)）或 `"typst"`（[Typst](https://typst.app/)，一种新的 LaTeX 公式排版方式）。默认为 KaTeX。
- `customMacros`：所有 LaTeX 块的自定义宏。格式为键值对，键为新命令名，值为宏的展开内容。例如：`{"\\R": "\\mathbb{R}"}`

> [!note] Typst 支持
>
> 目前，typst 不支持行内公式

## API

- 分类：Transformer
- 函数名：`Plugin.Latex()`
- 源码：[quartz/plugins/transformers/latex.ts](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/latex.ts)
