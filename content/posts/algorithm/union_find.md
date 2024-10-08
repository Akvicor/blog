---
title: "并查集"
date: 2019-03-24T20:55:12Z
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

**并查集**是一种树型的数据结构，用于处理一些**不交集（Disjoint Sets）**的合并及查询问题。有一个**联合-查找算法（union-find algorithm）**定义了两个用于此数据结构的操作：

- Find：确定元素属于哪一个子集。它可以被用来确定两个元素是否属于同一子集。
- Union：将两个子集合并成同一个集合。

<!--more-->

## 算法实现

### 变量

一个数组`parent`代表第i个元素所指向的父节点

一个数组`sz`代表以i为根的集合中的元素个数  （用以优化算法，非必须）

```cpp
int* parent; // parent[i]表示第i个元素所指向的父节点

// 使用sz还是rank优化需要根据题目的要求进行选择
int* sz;     // sz[i]表示以i为根的集合中元素个数
int* rank;   // rank[i]表示以i为根的集合所表示的树的层数

int count;   // 数据个数
```

### 函数

#### 初始化

循环对每个元素的parent和sz赋值，此时每个元素都以自己为根节点，且集合中元素个数均为1

```cpp
parent = new int[count];
sz = new int[count];
this->count = count;
for( int i = 0 ; i < count ; i ++ ){
	parent[i] = i;
	sz[i] = 1;
}
```

#### Find函数

assert需要`assert.h`头文件，处理异常数据p

```cpp
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
```

#### Union函数

寻找两个元素的根节点并将两个元素所在的集合合并

- 基于sz的优化

```cpp
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
	} else{
		parent[qRoot] = pRoot;
		sz[pRoot] += sz[qRoot];
	}
}
```

- 基于rank的优化

```cpp
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
	}else if( rank[qRoot] < rank[pRoot]){
		parent[qRoot] = pRoot;
	}else{ // rank[pRoot] == rank[qRoot]
		parent[pRoot] = qRoot;
		rank[qRoot] += 1;   // 此时, 我维护rank的值
	}
}
```

#### isConnected函数

用于检测两个元素是否属于同一集合

```cpp
// 查看元素p和元素q是否所属一个集合
// O(h)复杂度, h为树的高度
bool isConnected( int p , int q ){
	return find(p) == find(q);
}
```

#### Count函数

如`HDU-1232`需要检测一共有几个集合

```cpp
int count_sets(){
	int count = 0;
	for(int i = 0; i < this->count; i++){
		if(find(i) == i)
		count++;
	}
	return count;
}
```



## 完整算法

全部使用C++的类写的算法，使用方法如下

```cpp
int n;
cin >> n;
UF1::UnionFind uf = UF1::UnionFind(n);
```

也可以将其中的变量及函数放到min函数外面使用

### 基于sz优化

```cpp
#include <cassert>

using namespace std;

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
```

### 基于rank优化

```cpp
#include <cassert>

using namespace std;

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



