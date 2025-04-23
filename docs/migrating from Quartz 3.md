---
title: "从 Quartz 3 迁移"
---

由于你已经在本地拥有 Quartz，无需再次 fork 或 clone。只需切换到 alpha 分支，安装依赖，并导入你的旧 vault 即可。

```bash
git fetch
git checkout v4
git pull upstream v4
npm i
npx quartz create
```

如果遇到类似 `fatal: 'upstream' does not appear to be a git repository` 的错误，请确保你已将 `upstream` 添加为远程源：

```shell
git remote add upstream https://github.com/jackyzha0/quartz.git
```

运行 `npx quartz create` 时，你会被提示如何初始化内容文件夹。在这里，你可以选择导入或链接你之前的内容文件夹，Quartz 应该会像你期望的那样正常工作。

> [!note]
> 如果你想使用的现有内容文件夹在不同分支但路径相同，请在不同路径下再次 clone 仓库以便使用。

## 主要变更

1. **移除 Hugo 和 `hugo-obsidian`**：Hugo 在早期版本的 Quartz 中表现良好，但它也让非 Golang 和 Hugo 社区的用户难以理解 Quartz 的底层原理并进行自定义。Quartz 4 现在采用基于 Node 的静态站点生成流程，这将带来更有帮助的错误信息和更流畅的用户体验。
2. **完整热重载**：`hugo-obsidian` 与 Hugo 的集成存在诸多问题，导致 watch 模式下内容索引无法及时更新，输出结果不准确。Quartz 4 采用统一的解析、过滤和输出流程，每次更改都会重新运行，确保热重载始终准确。
3. **用 JSX 替换 Go 模板语法**：Quartz 3 使用 [Go 模板](https://pkg.go.dev/text/template) 创建页面布局，但其语法不适合复杂渲染（如 [文本处理](https://github.com/jackyzha0/quartz/blob/hugo/layouts/partials/textprocessing.html)），也难以进行有意义的布局更改。Quartz 4 使用 JavaScript 的 JSX 扩展语法，可以像写 HTML 一样编写布局代码，更易理解和维护。
4. **全新的可扩展[[配置]]和[[configuration#Plugins|插件]]系统**：Quartz 3 的配置需要了解 Hugo 的 partials，扩展也很困难。Quartz 4 的配置和插件系统为用户扩展而设计，同时便于升级新版本。

## 需要更新的内容

- 你需要更新部署脚本。详见 [[hosting]] 指南。
- 确保 GitHub 上的默认分支从 `hugo` 更新为 `v4`。
- [[folder and tag listings|文件夹和标签列表]] 也有变化。
  - 文件夹描述应放在 `content/<folder-name>/index.md`，其中 `<folder-name>` 是文件夹名。
  - 标签描述应放在 `content/tags/<tag-name>.md`，其中 `<tag-name>` 是标签名。
- Quartz 3 和 Quartz 4 之间部分 HTML 布局可能不同。如果你依赖特定的 HTML 层级或类名，可能需要更新自定义 CSS。
- 如果你自定义过 Quartz 3 的布局，需要将这些更改从 Go 模板迁移到 JSX，因为 Quartz 4 不再使用 Hugo。关于组件，请参阅 [[creating components]] 指南获取更多信息。

