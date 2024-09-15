---
title: "快速幂 a^b"
date: 2019-09-13 21:44:30 +08:00
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

快速幂

<!--more-->

根据数学常识，每一个正整数可以唯一表示为若干指数不重复的 2 的次幂的和。

{{<latex>}}3^{13} = 3^{(1101)_2} = 3^8 \cdot 3^4 \cdot 3^1{{</latex>}}

也就是说，如果 b 在二进制表示下有 k 位，其中第 {{<latex>}}i(0 \leq i < k){{</latex>}} 位的数字是 {{<latex>}}c_i{{</latex>}}，那么

{{<latex>}}b=c_{k-1}2^{k-1}+c_{k-2}2^{k-2}+...+c_{0}2^{0}{{</latex>}}

于是

{{<latex>}}a^b=a^{c_{k-1}*2^{k-1}}*a^{c_{k-2}*2^{k-2}}*...*a^{c_0}*2^0{{</latex>}}

因为 {{<latex>}}k=\lceil\log_2{(b+1)}\rceil{{</latex>}}，所以上式乘积的数量不多于 {{<latex>}}\lceil\log_2{(b+1)}\rceil{{</latex>}} 个

又因为 {{<latex>}}a^{2^i}=\left(a^{2^{i-1}}\right)^2{{</latex>}}，所以很容易通过 k 次递推求出每个乘积项，当 {{<latex>}}c_i=1{{</latex>}} 时，把该乘积项累积到答案中

因此为了计算 {{<latex>}}3^{13}{{</latex>}}，我们只需要将对应二进制位为 1 的整系数幂乘起来就行了

{{<latex>}}13 = (1101)_2{{</latex>}}

{{<latex>}}3^{13} = 6561 \cdot 81 \cdot 3 = 1594323{{</latex>}}

{{<latex>}}\begin{align} 3^1 &= 3 \\ 3^2 &= \left(3^1\right)^2 = 3^2 = 9 \\ 3^4 &= \left(3^2\right)^2 = 9^2 = 81 \\ 3^8 &= \left(3^4\right)^2 = 81^2 = 6561 \end{align}{{</latex>}}

## Code

<details>
<summary>Code 1</summary>

```cpp
LL quick_pow(LL a, LL b, LL p) {
    a %= p;
    LL res = 1 % p;
    for (; b; b >>= 1) {
        if (b & 1) res = res * a % p;
        a = a * a % p;
    }
    return res;
}
```

</details>

## Problem

[a^b](https://icpc.akvicor.com/problem/1021)



