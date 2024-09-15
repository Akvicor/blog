---
title: "约数定理（约数个数定理，约束和定理）"
date: 2019-05-26T14:51:38Z
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

约数个数定理可以计算出一个数约数的个数

<!--more-->


## 约数个数定理
对于一个大于1正整数n可以分解质因数：{{<latex>}}f(n)=\prod^k_{i=1}{p_i}^{a_i}={p_1}^{a_1}\cdot{p_2}^{a_2}\cdot...\cdot{p_k}^{a_k}{{</latex>}}

则n的正约数的个数就是 {{<latex>}}f(n)=\prod^k_{i=1}(a_i+1)=(a_1+1)+(a_2+1)..(a_k+1){{</latex>}}。

其中 {{<latex>}}a_1、a_2、a_3...a_k{{</latex>}} 是 {{<latex>}}p_1、p_2、p_3，p_k{{</latex>}} 的指数。

### 定理简证

首先同上，n可以分解质因数 {{<latex>}}n={p_1}^{a_1}\times{p_2}^{a_2}\times ... \times{p_k}^{a_k}{{</latex>}} ,

由约数定义可知 {{<latex>}}{p_1}^{a_1}{{</latex>}} 的约数有: {{<latex>}}{p_1}^0, {p_1}^1, {p_1}^2......{p_1}^{a_1}{{</latex>}} ，共 {{<latex>}}(a_1+1){{</latex>}} 个;同理 {{<latex>}}{p_2}^{a_2}{{</latex>}} 的约数有 {{<latex>}}(a_2+1){{</latex>}} 个...... {{<latex>}}{p_k}^{a_k}{{</latex>}} 的约数有 {{<latex>}}(a_k+1){{</latex>}} 个。

故根据乘法原理：n的约数的个数就是 {{<latex>}}(a_1+1)(a_2+1)(a_3+1)…(a_k+1){{</latex>}} 。

### 例
例：正整数378000共有多少个正约数？

解：将378000分解质因数 {{<latex>}}378000=2^4×3^3×5^3×7^1{{</latex>}}

由约数个数定理可知378000共有正约数 {{<latex>}}(4+1)×(3+1)×(3+1)×(1+1)=160{{</latex>}} 个。

### 实现

```cpp
int f1(int n) {
    int r = (int) sqrt(1.0 * n);
    int sum = 0;
    if (r * r == n) {
        sum++;
        r--;
    }
    for (int i = 1; i <= r; i++)
        if (n % i == 0) {
            sum += 2;
        }
    cout << sum << endl;
}

// 稍快于f1
int f2(int n){
    int s = 1, r;
    for (int i = 2; i * i <= n; i++) {
        r = 0;
        while (n % i == 0) {
            r++;
            n /= i;
        }
        if (r > 0) {
            r++;
            s *= r;
        }
    }
    if (n > 1)
        s *= 2;
    cout << s << endl;
}

ll f3(ll n) {
    ll res=1;
    for(ll i=2;i*i<=n;i++){
        ll k=0;
        while(n%i == 0){
            n = n/i;
            k++;
        }
        if(k) res *= (k+1);
    }
    if(n != 1) res=res*2;
    if(res==1){
        if(n==1) return 1;
        else return 2;
    }
    cout << res << endl;
}
```


## 约数和定理

对于一个大于1正整数n可以分解质因数：{{<latex>}}n={p_1}^{a_1}\cdot{p_2}^{a_2}\cdot...\cdot{p_k}^{a_k}{{</latex>}}

则由约数个数定理可知n的正约数有{{<latex>}}(a_i+1)(a_2+1)(a_3+1){{</latex>}}个，那么n的{{<latex>}}(a_i+1)(a_2+1)(a_3+1){{</latex>}}个正约数的和为

{{<latex>}}f(n)=(p_1^0+p_1^1+p_1^2+...+p_1^{a_1})(p_2^0+p_2^1+p_2^2+...++p_2^{a_2})...(p_k^0+p_k^1+p_k^2+...++p_k^{a_k}){{</latex>}}

### 定理证明

若n可以分解质因数： {{<latex>}}n=p_1^{a_1}*p_2^{a_2}*p_3^{a_3}*...*p_k^{a_k}{{</latex>}}

可知{{<latex>}}p_1^{a_1}{{</latex>}}的约数有：{{<latex>}}p_1^0,p_1^1,p_1^2,...,p_1^{a_1}{{</latex>}}

....

同理可知，{{<latex>}}p_k^{a_k}{{</latex>}}的约数有：{{<latex>}}p_k^0,p_k^1,p_k^2,...,p_k^{a_k}{{</latex>}}

实际上n的约数是在{{<latex>}}p_1^{a_1}、p_2^{a_2}、p_3^{a_3}、...、p_k^{a_k}{{</latex>}}每一个的约数中分别挑一个相乘得来，可知共有{{<latex>}}(a_1+1)(a_2+1)(a_3+1)...(a_k+1){{</latex>}}种挑法，即约数的个数。

由乘法原理可知它们的和为

{{<latex>}}f(n)=(p_1^0+p_1^1+p_1^2+...+p_1^{a_1})(p_2^0+p_2^1+p_2^2+...+p_2^{a_2})...(p_k^0+p_k^1+p_k^2+...+p_k^{a_k}){{</latex>}}

### 例

例：正整数360的所有正约数的和是多少？

解：

将360分解质因数可得{{<latex>}}360=2^3 * 3^2 * 5^1{{</latex>}}

由约数和定理可知，360所有正约数的和为

{{<latex>}}(2^0+2^1+2^2+2^3)×(3^0+3^1+3^2)×(5^0+5^1)=(1+2+4+8)(1+3+9)(1+5)=15×13×6=1170{{</latex>}}

可知360的约数有：

{{<latex>}}1、2、3、4、5、6、8、9、10、12、15、18、20、24、30、36、40、45、60、72、90、120、180、360{{</latex>}}

则它们的和为：

{{<latex>}}1+2+3+4+5+6+8+9+10+12+15+18+20+24+30+36+40+45+60+72+90+120+180+360=1170{{</latex>}}

### 实现

```cpp
ll qpow(ll x, ll y) {
    ll res = 1;
    while (y) {
        if (y & 1) res *= x;
        x *= x;
        y >>= 1;
    }
    return res;
}

ll getsum(ll n) {//返回n的约数和是多少.
    ll res = 1;
    for (ll i = 2; i * i <= n; i++) {
        ll k = 0;
        while (n % i == 0) {
            n = n / i;
            k++;
        }
        res *= ((1 - qpow(i, k + 1)) / (1 - i));
    }    //用等比数列公式(快速幂)算.
    if (n != 1) res *= (1 + n);
    cout << res << endl;
}
```


## 题目

求正约数应用例题[hihocoder – 1284](http://hihocoder.com/problemset/problem/1284)

直接暴力肯定不行, 所以想到求他们的约数, 则n的约数*m的约数/GCD(n,m)的约数就是答案. 求约数用以上定理. 

```cpp
ll getnum(ll n) { //得到a的约数个数.
    ll res = 1;
    for (ll i = 2; i * i <= n; i++) {
        ll k = 0;
        while (n % i == 0) {
            n = n / i;
            k++;
        }
        if (k) res *= (k + 1);
    }
    if (n != 1) res = res * 2;   //最后一个素数.
    if (res == 1) {    //本身就是素数或1.
        if (n == 1) return 1;
        else return 2;
    }
    return res;
}

void solve(ll n, ll m) {
    ll k = __gcd(m, n);
    ll num1 = getnum(n);
    ll num2 = getnum(m);
    ll num3 = getnum(k);
    ll t = __gcd(num3, num1 * num2);
    printf("%lld %lld\n", 1ll * num1 * num2 / t, 1ll * num3 / t);
}
```

