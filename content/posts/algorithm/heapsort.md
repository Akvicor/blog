---
title: "堆排序"
date: 2020-01-23 22:29:22 +08:00
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

- 排序算法
- 不稳定排序
- {{<latex>}}O(n\log(n)){{</latex>}}

<!--more-->

# 堆

**堆**是具有以下性质的完全二叉树：

根结点标号为 `0`，则对于根 `i` 的左子为 `2i+1` 右子为 `2i+2`

- **大顶堆：**每个结点的值都大于或等于其左右子结点的值 `arr[i] >= arr[2i+1] && arr[i] >= arr[2i+2]`
- **小顶堆：**每个结点的值都小于或等于其左右子结点的值 `arr[i] <= arr[2i+1] && arr[i] <= arr[2i+2]`

![](https://img.akvicor.com/i/2024/09/15/66e675b9dfb23.png)

# 基本思想步骤

一般升序采用大顶堆，降序采用小顶堆

1. 将待排序序列构造成一个大顶堆，此时，整个序列的最大值就是堆顶的根节点
2. 将根结点与末尾元素进行交换，此时末尾就为最大值
3. 然后将剩余n-1个元素重新构造成一个堆，这样会得到n个元素的次小值




