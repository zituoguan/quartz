---
title: Roam风格Markdown
tags:
  - plugin/transformer
---

该插件为 [Roam Research](https://roamresearch.com) 兼容性提供支持。更多信息请参见 [[Roam Research Compatibility]]。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[Configuration#Plugins|配置]] 页面。

该插件接受以下配置选项：

- `orComponent`：若为 `true`（默认），将 Roam 的 `{{ or:ONE|TWO|THREE }}` 短代码转换为 HTML 下拉选项。
- `TODOComponent`：若为 `true`（默认），将 Roam 的 `{{[[TODO]]}}` 短代码转换为 HTML 复选框。
- `DONEComponent`：若为 `true`（默认），将 Roam 的 `{{[[DONE]]}}` 短代码转换为已选中的 HTML 复选框。
- `videoComponent`：若为 `true`（默认），将 Roam 的 `{{[[video]]:URL}}` 短代码转换为嵌入式 HTML 视频。
- `audioComponent`：若为 `true`（默认），将 Roam 的 `{{[[audio]]:URL}}` 短代码转换为嵌入式 HTML 音频。
- `pdfComponent`：若为 `true`（默认），将 Roam 的 `{{[[pdf]]:URL}}` 短代码转换为嵌入式 HTML PDF 查看器。
- `blockquoteComponent`：若为 `true`（默认），将 Roam 的 `{{[[>]]}}` 短代码转换为 Quartz 引用块。

## API

- 分类：Transformer
- 函数名：`Plugin.RoamFlavoredMarkdown()`。
- 源码：[`quartz/plugins/transformers/roam.ts`](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/transformers/roam.ts)。
