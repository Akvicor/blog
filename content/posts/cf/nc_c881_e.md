---
title: "ABBA"
date: 2019-07-21 10:34:24 +08:00
draft: false
categories: [CF]
tags: ["牛客", "思维"]
card: false
weight: 0
---

**[ 牛客-C881 E - ABBA ](https://ac.nowcoder.com/acm/contest/881/E)**

<!--more-->

假如把A看成1，B看成-1。根据它的条件会发现，他们的前缀和满足一个规律：\[-m, n\]。然后枚举每个位置。

可以看作是直角坐标系，原点为0。

x轴正方向看作A的数量，值为左边位置的值+1，当超出范围时置为0。

y轴正方向看作B的数量，值为下边位置的值-1，当超出范围时置为0.

其他位置均只能从左侧或下侧走到，即值为左侧加下侧的总和，当超出范围时置为0.

<details>
<summary>Code</summary>

```cpp
/**
 *    author: Akvicor
 *    created: 2019-07-21 09-37-22
**/

#include <bits/stdc++.h>

using namespace std;

#ifdef DEBUG
#define FAST_IO 17
#else
#define FAST_IO ios::sync_with_stdio(false);cin.tie(0);cout.tie(0)
#define endl '\n'
#endif

#define rep(i, n) for(int i = 0; i < (n); ++i)
#define reep(i, n) for(int i = 0; i <= (n); ++i)
#define lop(i, a, n) for(int i = a; i < (n); ++i)
#define loop(i, a, n) for(int i = a; i <= (n); ++i)
#define prec(x) fixed << setprecision(x)
#define ms(s, n) memset(s, n, sizeof(s))
#define all(v) (v).begin(), (v).end()
#define sz(x) ((int)(x).size())
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define MOD(x) const int MOD = (int)x + 7
#define MAXN(x) const int MAXN = (int)x + 7

typedef long long LL;
typedef unsigned long long ULL;
typedef pair<int, int> PII;
typedef vector<int> VI;
typedef vector<PII> VII;

namespace SOL{

const double EPS = 1e-6;
const double PI = acos(-1.0);
const int INF = 0x3f3f3f3f;
const LL LINF = 0x7f7f7f7f7f7f7f7f;

/**  >------- Akvicor's Solution -------< **/

MOD(1e9);
MAXN(1e6);

void mod(long long & n){ n %= (long long)MOD; }
void mod(int & n){ n %= MOD; }

LL dp[3010][3010];

void solve(){
    FAST_IO;
    
    int n, m;
    while(cin >> n >> m){
	reep(i, n+m) reep(j, n+m) dp[i][j] = 0;
	dp[0][0] = 1;
	reep(i, n+m) reep(j, n+m){
		if(i) dp[i][j] += dp[i-1][j];
		if(j) dp[i][j] += dp[i][j-1];
		//dp[i][j] %= MOD;
		mod(dp[i][j]);
		if(i-j > n || j-i > m) dp[i][j] = 0;
	}
	cout << dp[n+m][n+m] << endl;
    }
    
}

/**  >----------------------------------< **/

}

int main(){

#ifdef DEBUG
    int DEBUGCNT = 0;
    clock_t DEBUGstart, DEBUGfinish;
    double DEBUGduration;
    cout << ">------- Akvicor's Solution -------<" << endl;
DEBUGLOOP:
    DEBUGstart = clock();
#endif

    SOL::solve();

#ifdef DEBUG
    DEBUGfinish = clock();
    DEBUGduration = (double)(DEBUGfinish - DEBUGstart)*1000 / CLOCKS_PER_SEC;
    if(DEBUGduration > 0.07)
    cout << " --> Test: #" << DEBUGCNT << " time: " << fixed << setprecision(4) << DEBUGduration << " ms <--" << endl;
    ++DEBUGCNT;
    if(DEBUGCNT < 100) goto DEBUGLOOP;
    cout << ">----------------------------------<" << endl;
#endif

    return 0;
}

```
</details>

----


