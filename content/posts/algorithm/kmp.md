---
title: "KMP 算法"
date: 2019-04-30T14:13:35Z
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

Knuth-Morris-Pratt 字符串查找算法，简称为 “KMP算法”，常用于在一个文本串S内查找一个模式串P 的出现位置，这个算法由Donald Knuth、Vaughan Pratt、James H. Morris三人于1977年联合发表，故取这3人的姓氏命名此算法。

<!--more-->

## 算法流程

- 假设现在文本串S匹配到 i 位置，模式串匹配到 j 位置
  - 如果 j=-1，或者当前字符匹配成功（即S[i]==P[i]），都令i++，j++，继续匹配下一个字符
  - 如果 j!=-1，且当前字符匹配失败（即S[i]!=P[j]），则令 i 不变，j=next[j]。这意味着失配时，模式串P相对于文本串S向右移动了j-next[j]位。

**next数组**中各值含义：代表当前字符之前的字符串中，有多大长度的相同前缀后缀。例如next[j]=k，代表j之前的字符串中有最大长度为k的相同前缀后缀。

这也意味着在某个字符失配时，该字符对应的next值会告诉你下一步匹配中，模式串应该跳到那个位置。如果next[j]=0或-1，则跳到模式串的开头字符，若next[j]=k 且 k>0，代表下次匹配跳到j之前的某个字符，而不是跳到开头，且具体跳过了k个字符。

### 计算next数组

```cpp
void kmp_pre(char s[], int len, int next[]){
    int i, j;
    j = next[0] = -1;
    i = 0;
    while(i < len){
        while(-1 != j && s[i]!=s[j]) j = next[j];
        next[++i] = ++j;
    }
}

//  a b c a b c a b c d e f a b c d e f g h i
// -1 0 0 0 1 2 3 4 5 6 0 0 0 1 2 3 0 0 0 0 0

//  a b c d a b d
// -1 0 0 0 0 1 2
```



