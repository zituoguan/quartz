---
title: "资源管理器"
tags:
  - component
---

Quartz 提供了一个资源管理器，允许你浏览站点上的所有文件和文件夹。它支持嵌套文件夹，并且高度可定制。

默认情况下，它会显示页面上的所有文件夹和文件。要将资源管理器显示在不同的位置，可以编辑 [[layout]]。

文件夹的显示名称由 `folder/index.md` 中的 `title` frontmatter 字段决定（详见 [[authoring content | 内容创作]]）。如果该文件不存在或没有 frontmatter，则会使用本地文件夹名称。

> [!info]
> 资源管理器默认使用本地存储来保存其状态。这可以确保在浏览不同页面时获得流畅体验。
>
> 若要清除/删除资源管理器在本地存储中的状态，请删除 `fileTree` 条目（如何在基于 Chromium 的浏览器中删除本地存储键的指南见[这里](https://docs.devolutions.net/kb/general-knowledge-base/clear-browser-local-storage/clear-chrome-local-storage/)）。你可以通过传递 `useSavedState: false` 参数来禁用此功能。

## 自定义

大多数配置可以通过向 `Component.Explorer()` 传递选项来完成。

例如，以下是默认配置：

```typescript title="quartz.layout.ts"
Component.Explorer({
  title: "Explorer", // 资源管理器组件的标题
  folderClickBehavior: "collapse", // 点击文件夹时的行为（"link" 跳转到文件夹页面，"collapse" 折叠文件夹）
  folderDefaultState: "collapsed", // 文件夹的默认状态（"collapsed" 折叠，"open" 展开）
  useSavedState: true, // 是否使用本地存储保存资源管理器的“状态”（哪些文件夹已展开）
  // 省略项稍后展示
  sortFn: ...,
  filterFn: ...,
  mapFn: ...,
  // 函数应用顺序
  order: ["filter", "map", "sort"],
})
```

传递自定义选项时，可以省略任意字段以保留默认值。

想要更高级的自定义？

- 移除资源管理器：从 `quartz.layout.ts` 中移除 `Component.Explorer()`
  - （可选）移除后，可以将 [[table of contents | 目录]] 组件移回布局的 `left` 区域
- 更改 `sort`、`filter` 和 `map` 行为：详见 [[#高级自定义]]
- 组件：
  - 包装器（外部组件，生成文件树等）：`quartz/components/Explorer.tsx`
  - 资源节点（递归，文件夹或文件）：`quartz/components/ExplorerNode.tsx`
- 样式：`quartz/components/styles/explorer.scss`
- 脚本：`quartz/components/scripts/explorer.inline.ts`

## 高级自定义

该组件允许你完全自定义其所有行为。你可以传递自定义的 `sort`、`filter` 和 `map` 函数。
所有可传递的函数都适用于 `FileTrieNode` 类，其属性如下：

```ts title="quartz/components/Explorer.tsx"
class FileTrieNode {
  isFolder: boolean
  children: Array<FileTrieNode>
  data: ContentDetails | null
}
```

```ts title="quartz/plugins/emitters/contentIndex.tsx"
export type ContentDetails = {
  slug: FullSlug
  title: string
  links: SimpleSlug[]
  tags: string[]
  content: string
}
```

所有函数都是可选的。默认情况下只会使用 `sort` 函数：

```ts title="默认排序函数"
// 排序顺序：文件夹优先，然后是文件。文件夹和文件均按字母排序
Component.Explorer({
  sortFn: (a, b) => {
    if ((!a.isFolder && !b.isFolder) || (a.isFolder && b.isFolder)) {
      return a.displayName.localeCompare(b.displayName, undefined, {
        numeric: true,
        sensitivity: "base",
      })
    }

    if (!a.isFolder && b.isFolder) {
      return 1
    } else {
      return -1
    }
  },
})
```

---

你可以为 `sortFn`、`filterFn` 和 `mapFn` 传递自定义函数。所有函数会按照 `order` 选项中指定的顺序执行（见 [[#自定义]]）。这些函数的行为类似于 `Array.prototype` 的对应方法，但它们会原地修改整个 `FileNode` 树，而不是返回新数组。

关于如何使用 `sort`、`filter` 和 `map`，可参考 [Array.prototype.sort()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)、[Array.prototype.filter()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) 和 [Array.prototype.map()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)。

类型定义如下：

```ts
type SortFn = (a: FileTrieNode, b: FileTrieNode) => number
type FilterFn = (node: FileTrieNode) => boolean
type MapFn = (node: FileTrieNode) => void
```

## 基本示例

以下示例展示了 `sort`、`map` 和 `filter` 的基本用法。

### 使用 `sort` 让文件优先

此示例会将所有内容按字母顺序排序。

```ts title="quartz.layout.ts"
Component.Explorer({
  sortFn: (a, b) => {
    return a.displayName.localeCompare(b.displayName)
  },
})
```

### 更改显示名称（`map`）

此示例会将所有 `FileNode`（文件夹和文件）的显示名称转换为全大写。

```ts title="quartz.layout.ts"
Component.Explorer({
  mapFn: (node) => {
    node.displayName = node.displayName.toUpperCase()
    return node
  },
})
```

### 移除部分元素（`filter`）

此示例通过提供要排除的文件夹/文件数组来移除资源管理器中的元素。
注意，此示例按标题过滤，你也可以按 slug 或 `FileTrieNode` 上的其他字段过滤。

```ts title="quartz.layout.ts"
Component.Explorer({
  filterFn: (node) => {
    // 要过滤掉的名称集合
    const omit = new Set(["authoring content", "tags", "advanced"])

    // 也可以用 node.slug 或 node.data 上的其他属性
    // 注意 node.data 只在磁盘上存在的文件才有
    // （例如没有关联 index.md 的隐式文件夹节点没有 data）
    return !omit.has(node.displayName.toLowerCase())
  },
})
```

### 按标签移除文件

你可以通过 `node.data.tags` 访问文件的标签。

```ts title="quartz.layout.ts"
Component.Explorer({
  filterFn: (node) => {
    // 排除带有 "explorerexclude" 标签的文件
    return node.data.tags?.includes("explorerexclude") !== true
  },
})
```

### 显示所有元素

默认情况下，资源管理器会过滤掉 `tags` 文件夹。
要覆盖默认过滤函数，可以将 filterFn 设置为 `undefined`。

```ts title="quartz.layout.ts"
Component.Explorer({
  filterFn: undefined, // 不应用过滤函数，所有文件和文件夹都可见
})
```

## 高级示例

> [!tip]
> 当编写更复杂的函数时，`layout` 文件可能会变得很拥挤。
> 你可以将排序函数定义在组件外部，然后传递进来。
>
> ```ts title="quartz.layout.ts"
> import { Options } from "./quartz/components/ExplorerNode"
>
> export const mapFn: Options["mapFn"] = (node) => {
>   // 在这里实现你的函数
> }
> export const filterFn: Options["filterFn"] = (node) => {
>   // 在这里实现你的函数
> }
> export const sortFn: Options["sortFn"] = (a, b) => {
>   // 在这里实现你的函数
> }
>
> Component.Explorer({
>   // ... 你的其他选项
>   mapFn,
>   filterFn,
>   sortFn,
> })
> ```

### 添加表情前缀

要为文件夹和文件添加表情前缀（📁 表示文件夹，📄 表示文件），可以这样写 map 函数：

```ts title="quartz.layout.ts"
Component.Explorer({
  mapFn: (node) => {
    if (node.isFolder) {
      node.displayName = "📁 " + node.displayName
    } else {
      node.displayName = "📄 " + node.displayName
    }
  },
})
```

