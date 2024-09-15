---
title: "Fraction Comparision"
date: 2019-07-21 10:46:27 +08:00
draft: false
categories: [CF]
tags: ["牛客", "思维"]
card: false
weight: 0
---

**[ 牛客-C881 J - Fraction Comparision ](https://ac.nowcoder.com/acm/contest/881/J)**

<!--more-->

先判断整数部分，再判断小数部分

<details>
<summary>Code</summary>

```cpp
/**
 *    author: Akvicor
 *    created: 2019-07-21 10-47-45
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

LL x, a, y, b;

void solve(){
    FAST_IO;
    
    while(cin >> x >> a >> y >> b){
        if(x/a > y/b) cout << ">" << endl;
	    else if(x/a < y/b) cout << "<" << endl;
	    else if( (x%a)*b < (y%b)*a ) cout << "<" << endl;
	    else if( (x%a)*b > (y%b)*a ) cout << ">" << endl;
	    else cout << "=" << endl;
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
    if(DEBUGduration > 0.07) cout << " --> Test: #" << DEBUGCNT << " time: " << fixed << setprecision(4) << DEBUGduration << " ms <--" << endl;
    ++DEBUGCNT;
    if(DEBUGCNT < 100) goto DEBUGLOOP;
    cout << ">----------------------------------<" << endl;
#endif

    return 0;
}

```
</details>

----


