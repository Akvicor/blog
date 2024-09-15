---
title: "素数判定"
date: 2019-05-28T23:07:55Z
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

所谓素数，是指恰好有两个约数的正整数。

- 埃氏筛法
- 区间筛法
- Miller-Rabin素性测试

<!--more-->

判定单一的一个数是不是素数，素性测试：

<details>
<summary>Code</summary>

```cpp
bool is_prime(int n){  /* 判定一个数是不是素数 ，假设输入的数都是正整数 */
    for (int i = 2; i * i <= n; i++) {
        if (!(n % i)) return false;
    }
    return n != 1;  /* 1是例外 */
}
```

</details>

如果只对一个数进行素数测试，通常{{<latex>}}O(\sqrt{n}){{</latex>}}的算法就足够了。但........

# Miller-Rabin素性测试

引理1(费马定理) 设p是素数，a为整数，且(a,p)=1，则{{<latex>}}a^{p-1} \equiv 1(\text{mod}p){{</latex>}}。

引理2(二次探测定理) 如果p是一个素数，且{{<latex>}}0<x<p{{</latex>}}，则方程{{<latex>}}x^2 \equiv 1(\text{mod}p){{</latex>}}的解为{{<latex>}}x=1,p-1{{</latex>}}

证明：易知{{<latex>}}x^2-1 \equiv 0(\text{mod}p){{</latex>}}

{{<latex>}}(x+1)(x-1)\equiv 0(\text{mod}p){{</latex>}}

{{<latex>}}p|(x-1)(x+1){{</latex>}}

因为p是质数 {{<latex>}}x=1或x=p-1{{</latex>}}

虽然Wilson定理（对于给定的正整数n，n是素数的充要条件为{{<latex>}}(n-1)!\equiv -1(\text{mod}n){{</latex>}}）给出了一个数是素数的充要条件，但是根据它来素性测试所需的计算量太大，无法实现对比较大整数的测试。目前，尽管高效的确定性素性算法尚未找到，但已有一些随机算法可用于素性测试及大整数的因式分解。

首先我们要知道费马定理只是n是素数的必要条件。即费马定理不成立，n一定是合数；费马定理成立，n可能是素数。

假设n是奇素数，则n-1必为偶数。令{{<latex>}}n-1=2^q\cdot m{{</latex>}}。

随机选取整数{{<latex>}}a(0<a<n){{</latex>}}，由费马定理，{{<latex>}}(a^{2^q\cdot m} = a^{n-1}) \equiv 1(\text{mod}n){{</latex>}}

由二次探测定理可知：{{<latex>}}a^{2^{q-1}\cdot m} \equiv 1(\text{mod}n){{</latex>}}或{{<latex>}}a^{2^{q-1}\cdot m}  \equiv n-1(\text{mod}n){{</latex>}}

若{{<latex>}}a^{2^{q-1}\cdot m}  \equiv 1(\text{mod}n){{</latex>}}成立，则再次由二次探测定理可知：{{<latex>}}a^{2^{q-2}\cdot m}  \equiv 1(\text{mod}p){{</latex>}} 或 {{<latex>}}a^{2^{q-2}\cdot m}  \equiv n-1(\text{mod}n){{</latex>}}，...。如此反复应用二次探测定理，直到某一步{{<latex>}}a^m\equiv 1(\text{mod}n){{</latex>}} 或 {{<latex>}}a^m\equiv n-1(\text{mod}n){{</latex>}}。总之，若n是素数，则 {{<latex>}}a^m\equiv 1(\text{mod}n){{</latex>}}，或存在{{<latex>}}0\leq r \leq q-1{{</latex>}}，使{{<latex>}}a^{2^r\cdot m}\equiv n-1(\text{mod}n){{</latex>}}

所以该算法的过程为

- 给定奇数n，为了判断是否为素数，首先测试 {{<latex>}}a^{2^q\cdot m}  \equiv 1(\text{mod}p){{</latex>}} 是否成立。若不成立，则n一定为合数；若成立继续进行下一步测试
- 考察下面的Miller序列： {{<latex>}}a^m(\text{mod}n), a^{2m}(\text{mod}n), a^{4m}(\text{mod}n),...,a^{2^{q-1}\cdot m}(\text{mod}n){{</latex>}}

若{{<latex>}}a^m\equiv1(\text{mod}n){{</latex>}}，或者存在某个整数{{<latex>}}0\leq r \leq q-1{{</latex>}}，使{{<latex>}}a^{2^r\cdot m}  \equiv n-1(\text{mod}p){{</latex>}}成立，则称n通过Miller测试。

可以证明 Miller-Rabin 算法给出错误结果的概率小于等于{{<latex>}}\frac{1}{4}{{</latex>}}，若反复测试k次，则错误概率可降至{{<latex>}}(\frac{1}{4})^k{{</latex>}}。这是一个很保守的估计，实际使用的效果要好得多。

<details>
<summary>Code</summary>

```cpp
const int S = 8;

LL mult_mod(LL a, LL b, LL c){
	a %= c;
	b %= c;
	LL ret = 0;
	LL tmp = a;
	while(b){
		if(b&1){
			ret += tmp;
			if(ret > c) ret -= c;
		}
		tmp <<= 1;
		if(tmp > c) tmp -= c;
		b >>= 1;
	}
	return ret;
}

LL pow_mod(LL a, LL n, LL mod){
	LL ret = 1;
	LL temp = a % mod;
	while(n){
		if(n & 1) ret = mult_mod(ret, temp, mod);
		temp = mult_mod(temp, temp, mod);
		n >>= 1;
	}
	return ret;
}

bool check(LL a, LL n, LL x, LL t){
	LL ret = pow_mod(a, x, n);
	LL last = ret;
	loop(i, 1, t){
		ret = mult_mod(ret, ret, n);
		if(ret == 1 && last != 1 && last != n-1) return true;
		last = ret;
	}
	if(ret != 1) return true;
	else return false;
}

bool Miller_Rabin(LL n){
	if(n < 2) return false;
	if(n == 2) return true;
	if((n&1)==0) return false;
	LL x = n-1;
	LL t = 0;
	while((x&1)==0){x >>= 1; ++t;}
	srand(time(NULL));
	rep(i, S){
		LL a = rand()%(n-1)+1;
		if(check(a, n, x, t)) return false;
	}
	return true;
}
```

</details>

# 埃氏筛法

要枚举n以内的素数，可以用埃氏筛法：这是一种古老的算法，首先，将2到n范围内的所有整数写下来。其中最小的数字2是素数。将表中所有2的倍数都划去，然后表中剩余最小的素数是3，将表中所有3的倍数也都划去。以此类推，如果表中剩余最小的素数是m的话，就将表中所有m的倍数都划去，像这样反复操作，就能依次枚举n以内的素数，埃氏筛法的时间复杂度仅有{{<latex>}}O(nlogn){{</latex>}}，接近线性：

<details>
<summary>Code</summary>

```cpp
bool is_prime[100];
int prime[100];
int sieve(int n) { /* 枚举n以内的素数 */
    /*  返回n以内素数的个数，素数存在prime数组里  */
    int p = 0;
    for (int i = 0; i <= n; i++)
        is_prime[i] = true;
    is_prime[0] = is_prime[1] = false;
    for (int i = 2; i <= n; i++) {
        if (is_prime[i]) {
            prime[p++] = i;
            for (int j = i * 2; j <= n; j += i)
                is_prime[j] = false;
        }
    }
    return p;
}
```

</details>

# 区间筛法

目的： 找出a~b这个区间有几个质数，哪些是质数 

做法： 在筛出`[0,sqrt(b)`之间的素数的同时，将在`[a,b)`这个区间的之中的该素数的倍数都划掉。

<details>
<summary>Code</summary>

```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>

using namespace std;
#define M 100000000
typedef long long ll;
bool is_prime_small[M];
bool is_prime[M];
ll ans = 0;

void prime(ll a, ll b) {
    for (ll i = 2; i * i < b; i++) is_prime_small[i] = true; //初始化2~b^(1/2)
    for (ll i = 0; i < b - a; i++) is_prime[i] = true; //因为a,b很大，所以用0~b-a 代表a~b
    for (ll i = 2; i * i < b; i++) {
        if (is_prime_small[i]) {
            for (ll j = 2 * i; j * j < b; j += i) is_prime_small[j] = false;
            for (ll j = max(2LL, (a + i - 1) / i) * i; j < b; j += i) { //找到从a开始的第一个合数
                if (is_prime[j - a]) {
                    ans++; //求出几个合数。  //或者也可以最后遍历b-a这区间，求出质数的个数。
                    is_prime[j - a] = false;
                }
            }
        }
    }
}

int main() {
    ll a, b;
    ans = 0;
    cin >> a >> b;
    prime(a, b);
    // for(ll i = 0;i < b-a;i++)
    //    if(is_prime[i]) ans++;
    cout << "ans = " << b - a - ans << endl;
    return 0;
}
```

</details>

# 例题

[hdu 2136](http://acm.hdu.edu.cn/showproblem.php?pid=2136)

<details>
<summary>Code</summary>

```cpp
#include <iostream>
#include <cstdio>
#include <algorithm>

#define M 1000001

using namespace std;

int num[M];
int is_prime[M];
int ans[M];

void slove() {
    for (int i = 0; i < M; i++) is_prime[i] = true;
    int k = 1;
    num[1] = 0;
    is_prime[1] = true;
    for (int i = 2; i < M; i++) {
        if (is_prime[i]) {
            num[i] = k++;
            ans[i] = num[i];
            for (int j = 2 * i; j < M; j += i) {
                is_prime[j] = false;
                ans[j] = num[i];
            }
        }
    }
}

int main() {
    slove();
    int a;
    while (scanf("%d", &a) == 1) {
        printf("%d\n", ans[a]);
    }
    return 0;
}
```

</details>


