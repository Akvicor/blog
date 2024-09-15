---
title: "初等数论四大定理"
date: 2019-08-02 19:54:53 +08:00
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

- 威尔逊定理
- 欧拉定理（数论中的欧拉定理）
- 中国剩余定理（又称孙子定理）
- 费马小定理

<!--more-->

<details>
<summary><strong>相关定义</strong></summary>

<details>
<summary>积性函数</summary>

积性函数：对于任意互质的整数{{<latex>}}a{{</latex>}}和{{<latex>}}b{{</latex>}}有性质{{<latex>}}f(ab)=f(a)f(b){{</latex>}}的数论函数

完全积性函数：对于任意整数{{<latex>}}a{{</latex>}}和{{<latex>}}b{{</latex>}}有性质{{<latex>}}f(ab)=f(a)f(b){{</latex>}}的数论函数

{{<latex>}}\phi(n) \quad - \quad \text{欧拉函数}{{</latex>}}

{{<latex>}}\mu(n) \quad - \quad \text{莫比乌斯函数，关于非平方数的质因子数目}{{</latex>}}

{{<latex>}}\gcd(n,k) \quad - \quad \text{最大公因子，当k固定的情况}{{</latex>}}

{{<latex>}}d(n) \quad - \quad \text{n的正因子数目}{{</latex>}}

{{<latex>}}\sigma(n) \quad - \quad \text{n的所有正因子之和}{{</latex>}}

{{<latex>}}\sigma k(n) \quad - \quad \text{因子函数，n的所有正因子的k次幂之和，当中k可为任何复数。}{{</latex>}}

{{<latex>}}1(n) \quad - \quad \text{不变的函数，定义为1(n)=1 （完全积性）}{{</latex>}}

{{<latex>}}Id(n) \quad - \quad \text{单位函数，定义为Id(n)=n （完全积性）}{{</latex>}}

{{<latex>}}Idk(n) \quad - \quad \text{幂函数，对于任何复数、实数k，定义为}Idk(n)=n^k \text{（完全积性）}{{</latex>}}

{{<latex>}}\epsilon(n) \quad - \quad \text{定义为：若}n=1, \epsilon(n)=1 \text{；若}n>1, \epsilon(n)=0 \text{分别称为“对于狄利克雷卷积的乘法单位” （完全积性）}{{</latex>}}

{{<latex>}}\lambda(n) \quad - \quad \text{刘维尔函数，关于能整数n的质因子的数目}{{</latex>}}

{{<latex>}}\gamma(n) \quad - \quad \text{定义为：} \gamma(n)=(-1)^{\omega(n)} \text{，在次加性函数}\omega(n)\text{是不同能整除n的质数的数目}{{</latex>}}

{{<latex>}}\text{另外，所有狄利克雷特征均是完全积性的}{{</latex>}}

**积性函数的性质**

**性质一**

这和算数基本定理有关

若将n表示成质因子分解式

{{<latex>}}n=p_{1}^{a_1}p_{2}^{a_2}...p_{k}^{a_k}{{</latex>}}

则有

{{<latex>}}f(n)=f(p_{1}^{a_1})f(p_{2}^{a_2})...f(p_{k}^{a_k}){{</latex>}}

**性质二**

若f为积性函数且有{{<latex>}}f(p^n)=f^n(p){{</latex>}}

则f为完全积性函数

**非积性函数**

冯·曼戈尔特函数：当n是质数p的整数幂，{{<latex>}}\Lambda(n)=\ln(p){{</latex>}}，否则{{<latex>}}\Lambda(n)=0{{</latex>}}

不大于正整数n的质数的数目{{<latex>}}\Pi(n){{</latex>}}

整数拆分的数目{{<latex>}}P(n){{</latex>}}：一个整数能表示成正整数之和的方法的数目

</details>

<details>
<summary>取余的性质</summary>

对于基本的四种运算而言，加减乘都符合“分配率”，唯独除法不满足。

1. {{<latex>}}(a-b)\%c = (a\%c-b\%c)\%c{{</latex>}}
2. {{<latex>}}(a+b)\%c = (a\%c+b\%c)\%c{{</latex>}}
3. {{<latex>}}(a\times b)\%c = (a\%c\times b\%c)\%c{{</latex>}}

如果要实现除法，那么必须把除法转换为乘法，假设{{<latex>}}b^{-1}{{</latex>}}是{{<latex>}}b{{</latex>}}关于{{<latex>}}c{{</latex>}}的逆元。

{{<latex>}}(a\div b)\%c 可以转化为 (a\times b^{-1})\%c = (a\%c\times b^{-1}\%c)\%c{{</latex>}}

</details>

<details>
<summary>完全剩余系</summary>

**完全剩余系：** 从模n的每个剩余类中各取一个数，得到一个由n个数组成的集合，叫做模n的一个完全剩余系。完全剩余系常用于数论中存在性证明。

**在模n的剩余类中各取一个元素，则这n个数就构成了模n的一个完全剩余系。**

命n为一个自然数，a，b为整数。如果a-b为n的整数倍，则称a，b关于n同余，用同余式{{<latex>}}a\equiv b\left(\text{mod}n\right){{</latex>}}记之。否则称a，b关于n不同余，记为{{<latex>}}a\not\equiv b\left(\text{mod}n\right){{</latex>}}。我们称n为同余式的模。同余式满足：

- 反射性（reflection），即{{<latex>}}a\equiv a \left(\text{mod}n\right){{</latex>}}
- 对称性（symmetry），即由{{<latex>}}a\equiv b\left(\text{mod}n\right){{</latex>}}可得{{<latex>}}b\equiv a\left(\text{mod}n\right){{</latex>}}
- 传递性（transitivity），即由 {{<latex>}}a\equiv b\left(\text{mod}n\right){{</latex>}},{{<latex>}}b\equiv c\left(\text{mod}n\right){{</latex>}} 可得 {{<latex>}}a\equiv c\left(\text{mod}n\right){{</latex>}}

因此，可以利用同余关系将整数分类，凡同余的数属于一个类，于是异类中的数皆不同余。共得到整数的n个类。在每一个类中各取一个数作为代表所成的集合称为模n的一个完全剩余系。

**举例**

取最小非负剩余为代表，则得完全剩余系{0,1,2,...,n-1}。剩余类的代表相加得一数属于另一类，这个类仅与相加两数所在的类有关，而与代表的选取无关。于是，可以定义剩余类间的加法，以0所在的类O为单位元，则剩余类的全体关于加法构成一个交换群。当然在剩余类之间可以定义乘法。但关于除法就不一定可能，例如：{{<latex>}}3\cdot 2\equiv 1\cdot 2\left(\text{mod}4\right){{</latex>}}，{{<latex>}}2\equiv 2\left(\text{mod}4\right){{</latex>}}，但是{{<latex>}}3\not\equiv 1 \left(\text{mod}4\right){{</latex>}}

一个数除以4的余数只能是0，1，2，3，{0，1，2，3}和{4，5，-2，11}是模4的完全剩余系。可以看出0和4，1和5，2和-2，3和11模4同余，这4组数分别属于四个剩余类。

**性质**

(m,n)=1 -> m、n两个数的最大公约数是1

1. 对于n个整数，其构成模n的完全系等价于其关于模n两两不同余
2. 若{{<latex>}}a_i \left(1\leq i \leq n\right){{</latex>}}构成模n的完系，{{<latex>}}k、m \in Z，\left(m,n\right)=1{{</latex>}}，则{{<latex>}}k+ma_i\left(1\leq i \leq n\right){{</latex>}}也构成模n的完系
3. 若{{<latex>}}a_i \left(1\leq i \leq n\right){{</latex>}}构成模n的完系，则{{<latex>}}\sum_{i=1}^{n}a_i = \frac{n\left(n+1\right)}{2}=\lbrace ^{\frac{n}{2}\left(\text{mod}n\right)}_{0\left(\text{mod}n\right)}{{</latex>}}

</details>

<details>
<summary>剩余类</summary>

**剩余类**，亦称同余类，是一种数学的用语，为数论的基本概念之一。

设模为n，则根据余数可将所有的整数分为n类，把所有与整数a模n同余的整数构成的集合叫做模n的一个剩余类，记做\[a\]。并把a叫做剩余类\[a\]的一个代表元。

**定义**

一个整数被正整数n除后，余数有n中情形：0，1，2，3，。。。，n-1，他们彼此对模n不同余。这表明，每个整数恰与这n个整数中的某一个对模n同余。这样一来，按模n是否同余对整数集进行分类，可以将整数集分成n个两两不相交的子集。我们把（所有）对模n同余的整数构成的一个集合叫做模n的一个剩余类。

**模m的剩余类具有以下性质**

1. 每一个整数恰包含在某一个类{{<latex>}}C_j{{</latex>}}里({{<latex>}}0 \leq j \leq m-1{{</latex>}})
2. 两个整数x，y属于同一类的充分必要条件是{{<latex>}}x\equiv y\left(\text{mod} m\right){{</latex>}}

**剩余类与完全剩余类**

由此可引出抽象代数中的重要概念，如群论中的陪集，环论中的剩余类等。任取n，这n个数(o, 1, ..., n-1)称为模n的一个完全剩余类。每个数称为相应类的代表元。最常用的完全剩余系是{0, 1, ..., n-1}。

1. 若{{<latex>}}n \in Z^{+}{{</latex>}}，则n个整数，{{<latex>}}r_0, r_1, ..., r_{n-1}{{</latex>}}构成一个完全剩余系数的充分必要条件是这n个除n的余数两两不相等。
2. 若{{<latex>}}n \in Z^{+}, k \in Z, \left(k, n\right)=1{{</latex>}}，当{{<latex>}}x_1, x_2,..., x_n{{</latex>}}为完全剩余系时，{{<latex>}}kx_1+m, kx_2+m,..., kx_n+m{{</latex>}}也为完全剩余系数。
3. 若{{<latex>}}m,n \in Z^{+}, \left(m, n\right)=1{{</latex>}}，则当{{<latex>}}a_0,a_1,...,a_{m-1},b_0,b_1,...,b_{n-1}{{</latex>}}是完全剩余系数时，{{<latex>}}na_i+mb_j\left(i=0, 1,...,m-1;j=0,1,...,n-1\right){{</latex>}}也构成{{<latex>}}m\times n{{</latex>}}完全剩余系

**剩余类与简化剩余类**

在剩余类选取一个与n互素代表元构成简化剩余类

1. {{<latex>}}n \in Z^{+}, k \in Z, \left(k, n\right)=1{{</latex>}}，当{{<latex>}}x_1, x_2,..., x_n{{</latex>}}为简化剩余系时，{{<latex>}}kx_1+m, kx_2+m,..., kx_n+m{{</latex>}}也为简化剩余系数。
2. 若{{<latex>}}m,n \in Z^{+}, \left(m, n\right)=1{{</latex>}}，则当{{<latex>}}a_0,a_1,...,a_t,b_0,b_1,...,b_s{{</latex>}}是简化剩余系数时，{{<latex>}}na_i+mb_j\left(i=0...t;j=0...s\right){{</latex>}}也构成{{<latex>}}m\times n{{</latex>}}完全剩余系

</details>

<details>
<summary>缩系</summary>

简化剩余系也称既约剩余系、缩系，是数学术语。

**缩系的定义：**如果一个模m的剩余类里面的数与m互素（显然，只需有一个与m互素，其余的均与m互素）就把它叫做一个与模m互素的剩余类，在与模m互素的全部剩余类中，各取一数所组成的集叫做模m的一组简化剩余系

若整数{{<latex>}}A_1, A_2,..., A_m{{</latex>}}模n分别对应{{<latex>}}0, 1, 2,..., n-1{{</latex>}}中所有m个与n互素的自然数，则称集合{{{<latex>}}A_1, A_2,..., A_m{{</latex>}}}为模n的一个缩系

**缩系的性质：**

1. 对于任意的与n互质的正整数k，若{{{<latex>}}A_1, A_2,..., A_m{{</latex>}}}为模n的一个缩系，(k,n)=1，则{{{<latex>}}k \times A_1, k \times A_2,..., k \times A_m{{</latex>}}}也为模n的一个缩系
2. {{<latex>}}\left(k \times A_1\right)...\left(k \times A_1\right) \equiv A_1 \times A_2...A_m\left(\text{mod}n\right){{</latex>}}

</details>

</details>

## 威尔逊定理

## 欧拉定理

## 中国剩余定理

## 费马小定理

费马小定理(Fermat's little theorem)是数论中的一个重要定理，在1636年由 皮埃尔·德·费马 提出。如果p是一个质数，而{{<latex>}}\gcd(a,p)=1{{</latex>}}(整数a不是p的倍数)，则有{{<latex>}}a^{p-1}\equiv 1\text{mod}p{{</latex>}}

根据费马小定理{{<latex>}}a^{p-1}\equiv 1\text{mod}p{{</latex>}}，可以发现{{<latex>}}a^{p-2} \times a\equiv 1\text{mod}p{{</latex>}}也成立。可知{{<latex>}}a^{p-2}{{</latex>}}是{{<latex>}}a{{</latex>}}关于{{<latex>}}p{{</latex>}}的逆元。所以求{{<latex>}}a{{</latex>}}的逆元，就直接用快速幂求{{<latex>}}a^{p-2}{{</latex>}}就可以

<details>
<summary>Code</summary>

```c++
LL power(LL a, int x) {
	LL ans = 1;
	while(x) {
		if(x&1) ans = (ans * a) %mod;
		a = (a * a) %mod;
		x >>= 1;
	}
	return ans;
}

LL inv(LL a) {
	return power(a, mod - 2);
}
```
</details>

实际上，它是欧拉定理的一个特殊情况（即{{<latex>}}a^{\phi(p)}=q^{p-1}\equiv 1(\text{mod}p){{</latex>}}）

有些学家独立制作相关的假说（有时也被错误的称为中国的假说），当成立时，p是素数。这是费马小定理的一个特殊情况。然而，这一假说的前设是错误的：例如，341，而341=11*31是一个伪素数。所有的伪素数都是此假说的反例。

如上所述，中国猜测仅有一半是正确的。符合中国猜测但不是素数的数被称为伪素数。

更极端的反例是卡迈克尔数：假设a与561互质，则{{<latex>}}a^{560}{{</latex>}}被561除都余1.这样的数被称为卡迈克尔数，561是最小的卡迈克尔数。



### 验证推倒

**引理1**

若a,b,c为任意三个整数，m为正整数，且(m,c)=1，则当{{<latex>}}a\cdot c\equiv b\cdot c(\text{mod}m){{</latex>}}时有{{<latex>}}a\equiv b(\text{mod}m){{</latex>}}

证明：{{<latex>}}a\cdot c\equiv b\cdot c(\text{mod}m){{</latex>}}可得{{<latex>}}a\cdot c - b\cdot c\equiv 0(\text{mod}m){{</latex>}}可得{{<latex>}}(a - b)\cdot c\equiv 0(\text{mod}m){{</latex>}}。因为(m,c)=1即m,c互质，c可以约去，{{<latex>}}a - b \equiv 0(\text{mod}m){{</latex>}}可得{{<latex>}}a \equiv b(\text{mod}m){{</latex>}}

**引理2**

设m是一个整数且m>1，b是一个整数且(m,b)=1。如果{{<latex>}}a[1],a[2],a[3],a[4],...,a[m]{{</latex>}}是模m的一个完全剩余系，则{{<latex>}}b\cdot a[1],b\cdot a[2],b\cdot a[3],b\cdot a[4],...,b\cdot a[m]{{</latex>}}也构成模m的一个完全剩余系。

证明：若存在2个整数{{<latex>}}b\cdot a[i]和b\cdot [j]{{</latex>}}同余即{{<latex>}}b\cdot a[i] \equiv b\cdot a[j](\text{mod}m)\quad (i\geq 1 \&\& j\geq 1){{</latex>}}，根据引理1则有{{<latex>}}a[i]\equiv a[j](\text{mod}m){{</latex>}}。根据完全剩余系的定义可知这是不可能的，因此不存在两个整数{{<latex>}}b\cdot a[i]和b\cdot [j]{{</latex>}}同余。

所以{{<latex>}}b\cdot a[1],b\cdot a[2],b\cdot a[3],b\cdot a[4],...,b\cdot a[m]{{</latex>}}构成模m的一个完全剩余系。

构造素数{{<latex>}}P{{</latex>}}的完全剩余系 {{<latex>}}P= \lbrace 1,2,3,...,p-1 \rbrace{{</latex>}}

因为{{<latex>}}(a,p)=1{{</latex>}}，由引理2可得 {{<latex>}}A = \lbrace a,2a,3a,...,(p-1)a \rbrace{{</latex>}} 也是{{<latex>}}P{{</latex>}}的一个完全剩余系。由完全剩余系的性质，{{<latex>}}1 \times 2 \times 3 \times ... \times(p-1) \equiv a,2a,3a,...,(p-1)a (\text{mod}p){{</latex>}}

即{{<latex>}}(p-1)! \equiv (p-1)! \cdot a^{p-1} (\text{mod}p){{</latex>}}

易知{{<latex>}}((p-1)!,p)=1{{</latex>}}，同余式两边可约去{{<latex>}}(p-1)!{{</latex>}}，得到{{<latex>}}a^{p-1}\equiv 1(\text{mod}p){{</latex>}}

### 应用

计算{{<latex>}}2^{100}{{</latex>}}除以13的余数

{{<latex>}}2^{100} \equiv 2^{12\times 8 + 4}(\text{mod}13){{</latex>}}

{{<latex>}}\equiv (2^{12})^8\cdot 2^4(\text{mod}13){{</latex>}}

{{<latex>}}\equiv 1^8\cdot 16(\text{mod}13){{</latex>}}

{{<latex>}}\equiv 16(\text{mod}13){{</latex>}}

{{<latex>}}\equiv 3(\text{mod}13){{</latex>}}

故余数为3

证明对于任意整数a而言，{{<latex>}}a^{13}-a{{</latex>}}恒为2730的倍数。13-1为12，12的正因数有1，2，3，4，6，12，分别加1，为2，3，4，5，7，13，其中2，3，5，7，13为质数，根据定理 {{<latex>}}a^{13}-a{{</latex>}} 为2的倍数、为3的倍数、为5的倍数、为7的倍数、为13的倍数，即{{<latex>}}2\times 3\times 5\times 7\times 13=2730{{</latex>}}的倍数。

```python
>>> n =221
>>> a = 38
>>> pow(a ,n -1,n)
1

"""221 may be a prime number."""
import random
def isprime(n,k=128):
    if n<2:
        return False
    for _ in range(k):
        a = random.randrange(1,n)
        if pow(a,n-1,n)!=1:
        return False
    return True
```

