---
title: "Green公式-判断多边形边界曲线顺/逆时针"
date: 2019-04-14T10:58:34Z
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

判断一个多边形的边界曲线是否是顺时针或者逆时针

<!--more-->

```cpp
double d = 0;
for (int i = 0; i < n - 1; i++) {
    d += -0.5 * ( y[i + 1] + y[i]) * (x[i + 1] - x[i]);
}
if ( d > 0)
    cout << "counter clockwise" << endl;
else
    cout << "clockwise" << endl;

```



