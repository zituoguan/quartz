---
title: 评论
tags:
  - component
---

Quartz 还支持集成多种评论提供商，让读者可以在你的网站上留言评论。

![[giscus-example.png]]

目前，Quartz 原生仅支持 [Giscus](https://giscus.app/)，但欢迎提交 PR 支持其他评论提供商！

## 评论提供商

### Giscus

首先，确保你用于 Quartz 的 [[设置 GitHub 仓库|GitHub]] 仓库满足以下要求：

1. **仓库为[公开](https://docs.github.com/en/github/administering-a-repository/managing-repository-settings/setting-repository-visibility#making-a-repository-public)**，否则访客无法查看讨论内容。
2. **已安装 [giscus](https://github.com/apps/giscus) 应用**，否则访客无法评论和点赞。
3. **已启用 Discussions 功能**，可[在仓库设置中开启](https://docs.github.com/en/github/administering-a-repository/managing-repository-settings/enabling-or-disabling-github-discussions-for-a-repository)。

然后，使用 [Giscus 官网](https://giscus.app/#repository) 获取你的 `repoId` 和 `categoryId`。请确保选择 `Announcements` 作为讨论类别。

![[giscus-repo.png]]

![[giscus-discussion.png]]

输入仓库和选择讨论类别后，Giscus 会生成一些 ID，Quartz 配置时需要用到。你无需手动添加脚本，Quartz 会自动处理，但需要在下一步填写这些值！

![[giscus-results.png]]

最后，在 `quartz.layout.ts` 文件中，编辑 `sharedPageComponents` 的 `afterBody` 字段，填入如下配置（将值替换为你自己的）：

```ts title="quartz.layout.ts"
afterBody: [
  Component.Comments({
    provider: 'giscus',
    options: {
      // data-repo
      repo: 'jackyzha0/quartz',
      // data-repo-id
      repoId: 'MDEwOlJlcG9zaXRvcnkzODcyMTMyMDg',
      // data-category
      category: 'Announcements',
      // data-category-id
      categoryId: 'DIC_kwDOFxRnmM4B-Xg6',
    }
  }),
],
```

### 个性化配置

Quartz 还支持 Giscus 的其他选项，你可以像配置 `repo`、`repoId`、`category`、`categoryId` 一样传递它们。

```ts
type Options = {
  provider: "giscus"
  options: {
    repo: `${string}/${string}`
    repoId: string
    category: string
    categoryId: string

    // 自定义主题文件夹的 URL
    // 默认 'https://${cfg.baseUrl}/static/giscus'
    themeUrl?: string

    // 浅色主题 .css 文件名
    // 默认 'light'
    lightTheme?: string

    // 深色主题 .css 文件名
    // 默认 'dark'
    darkTheme?: string

    // 页面与讨论的映射方式
    // 默认 'url'
    mapping?: "url" | "title" | "og:title" | "specific" | "number" | "pathname"

    // 是否严格匹配标题
    // 默认 true
    strict?: boolean

    // 是否启用主帖的表情反应
    // 默认 true
    reactionsEnabled?: boolean

    // 评论输入框相对评论的位置
    // 默认 'bottom'
    inputPosition?: "top" | "bottom"
  }
}
```

#### 自定义 CSS 主题

Quartz 支持 Giscus 的自定义主题。将 `.css` 文件放在 `quartz/static` 文件夹下，并设置相关配置即可。

例如，你有浅色主题 `light-theme.css`、深色主题 `dark-theme.css`，且你的 Quartz 站点部署在 `https://example.com/`：

```ts
afterBody: [
  Component.Comments({
    provider: 'giscus',
    options: {
      // 其他选项

      themeUrl: "https://example.com/static/giscus", // 对应 quartz/static/giscus/
      lightTheme: "light-theme", // 对应 quartz/static/giscus/light-theme.css
      darkTheme: "dark-theme", // 对应 quartz/static/giscus/dark-theme.css
    }
  }),
],
```

#### 条件显示评论

Quartz 支持通过 frontmatter 字段 `comments` 控制是否显示评论框。默认所有页面显示评论，如需关闭，在页面 frontmatter 设置 `comments: false`。

```
---
title: 此处禁用评论！
comments: false
---
```
