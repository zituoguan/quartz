---
title: CNAME
tags:
  - plugin/emitter
---

该插件会生成一个 `CNAME` 记录，将你的子域名指向你网站的默认域名。

如果你想为网站使用自定义域名（如 `quartz.example.com`），则需要使用此插件。

更多信息请参见 [[hosting|托管]] 页面。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参见 [[configuration#Plugins|配置]] 页面。

此插件无需配置选项。

## API

- 分类：Emitter
- 函数名：`Plugin.CNAME()`
- 源码：[`quartz/plugins/emitters/cname.ts`](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/emitters/cname.ts)
