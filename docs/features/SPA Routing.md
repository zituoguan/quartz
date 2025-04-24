单页应用（SPA）风格的渲染。这可以防止未样式化内容的闪烁，并提升 Quartz 的流畅度。

其底层实现方式是劫持页面导航，通过 `GET` 请求获取 HTML，然后使用 [micromorph](https://github.com/natemoo-re/micromorph) 对页面进行差异比较和选择性替换。这样可以在不完全刷新页面的情况下更改页面内容，从而减少浏览器需要加载的内容量。

## 配置

- 禁用 SPA 路由：在 `quartz.config.ts` 的 [[configuration]] 中将 `enableSPA` 字段设置为 `false`。
