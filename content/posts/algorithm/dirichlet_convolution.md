---
title: "狄利克雷卷积"
date: 2019-08-07 11:42:03 +08:00
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

Dirichlet卷积

<!--more-->

# 定义

定义数论函数{{<latex>}}f{{</latex>}}和{{<latex>}}g{{</latex>}}的狄利克雷卷积为{{<latex>}}h{{</latex>}}，则{{<latex>}}h(n)=\sum_{d|n}f(d) g(\frac{n}{d}){{</latex>}}记做{{<latex>}}h=f * g{{</latex>}}（ * 代表卷积）

# 性质

狄利克雷卷积满足交换律，结合律，对**加法**满足分配律

交换律：{{<latex>}}f * g = g * f{{</latex>}}

结合律：{{<latex>}}(f * g) * h = f * (g * h){{</latex>}}

分配律：{{<latex>}}f * (g+h)=f * g + f * h{{</latex>}}

单位元：{{<latex>}}f * e = e * f{{</latex>}}

逆元：对于每一个{{<latex>}}f(1)\neq 0{{</latex>}}的函数{{<latex>}}f{{</latex>}}，都有{{<latex>}}f * g=\epsilon{{</latex>}}

两个积性函数的狄利克雷卷积依旧为积性函数。{{<latex>}}f,g{{</latex>}}为积性函数，则{{<latex>}}f * g{{</latex>}}也为积性函数



{{<latex>}}e{{</latex>}}为单位元，它卷上任意的数论函数仍为原数论函数，简单来说，就是{{<latex>}}e(n)=[n=1]{{</latex>}}



**求一个函数的逆**

只需定义：{{<latex>}}g(n)=\frac{1}{f(1)}\left([n==1]-\sum_{i|n,i\neq1}f(i)g(\frac{n}{i})\right){{</latex>}}

这样的话：{{<latex>}}\sum_{i|n}f(i)g(\frac{n}{i})=f(1)g(n)+\sum_{i|n,i\neq1}f(i)g(\frac{n}{i})=[n==1]{{</latex>}}

# 常用的狄利克雷卷积

{{<latex>}}(1\cdot\mu)=e{{</latex>}}，因为{{<latex>}}\sum_{d|n}\mu(d)=[n=1]{{</latex>}}

{{<latex>}}\mu\cdot id=\phi{{</latex>}}，将欧拉函数的通式展开即可得到次式。

{{<latex>}}1\cdot id = \sigma{{</latex>}}

{{<latex>}}1\cdot 1 = \tau{{</latex>}}



我们能够通过简单的狄利克雷卷积运算轻易的证出莫比乌斯反演

若有{{<latex>}}F(n)=\sum_{d|n}f(d){{</latex>}}

则有{{<latex>}}F=1\cdot f{{</latex>}}，两边同时卷上{{<latex>}}\mu{{</latex>}}，可得

{{<latex>}}\mu \cdot F = \mu \cdot 1 \cdot f{{</latex>}}

又因为{{<latex>}}1\cdot \mu =e{{</latex>}}，所以{{<latex>}}f=\mu\cdot F{{</latex>}}

即{{<latex>}}f(n)=\sum_{d|n}\mu(d)\cdot F(\frac{n}{d}){{</latex>}}

我们甚至可以弄出一些很棒的东西，比如说

{{<latex>}}id=id\cdot e=id\cdot\mu\cdot 1=\phi\cdot1{{</latex>}}

{{<latex>}}n=id(n)=\sum_{d|n}\phi(d){{</latex>}}

{{<latex>}}\sigma=1\cdot id=1\cdot(1\cdot\phi)=(1\cdot 1)\cdot\phi=\tau\cdot\phi{{</latex>}}

{{<latex>}}\sigma(n)=\sum_{d|n}\tau(d)\cdot\phi(\frac{n}{d}){{</latex>}}



