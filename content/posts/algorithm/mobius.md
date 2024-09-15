---
title: "莫比乌斯反演"
date: 2019-08-06 17:34:22 +08:00
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

莫比乌斯反演是数论中的重要内容，对于一些函数{{<latex>}}f(n){{</latex>}}，如果很难直接求出它的值，而容易求出其倍数和或约数和{{<latex>}}g(n){{</latex>}}，那么可以通过莫比乌斯反演简化运算，求得{{<latex>}}f(n){{</latex>}}的值。

开始学习莫比乌斯反演前我们需要一些前置知识：**积性函数、Dirichlet卷积、莫比乌斯函数**

<!--more-->

## 数论分块与整除相

### 引理 1

{{<latex>}}
\forall a,b,c\in\mathbb{Z},\left\lfloor\frac{a}{bc}\right\rfloor=\left\lfloor\frac{\left\lfloor\frac{a}{b}\right\rfloor}{c}\right\rfloor 
{{</latex>}}

略证

{{<latex>}}
\begin{split} &\frac{a}{b}=\left\lfloor\frac{a}{b}\right\rfloor+r(0\leq r<1)\\\\
\Rightarrow &\left\lfloor\frac{a}{bc}\right\rfloor =\left\lfloor\frac{a}{b}\cdot\frac{1}{c}\right\rfloor =\left\lfloor \frac{1}{c}\left(\left\lfloor\frac{a}{b}\right\rfloor+r\right)\right\rfloor =\left\lfloor \frac{\left\lfloor\frac{a}{b}\right\rfloor}{c} +\frac{r}{c}\right\rfloor =\left\lfloor \frac{\left\lfloor\frac{a}{b}\right\rfloor}{c}\right\rfloor \end{split} 
{{</latex>}}

### 引理 2

{{<latex>}}
\forall{n} \in N, \left | \left\{ \lfloor \frac{n}{d} \rfloor \mid d \in N \right\}\right| \leq \lfloor 2\sqrt{n} \rfloor
{{</latex>}}

{{<latex>}}|V|{{</latex>}}表示集合{{<latex>}}V{{</latex>}}的元素个数

略证

对于{{<latex>}}d\leq\lfloor\sqrt{n}\rfloor{{</latex>}}，{{<latex>}}\lfloor \frac{n}{d}\rfloor{{</latex>}}有{{<latex>}}\lfloor\sqrt{n}\rfloor{{</latex>}}种取值

对于{{<latex>}}d>\lfloor\sqrt{n}\rfloor{{</latex>}}，有{{<latex>}}\lfloor \frac{n}{d}\rfloor\leq\lfloor\sqrt{n}\rfloor{{</latex>}}，也只有{{<latex>}}\lfloor\sqrt{n}\rfloor{{</latex>}}种取值

综上得证

### 数论分块

数论分块的过程大概如下：考虑含有{{<latex>}}\lfloor\frac{n}{i}\rfloor{{</latex>}}的求和式子（{{<latex>}}n{{</latex>}}为常数）

对于任意一个{{<latex>}}i(i\leq n){{</latex>}}，我们需要找到一个最大的{{<latex>}}j(i\leq j\leq n){{</latex>}}，使得{{<latex>}}\lfloor\frac{n}{i}\rfloor=\lfloor\frac{n}{j}\rfloor{{</latex>}}

而{{<latex>}}j=\left\lfloor\frac{n}{\left\lfloor\frac{n}{i}\right\rfloor}\right\rfloor{{</latex>}}

略证

{{<latex>}}
\begin{split} &\left\lfloor\frac{n}{i}\right\rfloor \leq \frac{n}{i}\\ \Rightarrow &\left\lfloor\frac{n}{ \left\lfloor\frac{n}{i}\right\rfloor }\right\rfloor \geq \left\lfloor\frac{n}{ \frac{n}{i} }\right\rfloor = \left\lfloor i \right\rfloor=i \\ \Rightarrow &i\leq \left\lfloor\frac{n}{ \left\lfloor\frac{n}{i}\right\rfloor }\right\rfloor \end{split} 
{{</latex>}}

即{{<latex>}}j=\left\lfloor\frac{n}{\left\lfloor\frac{n}{i}\right\rfloor}\right\rfloor{{</latex>}}

利用上述结论，我们每次以{{<latex>}}[i,j]{{</latex>}}为一块，分块求和即可

## 积性函数

### 定义

若{{<latex>}}\gcd(x,y)=1{{</latex>}}且{{<latex>}}f(xy)=f(x)f(y){{</latex>}}，则{{<latex>}}f(n){{</latex>}}为积性函数。

### 性质

若{{<latex>}}f(x){{</latex>}}和{{<latex>}}g(x){{</latex>}}均为积性函数，则以下函数也为积性函数

{{<latex>}}
\begin{aligned} h(x)&=f(x^p)\\ h(x)&=f^p(x)\\ h(x)&=f(x)g(x)\\ h(x)&=\sum_{d\mid x}f(d)g(\frac{x}{d}) \end{aligned} 
{{</latex>}}

例子

- 单位函数：{{<latex>}}\epsilon(n)=[n=1]{{</latex>}}
- 恒等函数：{{<latex>}}\operatorname{id}_k(n)=n^k\operatorname{id}_{1}(n){{</latex>}}通常记做{{<latex>}}\operatorname{id}(n){{</latex>}}
- 常数函数：{{<latex>}}1(n)=1{{</latex>}}
- 除数函数：{{<latex>}}\sigma_{k}(n)=\sum_{d\mid n}d^{k}\sigma_{0}(n){{</latex>}}通常简记做{{<latex>}}\operatorname{d}(n){{</latex>}}或{{<latex>}}\tau(n){{</latex>}}，{{<latex>}}\sigma_{1}(n){{</latex>}}通常简记做{{<latex>}}\sigma(n){{</latex>}}
- 欧拉函数：{{<latex>}}\varphi(n)=\sum_{i=1}^n [\gcd(i,n)=1]{{</latex>}}
- 莫比乌斯函数：{{<latex>}}\mu(n) = \begin{cases}1 & n=1 \\ 0 & \exists d:d^{2} \mid n \\ (-1)^{\omega(n)} & otherwise\end{cases}{{</latex>}}其中{{<latex>}}\omega(n){{</latex>}}表示{{<latex>}}n{{</latex>}}的本质不同质因子个数，是一个加性函数

## Dirichlet卷积

### 定义

定义两个数论函数{{<latex>}}f,g{{</latex>}}的Dirichlet卷积为

{{<latex>}}
(f*g)(n)=\sum_{d\mid n}f(d)g(\frac{n}{d}) 
{{</latex>}}

### 性质

Dirichlet卷积满足交换律和结合律

其中{{<latex>}}\varepsilon{{</latex>}}为Dirichlet卷积的单位元（任何函数卷{{<latex>}}\epsilon{{</latex>}}都为其本身）

### 例子

{{<latex>}}
\begin{aligned} \varepsilon=\mu*1&\Leftrightarrow\varepsilon(n)=\sum_{d\mid n}\mu(d)\\ d=1*1&\Leftrightarrow d(n)=\sum_{d\mid n}1\\ \sigma=d*1&\Leftrightarrow\varepsilon(n)=\sum_{d\mid n}d\\ \varphi=\mu*\text{ID}&\Leftrightarrow\varphi(n)=\sum_{d\mid n}d\cdot\mu(\frac{n}{d}) \end{aligned} 
{{</latex>}}

## 莫比乌斯函数

### 定义

{{<latex>}}\mu{{</latex>}}为莫比乌斯函数

### 性质

莫比乌斯函数不但是积性函数，还有如下性质：

{{<latex>}}
 \mu(n)= \begin{cases} 1&n=1\\ 0&n\text{ 含有平方因子}\\ (-1)^k&k\text{ 为 }n\text{ 的本质不同质因子个数}\\ \end{cases} 
{{</latex>}}

### 证明

{{<latex>}}
\varepsilon(n)= \begin{cases} 1&n=1\\ 0&n\neq 1\\ \end{cases} 
{{</latex>}}

其中{{<latex>}}\displaystyle\varepsilon(n)=\sum_{d\mid n}\mu(d){{</latex>}}即{{<latex>}}\varepsilon=\mu*1{{</latex>}}

设{{<latex>}}\displaystyle n=\prod_{i=1}^k{p_i}^{c_i},n'=\prod_{i=1}^k p_i{{</latex>}}

那么{{<latex>}}\displaystyle\sum_{d\mid n}\mu(d)=\sum_{d\mid n'}\mu(d)=\sum_{i=0}^k C_k^i\cdot(-1)^k{{</latex>}}

根据二项式定理，易知该式子的值在{{<latex>}}k=0{{</latex>}}即{{<latex>}}n=1{{</latex>}}时的值为{{<latex>}}1{{</latex>}}否则为{{<latex>}}0{{</latex>}}，这也同时证明了{{<latex>}}\sum_{d|n}{\mu(d)}=[n=1]{{</latex>}}

### 补充结论

反演推论：{{<latex>}}\displaystyle [gcd(i,j)=1] \Leftrightarrow\sum_{d\mid\gcd(i,j)}\mu(d){{</latex>}}

- **直接推导：**如果看懂了上一个结论，这个结论稍加思考便可以推出：如果{{<latex>}}\gcd(i,j)=1{{</latex>}}的话，那么代表着我们按上个结论中枚举的那个{{<latex>}}n{{</latex>}}是{{<latex>}}1{{</latex>}}，也就是式子的值是{{<latex>}}1{{</latex>}}，反之，有一个与{{<latex>}}[\gcd(i,j)=1]{{</latex>}}相同的值：0
- **利用{{<latex>}}\varepsilon{{</latex>}}函数：**根据上一结论，{{<latex>}}[\gcd(i,j)=1]\Rightarrow \varepsilon(\gcd(i,j)){{</latex>}}，将{{<latex>}}\varepsilon{{</latex>}}展开即可。

### 线性筛

由于{{<latex>}}\mu{{</latex>}}函数为积性函数，因此可以线性筛莫比乌斯函数（线性筛基本可以求所有的积性函数，尽管方法不尽相同）。

<details>
<summary>Code</summary>

```c++
void getMu() {
    mu[1] = 1;
    for (int i = 2; i <= n; ++i) {
        if (!flg[i]) p[++tot] = i, mu[i] = -1;
        for (int j = 1; j <= tot && i * p[j] <= n; ++j) {
            flg[i * p[j]] = 1;
            if (i % p[j] == 0) {
                mu[i * p[j]] = 0;
                break;
            }
            mu[i * p[j]] = -mu[i];
        }
    }
}
```

</details>

### 拓展

{{<latex>}}\varphi*1=\text{ID}\text{（ID 函数即 } f(x)=x\text{）}{{</latex>}}

将{{<latex>}}n{{</latex>}}分解质因数：{{<latex>}}\displaystyle n=\prod_{i=1}^k {p_i}^{c_i}{{</latex>}}

首先，因为{{<latex>}}\varphi{{</latex>}}是积性函数，故只要证明当{{<latex>}}n'=p^c{{</latex>}}时{{<latex>}}\displaystyle\varphi*1=\sum_{d\mid n'}\varphi(\frac{n'}{d})=\text{ID}{{</latex>}}成立即可。

因为{{<latex>}}p{{</latex>}}是质数，于是{{<latex>}}d=p^0,p^1,p^2,\cdots,p^c{{</latex>}}

易知如下过程

{{<latex>}}
\begin{aligned} \varphi*1&=\sum_{d\mid n}\varphi(\frac{n}{d})\\ &=\sum_{i=0}^c\varphi(p^i)\\ &=1+p^0\cdot(p-1)+p^1\cdot(p-1)+\cdots+p^{c-1}\cdot(p-1)\\ &=p^c\\ &=\text{ID}\\ \end{aligned} 
{{</latex>}}

该式子两侧同时卷{{<latex>}}\mu{{</latex>}}可得{{<latex>}}\displaystyle\varphi(n)=\sum_{d\mid n}d\cdot\mu(\frac{n}{d}){{</latex>}}

## 莫比乌斯反演

### 公式

设{{<latex>}}f(n),g(n){{</latex>}}为两个数论函数

如果有{{<latex>}}f(n)=\sum_{d\mid n}g(d){{</latex>}}

那么有{{<latex>}}g(n)=\sum_{d\mid n}\mu(d)f(\frac{n}{d}){{</latex>}}

### 证明

**暴力计算：**{{<latex>}}\sum_{d\mid n}\mu(d)f(\frac{n}{d})=\sum_{d\mid n}\mu(d)\sum_{k\mid \frac{n}{d}}g(k)=\sum_{k\mid n}g(k)\sum_{d\mid \frac{n}{k}}\mu(d)=g(n){{</latex>}}

用{{<latex>}}\displaystyle\sum_{d\mid n}g(d){{</latex>}}来替换{{<latex>}}f(\dfrac{n}{d}){{</latex>}}，再变换求和顺序。最后一步转为的依据：{{<latex>}}\displaystyle\sum_{d\mid n}\mu(d)=[n=1]{{</latex>}}

因此在{{<latex>}}\dfrac{n}{k}=1{{</latex>}}时第二个和式的值才为{{<latex>}}1{{</latex>}}。此时{{<latex>}}n=k{{</latex>}}，故原式等价于{{<latex>}}\displaystyle\sum_{k\mid n}[n=k]\cdot g(k)=g(n){{</latex>}}

**运用卷积：**

原问题为：已知{{<latex>}}f=g*1{{</latex>}}，证明{{<latex>}}g=f*\mu{{</latex>}}

易知如下转化：{{<latex>}}f*\mu=g*1*\mu\Rightarrow f*\mu=g{{</latex>}}（其中{{<latex>}}1*\mu=\varepsilon{{</latex>}}）

## 莫比乌斯反演拓展

对于数论函数{{<latex>}}f,g{{</latex>}}和完全积性函数{{<latex>}}t{{</latex>}}且{{<latex>}}t(1)=1{{</latex>}}：

{{<latex>}}
f(n)=\sum_{i=1}^nt(i)g\left(\left\lfloor\frac{n}{i}\right\rfloor\right)\\ \Leftrightarrow g(n)=\sum_{i=1}^n\mu(i)t(i)f\left(\left\lfloor\frac{n}{i}\right\rfloor\right)
{{</latex>}}

**我们来证明一下：**

{{<latex>}}
\begin{eqnarray} &&g(n)=\sum_{i=1}^n\mu(i)t(i)f\left(\left\lfloor\frac{n}{i}\right\rfloor\right)\\ &=&\sum_{i=1}^n\mu(i)t(i) \sum_{j=1}^{\left\lfloor\frac{n}{i}\right\rfloor}t(j) g\left(\left\lfloor\frac{\left\lfloor\frac{n}{i}\right\rfloor}{j}\right\rfloor\right)\\ &=&\sum_{i=1}^n\mu(i)t(i) \sum_{j=1}^{\left\lfloor\frac{n}{i}\right\rfloor}t(j) g\left(\left\lfloor\frac{n}{ij}\right\rfloor\right)\\ &=&\sum_{T=1}^n \sum_{i=1}^n\mu(i)t(i) \sum_{j=1}^{\left\lfloor\frac{n}{i}\right\rfloor}[ij=T] t(j)g\left(\left\lfloor\frac{n}{T}\right\rfloor\right) &&\text{【先枚举 ij 乘积】}\\ &=&\sum_{T=1}^n \sum_{i|T}\mu(i)t(i) t\left(\frac{T}{i}\right)g\left(\left\lfloor\frac{n}{T}\right\rfloor\right) &&\text{【}\sum_{j=1}^{\left\lfloor\frac{n}{i}\right\rfloor}[ij=T] \text{对答案的贡献为 1，于是省略】}\\ &=&\sum_{T=1}^ng\left(\left\lfloor\frac{n}{T}\right\rfloor\right) \sum_{i|T}\mu(i)t(i)t\left(\frac{T}{i}\right)\\ &=&\sum_{T=1}^ng\left(\left\lfloor\frac{n}{T}\right\rfloor\right) \sum_{i|T}\mu(i)t(T) &&\text{【t 是完全积性函数】}\\ &=&\sum_{T=1}^ng\left(\left\lfloor\frac{n}{T}\right\rfloor\right)t(T) \sum_{i|T}\mu(i)\\ &=&\sum_{T=1}^ng\left(\left\lfloor\frac{n}{T}\right\rfloor\right)t(T) \varepsilon(T) &&\text{【}\mu\ast 1= \varepsilon\text{】}\\ &=&g(n)t(1) &&\text{【当且仅当 T=1,}\varepsilon(T)=1\text{时】}\\ &=&g(n) && \square \end{eqnarray} 
{{</latex>}}

**解法二：**

转化一下，可以将式子写成

{{<latex>}}
\begin{eqnarray} &&\sum_{d=1}^{n}\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}ijd\cdot[gcd(i,j)=1]\\ &=&\sum_{d=1}^{n}d\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}ij\sum_{t\mid gcd(i,j)}\mu(t)\\ &=&\sum_{d=1}^{n}d\sum_{i=1}^{\lfloor\frac{n}{d}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{d}\rfloor}ij\sum_{t=1}^{\lfloor\frac{n}{d}\rfloor}\mu(t)[t\mid gcd(i,j)]\\ &=&\sum_{d=1}^{n}d\sum_{t=1}^{\lfloor\frac{n}{d}\rfloor}t^2 \mu(t)\sum_{i=1}^{\lfloor\frac{n}{td}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{td}\rfloor}ij[1\mid gcd(i,j)]\\ &=&\sum_{d=1}^{n}d\sum_{t=1}^{\lfloor\frac{n}{d}\rfloor}t^2 \mu(t)\sum_{i=1}^{\lfloor\frac{n}{td}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{td}\rfloor}ij \end{eqnarray} 
{{</latex>}}

容易知道

{{<latex>}}
\sum_{i=1}^{n}\sum_{j=1}^{m}ij=\frac{n(n+1)}{2}\cdot \frac{m(m+1)}{2} 
{{</latex>}}

设{{<latex>}}sum(n,m)=\sum_{i=1}^{n}\sum_{j=1}^{m}ij{{</latex>}}，继续接着前面的往下推

{{<latex>}}
\begin{eqnarray} &&\sum_{d=1}^{n}d\sum_{t=1}^{\lfloor\frac{n}{d}\rfloor}t^2 \mu(t)\sum_{i=1}^{\lfloor\frac{n}{td}\rfloor}\sum_{j=1}^{\lfloor\frac{m}{td}\rfloor}ij\\ &=&\sum_{d=1}^{n}d\sum_{t=1}^{\lfloor\frac{n}{d}\rfloor}t^2 \mu(t)\cdot sum(\lfloor\frac{n}{td}\rfloor,\lfloor\frac{m}{td}\rfloor)\\ &=&\sum_{T=1}^{n}sum(\lfloor\frac{n}{T}\rfloor,\lfloor\frac{m}{T}\rfloor)\sum_{d\mid T}d\cdot (\frac{T}{d})^2\mu(\frac{T}{d})\\ &=&\sum_{T=1}^{n}sum(\lfloor\frac{n}{T}\rfloor,\lfloor\frac{m}{T}\rfloor)(T\sum_{d\mid T}d\cdot\mu(d)) \end{eqnarray} 
{{</latex>}}

这时我们只要对每个{{<latex>}}T{{</latex>}}预处理出{{<latex>}}T\sum_{d\mid T}d\cdot\mu(d){{</latex>}}的值就行了，考虑如何快速求解

设{{<latex>}}f(n)=\sum_{d\mid n}d\cdot\mu(d){{</latex>}}

实际上{{<latex>}}f{{</latex>}}可以用线性筛筛出，具体的是

{{<latex>}}
f(n)= \begin{cases} 1-n &,n\in primes \\ f(\frac{x}{p}) &,p^2\mid n\\ f(\frac{x}{p})\cdot f(p) &,p^2\nmid n \end{cases} 
{{</latex>}}

其中{{<latex>}}p{{</latex>}}表示{{<latex>}}n{{</latex>}}的最小质因子

总时间复杂度{{<latex>}}O(n+\sqrt{n}){{</latex>}}

## 例题

[「HAOI 2011」Problem b ](https://www.lydsy.com/JudgeOnline/problem.php?id=2301)

[「SPOJ 5971」LCMSUM ](https://www.spoj.com/problems/LCMSUM/)

[「BZOJ 2154」Crash 的数字表格 ](https://www.lydsy.com/JudgeOnline/problem.php?id=2154)

[「SDOI2015」约数个数和 ](https://www.luogu.org/problemnew/show/P3327)

[「luogu 3768」简单的数学题 ](https://www.luogu.org/problemnew/show/P3768)



