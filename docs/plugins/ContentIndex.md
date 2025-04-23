---
title: ContentIndex
tags:
  - plugin/emitter
---

该插件为您的网站生成 RSS 和 XML 网站地图。[[RSS Feed]] 允许用户订阅您的网站内容，网站地图则帮助搜索引擎更好地索引您的站点。此外，插件还会生成一个 `contentIndex.json` 文件，供搜索和图谱等动态前端组件使用。

此插件会生成站点内容的综合索引，并额外生成如网站地图、RSS 源等资源。

> [!note]
> 有关如何添加、移除或配置插件的信息，请参阅 [[configuration#Plugins|配置]] 页面。

该插件支持以下配置选项：

- `enableSiteMap`：若为 `true`（默认），则生成包含所有站点 URL 的 sitemap XML 文件（`sitemap.xml`），便于搜索引擎发现内容。
- `enableRSS`：若为 `true`（默认），则生成包含最新内容更新的 RSS 源（`index.xml`）。
- `rssLimit`：定义 RSS 源中包含的最大条目数，帮助聚焦于最新或最相关的内容。默认值为 `10`。
- `rssFullHtml`：若为 `true`，RSS 源将包含完整的 HTML 内容，否则仅包含摘要。
- `rssSlug`：生成的 RSS 源 XML 文件的 slug。默认为 `"index"`。
- `includeEmptyFiles`：若为 `true`（默认），则在生成的索引和资源中包含无正文内容的文件。

## API

- 分类：Emitter
- 函数名：`Plugin.ContentIndex()`。
- 源码：[`quartz/plugins/emitters/contentIndex.ts`](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/emitters/contentIndex.ts)。
