---
title: "起床困难综合征"
date: 2019-09-14 10:10:36 +08:00
draft: false
categories: [CF]
tags: ["UOJ"]
card: false
weight: 0
---

[UOJ-2 起床困难综合征](https://uoj.ac/problem/2)

<!--more-->

位运算的主要特点之一是在二进制表示下不进位，正因如此，在 {{<latex>}}x_0{{</latex>}} 可以任意选择的情况下，参与位运算的各个位(bit)之间是独立无关的。对于任意的 {{<latex>}}k(0\leq k \leq 30){{</latex>}}，“ans 的 k 位是几” 只与 “{{<latex>}}x_0{{</latex>}} 的第 k 位是几” 有关，与其他位无关，所以我们可以从高位到低位，依次考虑 {{<latex>}}x_0{{</latex>}} 的每一位填 0 还是填 1。

{{<latex>}}x_0{{</latex>}} 的第 k 位填 1，当且仅当同时满足以下两个条件：

1. 已经填好的更高位构成的数值加上 `1 << k` 以后不超过 m。
2. 用每个参数的第 k 位参与位运算。若初值为 1，则 n 次运算后结果为 1，若初值为 0， 则 n 次运算后结果为 0。（若不管初值为 1 或 0，运算后的结果同时为 0 或 1，则填 0 更优，因为可以给后面的位留更大的选择空间）。

# Code

<details>
<summary>Code my</summary>

```cpp
#include <bits/stdc++.h>

using namespace std;

typedef enum{
    AND = 1, OR = 2, XOR = 3
} Bit;

vector<pair<Bit, int>> ops;

Bit get(const string& s){
    if(s[0] == 'A') return AND;
    else if(s[0] == 'O') return OR;
    else return XOR;
}

bool sol(int k, bool set){
    for(auto &i : ops){
        switch(i.first){
            case AND:
                set &= i.second >> k & 1;
                break;
            case OR:
                set |= i.second >> k & 1;
                break;
            case XOR:
                set ^= i.second >> k & 1;
        }
    }
    return set;
}

int main(){
    int n, m;
    cin >> n >> m;
    string op;
    int opi;
    for(int i = 0; i < n; ++i){
        cin >> op >> opi;
        ops.emplace_back(get(op), opi);
    }
    unsigned int num = 0, res = 0;
    for(int i = 30; i >= 0; --i){
        if(sol(i, 0)) res |= 1<<i;
        else if((num | (1 << i)) <= m && sol(i, 1)) res |= (1 << i), num |= (1 << i);
    }
    cout << res << endl;
}
```

</details>

<details>
<summary>Code 1</summary>

```c++
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<string>
using namespace std;
int n, m;
pair<string, int> a[100005];

int calc(int bit, int now) {
	for (int i = 1; i <= n; i++) {
		int x = a[i].second >> bit & 1;
		if (a[i].first == "AND") now &= x;
		else if (a[i].first == "OR") now |= x;
		else now ^= x;
	}
	return now;
}

int main() {
	cin >> n >> m;
	for (int i = 1; i <= n; i++) {
		char str[5];
		int x;
		scanf("%s%d", str, &x);
		a[i] = make_pair(str, x);
	}
	int val = 0, ans = 0;
	for (int bit = 29; bit >= 0; bit--) {
		int res0 = calc(bit, 0);
		int res1 = calc(bit, 1);
		if (val + (1 << bit) <= m && res0 < res1)
			val += 1 << bit, ans += res1 << bit;
		else
			ans += res0 << bit;
	}
	cout << ans << endl;
}

```

</details>

<details>
<summary>Code 2</summary>

```c++
#include <cstdio>

const int MAXN = 100000;
const int MAXM = 1e9;

enum OperatorType {
    And = 0, Or = 1, Xor = 2
};

struct BitwiseOperator {
    OperatorType type;
    bool bits[32];
} ops[MAXN];

int n;
unsigned int m;

inline bool check(const int k, const bool value) {
    bool flag = value;
    for (int i = 0; i < n; i++) {
        if (ops[i].type == And) flag &= ops[i].bits[k];
        else if (ops[i].type == Or) flag |= ops[i].bits[k];
        else if (ops[i].type == Xor) flag ^= ops[i].bits[k];
    }

    return flag;
}

inline unsigned int solve() {
    unsigned int num = 0, ans = 0;
    for (int i = 32 - 1; i >= 0; i--) {
        if (check(i, 0)) ans |= (1 << i);
        else if ((num | (1 << i)) <= m && check(i, 1)) ans |= (1 << i), num |= (1 << i);
    }

    return ans;
}

int main() {
    // freopen("sleep.in", "r", stdin);
    // freopen("sleep.out", "w", stdout);

    scanf("%d %u", &n, &m);

    for (int i = 0; i < n; i++) {
        BitwiseOperator &op = ops[i];
        char str[sizeof("AND")];
        int x;
        scanf("%s %d", str, &x);
        if (str[0] == 'A') op.type = And;
        else if (str[0] == 'O') op.type = Or;
        else if (str[0] == 'X') op.type = Xor;

        for (int i = 0; i < 32; i++) op.bits[i] = ((x & (1 << i)) == 0) ? false : true;
        // for (int i = 0; i < 32; i++) putchar(op.bits[i] ? '1' : '0');
        // putchar('\n');
    }

    printf("%u\n", solve());


    return 0;
}
```

</details>

