---
title: "容斥原理"
date: 2019-08-15 07:31:38 +08:00
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

假设班里有{{<latex>}}10{{</latex>}}个学生喜欢数学，{{<latex>}}15{{</latex>}}个学生喜欢语文，{{<latex>}}21{{</latex>}}个学生喜欢编程，班里至少喜欢一门学科的有多少个学生呢？

<!--more-->

是 {{<latex>}}10+15+21=46{{</latex>}} 个吗？不是的，因为有些学生可能同时喜欢数学和语文，或者语文和编程，甚至还有可能三者都喜欢。

为了叙述方便，我们把喜欢语文、数学、编程的学生集合分别用 {{<latex>}}A,B,C{{</latex>}} 表示，则学生总数等于 {{<latex>}}|A\cup B\cup C|{{</latex>}} 。

刚才已经讲过，如果把这三个集合的元素个数 {{<latex>}}|A|,|B|,|C|{{</latex>}} 直接加起来，会有一些元素重复统计了，因此需要扣掉 {{<latex>}}|A\cap B|,|B\cap C|,|C\cap A|{{</latex>}} ，但这样一来，又有一小部分多扣了，需要加回来，即 {{<latex>}}|A\cap B\cap C|{{</latex>}} 。

即：{{<latex>}}|A\cup B\cup C|=|A|+|B|+|C|-|A\cap B|-|B\cap C|-|C\cap A|+|A\cap B\cap C|{{</latex>}}

![](https://img.akvicor.com/i/2024/09/15/66e675d767ec2.png)

把上述问题推广到一般情况，就是我们熟知的容斥原理。

## 容斥原理

设 U 中元素有 n 种不同的属性，而第 i 种属性称为 {{<latex>}}P_i{{</latex>}} ，拥有属性 {{<latex>}}P_i{{</latex>}} 的元素构成集合 {{<latex>}}S_i{{</latex>}} ，那么

{{<latex>}}
\begin{split} \left|\bigcup_{i=1}^{n}S_i\right|=&\sum_{i}|S_i|-\sum_{i<j}|S_i\cap S_j|+\sum_{i<j<k}|S_i\cap S_j\cap S_k|-\cdots\\ &+(-1)^{m-1}\sum_{a_i<a_{i+1} }\left|\bigcap_{i=1}^{m}S_{a_i}\right|+\cdots+(-1)^{n-1}|S_1\cap\cdots\cap S_n| \end{split} 
{{</latex>}}

即

{{<latex>}}
\left|\bigcup_{i=1}^{n}S_i\right|=\sum_{m=1}^n(-1)^{m-1}\sum_{a_i<a_{i+1} }\left|\bigcap_{i=1}^mS_{a_i}\right| 
{{</latex>}}

### 证明

对于每个元素使用二项式定理计算其出现的次数。对于 {{<latex>}}x{{</latex>}} ，假设它出现在 {{<latex>}}T_1,T_2,\cdots,T_m{{</latex>}} 的集合中，那么它的出现次数为

{{<latex>}}
\begin{split} Cnt=&|\{T_i\}|-|\{T_i\cap T_j|i<j\}|+\cdots+(-1)^{k-1}\left|\left\{\bigcap_{i=1}^{k}T_{a_i}|a_i<a_{i+1}\right\}\right|\\ &+\cdots+(-1)^{m-1}|\{T_1\cap\cdots\cap T_m\}|\\ =&C_m^1-C_m^2+\cdots+(-1)^{m-1}C_m^m\\ =&C_m^0-\sum_{i=0}^m(-1)^iC_m^i\\ =&1-(1-1)^m=1 \end{split} 
{{</latex>}}

于是每个元素出现的次数为 1，那么合并起来就是并集。证毕。

### 补集

对于全集 U 下的 **集合的并** 可以使用容斥原理计算，而集合的交则用全集减去 **补集的并集** 求得：

{{<latex>}}
\left|\bigcap_{i=1}^{n}S_i\right|=|U|-\left|\bigcup_{i=1}^n\overline{S_i}\right| 
{{</latex>}}

右边使用容斥即可。

## 不定方程非负整数解计数

> 给出不定方程 {{<latex>}}\sum_{i=1}^nx_i=m{{</latex>}} 和 n 个限制条件 {{<latex>}}x_i\leq b_i{{</latex>}} ，其中 {{<latex>}}m,b_i\leq \mathbb{N}{{</latex>}} . 求方程的非负整数解的个数

### 没有限制时

如果没有 {{<latex>}}x_i<b_i{{</latex>}} 的限制，那么不定方程 {{<latex>}}\sum_{i=1}^nx_i=m{{</latex>}} 的非负整数解的数目为 {{<latex>}}C_{m+n-1}^{n-1}{{</latex>}} .

略证：插板法。

相当于你有 m 个球要分给 n 个盒子，允许某个盒子是空的。这个问题不能直接用组合数解决。

于是我们再加入 n-1 个球，于是问题就变成了在一个长度为 m+n-1 的球序列中选择 n-1 个球，然后这个 n-1 个球把这个序列隔成了 n 份，恰好可以一一对应放到 n 个盒子中。那么在 m+n-1 个球中选择 n-1 个球的方案数就是 {{<latex>}}C_{m+n-1}^{n-1}{{</latex>}} 。

### 容斥模型

接着我们尝试抽象出容斥原理的模型

1. 全集 U：不定方程 {{<latex>}}\sum_{i=1}^nx_i=m{{</latex>}} 的非负整数解
2. 元素：变量 {{<latex>}}x_i{{</latex>}} .
3. 属性： {{<latex>}}x_i{{</latex>}} 的属性即 {{<latex>}}x_i{{</latex>}} 满足的条件，即 {{<latex>}}x_i\leq b_i{{</latex>}} 的条件

目标：所有变量满足对应属性时集合的大小，即 {{<latex>}}|\bigcap_{i=1}^nS_i|{{</latex>}} .

这个东西可以用 {{<latex>}}\left|\bigcap_{i=1}^{n}S_i\right|=|U|-\left|\bigcup_{i=1}^n\overline{S_i}\right|{{</latex>}} 求解。 {{<latex>}}|U|{{</latex>}} 可以用组合数计算，后半部分自然使用容斥原理展开。

那么问题变成，对于一些 {{<latex>}}\overline{S_{a_i}}{{</latex>}} 的交集求大小。考虑 {{<latex>}}\overline{S_{a_i}}{{</latex>}} 的含义，表示 {{<latex>}}x_{a_i}\geq b_{a_i}+1{{</latex>}} 的解的数目。而交集表示同时满足这些条件。因此这个交集对应的不定方程中，有些变量有 **下界限制** ，而有些则没有限制。

能否消除这些下界限制呢？既然要求的是非负整数解，而有些变量的下界又大于 0，那么我们直接 **把这个下界减掉** ，就可以使得这些变量的下界变成 0，即没有下界啦。因此对于

{{<latex>}}
\left|\bigcap_{a_i<a_{i+1} }^{1\leq i\leq k}S_{a_i}\right| 
{{</latex>}}

的不定方程形式为

{{<latex>}}
\sum_{i=1}^nx_i=m-\sum_{i=1}^k(b_{a_i}+1) 
{{</latex>}}

于是这个也可以组合数计算啦。这个长度为 k 的 a 数组相当于在枚举子集。

## HAOI2008 硬币购物

> 4 种面值的硬币，第 i 种的面值是 {{<latex>}}C_i{{</latex>}} 。n 次询问，每次询问给出每种硬币的数量 {{<latex>}}D_i{{</latex>}} 和一个价格 {{<latex>}}S{{</latex>}} ，问付款方式。
> 
> {{<latex>}}n\leq 10^3,S\leq 10^5{{</latex>}}


如果用背包做的话复杂度是 {{<latex>}}O(4nS){{</latex>}} ，无法承受。这道题最明显的特点就是硬币一共只有四种。抽象模型，其实就是让我们求方程 {{<latex>}}\sum_{i=1}^4C_ix_i=S,x_i\leq D_i{{</latex>}} 的非负整数解的个数。

采用同样的容斥方式，{{<latex>}}x_i{{</latex>}}  的属性为 {{<latex>}}x_i\leq D_i{{</latex>}} . 套用容斥原理的公式，最后我们要求解

{{<latex>}}
\sum_{i=1}^4C_ix_i=S-\sum_{i=1}^kC_{a_i}(D_{a_i}+1) 
{{</latex>}}

也就是无限背包问题。这个问题可以预处理，算上询问，总复杂度 {{<latex>}}O(4S+2^4n){{</latex>}} .

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>

using namespace std;
const int S = 1e5 + 5;
int c[5], d[5], n, s;
long long f[S];

int main() {

    cin >> c[1] >> c[2] >> c[3] >> c[4] >> n;
    f[0] = 1;
    for (int j = 1; j <= 4; j++)
        for (int i = 1; i < S; i++)
            if (i >= c[j]) f[i] += f[i - c[j]];
    for (int i = 1; i <= n; i++) {
        cin >> d[1] >> d[2] >> d[3] >> d[4] >> s;
        long long ans = 0;
        for (int j = 1; j < 16; j++) {
            int m = s, bit = 0;
            for (int k = 1; k <= 4; k++)
                if ((j >> (k - 1)) & 1) m -= (d[k] + 1) * c[k], bit++;
            if (m >= 0) ans += (bit % 2 * 2 - 1) * f[m];
        }
        cout << f[s] - ans << endl;
    }
    return 0;
}
```

</details>

