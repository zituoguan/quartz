---
title: 语法高亮
tags:
  - feature/transformer
---

Quartz 的语法高亮完全在构建时完成。这意味着 Quartz 只会生成预先计算好的 CSS 来高亮正确的单词，因此不会有庞大的客户端包来进行语法高亮。

与一些客户端高亮器不同，它使用完整的 TextMate 解析器语法，而不是正则表达式，从而实现了高度准确的代码高亮。

简而言之，它生成的 HTML 与您在 VS Code 等编辑器中看到的代码完全一致。底层由 [Rehype Pretty Code](https://rehype-pretty-code.netlify.app/) 提供支持，该工具使用了 [Shiki](https://github.com/shikijs/shiki)。

> [!warning]
> 如果您的笔记中有大量代码片段，语法高亮会对构建速度产生影响。

## 格式化

行内的 `反引号` 内容会被格式化为代码。

````
```ts
export function trimPathSuffix(fp: string): string {
  fp = clientSideSlug(fp)
  let [cleanPath, anchor] = fp.split("#", 2)
  anchor = anchor === undefined ? "" : "#" + anchor

  return cleanPath + anchor
}
```
````

```ts
export function trimPathSuffix(fp: string): string {
  fp = clientSideSlug(fp)
  let [cleanPath, anchor] = fp.split("#", 2)
  anchor = anchor === undefined ? "" : "#" + anchor

  return cleanPath + anchor
}
```

### 标题

为代码块添加文件标题，在双引号（`""`）中填写文本：

````
```js title="..."

```
````

```ts title="quartz/path.ts"
export function trimPathSuffix(fp: string): string {
  fp = clientSideSlug(fp)
  let [cleanPath, anchor] = fp.split("#", 2)
  anchor = anchor === undefined ? "" : "#" + anchor

  return cleanPath + anchor
}
```

### 行高亮

在 `{}` 中放置数字范围。

````
```js {1-3,4}

```
````

```ts {2-3,6}
export function trimPathSuffix(fp: string): string {
  fp = clientSideSlug(fp)
  let [cleanPath, anchor] = fp.split("#", 2)
  anchor = anchor === undefined ? "" : "#" + anchor

  return cleanPath + anchor
}
```

### 单词高亮

一系列字符，如字面量正则表达式。

````
```js /useState/
const [age, setAge] = useState(50);
const [name, setName] = useState('Taylor');
```
````

```js /useState/
const [age, setAge] = useState(50)
const [name, setName] = useState("Taylor")
```

### 行内高亮

在行内代码后添加 `{:lang}`，即可像常规代码块一样高亮。

```
这是一个数组 `[1, 2, 3]{:js}`，包含数字 1 到 3。
```

这是一个数组 `[1, 2, 3]{:js}`，包含数字 1 到 3。

### 行号

语法高亮默认配置了行号。如果你想让行号从特定数字开始，使用 `showLineNumbers{number}`：

````
```js showLineNumbers{number}

```
````

```ts showLineNumbers{20}
export function trimPathSuffix(fp: string): string {
  fp = clientSideSlug(fp)
  let [cleanPath, anchor] = fp.split("#", 2)
  anchor = anchor === undefined ? "" : "#" + anchor

  return cleanPath + anchor
}
```

### 转义代码块

你可以通过在代码块外再包一层多一个反引号的围栏，来格式化嵌套代码块。

`````
````
```js /useState/
const [age, setAge] = useState(50);
const [name, setName] = useState('Taylor');
```
````
`````

## 自定义

语法高亮是 [[SyntaxHighlighting]] 插件的功能。自定义选项请参见插件页面。

