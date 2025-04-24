---
title: "OxHugo 兼容性"
tags:
  - feature/transformer
---

[org-roam](https://www.orgroam.com/) 是一个基于纯文本的个人知识管理系统，适用于 [emacs](https://en.wikipedia.org/wiki/Emacs)。[ox-hugo](https://github.com/kaushalmodi/ox-hugo) 是一个 org 导出后端，可以将 `org-mode` 文件导出为 [Hugo](https://gohugo.io/) 兼容的 Markdown。

由于 ox-hugo 生成的 Markdown 并不是纯 Markdown，而是带有 Hugo 特定语法的，因此我们需要对其进行转换以适配 Quartz。这一转换由 [[OxHugoFlavoredMarkdown]] 插件完成。虽然该插件是专为 `ox-hugo` 设计的，但它也适用于任何 Hugo 特定的 Markdown。

```typescript title="quartz.config.ts"
plugins: {
  transformers: [
    Plugin.FrontMatter({ delims: "+++", language: "toml" }), // 如果是 toml frontmatter
    // ...
    Plugin.OxHugoFlavouredMarkdown(),
    Plugin.GitHubFlavoredMarkdown(),
    // ...
  ],
},
```

## 用法

Quartz 默认无法识别 `org-roam` 文件，因为它们不是 Markdown。你需要使用像 `ox-hugo` 这样的外部工具，将 `org-roam` 文件导出为 Markdown 内容，并负责管理静态资源，确保它们在最终输出中可用。

## 配置

该功能由 [[OxHugoFlavoredMarkdown]] 插件提供。有关自定义选项，请参阅插件页面。
