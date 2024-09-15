---
title: "「佛御石之钵 -不碎的意志-」(困难版)"
date: 2019-10-28 13:03:19 +08:00
draft: false
categories: [CF]
tags: ["ComentOJ", "并查集"]
card: false
weight: 0
---

**[ ComentOJ-C72-4032 「佛御石之钵 -不碎的意志-」(困难版) ](https://cometoj.com/contest/72/problem/%EF%BC%A32?problem_id=4032)**

<!--more-->

每行开一个并查集，每个格子的祖先表示包括其在内的右边第一个为 0 的格子

一个总的并查集，表示两个格子是否属于一个集合。

时间复杂度 {{<latex>}}O\left( nm\alpha(nm) + nq \right){{</latex>}}

----

<details>
<summary>Code</summary>

```cpp
/* ***********************************************
Author        : Akvicor
Created Time  : Mon Oct 28 13:06:07 2019
File Name     : C2.cpp
************************************************ */

#include <bits/stdc++.h>

#define FAST_IO ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);
#define endl '\n'
#define ASB using namespace std; typedef long long ll; namespace AkvicorS {
#define ASE } int main() { return AkvicorS::sol(); }

ASB

int n, m, q, x1, y1, x2, y2, ans, cnt;
char s[1010];

const int MAXN = 1e3+10;

int fa[MAXN][MAXN];
int F[MAXN*MAXN];
int id[MAXN][MAXN];

int fx[4][2] = {{0, 1}, {0, -1}, {1, 0}, {-1, 0}};

inline int find(int * f, int x){return f[x] == x ? x : f[x]=find(f, f[x]);}

void merge(int x, int y){
	++ans;
	id[x][y] = ++cnt; // the number of '1'
	F[cnt] = cnt; // every '1' is the father to itself
	int u = find(fa[x], y), v = find(fa[x], y+1); // '1' 's root is the next '0'
	if(u != v) fa[x][u] = v; 

	// check the left, right, up and down
	for(int i = 0; i < 4; ++i){
		int tx = x + fx[i][0];
		int ty = y + fx[i][1];
		if(tx && ty && tx <= n && ty <= m && id[tx][ty]){
			u = find(F, id[x][y]), v = find(F, id[tx][ty]);
			if(u != v){ // if u and v is two union before
				--ans;
				F[u] = v;
			}

		}
	}
}

void init(){
	for(int i = 0; i <= n; ++i){
		for(int j = 0; j <= m+1; ++j){ // Must (j <= m+1) else The line 33 while throw segment falt
			fa[i][j] = j;
		}
	}
}

int sol(){
  FAST_IO;
  
  cin >> n >> m;
	init();
	for(int i = 1; i <= n; ++i){
		cin >> (s+1);
		for(int j = 1; j <= m; ++j){
			if(s[j] == '1'){
				merge(i, j);
			}
		}
	}

	cin >> q;

	while(q--){
		cin >> x1 >> y1 >> x2 >> y2;
		for(int x = x1; x <= x2; ++x){
			for(int y = find(fa[x], y1); y <= y2; y = find(fa[x], y)){
				merge(x, y); // next '0' and merge
			}
		}
		cout << ans << endl;
	}	
  
  return 0;
}

ASE


```
</details>

----


