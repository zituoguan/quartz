---
title: 引用
tags:
  - feature/transformer
---

Quartz 使用 [rehype-citation](https://github.com/timlrx/rehype-citation) 来支持解析 BibTex 参考文献文件。

在默认配置下，引用键 `[@templeton2024scaling]` 会被导出为 `(Templeton 等, 2024)`。

> [!example]- BibTex 文件
>
> ```bib title="bibliography.bib"
> @article{templeton2024scaling,
>   title={Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet},
>   author={Templeton, Adly and Conerly, Tom and Marcus, Jonathan and Lindsey, Jack and Bricken, Trenton and Chen, Brian and Pearce, Adam and Citro, Craig and Ameisen, Emmanuel and Jones, Andy and Cunningham, Hoagy and Turner, Nicholas L and McDougall, Callum and MacDiarmid, Monte and Freeman, C. Daniel and Sumers, Theodore R. and Rees, Edward and Batson, Joshua and Jermyn, Adam and Carter, Shan and Olah, Chris and Henighan, Tom},
>   year={2024},
>   journal={Transformer Circuits Thread},
>   url={https://transformer-circuits.pub/2024/scaling-monosemanticity/index.html}
> }
> ```

> [!note] 参考文献的行为
>
> 默认情况下，参考文献会被包含在文件末尾。要控制参考文献插入的位置，请使用 `[^ref]`
>
> 更多信息请参考 `rehype-citation` 文档。

## 自定义

引用解析是 [[plugins/Citations|Citation]] 插件的功能。**该插件默认未启用**。自定义选项请参见插件页面。
