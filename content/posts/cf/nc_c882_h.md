---
title: "Second Large Rectangle"
date: 2019-07-21 21:21:59 +08:00
draft: false
categories: [CF]
tags: ["牛客", "思维"]
card: false
weight: 0
---

**[ 牛客-C882 H - Second Large Rectangle ](https://ac.nowcoder.com/acm/contest/882/H)**

<!--more-->

每读入一行便处理一行，基础算法就是单调栈求最大矩阵

----

<details>
<summary>Code</summary>

```cpp
/**
 *    author: Akvicor
 *    created: 2019-07-21 21-00-00
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
#define pb(x) push_back(x)
#define mp(x, y) make_pair(x, y)
#define fi first
#define se second
#define MOD(x) const int MOD = (int)x
#define MAXN(x) const int MAXN = (int)x + 10

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

/**  >----------------------------------< **/

}

int main(){

#ifdef DEBUG
    int DEBUGCNT = 0;
    clock_t DEBUGstart, DEBUGfinish;
    double DEBUGduration;
    cout << ">------- Akvicor's Solution -------<" << endl;
    while (true) {
        DEBUGstart = clock();
#endif

        SOL::solve();

#ifdef DEBUG
        DEBUGfinish = clock();
        DEBUGduration = (double)(DEBUGfinish - DEBUGstart)*1000 / CLOCKS_PER_SEC;
        cout << " --> Test: #" << DEBUGCNT << " time: " << fixed << setprecision(4) << DEBUGduration << " ms <--" << endl;
        if (cin.eof()) break;
        ++DEBUGCNT;
    }
    cout << ">----------------------------------<" << endl;
#endif

    return 0;
}

```

</details>

----

<details>
<summary>Code - FAST</summary>

```c++
/**
 *    author: Akvicor
 *    created: 2019-07-23 08-52-07
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
#define per(i, a, n) for(int i = a; i >= (n); --i)
#define prec(x) fixed << setprecision(x)
#define ms(s, n) memset(s, n, sizeof(s))
#define all(v) (v).begin(), (v).end()
#define sz(x) ((int)(x).size())
#define mp(x, y) make_pair(x, y)
#define pb(x) push_back(x)
#define fi first
#define se second
#define MOD(x) const int MOD = (int)x
#define MAXN(x) const int MAXN = (int)x + 10

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

/**  >----------------------------------< **/

}

int main(){

#ifdef DEBUG
    int DEBUGCNT = 0;
    clock_t DEBUGstart, DEBUGfinish;
    double DEBUGduration;
    cout << endl << ">------- Akvicor's Solution -------<" << endl << endl;
    while (DEBUGCNT < 70) {
        cout << ">---> Test: #" << DEBUGCNT << " <---<" << endl;
        DEBUGstart = clock();
#endif

        SOL::solve();

#ifdef DEBUG
        DEBUGfinish = clock();
        DEBUGduration = (double)(DEBUGfinish - DEBUGstart)*1000 / CLOCKS_PER_SEC;
        cout << ">---> Test: #" << DEBUGCNT << " time: " << fixed << setprecision(4) << DEBUGduration << " ms <---<" << endl << endl;
        if (cin.eof()) break;
        if (!cin.good()) break;
        if (cin.fail()) break;
        if (cin.bad()) break;
        ++DEBUGCNT;
    }
    cout << ">----------------------------------<" << endl << endl;
#endif

    return 0;
}

```

</details>

----


