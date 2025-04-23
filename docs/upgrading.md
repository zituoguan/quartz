---
title: "升级 Quartz"
---

> [!note]
> 本指南专门针对将 Quartz 4 升级到更高版本。如果你是从 Quartz 3 升级，请参考 [[从 Quartz 3 迁移|迁移指南]] 获取更多信息。

要获取最新的 Quartz 更新，只需运行

```bash
npx quartz update
```

由于 Quartz 在底层使用了 [git](https://git-scm.com/) 进行版本管理，更新实际上是从官方 Quartz GitHub 仓库“拉取”更新。如果你有本地更改可能与更新冲突，你需要手动解决这些冲突（或者手动使用 `git pull origin upstream` 拉取）。

> [!hint]
> Quartz 会在更新前尝试缓存你的内容，以尽量避免合并冲突。如果在合并过程中遇到冲突，你可以停止合并，然后运行 `npx quartz restore` 从缓存中恢复你的内容。

如果你安装了 [GitHub 桌面应用](https://desktop.github.com/)，它会自动打开，帮助你解决冲突。否则，你需要在 VSCode 等文本编辑器中手动解决。有关手动解决冲突的更多帮助，请参阅 [GitHub 关于解决合并冲突的指南](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-using-the-command-line#competing-line-change-merge-conflicts)。

