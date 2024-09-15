---
title: "「火鼠的皮衣 -不焦躁的内心-」"
date: 2019-10-28 17:47:42 +08:00
draft: false
categories: [CF]
tags: ["ComentOJ"]
card: false
weight: 0
---

**[ ComentOJ-C72-4033 「火鼠的皮衣 -不焦躁的内心-」 ](https://cometoj.com/contest/72/problem/%EF%BC%A4?problem_id=4033)**

<!--more-->

给定 {{<latex>}}n, a, b, p{{</latex>}}（p不一定是质数），求 {{<latex>}}\sum\limits_{i=0}^{\lfloor\frac{n}{2}\rfloor} a^i b^{n-2i} {n\choose 2i}{{</latex>}} 对 {{<latex>}}p{{</latex>}} 取模的结果，T组询问。

首先对求和公式进行转化；

{{<latex>}}
\begin{equation}
\begin{aligned}
ans &= \sum\limits_{i=0}^{\lfloor\frac{n}{2}\rfloor} a^i b^{n-2i} {n\choose 2i} \\
    &= \sum\limits_{i=0}^{n}(\sqrt{a})^ib^{n-i}{n \choose i}\left[i\%2==0\right]
\end{aligned}
\end{equation}
{{</latex>}}

考虑把 {{<latex>}}\left[i\%2==0\right]{{</latex>}} 改写成 {{<latex>}}\frac{1^i + {(-1)}^i}{2}{{</latex>}}

{{<latex>}}
\begin{equation}
\begin{aligned}
ans &= \sum\limits_{i=0}^{n}(\sqrt{a})^ib^{n-i}{n \choose i}\left[i\%2==0\right] \\
    &= \sum\limits_{i=0}^{n}(\sqrt{a})^ib^{n-i}{n \choose i}\frac{1^i + {(-1)}^i}{2} \\
    &= \frac{1}{2}\left(\sum\limits_{i=0}^{n}(\sqrt{a})^ib^{n-i}{n \choose i} + \sum\limits_{i=0}^{n}(-\sqrt{a})^ib^{n-i}{n \choose i}\right)
\end{aligned}
\end{equation}
{{</latex>}}

根据二项式定理 {{<latex>}}{(a+b)}^n=\sum\limits^n_{k=0}{n \choose k}x^{n-k}y^k=\sum\limits^n_{k=0}{n \choose k}x^ky^{n-k}{{</latex>}}

{{<latex>}}
\begin{equation}
\begin{aligned}
ans &= \frac{1}{2}\left(\left(b+\sqrt{a}\right)^n+\left(b-\sqrt{a}\right)^n\right)
\end{aligned}
\end{equation}
{{</latex>}}

因为 a,b 固定，可以设 Fx 为当 n=x 时的答案，那么 Fx 一定是满足一个二项递推式，即 {{<latex>}}Fx=A\times F_{x-1}+B\times F_{x-2}{{</latex>}}。因为 a 不一定有二次剩余，所以不能用快速幂来做，p也不一定是质数，所以不能用 BM 算法求出 A,B（有一个求逆元的步骤）。不过可以通过手算 {{<latex>}}F_0,F_1,F_2,F_3{{</latex>}} 然后解一个二元一次方程组来算出 A,B。

{{<latex>}}
\begin{aligned}
F_0 &= 1 \\
F_1 &= b \\
F_2 &= b^2+a \\
F_3 &= b^3+3ab
\end{aligned}
{{</latex>}}

**或者**

考虑特征根方程：{{<latex>}}x^2=Ax+B{{</latex>}}，即 {{<latex>}}x^2-Ax-B=0{{</latex>}}，方程的两个解理应为答案式子中的两个特征根 {{<latex>}}b+\sqrt{a},b-\sqrt{a}{{</latex>}}。根据韦达定理，可知 A 为两根之和 {{<latex>}}(b+\sqrt{a})+(b-\sqrt{a})=2b{{</latex>}}，-B 为两根之积，即 {{<latex>}}B=-(b+\sqrt{a})(b-\sqrt{a})=a-b^2{{</latex>}}。那么求出 {{<latex>}}F_0=1, F_1=b{{</latex>}} 之后用矩阵乘法就可以在 {{<latex>}}O(log(n)){{</latex>}}的时间内求出。

**矩阵为**

{{<latex>}}
 \left (
 \begin{matrix}
   1 & b
  \end{matrix}
  \right )
\left (
 \begin{matrix}
   0 & a-b^2 \\
   1 & 2b
  \end{matrix}
\right )
{{</latex>}}

**或**

{{<latex>}}
 \left (
 \begin{matrix}
   b & 1
  \end{matrix}
  \right )
\left (
 \begin{matrix}
   2b & 1 \\
   a-b^2 & 0
  \end{matrix}
\right )
{{</latex>}}

----

<details>
<summary>Code 1</summary>

```cpp
/* ***********************************************
Author        : Akvicor
Created Time  : Mon Oct 28 14:54:35 2019
File Name     : D.cpp
************************************************ */

#include <bits/stdc++.h>

#define FAST_IO ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
#define endl '\n'
#define ASB using namespace std; typedef long long ll; namespace AkvicorS {
#define ASE } int main() { return AkvicorS::sol(); }

ASB

int t;
ll n, a, b, p;

inline ll mul(ll x, ll y){ return (__int128)x*y%p; }

struct Mat{
	int r, c;
	ll num[2][2];
	Mat(int _r=2, int _c=2){r=_r;c=_c;}
	Mat operator * (const Mat &obj) const{
		Mat res(r, obj.c);
		for(int i = 0; i < res.r; ++i){
			for(int j = 0; j < res.c; ++j){
				res.num[i][j] = 0;
				for(int k = 0; k < c; ++k){
					res.num[i][j] = (res.num[i][j] + mul(num[i][k], obj.num[k][j]))%p;
				}
			}
		}
		return res;
	}
}f, g;

int sol(){
  FAST_IO;
  
  cin >> t;
	while(t--){
		cin >> n >> a >> b >> p;
		a %= p; b %= p;
		f.r = 1;
		f.c = g.r = g.c = 2;
		
		f.num[0][0] = 1; f.num[0][1] = b;

		g.num[0][0] = 0; g.num[0][1] = (a+p-mul(b, b)) % p;
		g.num[1][0] = 1; g.num[1][1] = 2*b%p;
		for(; n; n >>= 1, g = g*g) if(n&1) f = f*g;
		cout << f.num[0][0] << endl;
	}
  
  return 0;
}

ASE
```
</details>

----


<details>
<summary>Code 2</summary>

```cpp
/* ***********************************************
Author        : Akvicor
Created Time  : Mon Oct 28 14:54:35 2019
File Name     : D.cpp
************************************************ */

#include <bits/stdc++.h>

#define FAST_IO ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
#define endl '\n'
#define ASB using namespace std; typedef long long ll; namespace AkvicorS {
#define ASE } int main() { return AkvicorS::sol(); }

ASB

int t;
ll n, a, b, p;
typedef pair<ll, ll> pll;

#define fi first
#define se second

ll mul(ll x, ll y){ return (__int128)x*y%p; }

pll operator * (pll a, pll b){
	return make_pair( ( mul(a.fi, b.fi) + mul( mul(a.se, b.se), ::AkvicorS::a) ) % p, ( mul(a.se, b.fi) + mul(a.fi, b.se) ) % p);
}

pll qpow(pll x, ll y){
	pll ans = make_pair(1, 0);
	for(; y; y>>=1, x=x*x) if(y&1) ans = ans * x;
	return ans;
}

int sol(){
  FAST_IO;
  
  cin >> t;
	while(t--){
		cin >> n >> a >> b >> p;
		cout << qpow(make_pair(b, 1), n).fi << endl;
	}
  
  return 0;
}

ASE

```
</details>

----


