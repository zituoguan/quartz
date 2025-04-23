---
title: Assets
tags:
  - plugin/emitter
---

该插件会将内容文件夹中所有非 Markdown 的静态资源（如图片、视频、HTML 等）进行输出。插件会遵循全局 [[configuration]] 中的 `ignorePatterns` 配置。

请注意，所有静态资源将在生成的网站中通过其路径进行访问，例如：`host.me/path/to/static.pdf`

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|配置]] 页面。

该插件没有可配置选项。

## API

- 分类：Emitter
- 函数名：`Plugin.Assets()`
- 源码：[quartz/plugins/emitters/assets.ts](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/emitters/assets.ts)
