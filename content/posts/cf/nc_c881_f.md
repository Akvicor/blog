---
title: "Random Point in Triangle"
date: 2019-07-21 09:06:12 +08:00
draft: false
categories: [CF]
tags: ["牛客", "思维"]
card: false
weight: 0
---

**[ 牛客-C881 F - Random Point in Triangle ](https://ac.nowcoder.com/acm/contest/881/F)**

<!--more-->

![](https://img.akvicor.com/i/2024/09/15/66e68fa0d674e.png)

![](https://img.akvicor.com/i/2024/09/15/66e68fa0d674e.png)

<details>
<summary>Code</summary>

```cpp
/**
 *    author: Akvicor
 *    created: 2019-07-21 09-33-42
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

LL x1, y1, x2, y2, x3, y3;

void solve(){
    FAST_IO;
    
    while(cin >> x1 >> y1 >> x2 >> y2 >> x3 >> y3){
    	cout << 11ll * labs( (x3-x1) * (y2-y1) - (x2-x1)*(y3-y1) ) << endl;
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


