---
title: "Strange Towers of Hanoi"
date: 2019-09-30 07:58:40 +08:00
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

将汉诺塔中的3跟柱子改为4根，求盘子数为1到12时将全部盘子从第一根移动到最后一根需要移动的次数。

<!--more-->

考虑正常的汉诺塔规则，若有 `n` 个圆盘，那么就要将前 `n−1` 个圆盘移动到 2 号柱，再把最大的圆盘移动到 3 号柱，最后将前 `n−1` 个圆盘移动到 3 号柱。那么将 `n−1` 个圆盘移动又要涉及到 `n−2` 个圆盘，以此类推，所以，3个柱子得到的方程是 $f[i]=f[i-1] \times 2 + 1$

<details>
<summary>Code</summary>

```cpp
void hanoi(int n, char A, char B, char C){
    if(n == 1) printf("%c -> %c\n", A, C);
    else{
        hanoi(n-1, A, C, B); // 把 A 上面的 n-1 块借助 C 移动到 B
        printf("%c -> %c\n", A, C); // 第 n 块从 A 移动到 C
        hanoi(n-1, B, A, C); // 把 B 上面的 n-1 块借助 A 移动到 C
    }
}
```

</details>

