---
title: "逆元的求法（欧拉定理、阶乘逆元、费马小定理、模质数p的情况）"
date: 2019-08-01 15:58:43 +08:00
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

- 乘法逆元

<!--more-->

## 乘法逆元

对于缩系中的元素，每个数a均有唯一的与之对应的乘法逆元x，使得{{<latex>}}ax\equiv 1\left(\text{mod}n\right){{</latex>}}，一个数有逆元的充分必要条件是gcd(a,n)=1，此时逆元唯一存在

在一般数学中，我们所说的逆元就是倒数，但是在数论中，如果一个数字a存在一个对p的逆元x，就可以写成{{<latex>}}ax\equiv 1\left(\text{mod}p\right){{</latex>}}的形式(此处p与a互质，若不互质，则不存在逆元)

逆元的含义：模n意义下，1个数a如果有逆元x，那么除以a相当于乘以x。

满足{{<latex>}}a \times k\equiv 1(\text{mod}p)的k值就是a关于p的乘法逆元{{</latex>}}

当我们要求{{<latex>}}\frac{a}{b}\text{mod}p{{</latex>}}的值时，我们就可以用到乘法逆元。我们可以用过求b关于p的乘法逆元k，将a乘上k再模p，即{{<latex>}}(a \times k)\text{mod}p{{</latex>}}。其结果与{{<latex>}}\frac{a}{b}\text{mod}p{{</latex>}}等价

<details>
<summary>证明</summary>

根据{{<latex>}}b \times k\equiv 1(\text{mod}p){{</latex>}}有{{<latex>}}b \times k=p \times x+1{{</latex>}}

{{<latex>}}k=\frac{p \times x+1}{b}{{</latex>}}

把k带入{{<latex>}}(a \times k)\text{mod}p{{</latex>}}得

{{<latex>}}(\frac{a \times (p \times x+1)}{b})\text{mod}p{{</latex>}}

{{<latex>}}=(\frac{a \times p \times x}{b}+\frac{a}{b})\text{mod}p{{</latex>}}

{{<latex>}}=[(\frac{a \times p \times x}{b}\text{mod}p)+\frac{a}{b}]\text{mod}p{{</latex>}}

{{<latex>}}=[(\frac{p \times (a \times x)}{b}\text{mod}p)+\frac{a}{b}]\text{mod}p{{</latex>}}

{{<latex>}}//p \times [\frac{a \times x}{b}]\text{mod}p=0{{</latex>}}

所以原式等于：{{<latex>}}\frac{a}{b}\text{mod}p{{</latex>}}

</details>

### 拓展欧几里得

**拓展欧几里得:**{{<latex>}}ax+by=\gcd(a,b){{</latex>}}的解一定存在

当我们要求{{<latex>}}a{{</latex>}}关于{{<latex>}}p{{</latex>}}的逆元时，若逆元存在，则{{<latex>}}\gcd(a,p)=1{{</latex>}}。假设逆元为{{<latex>}}x{{</latex>}}，即：{{<latex>}}ax \equiv 1(\text{mod}p){{</latex>}}

我们可以展开一下变成{{<latex>}}ax=1+pk{{</latex>}}，由于{{<latex>}}k{{</latex>}}可正可负，我们可以得到{{<latex>}}ax+pk=1{{</latex>}}，其实就是{{<latex>}}ax+by=\gcd(a,b){{</latex>}}。

我们用拓展欧几里得求出一个最小的x就是{{<latex>}}a{{</latex>}}关于{{<latex>}}p{{</latex>}}的一个逆元。

<!-- 这个方程可以转化为{{<latex>}}ax-my=1{{</latex>}}，然后套用求二元一次方程的方法，用拓展欧几里得算法求得一组{{<latex>}}x_0,y_0{{</latex>}}和gcd，检查gcd是否为1，gcd不为1则说明逆元不存在，若为1，则调整{{<latex>}}x_0{{</latex>}}到{{<latex>}}0\sim m-1{{</latex>}}的范围中即可 -->

**求解：**

现在我们已经有了{{<latex>}}ax+by=\gcd(a,b){{</latex>}}了。我们想试着求出一个特解{{<latex>}}x{{</latex>}}。

根据欧几里得算法我们可以知道{{<latex>}}\gcd(a,b)=\gcd(b,a\%b){{</latex>}}。

而且我们可以看出{{<latex>}}bx+(a\%b)y = \gcd(b,\%b){{</latex>}}

由此我们可得：(由于两边{{<latex>}}x{{</latex>}},{{<latex>}}y{{</latex>}}值不用，我们用{{<latex>}}x'{{</latex>}}和{{<latex>}}y'{{</latex>}}进行区分)

{{<latex>}}bx'+(a\%b)y'=ax+by{{</latex>}}

我们想要把式子简化一下，可以从{{<latex>}}a\%b{{</latex>}}入手，即{{<latex>}}a\%b=a-\lfloor \frac{a}{b} \rfloor\times b{{</latex>}}。

所以我们可以化简得到{{<latex>}}ax+by=bx'+(a-\lfloor \frac{a}{b} \rfloor\times b)y'{{</latex>}}

移项：{{<latex>}}ax+by=ay'+b(x'-\lfloor \frac{a}{b} \rfloor y'){{</latex>}}

系数相等，所以我们可以解得

{{<latex>}}
\begin{equation}
\left\{
             \begin{array}{lr}
             x=y' &  \\
             y=(x'-\lfloor \frac{a}{b} \rfloor y') &  
             \end{array}
\right.
\end{equation}
{{</latex>}}

根据欧几里得算法，我们一直递归下去，总会到{{<latex>}}a\%b=0{{</latex>}}

所以式子变成了{{<latex>}}ax=\gcd(a,b){{</latex>}}。此时我们取一个特解{{<latex>}}x=1,y=0{{</latex>}}。然后往回推，就可以得到一开始的那个x。

此时解出来的{{<latex>}}x{{</latex>}}就是{{<latex>}}a{{</latex>}}关于{{<latex>}}p{{</latex>}}的一个逆元。

PS：算法效率较高，常数较小，时间复杂度{{<latex>}}O(\ln{n}){{</latex>}}

<details>
<summary>Code</summary>

```cpp
void exgcd(LL a, LL b, LL &x, LL &y) {
	if (b == 0) {
		x = 1;y = 0;
	} else {
		exgcd(b, a%b, y, x);
		y -= (a/b) * x;
	}
}

void extgcd(LL a, LL b, LL &d, LL &x, LL &y) {
    if (!b) {
        d = a;        
        x = 1;
        y = 0;
    } else {
        extgcd(b, a % b, d, y, x);
        y -= x * (a / b);
    }
}

LL inverse(LL a, LL n) {
    LL d, x, y;
    extgcd(a, n, d, x, y);
    return d == 1 ? (x + n) % n : -1;
}
```

</details>

### 递推求阶乘逆元

我们经常会用到阶乘的逆元，我们可以考虑用费马小定理先求出最大的那个阶乘的逆元，然后再往回推

我们可以把{{<latex>}}n!{{</latex>}}的逆元表示为{{<latex>}}\left[n!\right]^{-1}{{</latex>}}。

我们要求{{<latex>}}(n-1)!{{</latex>}}的逆元，我们可以考虑给{{<latex>}}(n-1)!{{</latex>}}乘上一个{{<latex>}}n{{</latex>}}把他变为{{<latex>}}n!{{</latex>}}。

即{{<latex>}}(n-1)!\times n\left[n!\right]^{-1} \equiv 1\text{mod}p{{</latex>}}

因此{{<latex>}}\left[n!\right]^{-1}{{</latex>}}是{{<latex>}}(n-1)!{{</latex>}}关于{{<latex>}}p{{</latex>}}的一个逆元。

<details>
<summary>Code</summary>

```cpp
void init() {
	fact[0] = 1;
	for (int i = 1; i < maxn; i++) {
		fact[i] = fact[i - 1] * i %mod;
	}
	inv[maxn - 1] = power(fact[maxn - 1], mod - 2);
	for (int i = maxn - 2; i >= 0; i--) {
		inv[i] = inv[i + 1] * (i + 1) %mod;
	}
}
```

</details>

### 费马小定理

在模为素数p的情况下，有费马小定理{{<latex>}}a^{p-1}\equiv 1(\text{mod}p){{</latex>}}，（左右同除a）得{{<latex>}}a^{p-2}=a^{-1}(\text{mod}p){{</latex>}}，也就是说a的逆元为{{<latex>}}a^{p-2}{{</latex>}}

<details>
<summary>符号解释</summary>

既约{{<latex>}}\perp{{</latex>}}，{{<latex>}}a\perp m{{</latex>}}，a与m既约、不可约、互素：

既约，或称不可约，或称互质，或称互素，a,m既约，记做{{<latex>}}a \perp m{{</latex>}}或{{<latex>}}(a,m)=1{{</latex>}}即a,m二者的最大公约数为1，已经约去公因子到不可再约了。

</details>

而在模数不为素数p得情况下，有欧拉定理{{<latex>}}a^{phi(m)}\equiv q (\text{mod}m)\quad(a \perp m){{</latex>}} 

同理{{<latex>}}a^{-1}=a^{phi(m)-1}{{</latex>}}

因此逆元x便可以套用快速幂求得了{{<latex>}}x=a^{phi(m)-1}{{</latex>}}

如何判断a是否有逆元？检验逆元的性质，看求出的幂值x与a相乘是否为1即可

PS：这种算法复杂度为{{<latex>}}O(\log_2(N)){{</latex>}} 在几次测试中，常数似乎较上种方法大

当p比较大的时候需要用快速幂求解

<details>
<summary>Code</summary>

```cpp
LL pow_mod(LL x, LL n, LL mod) {
    LL res = 1;
    while (n > 0) {
        if (n & 1)res = res * x % mod;
        x = x * x % mod;
        n >>= 1;
    }
    return res;
}
```

</details>

当模p不是素数的时候需要用到欧拉定理

{{<latex>}}a^{phi(p)}\equiv 1 \quad (\text{mod}p){{</latex>}}

{{<latex>}}a \times a^{phi(p)-1}\equiv 1 \quad (\text{mod}p){{</latex>}}

{{<latex>}}a^{-1}\equiv a^{phi(p)-1} \quad (\text{mod}p){{</latex>}}

所以时间复杂度即求出单个欧拉函数的值

(当p为素数的时候{{<latex>}}phi(p)=p-1{{</latex>}}，则{{<latex>}}phi(p)-1=p-2{{</latex>}}可以看出欧拉定理是费马小定理的推广)

PS：这里就贴出欧拉定理的板子，很少会用欧拉定理求逆元

### 特殊情况

#### 一

当N是质数，这点也很好理解。当N是质数，{{<latex>}}0<a<N{{</latex>}}时，则a肯定存在逆元。而解出的就满足，故它是a的逆元。

[CF 696C](https://codeforces.com/problemset/problem/696/C)

#### 二

求逆元一般公式(条件b|a)

{{<latex>}}ans=\frac{a}{b}\text{mod}m = \frac{a\text{mod}(mb)}{b}{{</latex>}}

**证明：**

{{<latex>}}
\frac{a}{b}\text{mod}k=d \\\\
\frac{a}{b}=kx+d \\\\
a=(kb)x+bd \\\\
a\text{mod}(kb)=bd \\\\
\frac{a\text{mod}(kb)}{b} = d
{{</latex>}}

PS：实际上{{<latex>}}\frac{a\text{mod}(bm)}{b}{{</latex>}}这种对于所有的都适用，不区分互补互素，而费马小定理和拓展欧几里得算法求逆元是有局限性的，它们都会要求a与m互素，如果a与m不互素，那就没有逆元，这个时候需要{{<latex>}}\frac{a\text{mod}(bm)}{b}{{</latex>}}来搞（此时就不是逆元的概念了）。但是当a与m互素的时候，bm可能会很大，不适合套这个一般公式，所以大部分时候还是用逆元来搞

### 通过递推求1~n的逆元

**有时候会遇到这样一种问题**，在模质数p下，求{{<latex>}}q\sim n{{</latex>}}逆元{{<latex>}}n<p{{</latex>}}（这里p为奇质数）。可以O(n)求出所有逆元，有一个递推式如下

{{<latex>}}inv[i]=(M-\frac{M}{i}) \times inv[M\%i]\%M{{</latex>}}

**推导过程如下**，设{{<latex>}}t=\frac{M}{i} \quad k=M\%i{{</latex>}}

{{<latex>}}t \times i+k \equiv 0(\text{mod}M){{</latex>}}

{{<latex>}}-t \times i \equiv k(\text{mod}M){{</latex>}}

左右同时除{{<latex>}}i \times k{{</latex>}}，得到

{{<latex>}}-t \times inv[k] \equiv inv[i](\text{mod}M){{</latex>}}

再把t和k替换掉

{{<latex>}}inv[i]=(M-\frac{M}{i}) \times inv[M\%i]\%M{{</latex>}}

初始化{{<latex>}}inv[1]=1{{</latex>}}，这样就可以通过递推法求出1->n模奇素数p的所有逆元了

另外有个结论 1->p 模 p的所有逆元值对应 1->p 中所有的数，比如p=7，那么 1->6 对应的逆元是 1 4 5 2 3 6

<details>
<summary>Code</summary>

```cpp
const int N = 1e5 + 5;
int inv[N];

void inverse(int n, int p) {
    inv[1] = 1;
    for (int i = 2; i <= n; ++i) {
        inv[i] = (ll) (p - p / i) * inv[p % i] % p;
    }
}
```

</details>


### 通过递推求n!的逆元

递推公式：{{<latex>}}invf[i]=invf[i+1]\times(i+1)\%p{{</latex>}}

<details>
<summary>Code</summary>

```cpp
// 快速幂求逆元
int quick_inverse(int n, int p) {
	int ret = 1;
	int exponent = p - 2;
	for (int i = exponent; i; i >>= 1, n = n * n % p) {
		if (i & 1) {
			ret = ret * n % p;
		}
	} 
	return ret;
}
int invf[N], factor[N];
void get_factorial_inverse(int n, int p) {
	factor[0] = 1;
	for (int i = 1; i <= n; ++i) {
		factor[i] = i * factor[i - 1] % p;
	}
	invf[n] = quick_inverse(factor[n], p);
	for (int i = n-1; i >= 0; --i) {
		invf[i] = invf[i + 1] * (i + 1) % p;
	}
}
```

</details>

## 题目

[POJ-1845](http://poj.org/problem?id=1845)

