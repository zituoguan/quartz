---
title: "图谱视图"
tags:
  - component
---

Quartz 提供了一个图谱视图（graph-view），可以显示本地图谱视图和全局图谱视图。

- 本地图谱视图显示与当前文件相互链接的文件。换句话说，它展示了距离当前笔记最多一跳的所有笔记。
- 全局图谱视图可以通过点击本地图谱视图右上角的图谱图标进行切换。它会显示你所有的笔记以及它们之间的连接关系。

默认情况下，节点的半径与该文件的内部链接（包括入链和出链）总数成正比。

此外，类似于浏览器会用不同颜色高亮已访问的链接，图谱视图也会用不同颜色显示你访问过的节点。

> [!info]
> 图谱视图需要在 [[configuration]] 中启用 `ContentIndex` emitter 插件。

## 自定义

大多数配置可以通过向 `Component.Graph()` 传递选项来完成。

例如，以下是默认配置：

```typescript title="quartz.layout.ts"
Component.Graph({
  localGraph: {
    drag: true, // 是否允许拖动画布
    zoom: true, // 是否允许缩放
    depth: 1, // 显示几跳范围内的笔记
    scale: 1.1, // 默认视图缩放比例
    repelForce: 0.5, // 节点之间的排斥力
    centerForce: 0.3, // 节点居中时的吸引力
    linkDistance: 30, // 链接的默认长度
    fontSize: 0.6, // 节点标签字体大小
    opacityScale: 1, // 缩小时标签透明度变化速度
    removeTags: [], // 要从图谱中移除的标签
    showTags: true, // 是否在图谱中显示标签
    enableRadial: false, // 是否径向约束图谱，类似 Obsidian
  },
  globalGraph: {
    drag: true,
    zoom: true,
    depth: -1,
    scale: 0.9,
    repelForce: 0.5,
    centerForce: 0.3,
    linkDistance: 30,
    fontSize: 0.6,
    opacityScale: 1,
    removeTags: [], // 要从图谱中移除的标签
    showTags: true, // 是否在图谱中显示标签
    enableRadial: true, // 是否径向约束图谱，类似 Obsidian
  },
})
```

在传递自定义选项时，你可以省略任何字段，未指定的字段会使用默认值。

想要更进一步自定义？

- 移除图谱视图：删除 `quartz.layout.ts` 中所有 `Component.Graph()` 的用法。
- 组件：`quartz/components/Graph.tsx`
- 样式：`quartz/components/styles/graph.scss`
- 脚本：`quartz/components/scripts/graph.inline.ts`

