---
title: "最短Hamilton路径"
date: 2019-09-14 08:40:49 +08:00
draft: false
categories: [CF]
tags: ["AcWing", "二进制状态压缩"]
card: false
weight: 0
---

[ACW-93 最短Hamilton路径](https://www.acwing.com/problem/content/93/)

<!--more-->

首先，很容易想到一种“朴素（brute-force）”做法，就是枚举n个点的全排列，计算路径长度取最小值，时间复杂度 {{<latex>}}O(n*n!){{</latex>}}。

如果利用二进制状态压缩 DP 可以优化到 {{<latex>}}O(n^2*2^n){{</latex>}}

在任意时刻，我们可以使用一个 n 位二进制数来表示那些点已经被访问过，那些点没有被访问过。如果第 i 位为 1，则代表第 i 位已经被访问过，反之则未被访问过。在任意时刻，我们还需要知道当前所在的位置，因此我们可以使用 `F[i, j]` {{<latex>}}0 \leq i < 2^n, 0 \leq j < n{{</latex>}} 表示 “点未被经过的状态” 对应的二进制数为 i，且目前处于点 j 时的最短路径。

在起点时，有 `F[1,0] = 0`，即只经过了点 0，（i 只有第 0 位为 1），目前处于起点 0，最短路长度为 0。最终目标是 `F[(1<<n)-1, n-1]`，即经过所有点（i的所有位均为1），处于终点 n-1 的最短路。

在任意时刻，有公式 `F[i, j] = min{F[i, j], F[i^(1<<j), k] + weight(k, j)}` 其中 {{<latex>}}0 \leq k < n{{</latex>}} 并且 `((i >> j) & 1) = 1`，即当前时刻 “未被经过的点的状态” 对应的二进制数为 i，处于点 j。因为 j 只能被恰好经过一次，所以一定是刚刚经过的。故在上一时刻 “被经过的点的状态” 对应的二进制数的第 j 位应该赋值为 0，也就是 `i ^ (1 << j)`。

另外，上一时刻所处的位置可能是 `i ^ (1 << j)` 中任意一个是 1 的位数 k，从 k 走到 j 需要经过 `weight(k, j)` 的路程，可以考虑所有这样的 k 取最小值。

## Code

<details>
<summary>Code 1</summary>

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
int f[1 << 20][20], weight[20][20], n;

int hamilton(int n, int weight[20][20]) {
	memset(f, 0x3f, sizeof(f));
	f[1][0] = 0;
	for (int i = 1; i < 1 << n; i++)
		for (int j = 0; j < n; j++) if (i >> j & 1)
			for (int k = 0; k < n; k++) if ((i^1<<j) >> k & 1)
				f[i][j] = min(f[i][j], f[i^1<<j][k]+weight[k][j]);
	return f[(1 << n) - 1][n - 1];
}

int main() {
	cin >> n;
	for(int i = 0; i < n; i++)
		for(int j = 0; j < n; j++)
			scanf("%d", &weight[i][j]);
	cout << hamilton(n, weight) << endl;
}
```

</details>

