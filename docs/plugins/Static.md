---
title: Static
tags:
  - plugin/emitter
---

该插件会发出 Quartz 所需的所有静态资源。例如，用于需要固定位置的字体和图片（如横幅和图标）。该插件会遵循全局 [[configuration]] 中的 `ignorePatterns` 配置。

> [!important]
> 这与 [[Assets]] 不同。[[Static]] 插件的资源位于 `quartz/static` 下，而 [[Assets]] 会渲染所有位于 `content` 下的静态资源，适用于你的 Markdown 内容中直接引用的图片、视频、音频等。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|配置]] 页面。

该插件没有配置选项。

## API

- 分类：Emitter
- 函数名：`Plugin.Static()`
- 源码：[`quartz/plugins/emitters/static.ts`](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/emitters/static.ts)
