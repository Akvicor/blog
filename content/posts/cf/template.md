---
title: "Template"
date: 2019-09-11T20:37:42Z
draft: false
categories: [CF]
tags: []
card: false
weight: 0
---

**Codeforces Template**

<!--more-->

## VIM

```
syntax on
set cindent
set nu
set tabstop=4
set shiftwidth=4
set background=dark
 
map <C-A> ggVG"+y
map <F5> :call Run()<CR>
func! Run()
	exec "w"
	exec "!g++ -Wall % -o %<"
	exec "!./%<"
endfunc
```

```bash
#!/bin/bash

for name in {A..M};
do
    cp a.cpp $name.cpp
done
```

```
filetype plugin indent on
colorscheme desert
syntax on
set nu
set backspace=2
set hlsearch 
set syntax=on
set tabstop=4
set shiftwidth=4
set smarttab
set smartindent
set showmatch
set matchtime=0
set report=0
:inoremap ( ()<ESC>i
:inoremap [ []<ESC>i
:inoremap { {}<ESC>i
:inoremap {<CR> {<CR>}<ESC>O
:inoremap ) <c-r>=Close(')')<CR>
:inoremap ] <c-r>=Close(']')<CR>
:inoremap } <c-r>=Close('}')<CR>
function Close(char)
    if getline('.')[col('.') - 1] == a:char
        return "\<Right>"
    else
        return a:char
    endif
endfunction

map <C-A> ggVG"+y
map <F5> :call Run()<CR>
func! Run()
    exec "w"
    exec "!g++-7 -O2 -std=c++11 -Wall % -o %<"
    exec "!./%<"
endfunc

map <F12> :call SetTitle()<CR>
func SetTitle()
let l = 0
let l = l + 1 | call setline(l,'/* ***********************************************')
let l = l + 1 | call setline(l,'Author        : Akvicor')
let l = l + 1 | call setline(l,'Created Time  : '.strftime('%c'))
let l = l + 1 | call setline(l,'File Name     : '.expand('%'))
let l = l + 1 | call setline(l,'************************************************ */')
let l = l + 1 | call setline(l,'')

let l = l + 1 | call setline(l,'#include <bits/stdc++.h>')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'#define FAST_IO ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'using namespace std;')
let l = l + 1 | call setline(l,'')
let l = l + 1 | call setline(l,'int main(){')
let l = l + 1 | call setline(l,'    FAST_IO;')
let l = l + 1 | call setline(l,'    ')
let l = l + 1 | call setline(l,'    return 0;')
let l = l + 1 | call setline(l,'}')
endfunc
```

## 快速幂

```cpp
LL quick_pow(LL a, LL b, LL p) {
	LL res = 1 % p;
	for (; b; b >>= 1) {
		if (b & 1) res = res * a % p;
		a = a * a % p;
	}
	return res;
}
```

## 快速乘

```cpp
// O(1)
ll mul(ll a, ll b, ll p) {
	return ((__int128)a*b)%p;
}
// O(1)
ll mul(ll a, ll b, ll p) {
	a %= p; b %= p;
	ll c = (long double)a * b / p;
	ll ans = a * b - c * p;
	return (ans % p + p) % p;
}
// O(log)
ll mul(ll a, ll b, ll p) {
	ll ans = 0;
	while (b) {
		if (b & 1) ans = (ans + a) % p;
		a = a * 2 % p;
		b >>= 1;
	}
	return ans;
}
```

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

## 欧拉函数线性筛

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

## Green公式-判断多边形边界曲线顺/逆时针

```cpp
double d = 0;
for (int i = 0; i < n - 1; i++) {
    d += -0.5 * ( y[i + 1] + y[i]) * (x[i + 1] - x[i]);
}
if ( d > 0)
    cout << "counter clockwise" << endl;
else
    cout << "clockwise" << endl;
```

## dijkstra

```cpp
const int INF = 0x3f3f3f3f;
#define maxn 1000+100
int path[maxn][maxn], dis[maxn]; // 地图  起点到达i的距离
bool vis[maxn]; // 是否访问过
int n, t; //  地标数  路径数

void init() {
    for (int i = 0; i < maxn; ++i) {
        for (int j = 0; j < maxn; ++j) {
            path[i][j] = INF;
        }
        path[i][i] = 0;
    }
}

void dijkstra(){
    // 将1到i的距离初始化
    for (int i = 1; i <= n; i++) {
        dis[i] = path[1][i];
        vis[i] = false;
    }
    vis[1] = true; // 标记起点为访问过的
    dis[1] = 0; // 起点到起点的距离为0
    for (int i = 2; i <= n; i++) { // 从2号节点遍历到n号节点
        int now = -1, minL = INF;
        // 遍历一遍dis数组，寻找到未访问过的到1最短的路径长度
        for (int j = 1; j <= n; j++) { // 从1到n
            if (!vis[j] && minL > dis[j]) { // 如果没有访问过当前节点，并且当前从1到j的距离小于最大长度
                now = j; // 更新最短路径的位置
                minL = dis[j]; // 最短距离等于当前
            }
        }
        if (now == -1) break; // 未找到
        vis[now] = true; // 将now标记为访问过
        // 从1开始寻找到j最短的路径
        for (int j = 1; j <= n; j++) {
            // 没有访问过 并且当前已走的长度加上走到j节点的长度小于当前dis[j]
            if (!vis[j] && (dis[now] + path[now][j]) < dis[j]) {
                dis[j] = dis[now] + path[now][j]; // 更新从起点到j的最短路径
            }
        }
    }
}

int main() {
    freopen("in.txt", "r", stdin); // 从指定文件输入数据 提交时注释掉
//    memset(path, INT_MAX, sizeof(path));
    while (scanf("%d %d", &n, &t) != EOF) {
        init();
        for (int i = 0; i < t; i++) {
            int from, to, len;
            scanf("%d %d %d", &from, &to, &len);
            if (path[from][to] > len)
                path[from][to] = path[to][from] = len;
        }
        dijkstra();
        for (int i = 1; i <= n; i++) {
            printf("1 to %d min= %d\n", i, dis[i]);
        }
    }
    return 0;
}
```

## 素性测试

```cpp
bool is_prime(int n){  /* 判定一个数是不是素数 ，假设输入的数都是正整数 */
    for (int i = 2; i * i <= n; i++) {
        if (!(n % i)) return false;
    }
    return n != 1;  /* 1是例外 */
}


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

## 埃氏筛法

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

## 区间筛法

```cpp
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

## 线段树

```cpp
class SegmentTree{

private:
    int * tree;
    int N;

public:
    int * temp;

    SegmentTree(unsigned int n){
        tree = new int [(n<<2u) +10u];
        temp = new int [(n<<2u) +10u];
        this->N = n;
    }

    ~SegmentTree(){
        delete[] tree;
        delete[] temp;
    }

    /**
     * 建树
     */
    void init(){
        build(1, 1, this->N);
    }

    /**
     * 建树
     * @param p p树
     * @param l 区间左端点
     * @param r 区间右端点
     */
    void build(int p, int l, int r){
        if(l == r){
            tree[p] = temp[l];
            return;
        }
        int mid = l + (r - l) / 2;
        build(p * 2, l, mid);
        build(p * 2 + 1, mid + 1, r);
        tree[p] = tree[p * 2] + tree[p * 2 + 1];
    }

    /**
     * 更新
     * @param p 当前节点编号
     * @param l 区间左界
     * @param r 区间右界
     * @param x 需要修改的节点编号
     * @param num 操作
     */
    void change(int p, int l, int r, int x, int num){
        if(l == r){
            tree[p] += num;
            if(tree[p]<0) tree[p] = 0;
            return;
        }
        int mid = l + (r - l) / 2;
        if(x <= mid) change(p * 2, l, mid, x, num);
        else change(p * 2 + 1, mid + 1, r, x, num);
        tree[p] = tree[p * 2] + tree[p * 2 + 1];
    }


    /**
     * 查询
     * @param p 在p这棵树
     * @param l p的左区间端点
     * @param r p的右区间端点
     * @param x 查询区间左端点
     * @param y 查询区间右端点
     * @return 区间和
     */
    int find(int p, int l, int r, int x, int y){
        if(x <= l && r <= y) return tree[p];
        int mid = l + (r - l) / 2;
        if(y <= mid) return find(p * 2, l, mid, x, y);
        if(x > mid) return find(p * 2 + 1, mid + 1, r, x, y);
        return find(p * 2, l, mid, x, mid) + find(p * 2 + 1, mid + 1, r, mid + 1, y);
    }

};
```


## 最大子列和问题

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

## 约数个数定理

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

## 大整数

```cpp
#include <bits/stdc++.h>
using namespace std;
// base and base_digits must be consistent
constexpr int base = 1000000000;
constexpr int base_digits = 9;

struct bigint {
    // value == 0 is represented by empty z
    vector<int> z; // digits

    // sign == 1 <==> value >= 0
    // sign == -1 <==> value < 0
    int sign;

    bigint() : sign(1) {}
    bigint(long long v) { *this = v; }

    bigint &operator=(long long v) {
        sign = v < 0 ? -1 : 1; v *= sign;
        z.clear(); for (; v > 0; v = v / base) z.push_back((int) (v % base));
        return *this;
    }

    bigint(const string &s) { read(s); }

    bigint &operator+=(const bigint &other) {
        if (sign == other.sign) {
            for (int i = 0, carry = 0; i < other.z.size() || carry; ++i) {
                if (i == z.size())
                    z.push_back(0);
                z[i] += carry + (i < other.z.size() ? other.z[i] : 0);
                carry = z[i] >= base;
                if (carry)
                    z[i] -= base;
            }
        } else if (other != 0 /* prevent infinite loop */) {
            *this -= -other;
        }
        return *this;
    }

    friend bigint operator+(bigint a, const bigint &b) { return a += b; }

    bigint &operator-=(const bigint &other) {
        if (sign == other.sign) {
            if (sign == 1 && *this >= other || sign == -1 && *this <= other) {
                for (int i = 0, carry = 0; i < other.z.size() || carry; ++i) {
                    z[i] -= carry + (i < other.z.size() ? other.z[i] : 0);
                    carry = z[i] < 0;
                    if (carry)
                        z[i] += base;
                }
                trim();
            } else {
                *this = other - *this;
                this->sign = -this->sign;
            }
        } else {
            *this += -other;
        }
        return *this;
    }

    friend bigint operator-(bigint a, const bigint &b) { return a -= b; }

    bigint &operator*=(int v) {
        if (v < 0) sign = -sign, v = -v;
        for (int i = 0, carry = 0; i < z.size() || carry; ++i) {
            if (i == z.size())
                z.push_back(0);
            long long cur = (long long) z[i] * v + carry;
            carry = (int) (cur / base);
            z[i] = (int) (cur % base);
        }
        trim();
        return *this;
    }

    bigint operator*(int v) const { return bigint(*this) *= v; }

    friend pair<bigint, bigint> divmod(const bigint &a1, const bigint &b1) {
        int norm = base / (b1.z.back() + 1);
        bigint a = a1.abs() * norm;
        bigint b = b1.abs() * norm;
        bigint q, r;
        q.z.resize(a.z.size());

        for (int i = (int) a.z.size() - 1; i >= 0; i--) {
            r *= base;
            r += a.z[i];
            int s1 = b.z.size() < r.z.size() ? r.z[b.z.size()] : 0;
            int s2 = b.z.size() - 1 < r.z.size() ? r.z[b.z.size() - 1] : 0;
            int d = (int) (((long long) s1 * base + s2) / b.z.back());
            r -= b * d;
            while (r < 0)
                r += b, --d;
            q.z[i] = d;
        }

        q.sign = a1.sign * b1.sign;
        r.sign = a1.sign;
        q.trim();
        r.trim();
        return {q, r / norm};
    }

    friend bigint sqrt(const bigint &a1) {
        bigint a = a1;
        while (a.z.empty() || a.z.size() % 2 == 1)
            a.z.push_back(0);

        int n = a.z.size();

        int firstDigit = (int) ::sqrt((double) a.z[n - 1] * base + a.z[n - 2]);
        int norm = base / (firstDigit + 1);
        a *= norm;
        a *= norm;
        while (a.z.empty() || a.z.size() % 2 == 1)
            a.z.push_back(0);

        bigint r = (long long) a.z[n - 1] * base + a.z[n - 2];
        firstDigit = (int) ::sqrt((double) a.z[n - 1] * base + a.z[n - 2]);
        int q = firstDigit;
        bigint res;

        for (int j = n / 2 - 1; j >= 0; j--) {
            for (;; --q) {
                bigint r1 = (r - (res * 2 * base + q) * q) * base * base +
                            (j > 0 ? (long long) a.z[2 * j - 1] * base + a.z[2 * j - 2] : 0);
                if (r1 >= 0) {
                    r = r1;
                    break;
                }
            }
            res *= base;
            res += q;

            if (j > 0) {
                int d1 = res.z.size() + 2 < r.z.size() ? r.z[res.z.size() + 2] : 0;
                int d2 = res.z.size() + 1 < r.z.size() ? r.z[res.z.size() + 1] : 0;
                int d3 = res.z.size() < r.z.size() ? r.z[res.z.size()] : 0;
                q = (int) (((long long) d1 * base * base + (long long) d2 * base + d3) / (firstDigit * 2));
            }
        }

        res.trim();
        return res / norm;
    }

    bigint operator/(const bigint &v) const { return divmod(*this, v).first; }

    bigint operator%(const bigint &v) const { return divmod(*this, v).second; }

    bigint &operator/=(int v) {
        if (v < 0) sign = -sign, v = -v;
        for (int i = (int) z.size() - 1, rem = 0; i >= 0; --i) {
            long long cur = z[i] + rem * (long long) base;
            z[i] = (int) (cur / v);
            rem = (int) (cur % v);
        }
        trim();
        return *this;
    }

    bigint operator/(int v) const { return bigint(*this) /= v; }

    int operator%(int v) const {
        if (v < 0) v = -v;
        int m = 0;
        for (int i = (int) z.size() - 1; i >= 0; --i)
            m = (int) ((z[i] + m * (long long) base) % v);
        return m * sign;
    }

    bigint &operator*=(const bigint &v) { return *this = *this * v; }
    bigint &operator/=(const bigint &v) { return *this = *this / v; }

    bool operator<(const bigint &v) const {
        if (sign != v.sign)
            return sign < v.sign;
        if (z.size() != v.z.size())
            return z.size() * sign < v.z.size() * v.sign;
        for (int i = (int) z.size() - 1; i >= 0; i--)
            if (z[i] != v.z[i])
                return z[i] * sign < v.z[i] * sign;
        return false;
    }

    bool operator>(const bigint &v) const { return v < *this; }
    bool operator<=(const bigint &v) const { return !(v < *this); }
    bool operator>=(const bigint &v) const { return !(*this < v); }

    bool operator==(const bigint &v) const { return !(*this < v) && !(v < *this); }

    bool operator!=(const bigint &v) const { return *this < v || v < *this; }

    void trim() {
        while (!z.empty() && z.back() == 0) z.pop_back();
        if (z.empty()) sign = 1;
    }

    bool isZero() const { return z.empty(); }

    friend bigint operator-(bigint v) {
        if (!v.z.empty()) v.sign = -v.sign;
        return v;
    }

    bigint abs() const {
        return sign == 1 ? *this : -*this;
    }

    long long longValue() const {
        long long res = 0;
        for (int i = (int) z.size() - 1; i >= 0; i--)
            res = res * base + z[i];
        return res * sign;
    }

    friend bigint gcd(const bigint &a, const bigint &b) {
        return b.isZero() ? a : gcd(b, a % b);
    }

    friend bigint lcm(const bigint &a, const bigint &b) {
        return a / gcd(a, b) * b;
    }

    void read(const string &s) {
        sign = 1;
        z.clear();
        int pos = 0;
        while (pos < s.size() && (s[pos] == '-' || s[pos] == '+')) {
            if (s[pos] == '-')
                sign = -sign;
            ++pos;
        }
        for (int i = (int) s.size() - 1; i >= pos; i -= base_digits) {
            int x = 0;
            for (int j = max(pos, i - base_digits + 1); j <= i; j++)
                x = x * 10 + s[j] - '0';
            z.push_back(x);
        }
        trim();
    }

    friend istream &operator>>(istream &stream, bigint &v) {
        string s; stream >> s;
        v.read(s);
        return stream;
    }

    friend ostream &operator<<(ostream &stream, const bigint &v) {
        if (v.sign == -1)
            stream << '-';
        stream << (v.z.empty() ? 0 : v.z.back());
        for (int i = (int) v.z.size() - 2; i >= 0; --i)
            stream << setw(base_digits) << setfill('0') << v.z[i];
        return stream;
    }

    static vector<int> convert_base(const vector<int> &a, int old_digits, int new_digits) {
        vector<long long> p(max(old_digits, new_digits) + 1);
        p[0] = 1;
        for (int i = 1; i < p.size(); i++)
            p[i] = p[i - 1] * 10;
        vector<int> res;
        long long cur = 0;
        int cur_digits = 0;
        for (int v : a) {
            cur += v * p[cur_digits];
            cur_digits += old_digits;
            while (cur_digits >= new_digits) {
                res.push_back(int(cur % p[new_digits]));
                cur /= p[new_digits];
                cur_digits -= new_digits;
            }
        }
        res.push_back((int) cur);
        while (!res.empty() && res.back() == 0)
            res.pop_back();
        return res;
    }

    typedef vector<long long> vll;

    static vll karatsubaMultiply(const vll &a, const vll &b) {
        int n = a.size();
        vll res(n + n);
        if (n <= 32) {
            for (int i = 0; i < n; i++)
                for (int j = 0; j < n; j++)
                    res[i + j] += a[i] * b[j];
            return res;
        }

        int k = n >> 1;
        vll a1(a.begin(), a.begin() + k);
        vll a2(a.begin() + k, a.end());
        vll b1(b.begin(), b.begin() + k);
        vll b2(b.begin() + k, b.end());

        vll a1b1 = karatsubaMultiply(a1, b1);
        vll a2b2 = karatsubaMultiply(a2, b2);

        for (int i = 0; i < k; i++)
            a2[i] += a1[i];
        for (int i = 0; i < k; i++)
            b2[i] += b1[i];

        vll r = karatsubaMultiply(a2, b2);
        for (int i = 0; i < a1b1.size(); i++)
            r[i] -= a1b1[i];
        for (int i = 0; i < a2b2.size(); i++)
            r[i] -= a2b2[i];

        for (int i = 0; i < r.size(); i++)
            res[i + k] += r[i];
        for (int i = 0; i < a1b1.size(); i++)
            res[i] += a1b1[i];
        for (int i = 0; i < a2b2.size(); i++)
            res[i + n] += a2b2[i];
        return res;
    }

    bigint operator*(const bigint &v) const {
        vector<int> a6 = convert_base(this->z, base_digits, 6);
        vector<int> b6 = convert_base(v.z, base_digits, 6);
        vll a(a6.begin(), a6.end());
        vll b(b6.begin(), b6.end());
        while (a.size() < b.size())
            a.push_back(0);
        while (b.size() < a.size())
            b.push_back(0);
        while (a.size() & (a.size() - 1))
            a.push_back(0), b.push_back(0);
        vll c = karatsubaMultiply(a, b);
        bigint res;
        res.sign = sign * v.sign;
        for (int i = 0, carry = 0; i < c.size(); i++) {
            long long cur = c[i] + carry;
            res.z.push_back((int) (cur % 1000000));
            carry = (int) (cur / 1000000);
        }
        res.z = convert_base(res.z, 6, base_digits);
        res.trim();
        return res;
    }

};

bigint random_bigint(int n) {
    string s;
    for (int i = 0; i < n; i++) {
        s += rand() % 10 + '0';
    }
    return bigint(s);
}

// random tests
void bigintTest() {
    bigint x = bigint("120");
    bigint y = bigint("5");
    cout << x / y << endl;

    for (int i = 0; i < 1000; i++) {
        int n = rand() % 100 + 1;
        bigint a = random_bigint(n);
        bigint res = sqrt(a);
        bigint xx = res * res;
        bigint yy = (res + 1) * (res + 1);

        if (xx > a || yy <= a) {
            cout << i << endl;
            cout << a << " " << res << endl;
            break;
        }

        int m = rand() % n + 1;
        bigint b = random_bigint(m) + 1;
        res = a / b;
        xx = res * b;
        yy = b * (res + 1);

        if (xx > a || yy <= a) {
            cout << i << endl;
            cout << a << " " << b << " " << res << endl;
            break;
        }
    }

    bigint a = random_bigint(10000);
    bigint b = random_bigint(2000);
    clock_t start = clock();
    bigint c = a / b;
    printf("time=%.3lfsec\n", (clock() - start) * 1. / CLOCKS_PER_SEC);
}

string str(bigint b) {
    stringstream ss; ss << b;
    string s; ss >> s;
    return s;
}
```

## 并查集

```cpp
namespace UF1{

    class UnionFind{

    private:
        int* parent; // parent[i]表示第i个元素所指向的父节点
        int* sz;     // sz[i]表示以i为根的集合中元素个数
        int count;   // 数据个数

    public:
        // 构造函数
        UnionFind(int count){
            parent = new int[count];
            sz = new int[count];
            this->count = count;
            for( int i = 0 ; i < count ; i ++ ){
                parent[i] = i;
                sz[i] = 1;
            }
        }

        // 析构函数
        ~UnionFind(){
            delete[] parent;
            delete[] sz;
        }

        // 查找过程, 查找元素p所对应的集合编号
        // O(h)复杂度, h为树的高度
        int find(int p){
            assert( p >= 0 && p < count );
            // 不断去查询自己的父亲节点, 直到到达根节点
            // 根节点的特点: parent[p] == p
            while( p != parent[p] )
                p = parent[p];
            return p;
        }

        // 查看元素p和元素q是否所属一个集合
        // O(h)复杂度, h为树的高度
        bool isConnected( int p , int q ){
            return find(p) == find(q);
        }

        // 合并元素p和元素q所属的集合
        // O(h)复杂度, h为树的高度
        void unionElements(int p, int q){

            int pRoot = find(p);
            int qRoot = find(q);

            if( pRoot == qRoot )
                return;

            // 根据两个元素所在树的元素个数不同判断合并方向
            // 将元素个数少的集合合并到元素个数多的集合上
            if( sz[pRoot] < sz[qRoot] ){
                parent[pRoot] = qRoot;
                sz[qRoot] += sz[pRoot];
            }
            else{
                parent[qRoot] = pRoot;
                sz[pRoot] += sz[qRoot];
            }
        }
    };
}


namespace UF2{

    class UnionFind{

    private:
        int* rank;   // rank[i]表示以i为根的集合所表示的树的层数
        int* parent; // parent[i]表示第i个元素所指向的父节点
        int count;   // 数据个数

    public:
        // 构造函数
        UnionFind(int count){
            parent = new int[count];
            rank = new int[count];
            this->count = count;
            for( int i = 0 ; i < count ; i ++ ){
                parent[i] = i;
                rank[i] = 1;
            }
        }

        // 析构函数
        ~UnionFind(){
            delete[] parent;
            delete[] rank;
        }

        // 查找过程, 查找元素p所对应的集合编号
        // O(h)复杂度, h为树的高度
        int find(int p){
            assert( p >= 0 && p < count );
            // 不断去查询自己的父亲节点, 直到到达根节点
            // 根节点的特点: parent[p] == p
            while( p != parent[p] )
                p = parent[p];
            return p;
        }

        // 查看元素p和元素q是否所属一个集合
        // O(h)复杂度, h为树的高度
        bool isConnected( int p , int q ){
            return find(p) == find(q);
        }

        // 合并元素p和元素q所属的集合
        // O(h)复杂度, h为树的高度
        void unionElements(int p, int q){

            int pRoot = find(p);
            int qRoot = find(q);

            if( pRoot == qRoot )
                return;

            // 根据两个元素所在树的元素个数不同判断合并方向
            // 将元素个数少的集合合并到元素个数多的集合上
            if( rank[pRoot] < rank[qRoot] ){
                parent[pRoot] = qRoot;
            }
            else if( rank[qRoot] < rank[pRoot]){
                parent[qRoot] = pRoot;
            }
            else{ // rank[pRoot] == rank[qRoot]
                parent[pRoot] = qRoot;
                rank[qRoot] += 1;   // 此时, 我维护rank的值
            }
        }
    };
}
```

## 直线划分平面

```cpp
while(cin>>n)
    cout<<n*(n+1)/2+1<<endl;

```

## 判断两个线段相交

```cpp
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

## 网络流

```cpp
struct Flow_Dinic {
        Flow_Dinic(int n) {
            head = vector<int>(n + 10, -1);
            level = vector<int>(n + 10);
        }

        struct edge {
            int to, cap, next;
        };

        vector<int> head;
        vector<int> level;

        int s = 0, t = 0; // max_flow from s to t

        vector<struct edge> edge;

        void add_edge(int u, int v, int c) {
            edge.push_back((struct edge) {v, c, head[u]});
            head[u] = edge.size() - 1;
            edge.push_back((struct edge) {u, 0, head[v]});
            head[v] = edge.size() - 1;
        }

        bool bfs() {
            fill(level.begin(), level.end(), 0);
            level[s] = 1;
            queue<int> que;
            que.push(s);
            while (!que.empty()) {
                for (int i = head[que.front()]; ~i; i = edge[i].next) {
                    if (edge[i].cap && !level[edge[i].to]) {
                        level[edge[i].to] = level[que.front()] + 1;
                        que.push(edge[i].to);
                        if (edge[i].to == t) return true;
                    }
                }
                que.pop();
            }
            return false;
        }

        int dfs(int f, int u) {
            if (u == t) return f;
            int d = 0, used = 0;
            for (int i = head[u]; ~i; i = edge[i].next) {
                if (edge[i].cap && level[u] == level[edge[i].to] - 1) {
                    if ((d = dfs(min(f - used, edge[i].cap), edge[i].to))) {
                        edge[i].cap -= d;
                        edge[i ^ 1].cap += d;
                        used += d;
                    }
                }
            }
            if (!used) level[u] = 0;
            return used;
        }

        long long run(int _s, int _t) {
            s = _s;
            t = _t;
            long long max_flow = 0;
            while (bfs()) {
                int d = 0;
                while ((d = dfs(0x3f3f3f3f, s))) max_flow += d;
            }
            return max_flow;
        }
    };


struct Dinic {
    Dinic(int n) {
        G = vector<vector<int> >(n + 10);
        d = vector<int> (n+10);
        vis = vector<bool> (n+10);
        cur = vector<int> (n+10);
    }

    struct Edge {
        int from, to, cap, flow;
    };

    int s, t; //节点数,边数,源点编号,汇点编号
    vector<Edge> edges; //边表,edges[e]和edges[e^1]互为反向弧
    vector<vector<int> > G; //邻接表,G[i][j]表示节点i的第j条边在e中的序号
    vector<bool> vis; //bfs用
    vector<int> d; //从起点到i的距离
    vector<int> cur; //当前弧下标
    void add_edge(int from, int to, int cap) {
        edges.push_back({from, to, cap, 0});
        edges.push_back({to, from, 0, 0});
        G[from].push_back(edges.size() - 2);
        G[to].push_back(edges.size() - 1);
    }

    bool BFS() {
        fill(vis.begin(), vis.end(), false);
        queue<int> q;
        q.push(s);
        d[s] = 0;
        vis[s] = true;
        while (!q.empty()) {
            for (int id : G[q.front()]) {
                Edge &e = edges[id];
                if (!vis[e.to] && e.cap > e.flow) {
                    vis[e.to] = true;
                    d[e.to] = d[q.front()] + 1;
                    q.push(e.to);
                }
            }
            q.pop();
        }
        return vis[t];
    }

    long long DFS(int u, int a) {
        if (u == t || a == 0) return a;
        int flow = 0, f;
        for (int &i = cur[u]; i < (int) G[u].size(); ++i) {
            Edge &e = edges[G[u][i]];
            if (d[u] + 1 == d[e.to] &&
                (f = DFS(e.to, min(a, e.cap - e.flow))) > 0) {
                e.flow += f;
                edges[G[u][i] ^ 1].flow -= f;
                flow += f;
                a -= f;
                if (a == 0) break;
            }
        }
        return flow;
    }

    long long run(int _s, int _t) {
        s = _s;
        t = _t;
        long long flow = 0;
        while (BFS()) {
            fill(cur.begin(), cur.end(), 0);
            flow += DFS(s, 0x3f3f3f3f);
        }
        return flow;
    }
};
```


## 最大子矩阵

```cpp
MOD(1e9+7);
MAXN(1010);

int mp[MAXN][MAXN], l[MAXN], r[MAXN];
priority_queue <int> ans;

void solve(){
    FAST_IO;
    
    int n, m;
	cin >> n >> m;
	loop(i, 1, n) loop(j, 1, m){
		char c; cin >> c;
		mp[i][j] = (c == '1');
	}
	loop(i, 2, n) loop(j, 1, m)
		if(mp[i-1][j] && mp[i][j]) mp[i][j] += mp[i-1][j];
	loop(i, 1, n){
		set<pair<int, PII> > S;
		stack<int> A, B;
		loop(j, 1, m){
			while(!A.empty() && mp[i][A.top()] >= mp[i][j]) A.pop();
			l[j] = A.size() ? A.top()+1 : 1;
			A.push(j);
		}
		for(int j = m; j >= 1; --j){
			while(!B.empty() && mp[i][B.top()] >= mp[i][j]) B.pop();
			r[j] = B.size() ? B.top() - 1 : m;
			B.push(j);
		}
		loop(j, 1, m){
			if(mp[i][j]){
				pair<int, PII> temp = mp(r[j], mp(l[j], mp[i][j]));
				pair<int ,PII> temp2;
				if(!S.count(temp)){
					ans.push((r[j] - l[j]+1)*mp[i][j]);
					S.insert(temp);
				}
				if(r[j] - l[j]){
					temp = mp(r[j], mp(l[j]+1, mp[i][j]));
					temp2 = mp(r[j]-1, mp(l[j], mp[i][j]));
					if(!S.count(temp)){
						ans.push((r[j] - l[j]) * mp[i][j]);
						S.insert(temp);
					}else if(S.count(temp2)){
						ans.push((r[j]-l[j])*mp[i][j]);
						S.insert(temp2);
					}
				}
			}
		}
	}
	if(ans.size()<2) ans.push(0);
	ans.pop();
	cout << ans.top() << endl;
    
}


// 速度更快，但只能求前几大

MOD(1e9+7);
MAXN(1e3);

int n, m;
string s;
int high[MAXN];

int mxx, mxx2;
int stkhg[MAXN], stkpos[MAXN];
int cnt;

void solve(){
    FAST_IO;
    
    cin >> n >> m;
	high[m+1] = 0;
	loop(i, 1, n){
		cout << ">-- Line #" << i << " --<" << endl;
		cin >> s;
		loop(j, 1, m) high[j] = s[j-1]=='0' ? 0 : high[j]+1;
#ifdef DEBUG
		cout << "str: " << s << endl;
		cout << "high: ";
		loop(j, 1, m+1) cout << high[j] << ' ';
		cout << endl;
#endif
		stkhg[0] = stkpos[0] = cnt = 0;
		loop(j, 1, m+1){
			if(high[j] > stkhg[cnt]){ // 如果比上一格高，说明可以构成矩形，就push进栈
				stkhg[++cnt] = high[j];
				stkpos[cnt] = j;
			}else if(high[j] < stkhg[cnt]){ // 如果比上一格矮，
				// 那么高度等于上一格高度的矩形已经完全找出来了，
				// 当前格比上一格矮，不能参与构成上个矩形
				while(high[j] < stkhg[cnt]){ // 将栈中高于当前位置的高度全部出栈
					int area = (j-stkpos[cnt]) * stkhg[cnt]; // 当前位置坐标-比当前格高的格的坐标就是宽度
					// 更新前两大矩形
					if(area >= mxx){
						mxx2 = mxx;
						mxx = area;
						mxx2 = max(mxx2, max(area-stkhg[cnt], area-(j-stkpos[j])));
					}else if(area > mxx2){
						mxx2 = area;
					}
					--cnt;
				}
				if(stkhg[cnt] != high[j]){
					stkhg[++cnt] = high[j];
				}
			}
#ifdef DEBUG
			cout << j << " stkhg: ";
			loop(j, 1, m+1) cout << stkhg[j] << ' ';
			cout << endl;
			cout << j << " stkpos: ";
			loop(j, 1, m+1) cout << stkpos[j] << ' ';
			cout << endl;
			cout << "mxx: " << mxx << " mxx2: " << mxx2 << endl;
#endif
		}
	}
	cout << mxx2 << endl;
    
}
```

## 拓展欧几里得

{{<latex>}}ax+by=\gcd(a, b){{</latex>}}

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

## 递推求1~n的逆元

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

## 递推求n!的逆元

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

## lowbit

```cpp
lowbit(n) = n&(~n+1) = n&(-n)
```

## 数的那些位为1

```cpp
void f2(int n){
    int H[37];
    for(int i = 0; i < 36; ++i) H[(1ll << i) % 37] = i;
    while(n > 0){
        cout << H[(n & -n) % 37] << ' ';
        n -= n & -n;
    }
    cout << endl;
}
```

## 最短Hamilton路径

```cpp
int f[1 << 20][20], weight[20][20], n;

int hamilton(int n, int weight[20][20]) {
	memset(f, 0x3f, sizeof(f));
	f[1][0] = 0;
	for (int i = 1; i < 1 << n; i++)
		for (int j = 0; j < n; j++) if (i >> j & 1)
			for (int k = 0; k < n; k++) if ((i^1<<j) >> k & 1)
				f[i][j] = min(f[i][j], f[i^1<<j][k]+weight[k][j]);
	return f[(1 << n) - 1][n - 1];
}

int main() {
	cin >> n;
	for(int i = 0; i < n; i++)
		for(int j = 0; j < n; j++)
			scanf("%d", &weight[i][j]);
	cout << hamilton(n, weight) << endl;
}
```

## 卡特兰数

### 前三十项卡特兰数表

```
[1,1,2,5,14,42,132,429,1430,4862,16796,58786,
 208012,742900,2674440,9694845,35357670,129644790,
 477638700,1767263190,6564120420,24466267020,
 91482563640,343059613650,1289904147324,
 4861946401452,18367353072152,69533550916004,
 263747951750360,1002242216651368,3814986502092304]
```

### 卡特兰数求模模板

```cpp
const int C_maxn = 1e4 + 10;
LL CatalanNum[C_maxn];
LL inv[C_maxn];
inline void Catalan_Mod(int N, LL mod)
{
    inv[1] = 1;
    for(int i=2; i<=N+1; i++)///线性预处理 1 ~ N 关于 mod 的逆元
        inv[i] = (mod - mod / i) * inv[mod % i] % mod;

    CatalanNum[0] = CatalanNum[1] = 1;

    for(int i=2; i<=N; i++)
        CatalanNum[i] = CatalanNum[i-1] * (4 * i - 2) %mod * inv[i+1] %mod;
}
```

### 卡特兰大数模板

```cpp
#include<bits/stdc++.h>
using namespace std;
const int C_maxn = 100 + 10;///项数

int Catalan_Num[C_maxn][1000];///保存卡特兰大数、第二维为具体每个数位的值
int NumLen[C_maxn];///每个大数的数长度、输出的时候需倒序输出

void catalan() //求卡特兰数
{
    int i, j, len, carry, temp;
    Catalan_Num[1][0] = NumLen[1] = 1;
    len = 1;
    for(i = 2; i < 100; i++)
    {
        for(j = 0; j < len; j++) //乘法
        Catalan_Num[i][j] = Catalan_Num[i-1][j]*(4*(i-1)+2);
        carry = 0;
        for(j = 0; j < len; j++) //处理相乘结果
        {
            temp = Catalan_Num[i][j] + carry;
            Catalan_Num[i][j] = temp % 10;
            carry = temp / 10;
        }
        while(carry) //进位处理
        {
            Catalan_Num[i][len++] = carry % 10;
            carry /= 10;
        }
        carry = 0;
        for(j = len-1; j >= 0; j--) //除法
        {
            temp = carry*10 + Catalan_Num[i][j];
            Catalan_Num[i][j] = temp/(i+1);
            carry = temp%(i+1);
        }
        while(!Catalan_Num[i][len-1]) //高位零处理
        len --;
        NumLen[i] = len;
    }
}

int main(void)
{
    catalan();
    for(int i=1; i<=30; i++){
        for(int j=NumLen[i]-1; j>=0; j--){
            printf("%d", Catalan_Num[i][j]);
        }puts("");
    }
    return 0;
}
```

