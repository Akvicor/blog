---
title: "Markdown 添加数学公式"
date: 2019-03-04T20:26:14Z
draft: false
categories: [LaTeX]
tags: []
card: false
weight: 0
---

一些扩展的`Markdown`语法支持采用`LaTex`语法写数学公式，而在网页中使用`Mathjax`插件来显示数学公式。

本教程介绍**如何在Markdown中书写数学公式**。

## 插入数学公式

在Markdown中插入数学公式的语法是`$数学公式$`和`$$数学公式$$`。

**行内公式**是可以让公式在文中与文字或其他东西混编，不独占一行。
<!--more-->
- **示例**

  ```
  $$ 质能方程E = mc^2 $$
  ```

- **显示**

  > $$质能方程E = mc^2$$

**独立公式**使公式单独占一行，不与文中其他文字等混编。

- **示例**

  ```
  质能方程
  $$
  E = mc^2
  $$
  ```

- **显示**

  > 质能方程
  >
  > $$
  > E = mc^2
  > $$
  > 

## 普通公式

普通的加减乘除数学公式的输入方法与平常的书写一样。

- **示例**

  ```
  $$
  x = 100 * y + z - 10 / 33 + 10 % 3
  $$
  ```

- **显示**

  > 
  >
  > $$
  > x = 100 * y + z - 10 / 33 + 10 % 3
  > $$
  > 

## 上下标

使用`^`来表示上标，`_`来表示下标，同时如果上下标的内容多于一个字符，可以使用`{}`来将这些内容括起来当做一个整体。
与此同时，上下标是可以嵌套的。

- **示例**

  ```
  $$
  x = a_{1}^n + a_{2}^n + a_{3}^n
  $$
  ```

- **显示**

  > 
  >
  > $$
  > x = a_{1}^n + a_{2}^n + a_{3}^n
  > $$
  > 

如果希望左右两边都能有上下标，可以使用`\sideset`语法

- **示例**

  ```
  $$
  \sideset{^1_2}{^3_4}A
  $$
  ```

- **显示**

  > 
  >
  > $$
  > \sideset{^1_2}{^3_4}A
  > $$
  > 

## 括号

`()`，`[]`和`|`都表示它们自己，但是`{}`因为有特殊作用因此当需要显示大括号时一般使用`\lbrace \rbrace`来表示。

- **示例**

  ```mathematica
  $$
  f(x, y) = 100 * \lbrace[(x + y) * 3] - 5\rbrace
  $$
  ```

- **显示**

  > 
  >
  > $$
  > f(x, y) = 100 * \lbrace[(x + y) * 3] - 5\rbrace
  > $$
  > 

原始符号不会随着公式大小自动缩放，需要使用 \left 和 \right 来实现自动缩放：
　　

```mathematica
$$\left \lbrace \sum_{i=0}^n i^3 = \frac{(n^2+n)(n+6)}{9} \right \rbrace$$
```



效果：

$$\left \lbrace \sum_{i=0}^n i^3 = \frac{(n^2+n)(n+6)}{9} \right \rbrace$$

不使用\left 和 \right的效果：



```mathematica
$$ \lbrace \sum_{i=0}^n i^3 = \frac{(n^2+n)(n+6)}{9}  \rbrace$$
```



$$ \lbrace \sum_{i=0}^n i^3 = \frac{(n^2+n)(n+6)}{9}  \rbrace$$

## 分数

分数使用`\frac{分母}{分子}`这样的语法，不过推荐使用`\cfrac`来代替`\frac`，显示公式不会太挤。

- **示例**

  ```mathematica
  $$
  \frac{1}{3} 与 \cfrac{1}{3}
  $$
  ```

- **显示**


  > $$
  >  \frac{1}{3} 与 \cfrac{1}{3}
  > $$


## 开方

开方使用`\sqrt[次数]{被开方数}`这样的语法

- **示例**

  ```
  $$
  \sqrt[3]{X}
  $$
  $$
  \sqrt{5 - x}
  $$
  ```

- **显示**


  > $$
  > \sqrt[3]{X}
  > $$
  > 
  > $$
  > \sqrt{5 - x}
  > $$

## 求和与积分

求和使用\sum,可加上下标，积分使用\int可加上下限，双重积分用\iint:
　　

```
$ \sum_{i=0}^n $
$ \int_1^\infty $
$ \iint_1^\infty $
```



分别显示:$ \sum_{i=0}^n $ 和$ \int_1^\infty $以及$ \iint_1^\infty $

## 极限

极限使用\lim:
　　

```
$ \lim_{x \to 0} $
```



显示为：$ \lim_{x \to 0} $

## 表格与矩阵

表格样式lcr表示居中，|加入一条竖线，\hline表示行间横线，列之间用&分隔，行之间用\分隔
　　

```
$$\begin{array}{c|lcr}
n & \text{Left} & \text{Center} & \text{Right} \\\\
\hline
1 & 1.97 & 5 & 12 \\\\
2 & -11 & 19 & -80 \\\\
3 & 70 & 209 & 1+i \\\\
\end{array}$$
```



显示效果为：

$$\begin{array}{c|lcr}
n & \text{Left} & \text{Center} & \text{Right} \\\\
\hline
1 & 1.97 & 5 & 12 \\\\
2 & -11 & 19 & -80 \\\\
3 & 70 & 209 & 1+i \\\\
\end{array}$$

表格的插入也可以使用以下方式：



```
名称|说明
---|---|---
temperature|  室内温度
set temperature|  设定温度
height|  室内高度
```



显示效果：

| 名称            | 说明     |
| --------------- | -------- |
| temperature     | 室内温度 |
| set temperature | 设定温度 |
| height          | 室内高度 |

矩阵显示和表格很相似
　　

```
$$\left[
\begin{matrix}
V_A \\\\
V_B \\\\
V_C \\\\
\end{matrix}
\right] =
\left[
\begin{matrix}
1 & 0 & L \\\\
-cosψ & sinψ & L \\\\
-cosψ & -sinψ & L
\end{matrix}
\right]
\left[
\begin{matrix}
V_x \\\\
V_y \\\\
W \\\\
\end{matrix}
\right] $$
```



显示效果：　

$$\left[
\begin{matrix}
V_A \\\\
V_B \\\\
V_C \\\\
\end{matrix}
\right] =
\left[
\begin{matrix}
1 & 0 & L \\\\
-cosψ & sinψ & L \\\\
-cosψ & -sinψ & L
\end{matrix}
\right]
\left[
\begin{matrix}
V_x \\\\
V_y \\\\
W \\\\
\end{matrix}
\right] $$

## 希腊字母

见下表

| 代码       | 大写 | 代码       | 小写 |
| ---------- | ---- | ---------- | ---- |
| `A`        | A    | `\alpha`   | α    |
| `B`        | B    | `\beta`    | β    |
| `\Gamma`   | Γ    | `\gamma`   | γ    |
| `\Delta`   | Δ    | `\delta`   | δ    |
| `E`        | E    | `\epsilon` | ϵ    |
| `Z`        | Z    | `\zeta`    | ζ    |
| `H`        | H    | `\eta`     | η    |
| `\Theta`   | Θ    | `\theta`   | θ    |
| `I`        | I    | `\iota`    | ι    |
| `K`        | K    | `\kappa`   | κ    |
| `\Lambda`  | Λ    | `\lambda`  | λ    |
| `M`        | M    | `\mu`      | μ    |
| `N`        | N    | `\nu`      | ν    |
| `\Xi`      | Ξ    | `\xi`      | ξ    |
| `O`        | O    | `\omicron` | ο    |
| `\Pi`      | Π    | `\pi`      | π    |
| `P`        | P    | `\rho`     | ρ    |
| `\Sigma`   | Σ    | `\sigma`   | σ    |
| `T`        | T    | `\tau`     | τ    |
| `\Upsilon` | Υ    | `\upsilon` | υ    |
| `\Phi`     | Φ    | `\phi`     | ϕ    |
| `X`        | X    | `\chi`     | χ    |
| `\Psi`     | Ψ    | `\psi`     | ψ    |
| `\Omega`   | Ω    | `\omega`   | ω    |

## 其他字符

### 关系运算符

| 符号 | 代码         |
| ---- | ------------ |
| ±    | `\pm`        |
| ×    | `\times`     |
| ÷    | `\div`       |
| ∣    | `\mid`       |
| ∤    | `\nmid`      |
| ⋅    | `\cdot`      |
| ∘    | `\circ`      |
| ∗    | `\ast`       |
| ⨀    | `\bigodot`   |
| ⨂    | `\bigotimes` |
| ⨁    | `\bigoplus`  |
| ≤    | `\leq`       |
| ≥    | `\geq`       |
| ≠    | `\neq`       |
| ≈    | `\approx`    |
| ≡    | `\equiv`     |
| ∑    | `\sum`       |
| ∏    | `\prod`      |
| ∐    | `\coprod`    |

### 集合运算符

| 符号 | 代码        |
| ---- | ----------- |
| ∅    | `\emptyset` |
| ∈    | `\in`       |
| ∉    | `\notin`    |
| ⊂    | `\subset`   |
| ⊃    | `\supset`   |
| ⊆    | `\subseteq` |
| ⊇    | `\supseteq` |
| ⋂    | `\bigcap`   |
| ⋃    | `\bigcup`   |
| ⋁    | `\bigvee`   |
| ⋀    | `\bigwedge` |
| ⨄    | `\biguplus` |
| ⨆    | `\bigsqcup` |

### 对数运算符

| 符号 | 代码   |
| ---- | ------ |
| log  | `\log` |
| lg   | `\lg`  |
| ln   | `\ln`  |

### 三角运算符

| 符号 | 代码     |
| ---- | -------- |
| ⊥    | `\bot`   |
| ∠    | `\angle` |
| sin  | `\sin`   |
| cos  | `\cos`   |
| tan  | `\tan`   |
| cot  | `\cot`   |
| sec  | `\sec`   |
| csc  | `\csc`   |

### 微积分运算符

| 符号 | 代码         |
| ---- | ------------ |
| ′    | `\prime`     |
| ∫    | `\int`       |
| ∬    | `\iint`      |
| ∭    | `\iiint`     |
| ⨌    | `\iiiint`    |
| ∮∮   | `\oint`      |
| lim  | `\lim`       |
| ∞    | `\infty`     |
| ∇    | `\nabla`     |
| d    | `\mathrm{d}` |
