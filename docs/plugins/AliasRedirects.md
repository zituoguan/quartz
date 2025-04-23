---
title: AliasRedirects
tags:
  - plugin/emitter
---

该插件会为内容文件前言（frontmatter）中定义的别名（alias）和永久链接（permalink）生成 HTML 重定向页面。

例如，`foo.md` 文件的前言如下：

```md title="foo.md"
---
title: "Foo"
alias:
  - "bar"
---
```

此时，访问 `host.me/bar` 会被永久重定向到 `host.me/foo`。

请注意，这些是永久重定向。

该发射器支持以下别名字段：

- `aliases`
- `alias`

> [!note]
> 有关如何添加、移除或配置插件的信息，请参阅 [[configuration#Plugins|配置]] 页面。

该插件没有配置选项。

## API

- 分类：Emitter
- 函数名：`Plugin.AliasRedirects()`
- 源码：[quartz/plugins/emitters/aliases.ts](https://github.com/jackyzha0/quartz/blob/v4/quartz/plugins/emitters/aliases.ts)
