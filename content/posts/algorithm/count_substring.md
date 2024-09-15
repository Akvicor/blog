---
title: "统计字符串中子串数目"
date: 2019-03-22T22:37:09Z
draft: false
categories: [Algorithm]
tags: []
card: false
weight: 0
---

统计一个字符串在另一个字符串中出现的次数，包含重叠和非重叠两种情况

<!--more-->

## 子串可重叠情况

```cpp
int count_substring_in_string_overlapping(const std::string &str, const std::string &sub) {
    int num = 0;
    for (size_t i = 0; (i = str.find(sub, i)) != std::string::npos; num++, i++);
    return num;
}
```

## 子串不可重叠情况

```cpp
int count_substring_in_string_non_overlapping(const std::string &str, const std::string &sub) {
    int num = 0;
    size_t len = sub.length();
    if (len == 0)len = 1;
    for (size_t i = 0; (i = str.find(sub, i)) != std::string::npos; num++, i += len);
    return num;
}
```

