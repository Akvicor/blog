---
title: "64位整数乘法"
date: 2019-09-13 22:26:09 +08:00
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

快速乘

<!--more-->

## O(log) 快速幂思想

类似于快速幂的思想，把整数 b 用二进制表示，即 

{{<latex>}}b=c_{k-1}2^{k-1}+c_{k-2}2^{k-2}+...+c_{0}2^{0}{{</latex>}}

那么

{{<latex>}}a * b=c_{k-1} * a * 2^{k-1}+c_{k-2} * a * 2^{k-2}+...+c_{0} * a * 2^{0}{{</latex>}}

因为 {{<latex>}}a * 2^i=(a * 2^{i-1}) * 2{{</latex>}}，若已求出 {{<latex>}}a * 2^{i-1}\text{mod}p{{</latex>}}，则计算 {{<latex>}}a * 2^{i-1}\text{mod}p{{</latex>}} 时，运算过程中的每一步结果都不超过 {{<latex>}}2 * 10^{18}{{</latex>}}

## O(1) 特殊情况下易精度丢失导致答案错误

利用 {{<latex>}}a * b\text{mod}p = a * b-\lfloor a * b/p \rfloor * p{{</latex>}}

首先，当 {{<latex>}}a,b < p{{</latex>}} 时，{{<latex>}}a * b/p{{</latex>}} 下取整以后也一定小于 {{<latex>}}p{{</latex>}}。我们可以用浮点数执行 {{<latex>}}a * b/p{{</latex>}} 的运算，而不用关心小数点之后的部分。long double 在十进制下有效位为 18~19 位。当浮点数的精度不足以保存精确数值时，它会像科学计数法一样舍弃低位，正好符合我们的要求。

虽然 {{<latex>}}a * b{{</latex>}} 和 {{<latex>}}\lfloor a * b/p \rfloor * p{{</latex>}} 可能很大，但是两者的差一定在 0~p-1之间。所以我们用 long long 来保存 {{<latex>}}a * b{{</latex>}} 和 {{<latex>}}\lfloor a * b/p \rfloor * p{{</latex>}} 各自的记过。整数运算溢出相当于舍弃高位，也正好符合我们的要求。

## Code

<details>
<summary>Code 1</summary>

```cpp
// O(1)
ll mul(ll a, ll b, ll p) {
	return ((__int128)a*b)%p;
}
// O(1)
ll mul(ll a, ll b, ll p) {
	a %= p; b %= p;
	ll c = (long double)a * b / p;
	ll ans = a * b - c * p;
	return (ans % p + p) % p;
}
// O(log)
ll mul(ll a, ll b, ll p) {
	ll ans = 0;
	while (b) {
		if (b & 1) ans = (ans + a) % p;
		a = a * 2 % p;
		b >>= 1;
	}
	return ans;
}
```

</details>

## Problem

[64 bit integer multiplication](https://icpc.akvicor.com/problem/1022)


