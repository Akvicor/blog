---
title: "序列"
date: 2019-11-18 14:13:08 +08:00
draft: false
categories: [CF]
tags: ["ComentOJ"]
card: false
weight: 0
---
**[ ComentOJ-C73-4116 序列 ](https://cometoj.com/contest/73/problem/C?problem_id=4116)**

<!--more-->

由于是统计极大连续段（连续子区间）的个数，所以我们可以像DP计数一样，记录以x位置结束（区间右端点）的连续子区间个数f\[x\]。例如此题在初始情况下，由于数组内元素全为0，若长为3，则f\[3\]=1。

由于第 i 次操作将区间 \[l, r\] 赋值为 i ，所以每次操作后 l-1 与 l 、r+1 与 r 的值必定不同，所以每次修改后 f\[l-1\] 与 f\[r\] 的贡献值会增大，增加的个数则是倍增的个数，即 {{<latex>}}2^{i-1}{{</latex>}}

对于其余位置，每次修改后直接倍增*2即可。

----

<details>
<summary>Code</summary>

```cpp
#include <bits/stdc++.h>

using namespace std;
typedef long long ll;

const int MAXN = 2019;
const ll MOD = 20050321;

int n, m;
ll f[MAXN], p[MAXN];

#define DEBUG cout << "The Array: " ; for(int i = 1; i <= n; ++i) cout << f[i] << ' '; cout << endl;
int main(){
        ios::sync_with_stdio(false);
        cin.tie(0); cout.tie(0);

        cin >> n >> m;
        //cout << endl << "n: " << n << " m: " << m << endl << endl;
        p[0] = 1; for(int i = 1; i <= m; ++i) p[i] = (p[i-1]*2) % MOD;
        f[n] = 1;
        //DEBUG; cout << endl;
        for(int i = 1; i <= m; ++i){
                
                int l, r; cin >> l >> r;
                //cout << "l: " << l << " r: " << r << endl;

                for(int j = 1; j < l-1; ++j) f[j] = (f[j]*2) % MOD;
                for(int j = r+1; j <= n; ++j) f[j] = (f[j]*2) % MOD;

                f[l-1] = (f[l-1] + p[i-1]) % MOD;
                f[r] = (f[r] + p[i-1]) % MOD;
                //DEBUG;
                ll ans = 0;
                for(int j = 1; j <= n; ++j) ans = (ans + f[j]) % MOD;
                //cout << "The Ans: ";
                cout << ans << endl;
        }
}
```

</details>

----


