---
title: "递归实现枚举"
date: 2019-10-08 20:49:54 +08:00
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

枚举

<!--more-->

# 指数型

```cpp
/* ***********************************************
Author        : Akvicor
Created Time  : Wed Sep 18 21:07:30 2019
File Name     : 1026.cpp
************************************************ */

#include <bits/stdc++.h>

#define FAST_IO ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);

using namespace std;

vector<int> vt;

int n;
void dfs(int x){
	//vt.push_back(x);

	if(x == n+1){
		for(int i = 0; i < vt.size(); ++i){
			if(i != 0) cout << ' ';
			cout << vt[i];
		}cout << endl;
		return;
	}
	
	dfs(x+1);
	vt.push_back(x);
	dfs(x+1);
	vt.pop_back();
	
}

int main()
{
	cin >> n;
	dfs(1);
    return 0;
}
```

# 组合

```cpp
/* ***********************************************
Author        : Akvicor
Created Time  : Thu Sep 19 18:31:55 2019
File Name     : 1027.cpp
************************************************ */

#include <bits/stdc++.h>

#define FAST_IO ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);

using namespace std;
int n, m;
vector<int> vt;
void dfs(int x, int k){
	if(k > m || k + n-x+1 < m) return;
	if(k == m){
		for(int i = 0; i < vt.size(); ++i){
			cout << vt[i] << ' ';
		}cout << endl;
		return;
	}
	vt.push_back(x);
	dfs(x+1, k+1);
	vt.pop_back();
	dfs(x+1, k);
}

int main()
{
    FAST_IO;
	cin >> n >> m;
	dfs(1, 0);
    return 0;
}
```

# 排列

```cpp
/* ***********************************************
Author        : Akvicor
Created Time  : Thu Sep 19 19:10:29 2019
File Name     : a.cpp
************************************************ */

#include <bits/stdc++.h>

#define FAST_IO ios::sync_with_stdio(false); cin.tie(0); cout.tie(0);

using namespace std;

int n;

bool vis[100];
vector<int> v;

void dfs(int k){
	if(k == n){
		for(auto &i : v) cout << i << ' ';
		cout << endl;
		return;
	}
	for(int i = 1; i <= n; ++i){
		if(!vis[i]) {
			vis[i] = true;
			v.push_back(i);
			dfs(k+1);
			v.pop_back();
			vis[i] = false;
		}
	}
}

void nper(int n){
	vector<int> v(n);
	for(int i = 0; i < n; ++i) v[i] = i+1;
	do{
		for(int i = 0; i < v.size(); ++i){
			cout << v[i] << ' ';
		}cout << endl;
	}while(next_permutation(v.begin(), v.end()));
}

int main()
{
    FAST_IO;
    cin >> n;
	dfs(0);
    return 0;
}
```


