---
title: 设置你的 GitHub 仓库
---

首先，确保你已经将 Quartz [[index#🪴 Get Started|克隆并在本地设置]]。

然后，在 GitHub.com 上创建一个新的仓库。**不要**初始化新的仓库（不要添加 `README`、许可证或 `gitignore` 文件）。

![[github-init-repo-options.png]]

在 GitHub.com 的仓库快速设置页面顶部，点击剪贴板图标复制远程仓库的 URL。

![[github-quick-setup.png]]

在你选择的终端中，导航到 Quartz 文件夹的根目录。然后运行以下命令，将 `REMOTE-URL` 替换为你刚刚复制的仓库地址。

```bash
# 列出所有已跟踪的仓库
git remote -v

# 如果 origin 不匹配你自己的仓库，将你的仓库设置为 origin
git remote set-url origin REMOTE-URL

# 如果没有 upstream 远程仓库，添加它以便后续更新
git remote add upstream https://github.com/jackyzha0/quartz.git
```

然后，你可以同步内容，将其上传到你的仓库。这是一个帮助命令，用于首次将内容推送到你的仓库。

```bash
npx quartz sync --no-pull
```

> [!warning]- `fatal: --[no-]autostash option is only valid with --rebase`
> 你可能使用了过时的 `git` 版本。更新 `git` 应该可以解决此问题。

以后每次想要将更新推送到仓库时，只需运行 `npx quartz sync` 即可。

> [!hint] 标志与选项
> 查看完整帮助选项，可以运行 `npx quartz sync --help`。
>
> 大多数选项都有合理的默认值，但如果你有自定义设置，可以覆盖它们：
>
> - `-d` 或 `--directory`：内容文件夹，通常为 `content`
> - `-v` 或 `--verbose`：输出更多日志信息
> - `--commit` 或 `--no-commit`：是否为更改创建 `git` 提交
> - `--push` 或 `--no-push`：是否将更新推送到你在 GitHub 上的 Quartz 分叉
> - `--pull` 或 `--no-pull`：推送前是否尝试从 GitHub 分叉拉取更新（例如来自其他设备）

