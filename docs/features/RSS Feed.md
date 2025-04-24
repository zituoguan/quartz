Quartz 通过生成一个 `index.xml` 文件，为你的网站所有内容提供 RSS 订阅功能。RSS 订阅器可以订阅该文件。由于 RSS 规范的要求，你需要在 [[configuration]] 中正确设置 `baseUrl` 属性，RSS 订阅器才能正确识别。

> [!info]
> 部署后，生成的 RSS 链接默认会在 `https://${baseUrl}/index.xml` 提供。
>
> 你可以通过给 [[ContentIndex]] 插件传递 `rssSlug` 选项，自定义 `index.xml` 的路径。

## 配置

此功能由 [[ContentIndex]] 插件提供。更多自定义选项请参见插件页面。
