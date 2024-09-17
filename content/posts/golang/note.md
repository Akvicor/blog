---
title: "Note"
date: 2023-02-10 14:57:47 +08:00
draft: false
categories: [Golang]
tags: []
card: false
weight: 0
---

## foreach变量

### 错误代码

```go
for _, v := range histories {
	InsertByHistory(&v)
}
```

### 正确代码

```go
for _, v := range histories {
	h := v
	InsertByHistory(&h)
}
```

### 笔记

如以上代码，传递指针时必须声明一个新变量存储`v`，否则会导致传递给函数的是`histories`最后一个元素的首地址

