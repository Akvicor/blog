---
title: "欧拉函数证明"
date: 2019-08-13 09:25:22 +08:00
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

给定任意正整数{{<latex>}}n{{</latex>}}，那么在小于等于{{<latex>}}n{{</latex>}}的所有正整数之中，有多少个与{{<latex>}}n{{</latex>}}构成互质关系？

计算这个值的方法就叫做欧拉函数{{<latex>}}\phi(n){{</latex>}}表示：在{{<latex>}}1{{</latex>}}到{{<latex>}}n{{</latex>}}之中，与n构成互质关系的数的数量。

<!--more-->

## 分析

### 情况一

如果{{<latex>}}n=1{{</latex>}}，则{{<latex>}}\phi(1)=1{{</latex>}}。因为1与任何数（包括自身）都构成互质关系

### 情况二

如果{{<latex>}}n{{</latex>}}是质数，则{{<latex>}}\phi(n)=n-1{{</latex>}}。因为质数与小于它的每一个数，都构成互质关系。比如{{<latex>}}5{{</latex>}}与{{<latex>}}1、2、3、4{{</latex>}}都构成互质关系。

### 情况三

如果{{<latex>}}n{{</latex>}}是质数的某一个次方，即{{<latex>}}n=p^k{{</latex>}}（{{<latex>}}p{{</latex>}}为质数，{{<latex>}}k{{</latex>}}为大于等于{{<latex>}}1{{</latex>}}的整数），则{{<latex>}}\phi(p^k)=p^k-p^{k-1}{{</latex>}}

比如{{<latex>}}\phi(8)=\phi(2^3)=2^3-2^2=8-4=4{{</latex>}}

这是因为只有当一个数不包含质数{{<latex>}}p{{</latex>}}，才有可能与{{<latex>}}n{{</latex>}}互质。而包含质数{{<latex>}}p{{</latex>}}的数一共有{{<latex>}}p^{k-1}{{</latex>}}个。

即{{<latex>}}1\times p、2\times p、3\times p、... 、p^{k-1}\times p{{</latex>}}，把他们去除，剩下的就是与{{<latex>}}n{{</latex>}}互质的数。

上面的式子还可以写成下面的形式：

{{<latex>}}\phi(p^k)=p^k-p^{k-1}=p^k(1-\frac{1}{p}){{</latex>}}

可以看出，上面的第二种情况是{{<latex>}}k=1{{</latex>}}时的特例

### 情况四

如果n可以分解成两个互质的整数之积：{{<latex>}}n=p_1\times p_2{{</latex>}}

则：{{<latex>}}\phi(n)=\phi(p_1p_2)=\phi(p_1)\phi(p_2){{</latex>}}

即积的欧拉函数等于各个因子的欧拉函数之积。比如{{<latex>}}\phi(56)=\phi(8\times 7)=\phi(8)\phi(7)=4\times 6=24{{</latex>}}

对于素数{{<latex>}}p{{</latex>}}，{{<latex>}}\phi(p)=p-1{{</latex>}}，对于两个素数{{<latex>}}p,q{{</latex>}}，{{<latex>}}\phi(pq)=pq-1{{</latex>}}

欧拉函数是积性函数，但不是完全积性函数

#### 欧拉函数的积性

若{{<latex>}}m,n{{</latex>}}互质，则{{<latex>}}\phi(mn)=\phi(m)\phi(n){{</latex>}}。

由“{{<latex>}}m,n{{</latex>}}互质”可知{{<latex>}}m,n{{</latex>}}无公因数，所以：

{{<latex>}}\phi(m)\phi(n)=m(1-\frac{1}{p_1})(1-\frac{1}{p_2})(1-\frac{1}{p_3})...(1-\frac{1}{p_n})\cdot n(1-\frac{1}{p_1'})(1-\frac{1}{p_2'})(1-\frac{1}{p_3'})...(1-\frac{1}{p_n'}){{</latex>}}

其中{{<latex>}}p_1、p_2、p_3...p_n{{</latex>}}为{{<latex>}}m{{</latex>}}的质因数，{{<latex>}}p_1'、p_2'、p_3'...p_n'{{</latex>}}为{{<latex>}}n{{</latex>}}的质因数，而{{<latex>}}m,n{{</latex>}}无公因数。

所以{{<latex>}}p_1、p_2、p_3...p_n、p_1'、p_2'、p_3'...p_n{{</latex>}}互不相同，所以{{<latex>}}p_1、p_2、p_3...p_n、p_1'、p_2'、p_3'...p_n{{</latex>}}均为{{<latex>}}mn{{</latex>}}的质因数且为{{<latex>}}mn{{</latex>}}的质因数的全集。

所以{{<latex>}}\phi(mn)=mn(1-\frac{1}{p_1})(1-\frac{1}{p_2})(1-\frac{1}{p_3})...(1-\frac{1}{p_n})\cdot (1-\frac{1}{p_1'})(1-\frac{1}{p_2'})(1-\frac{1}{p_3'})...(1-\frac{1}{p_n'}){{</latex>}}

所以{{<latex>}}\phi(mn)=\phi(m)\phi(n){{</latex>}}。

以上部分涉及到“中国剩余定理”

### 情况五

因为任意一个大于1的正整数，都可以写成一系列质数的积。（分解质因数）

{{<latex>}}n=p_1^{k_1}p_2^{k_2}...p_r^{k_r}{{</latex>}}

根据结论四，得到

{{<latex>}}\phi(n)=\phi(p_1^{k_1})\phi(p_2^{k_2})...\phi(p_r^{k_r}){{</latex>}}

再根据结论三，得到

{{<latex>}}\phi(n)=p_1^{k_1}p_2^{k_2}...p_r^{k_r}(1-\frac{1}{p_1})(1-\frac{1}{p_2})...(1-\frac{1}{p_r}){{</latex>}}

也就等于

{{<latex>}}\phi(n)=n(1-\frac{1}{p_1})(1-\frac{1}{p_2})...(1-\frac{1}{p_r}){{</latex>}}

### 结论

比如{{<latex>}}\phi(1323)=(3^3\times 7^2)=1323(1-\frac{1}{3})(1-\frac{1}{7})=756{{</latex>}}

即{{<latex>}}\phi(mn)=\phi(n)\phi(m){{</latex>}}只在{{<latex>}}(m,n)=1{{</latex>}}时成立。

对于一个正整数{{<latex>}}N{{</latex>}}的素数幂分解{{<latex>}}N=p_1^{q_1}p_2^{q_2}...p_n^{q_n}{{</latex>}}

{{<latex>}}\phi(N)=N(1-\frac{1}{p_1})(1-\frac{1}{p_2})...(1-\frac{1}{p_n}){{</latex>}}

除了{{<latex>}}N=2, \phi(N){{</latex>}}都是偶数

设{{<latex>}}N{{</latex>}}为正整数，{{<latex>}}\sum{\phi(d)}=N(d|N){{</latex>}}


## 欧拉函数

根据性质二，我们可以在{{<latex>}}O(\sqrt{n}){{</latex>}}的时间复杂度内求出一个数的欧拉函数值。

<details>
<summary>Code</summary>

```cpp
//直接求解欧拉函数  
int euler(int n) { //返回euler(n)   
    int res = n, a = n;
    for (int i = 2; i * i <= a; i++) {
        if (a % i == 0) {
            res = res / i * (i - 1);//先进行除法是为了防止中间数据的溢出   
            while (a % i == 0) a /= i;
        }
    }
    if (a > 1) res = res / a * (a - 1);
    return res;
}
```

</details>

## 欧拉函数线性筛

如果要求{{<latex>}}N{{</latex>}}以内的所有数的欧拉函数 {{<latex>}}O(n){{</latex>}}

<details>
<summary>Code</summary>

```cpp
const int MAXN = 1e6;
//欧拉线性筛：在线性时间内筛素数的同时求出所有数的欧拉函数
int tot;
int phi[MAXN]; //保存各个数字的欧拉函数
int prime[MAXN]; //按顺序保存素数
bool mark[MAXN]; //判断是否是素数
void get_phi(int N){
    phi[1] = 1;
    for(int i = 2; i <= MAXN; i++){ //相当于分解质因数的逆过程
        if(!mark[i]){
            prime[++tot] = i;
            phi[i] = i-1;
        }
        for(int j = 1; j <= tot; j++){
            if(i * prime[j] > N) break;
            mark[i * prime[j]] = 1; //确定i*prime[j]不是素数
            if(i % prime[j] == 0){ //判断prime[j] 是否为 i的约数
                phi[i * prime[j]] = phi[i] * prime[j];
                break;
            }
            else{ //prime[j] - 1 就是 phi[prime[j]],利用了欧拉函数的积性
                phi[i * prime[j]] = phi[i] * (prime[j] - 1);
            }
        }
    }
}
```

</details>



