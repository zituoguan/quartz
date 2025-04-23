---
title: 高阶布局组件
---

Quartz 提供了多个高阶组件，帮助进行布局组合和响应式设计。这些组件用于包裹其他组件，以添加额外功能或修改其行为。

## `Flex` 组件

`Flex` 组件创建一个[弹性盒子布局](https://developer.mozilla.org/zh-CN/docs/Web/CSS/flex)，可以以多种方式排列子组件。它特别适用于创建响应式布局，以及将组件组织为行或列。

```typescript
type FlexConfig = {
  components: {
    Component: QuartzComponent
    grow?: boolean // 组件是否应扩展以填充空间
    shrink?: boolean // 组件是否应在需要时收缩
    basis?: string // 组件的初始主轴尺寸
    order?: number // 在 flex 容器中的顺序
    align?: "start" | "end" | "center" | "stretch" // 交叉轴对齐方式
    justify?: "start" | "end" | "center" | "between" | "around" // 主轴对齐方式
  }[]
  direction?: "row" | "row-reverse" | "column" | "column-reverse"
  wrap?: "nowrap" | "wrap" | "wrap-reverse"
  gap?: string
}
```

### 示例用法

```typescript
Component.Flex({
  components: [
    {
      Component: Component.Search(),
      grow: true, // 搜索框会扩展填充可用空间
    },
    { Component: Component.Darkmode() }, // 暗色模式组件保持其自然大小
  ],
  direction: "row",
  gap: "1rem",
})
```

## `MobileOnly` 组件

`MobileOnly` 组件是一个包装器，使其子组件仅在移动设备上可见。这对于创建响应式布局非常有用，可以让某些组件只在小屏幕上显示。

### 示例用法

```typescript
Component.MobileOnly(Component.Spacer())
```

## `DesktopOnly` 组件

`DesktopOnly` 组件与 `MobileOnly` 相对。它使其子组件仅在桌面设备上可见，有助于创建响应式布局，让某些组件只在大屏幕上显示。

### 示例用法

```typescript
Component.DesktopOnly(Component.TableOfContents())
```

## `ConditionalRender` 组件

`ConditionalRender` 组件是一个包装器，根据提供的条件函数有条件地渲染其子组件。适用于创建动态布局，仅在特定条件下显示组件。

```typescript
type ConditionalRenderConfig = {
  component: QuartzComponent
  condition: (props: QuartzComponentProps) => boolean
}
```

### 示例用法

```typescript
Component.ConditionalRender({
  component: Component.Search(),
  condition: (props) => props.displayClass !== "fullpage",
})
```

上面的例子只会在页面不是全屏模式时渲染搜索组件。

```typescript
Component.ConditionalRender({
  component: Component.Breadcrumbs(),
  condition: (page) => page.fileData.slug !== "index",
})
```

上面的例子会在根目录的 `index.md` 页面隐藏面包屑导航。

