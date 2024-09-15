---
title: "欧拉降幂 && 快速幂"
date: 2019-02-18T16:52:05Z
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---


__欧拉φ函数__：在数论中，对正整数n，欧拉函数是小于或等于n的正整数中与n互质的数的数目。此函数以其首名研究者欧拉命名，它又称为φ函数、欧拉商数等。

φ(1)=1;  φ(2)=1;  φ(3)=2;  φ(4)=2;  φ(9)=6

<!--more-->

## 欧拉φ函数

```cpp
int GetEuler(int n) {  //欧拉函数
     int res = n;
     for(int i = 2;i*i <= n; ++i){
         if(a%i == 0){
             res -= res/i; 
             while(n%i == 0) 
                 n /= i;
         }
     }
     if(a > 1) // 因为是遍历到sqrt(n)，所以可能存在未除尽或者n本身就为质数的情况
         res -= res/n;
     return res;
}

int Phi(int x) {  //欧拉函数
    int i, re = x;
    for (i = 2; i * i <= x; i++)
        if (x % i == 0) {
            re /= i;
            re *= i - 1;
            while (x % i == 0)
                x /= i;
        }
    if (x ^ 1) re /= x, re *= x - 1;
    return re;
}
```

## 欧拉降幂公式

a<sup>b</sup>(modc) = a<sup>bmodφ(c)+φ(c)</sup>mod(c)

## 快速幂

```cpp
LL quickPow(LL x, LL y, LL mod) {
    LL res = 1;
    for (; y; y >>= 1, x = x * x % mod)
        if (y & 1)res = res * x % mod;
    return res;
}

long long quickPow(long long a, int n) {
    long long answer = 1;
    for (; n; n /= 2, a = a * a % mod)
        if (n % 2 == 1) answer = answer * a % mod;
    return answer % mod;
}
```

## 应用题目

__题目目标__：2的n次方MOD1e9+7（1<=n<=10<sup>100000</sup>）

```cpp
#include<iostream>
#include<cstring>

using namespace std;
typedef long long LL;
const int MOD = 1e9 + 7;

int GetEuler(int n)  //欧拉函数
{
    int i;
    int res = n, a = n;
    for (i = 2; i * i <= a; ++i) {
        if (a % i == 0) {
            res -= res / i;
            while (a % i == 0)
                a /= i;
        }
    }
    if (a > 1)
        res -= res / a;
    return res;
}

LL quickPow(LL x, LL y, LL mod) {
    LL res = 1;
    for (; y; y >>= 1, x = x * x % mod)
        if (y & 1)res = res * x % mod;
    return res;
}

char n[100006];
char m[100006];

int main() {

    scanf("%s %s", n, m);
    int phi_c = GetEuler(MOD);
    int len = strlen(n);
    LL sum = 0;
    for (int i = 0; i < len; ++i) {
        sum = (sum * 10 + n[i] - '0') % phi_c;
    }
    cout << quickPow(2,sum+phi_c,MOD) << endl;
}
```

## 快速幂方法推导

### 算法1.直接设计算法

```cpp
int ans = 1;
for(int i = 1;i<=b;i++)
{
   ans = ans * a;
}
ans = ans % c;
```

__缺点__：这个算法存在着明显的问题，如果a和b过大，很容易就会溢出。

我们先来看看第一个改进方案：在讲这个方案之前，要先看这样一个公式：a<sup>b</sup> mod c = (a mod c)<sup>b</sup> mod c

### 算法2.改进算法

```cpp
int ans = 1;
a = a % c; //加上这一句
for(int i = 1;i<=b;i++)
{
   ans = ans * a;
}
ans = ans % c;
```

既然某个因子取余之后相乘再取余保持余数不变，那么新算得的ans也可以进行取余，所以得到比较良好的改进版本。

### 算法3.进一步改进算法

```cpp
int ans = 1;
a = a % c; //加上这一句
for(int i = 1;i<=b;i++)
{
   ans = (ans * a) % c;//这里再取了一次余
}
ans = ans % c;
```

这个算法在时间复杂度上没有改进，仍为O(b)，不过已经好很多的，但是在c过大的条件下，还是很有可能超时，所以，我们推出以下的快速幂算法。

### 算法4.快速幂算法

快速幂算法依赖于以下明显的公式：

a<sup>b</sup> __mod__ c = ((a<sup>2</sup>)<sup>b/2</sup>) __mod__ c		,b是偶数

a<sup>b</sup> __mod__ c = ((a<sup>2</sup>)<sup>b/2</sup>×a) __mod__ c		,b是奇数

```cpp
int PowerMod(int a, int b, int c)
{
    int ans = 1;
    a = a % c;
    while(b>0) {
        if(b % 2 = = 1)
        	ans = (ans * a) % c;
        b = b/2;
        a = (a * a) % c;
    }
    return ans;
}
```

本算法的时间复杂度为O(logb)

