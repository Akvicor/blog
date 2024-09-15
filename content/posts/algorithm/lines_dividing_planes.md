---
title: "直线划分平面"
date: 2019-05-31T20:01:42Z
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

如果一个平面中有n条直线，最多能将平面划分成多少区域。

<!--more-->

当1条线时  2个平面

当2条线时  4个平面（交叉1根线等多出2个平面）

当3条线时  7个平面  (交叉2根线等多出3个平面)

当4条线时  11个平面 (交叉3根线等多出4个平面)

.....

其实已经可以递推了，前n项和

{{<latex>}}f(n)=\frac{n*(n+1)}{2}+1{{</latex>}}

```cpp
while(cin>>n)
    cout<<n*(n+1)/2+1<<endl;
```

