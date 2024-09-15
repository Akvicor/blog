---
title: "判断两个线段相交"
date: 2019-05-29T13:20:20Z
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

如何判断两条直线是否相交？

这很容易。平面直线，无非就是两种关系：相交 或 平行。因此，只需判断它们是否平行即可。而直线平行，等价于它们的斜率相等，只需分别计算出它们的斜率，即可做出判断。

但倘若我把“直线”换成“线段”呢——如何判断两条线段是否相交？

这就有些难度了。和 直线 不同，线段 是有固定长度的，即使它们所属的两条直线相交，这两条线段也不一定相交。

<!--more-->

# 问题分析

对于“判断两条直线是否相交”这个问题，我们之所以能迅速而准确地进行判断，是因为“相交”与“不相交”这两个状态有着明显的不同点，即**斜率是否相等**。

那么现在，为了判断两条线段是否相交，我们也要找出“相交”与“不相交”这两个状态的不同点。

假设现在有两条线段 AB 和 CD，我们画出它们之间的三种关系：

![1](https://img.akvicor.com/i/2024/09/15/66e67623c0416.png)

![2](https://img.akvicor.com/i/2024/09/15/66e6762d6e515.png)

![3](https://img.akvicor.com/i/2024/09/15/66e67642e5873.png)

其中，情况 1 为不相交，情况 2、3 为相交。

作出向量 AC、AD、BC、BD。

首先介绍一个概念： **向量有序对的旋转方向**。这个概念指：对于共起点有序向量二元组(a, b)，其旋转方向为 **使 a 能够旋转一个小于 180 度的角并与 b 重合的方向**，简记为 `direct(a, b)`。若 `a` 和 `b` 反向共线，则旋转方向取任意值。

举个例子：图一中，`direct(AC, AD)` 为顺时针方向。

接下来我们要分析四个值：`direct(AC, AD)`、`direct(BC, BD)`、`direct(CA, CB)`、`direct(DA, DB)`。

对于图一，`direct(AC, AD)` 和 `direct(BC, BD)` 都为顺时针，`direct(CA, CB)` 为逆时针，`direct(DA, DB)` 为顺时针。

对于图二，`direct(AC, AD)` 为顺时针，`direct(BC, BD)` 为任意方向，`direct(CA, CB)` 为逆时针，`direct(DA, DB)` 为顺时针。

对于图三，`direct(AC, AD)`、`direct(DA, DB)` 为顺时针，`direct(BC, BD)`、`direct(CA, CB)` 为逆时针。

不难发现，两条线段相交的充要条件是：`direct(AC, AD) != direct(BC, BD)` 且 `direct(CA, CB) != direct(DA, DB)`。这便是“相交”与“不相交”这两个状态的不同点。

然而你可能会觉得：旋转方向这么一个虚无飘渺的东西，怎么用程序去描述啊？

再来看一幅图：

![](https://img.akvicor.com/i/2024/09/15/66e676554ab48.png)

再来定义有向角：

**有向角 `<a, b>` 为向量 `a` 逆时针旋转到与向量 `b` 重合所经过的角度。**

不难看出，对于向量 `a`、`b`：

若 `direct(a, b)` 为逆时针，则 `0 <= <a, b> <= 180`，从而 `sin<a, b> >= 0`。

若 `direct(a, b)` 为顺时针，则 `180 <= <a, b> <= 360`，从而 `sin<a, b> <= 0`。

这样一来，我们可以将旋转方向的问题转化为 **求有向角正弦值** 的问题。而这个问题，是很容易的。

如上图，记

{{<latex>}}OA=(x_1,y_1),OB=(x_2,y_2){{</latex>}}

{{<latex>}}|OA|=r_1, |OB|=r_2{{</latex>}}

则：{{<latex>}}sin(<OA, OB>){{</latex>}}

{{<latex>}}=\sin\theta{{</latex>}}

{{<latex>}}=\sin(\alpha-\beta){{</latex>}}

{{<latex>}}=\sin\alpha\cos\beta-\sin\beta\cos\alpha{{</latex>}}

{{<latex>}}=\frac{(\sin\alpha\cos\beta-\sin\beta\cos\alpha)r_1\cdot r_2}{r_1\cdot r_2}{{</latex>}}

{{<latex>}}=\frac{x_1 \cdot y_2-x_2 \cdot y_1}{r_1 \cdot r_2}{{</latex>}}

而这里，我们要的只是 `sin(<OA, OB>)` 的符号，而 `r1` 和 `r2` 又都是恒正的，因此只需判断 `x1 * y2 - x2 * y1` 的符号即可。

这个方法的数学背景是 叉乘，可以前往 [Wikipedia](https://zh.wikipedia.org/wiki/%E5%90%91%E9%87%8F%E7%A7%AF) 了解更多。

# 思路小结

- 由点 A，B，C，D 计算出向量 AC，AD，BC，BD

- 计算 `sin(<AC, AD>) * sin(<BC, BD>)` 和 `sin(<CA, CB>) * sin(<DA, DB>)`，若皆为非正数，则相交；否则，不相交。

# 实现

```python
# 点
class Point(object):

    def __init__(self, x, y):
        self.x, self.y = x, y

# 向量
class Vector(object):

    def __init__(self, start_point, end_point):
        self.start, self.end = start_point, end_point
        self.x = end_point.x - start_point.x
        self.y = end_point.y - start_point.y


ZERO = 1e-9

def negative(vector):
    """取反"""
    return Vector(vector.end_point, vector.start_point)

def vector_product(vectorA, vectorB):
    '''计算 x_1 * y_2 - x_2 * y_1'''
    return vectorA.x * vectorB.y - vectorB.x * vectorA.y

def is_intersected(A, B, C, D):
    '''A, B, C, D 为 Point 类型'''
    AC = Vector(A, C)
    AD = Vector(A, D)
    BC = Vector(B, C)
    BD = Vector(B, D)
    CA = negative(AC)
    CB = negative(BC)
    DA = negative(AD)
    DB = negative(BD)

    return (vector_product(AC, AD) * vector_product(BC, BD) <= ZERO) \
        and (vector_product(CA, CB) * vector_product(DA, DB) <= ZERO)
```



