---
title: 欢迎来到 Quartz 4
---

Quartz 是一个快速、功能齐全的静态网站生成器，可以将 Markdown 内容转换为功能完善的网站。已有数千名学生、开发者和教师[[showcase|正在使用 Quartz]]来发布个人笔记、网站和[数字花园](https://jzhao.xyz/posts/networked-thought)。

## 🪴 快速开始

Quartz 需要**至少 [Node](https://nodejs.org/) v20** 和 `npm` v9.3.1 才能正常运行。请确保你的电脑已安装这些工具。

然后，在你喜欢的终端中，依次输入以下命令：

```shell
git clone https://github.com/jackyzha0/quartz.git
cd quartz
npm i
npx quartz create
```

这将引导你初始化 Quartz 并添加内容。完成后，你可以了解如何：

1. 在 Quartz 中[[authoring content|撰写内容]]
2. [[configuration|配置]] Quartz 的行为
3. 更改 Quartz 的[[layout|布局]]
4. [[build|构建和预览]] Quartz
5. 将更改同步到[[setting up your GitHub repository|GitHub]]
6. [[hosting|在线托管]] Quartz

如果你更喜欢视频教程，可以参考 Nicole van der Hoeven 的
[Quartz 安装视频指南](https://www.youtube.com/watch?v=6s6DT1yN4dw&t=227s)。

## 🔧 功能特性

- [[Obsidian compatibility|Obsidian 兼容性]]、[[full-text search|全文搜索]]、[[graph view|图谱视图]]、[[wikilinks|维基链接与转录]]、[[backlinks|反向链接]]、[[features/Latex|Latex]]、[[syntax highlighting|语法高亮]]、[[popover previews|弹出预览]]、[[Docker Support|Docker 支持]]、[[i18n|国际化]]、[[comments|评论]]等[更多功能](./features/)，开箱即用
- 配置更改时热重载，内容编辑时增量重建
- 简单的 JSX 布局和[[creating components|页面组件]]
- [[SPA Routing|极快的页面加载]]和极小的包体积
- 通过[[making plugins|插件]]实现完全可定制的解析、过滤和页面生成

完整功能列表请访问[功能页面](./features/)。你可以在[[philosophy|理念]]页面了解这些功能背后的原因，在[[architecture|架构]]页面了解技术细节。

### 🚧 故障排查与更新

遇到 Quartz 问题？请尝试使用搜索功能查找你的问题。如果还没有，建议先[[upgrading|升级]]到最新版本，看看是否已修复。

如果问题依然存在，欢迎[提交 issue](https://github.com/jackyzha0/quartz/issues)报告 bug，或在我们的 [Discord 社区](https://discord.gg/cRFFHYye7t)寻求帮助。
