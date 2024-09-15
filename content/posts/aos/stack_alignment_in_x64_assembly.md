---
title: "Stack alignment in x64 assembly"
date: 2022-08-23 17:29:21 +08:00
draft: false
categories: [AOS]
tags: []
card: false
weight: 0
---

gcc会在`call`指令之前让`rsp`按`16`字节对齐。

cpu在`call`的时候将`rip`压栈`rsp -= 8`，所以进入被调用者之后`rsp = 8 (mod 16)`。如果在这个函数里面还要`call`其他函数的话就要将`rsp`减掉奇数个`8`让它重新`16`字节对齐。


