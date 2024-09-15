---
title: "最大子列和问题"
date: 2019-05-27T14:45:42Z
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

求取数组中最大连续子序列和，例如给定数组为A={1， 3， -2， 4， -5}， 则最大连续子序列和为{{<latex>}}6{{</latex>}}，即 {{<latex>}}1+3+(-2)+4=6{{</latex>}} 。

方法一共有三种，复杂度分别为 {{<latex>}}O(N^2){{</latex>}}、{{<latex>}}O(NlgN){{</latex>}}、{{<latex>}}O(N){{</latex>}}

<!--more-->

<details>
<summary>解法1 - 暴力 - {{<latex>}}O(N^2){{</latex>}}</summary>

因为最大连续子序列和只可能从数组0到n-1中的某个位置，我们可以遍历0到n-1个位置，计算由这个位置开始的所有连续子序列和中的最大值。最终求出最大值即可。

更详细的讲，就是计算从位置0开始的最大连续子序列和，从位置1开始的最大连续子序列和。。。直到从位置n-1开始的最大连续子序列和，最后求出所有这些连续子序列和中的最大值就是答案。

```cpp
int maxsequence(int arr[], int len) {
    int max = arr[0];
    for (int i=0; i<len; i++) {
        int sum = 0;
        for (int j=i; j<len; j++) {
            sum += arr[j];
            if (sum > max) max = sum;
        }
    }
    return max;
}
```

</details>

<details>
<summary>解法2 - 分治 - {{<latex>}}O(NlgN){{</latex>}}</summary>

运用分治的思想来求解，最大连续子序列和要么出现在数组左半部分，要么出现在数组右半部分，要么横跨左右两个部分。因此求出这三种情况下的最大值就可以得到最大连续子序列和。

```cpp
/*求三个数最大值*/
int max3(int i, int j, int k)
{
    if (i>=j && i>=k)
            return i;
    return max3(j, k, i);
}
int maxsequence2(int a[], int l, int u)
{
    if (l > u) return 0;
    if (l == u) return a[l];
    int m = (l + u) / 2;
 
    /*求横跨左右的最大连续子序列左半部分*/   
    int lmax=a[m], lsum=0;
    for (int i=m; i>=l; i--) {
        lsum += a[i];
        if (lsum > lmax)
            lmax = lsum;
    }
    
    /*求横跨左右的最大连续子序列右半部分*/   
    int rmax=a[m+1], rsum = 0;
    for (int i=m+1; i<=u; i++) {
        rsum += a[i];
        if (rsum > rmax)
            rmax = rsum;
    }
    return max3(lmax+rmax, maxsequence2(a, l, m), maxsequence2(a, m+1, u)); //返回三者最大值
}
```
</details>

<details>
<summary>解法3 - {{<latex>}}O(N){{</latex>}}</summary>

因为最大连续子序列和只可能是以位置0～n-1中某个位置结尾。当遍历到第i个元素时，判断在它前面的连续子序列和是否大于0，如果大于0，则以位置i结尾的最大连续子序列和为元素i和前面的连续子序列和想加；否则，则以位置i结尾的最大连续子序列和为元素i。

```cpp
int maxsequence3(int a[], int len)
{
    int maxsum, maxhere;
    maxsum = maxhere = a[0]; //初始化最大和为a【0】
    for (int i=1; i<len; i++) {
        if (maxhere <= 0)
            maxhere = a[i]; //如果前面位置最大连续子序列和小于等于0，则以当前位置i结尾的最大连续子序列和为a[i]
        else
            maxhere += a[i]; //如果前面位置最大连续子序列和大于0，则以当前位置i结尾的最大连续子序列和为它们两者之和
        if (maxhere > maxsum) {
            maxsum = maxhere; //更新最大连续子序列和
        }
    }
    return maxsum;
}
```
</details>

