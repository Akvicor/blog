---
title: "Equivalent Prefixes"
date: 2019-07-21 11:09:17 +08:00
draft: false
categories: [CF]
tags: ["牛客", "思维"]
card: false
weight: 0
---

**[ 牛客-C881 Equivalent Prefixes ](https://ac.nowcoder.com/acm/contest/881/A)**

<!--more-->

因为是从1-p，所以可以维护两个递增且比a\[i\]小的栈，如果过程中两个栈的元素数量不一样多，说明到此位置时，最小值的位置不相同。

<details>
<summary>Code</summary>

```cpp
/**
 *    author: Akvicor
 *    created: 2019-07-18 12-32-25
**/

#include <bits/stdc++.h>

using namespace std;

#ifdef DEBUG
#define FAST_IO 17
#else
#define FAST_IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
#define endl '\n'
#endif

#define LL long long
#define ULL unsigned long long
#define rep(i, n) for(int i = 0; i < (n); ++i)
#define reep(i, n) for(int i = 0; i <= (n); ++i)
#define lop(i, a, n) for(int i = a; i < (n); ++i)
#define loop(i, a, n) for(int i = a; i <= (n); ++i)
#define ALL(v) (v).begin(), (v).end()
#define PB push_back
#define VI vector<int>
#define PII pair<int,int>
#define FI first
#define SE second
#define SZ(x) ((int)(x).size())

const double EPS = 1e-6;
const double PI = acos(-1.0);
const int INF = 0x3f3f3f3f;
const LL LINF = 0x7f7f7f7f7f7f7f7f;
const int MAXN = (int)1e6 + 10;
const int MOD = (int)1e9 + 7;

int n;
int a[MAXN], b[MAXN];
stack<int> x, y;

int main(){
	FAST_IO;
	int ans;
	while(cin >> n){
		while(!x.empty()) x.pop();
		while(!y.empty()) y.pop();
		rep(i, n) cin >> a[i];
		rep(i, n) cin >> b[i];
		for(int i = 0; i < n; ++i){
			while(!x.empty() && x.top() > a[i]) x.pop();
			x.push(a[i]);
			while(!y.empty() && y.top() > b[i]) y.pop();
			y.push(b[i]);
			if(x.size() == y.size()) ans = i;
			else break;
		}
		cout << ans+1 << endl;
	}

	return 0;
}


```
</details>

----


