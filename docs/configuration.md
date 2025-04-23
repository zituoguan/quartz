---
title: 配置
---

Quartz 旨在实现极高的可配置性，即使你不会编程也能轻松上手。大多数配置只需编辑 `quartz.config.ts` 或在 `quartz.layout.ts` 中更改 [[layout|布局]] 即可完成。

> [!tip]
> 如果你使用支持 TypeScript 的文本编辑器（如 VSCode）编辑 Quartz 配置，当你配置出错时会有警告，帮助你避免配置错误！

Quartz 的配置主要分为两部分：

```ts title="quartz.config.ts"
const config: QuartzConfig = {
  configuration: { ... },
  plugins: { ... },
}
```

## 通用配置

这部分配置影响整个站点。以下是所有可配置项的简要说明：

- `pageTitle`：站点标题。生成 [[RSS Feed]] 时也会用到。
- `pageTitleSuffix`：添加在页面标题末尾的字符串。仅影响浏览器标签页标题，不影响页面顶部显示的标题。
- `enableSPA`：是否启用站点的 [[SPA 路由]]。
- `enablePopovers`：是否启用站点的 [[弹出预览]]。
- `analytics`：站点分析工具配置。可选值包括：
  - `null`：不使用分析工具；
  - `{ provider: 'google', tagId: '<your-google-tag>' }`：使用 Google Analytics；
  - `{ provider: 'plausible' }`（托管）或 `{ provider: 'plausible', host: '<your-plausible-host>' }`（自托管）：使用 [Plausible](https://plausible.io/)；
  - `{ provider: 'umami', host: '<your-umami-host>', websiteId: '<your-umami-website-id>' }`：使用 [Umami](https://umami.is/)；
  - `{ provider: 'goatcounter', websiteId: 'my-goatcounter-id' }`（托管）或 `{ provider: 'goatcounter', websiteId: 'my-goatcounter-id', host: 'my-goatcounter-domain.com', scriptSrc: 'https://my-url.to/counter.js' }`（自托管）：使用 [GoatCounter](https://goatcounter.com)；
  - `{ provider: 'posthog', apiKey: '<your-posthog-project-apiKey>', host: '<your-posthog-host>' }`：使用 [Posthog](https://posthog.com/)；
  - `{ provider: 'tinylytics', siteId: '<your-site-id>' }`：使用 [Tinylytics](https://tinylytics.app/)；
  - `{ provider: 'cabin' }` 或 `{ provider: 'cabin', host: 'https://cabin.example.com' }`（自定义域名）：使用 [Cabin](https://withcabin.com)；
  - `{provider: 'clarity', projectId: '<your-clarity-id-code' }`：使用 [Microsoft clarity](https://clarity.microsoft.com/)。项目 ID 可在概览页顶部找到。
- `locale`：用于 [[i18n]] 和日期格式化
- `baseUrl`：用于 sitemap 和 RSS feed 需要绝对 URL 的场景，指明站点的主地址。通常为你部署后的站点地址（如 `quartz.jzhao.xyz`）。不要包含协议（如 `https://`）或任何前后斜杠。
  - 如果你在 GitHub Pages 上托管且没有自定义域名，也应包含子路径。例如仓库为 `jackyzha0/quartz`，GitHub Pages 部署地址为 `https://jackyzha0.github.io/quartz`，则 `baseUrl` 应为 `jackyzha0.github.io/quartz`。
  - 注意，Quartz 4 会尽量避免使用该配置，优先使用相对路径，以确保无论部署到哪里都能正常访问。
- `ignorePatterns`：Quartz 在查找 `content` 文件夹内文件时应忽略的 [glob](<https://en.wikipedia.org/wiki/Glob_(programming)>) 模式列表。详见 [[private pages]]。
- `defaultDateType`：页面和页面列表默认显示的日期类型（创建、修改或发布）。
- `theme`：站点外观配置。
  - `cdnCaching`：若为 `true`（默认），使用 Google CDN 缓存字体，速度更快。若需自包含字体可设为 `false`。
  - `typography`：字体设置。可选任意 [Google Fonts](https://fonts.google.com/) 上的字体。
    - `title`：站点标题字体（可选，默认与 `header` 相同）
    - `header`：标题字体
    - `code`：代码块和行内代码字体
    - `body`：正文字体
  - `colors`：站点配色方案。
    - `light`：页面背景
    - `lightgray`：边框
    - `gray`：图谱链接、较重边框
    - `darkgray`：正文文本
    - `dark`：标题文本和图标
    - `secondary`：链接颜色、当前 [[graph view|图谱]] 节点
    - `tertiary`：悬停状态和已访问 [[graph view|图谱]] 节点
    - `highlight`：内部链接背景、高亮文本、[[syntax highlighting|代码高亮行]]
    - `textHighlight`：Markdown 高亮文本背景

## 插件

你可以将 Quartz 插件理解为对内容的一系列转换。

![[quartz transform pipeline.png]]

```ts title="quartz.config.ts"
plugins: {
  transformers: [...],
  filters: [...],
  emitters: [...],
}
```

- [[tags/plugin/transformer|Transformers]] 对内容进行**映射**（如解析 frontmatter、生成描述）
- [[tags/plugin/filter|Filters]] 对内容进行**过滤**（如过滤草稿）
- [[tags/plugin/emitter|Emitters]] 对内容进行**归约**（如生成 RSS feed 或按标签列出所有文件的页面）

你可以通过在 `transformers`、`filters` 和 `emitters` 字段中添加、移除或调整插件顺序，定制 Quartz 的行为。

> [!note]
> 每个节点会依次被所有 transformer 修改。有些 transformer 对顺序敏感，请注意其前后关系。

请确保将插件添加到对应类型的字段。例如，要添加 [[ExplicitPublish]] 插件（属于 [[tags/plugin/filter|Filter]]），应如下操作：

```ts title="quartz.config.ts"
filters: [
  ...
  Plugin.ExplicitPublish(),
  ...
],
```

如需移除插件，只需在 `quartz.config.ts` 中删除所有相关行。

部分插件支持自定义配置参数，未传递时将使用默认设置。

例如 [[plugins/Latex|Latex]] 插件允许通过 `renderEngine` 字段选择 Katex 或 MathJax：

```ts title="quartz.config.ts"
transformers: [
  Plugin.FrontMatter(), // 使用默认选项
  Plugin.Latex({ renderEngine: "katex" }), // 自定义选项
]
```

部分插件在 [`quartz.config.ts`](https://github.com/jackyzha0/quartz/blob/v4/quartz.config.ts) 中默认包含，更多插件及其配置见下方链接。

你可以在 [[tags/plugin|这里]] 查看所有插件及其配置选项。

如需自定义插件，请参考 [[making plugins|自定义插件开发指南]]。

## 字体

字体可用 `string` 或 `FontSpecification` 指定：

```ts
// 字符串写法
typography: {
  header: "Schibsted Grotesk",
  ...
}

// FontSpecification 写法
typography: {
  header: {
    name: "Schibsted Grotesk",
    weights: [400, 700],
    includeItalic: true,
  },
  ...
}
```
