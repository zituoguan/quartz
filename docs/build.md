---
title: "构建你的 Quartz"
---

一旦你已经[[index#🪴 Get Started|初始化]]了 Quartz，让我们看看它在本地的效果：

```bash
npx quartz build --serve
```

这将启动一个本地 Web 服务器，在你的电脑上运行 Quartz。打开网页浏览器并访问 `http://localhost:8080/` 进行查看。

> [!hint] 标志和选项
> 如需完整的帮助选项，可以运行 `npx quartz build --help`。
>
> 这些选项大多数都有合理的默认值，但如果你有自定义需求，也可以覆盖它们：
>
> - `-d` 或 `--directory`：内容文件夹，通常为 `content`
> - `-v` 或 `--verbose`：输出更多日志信息
> - `-o` 或 `--output`：输出文件夹，通常为 `public`
> - `--serve`：运行本地热重载服务器以预览你的 Quartz
> - `--port`：本地预览服务器使用的端口
> - `--concurrency`：用于解析笔记的线程数

> [!warning] 不适用于生产环境
> Serve 模式仅用于本地预览。
> 生产环境部署请参见[[hosting]]页面。
