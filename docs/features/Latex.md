---
title: LaTeX
tags:
  - feature/transformer
---

Quartz 默认使用 [Katex](https://katex.org/) 在构建时对行内和块级数学表达式进行排版。

## 语法

### 块级数学公式

块级数学公式可以通过用 `$$` 包裹数学表达式来渲染。

```
$$
f(x) = \int_{-\infty}^\infty
    f\hat(\xi),e^{2 \pi i \xi x}
    \,d\xi
$$
```

$$
f(x) = \int_{-\infty}^\infty
    f\hat(\xi),e^{2 \pi i \xi x}
    \,d\xi
$$

$$
\begin{aligned}
a &= b + c \\ &= e + f \\
\end{aligned}
$$

$$
\begin{bmatrix}
1 & 2 & 3 \\
a & b & c
\end{bmatrix}
$$

$$
\begin{array}{rll}
E \psi &= H\psi & \text{展开哈密顿算符} \\
&= -\frac{\hbar^2}{2m}\frac{\partial^2}{\partial x^2} \psi + \frac{1}{2}m\omega x^2 \psi & \text{使用假设 $\psi(x) = e^{-kx^2}f(x)$，希望消去 $x^2$ 项} \\
&= -\frac{\hbar^2}{2m} [4k^2x^2f(x)+2(-2kx)f'(x) + f''(x)]e^{-kx^2} + \frac{1}{2}m\omega x^2 f(x)e^{-kx^2} & \text{两边同时去掉 $e^{-kx^2}$ 项} \\
& \Downarrow \\
Ef(x) &= -\frac{\hbar^2}{2m} [4k^2x^2f(x)-4kxf'(x) + f''(x)] + \frac{1}{2}m\omega x^2 f(x) & \text{选择 $k=\frac{im}{2}\sqrt{\frac{\omega}{\hbar}}$ 来消去 $x^2$ 项，通过 $-\frac{\hbar^2}{2m}4k^2=\frac{1}{2}m \omega$} \\
&= -\frac{\hbar^2}{2m} [-4kxf'(x) + f''(x)] \\
\end{array}
$$

> [!warn]
> 由于 [底层解析库](https://github.com/remarkjs/remark-math) 的限制，Quartz 中的块级数学公式要求 `$$` 分隔符必须像上面那样单独占一行。

### 行内数学公式

同样，行内数学公式可以通过用单个 `$` 包裹数学表达式来渲染。例如，`$e^{i\pi} = -1$` 会显示为 $e^{i\pi} = -1$

### 转义符号

有时你可能在一段文字中出现多个 `$`，这可能会意外触发 MathJax/Katex 的解析。

为避免这种情况，可以通过 `\$` 来转义美元符号。

例如：

- 错误：`I have $1 and you have $2` 会显示为 I have $1 and you have $2
- 正确：`I have \$1 and you have \$2` 会显示为 I have \$1 and you have \$2

### 使用 mhchem

在 `quartz/plugins/transformers/latex.ts` 文件顶部（在其他所有 import 之前）添加如下导入：

```ts title="quartz/plugins/transformers/latex.ts"
import "katex/contrib/mhchem"
```

## 自定义

LaTeX 解析是 [[plugins/Latex|Latex]] 插件的功能。自定义选项请参见插件页面。
