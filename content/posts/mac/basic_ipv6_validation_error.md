---
title: "以太网连接 报无效的服务器地址 BasicIPv6ValidationError"
date: 2019-05-31T20:00:44Z
draft: false
categories: [Mac]
tags: []
card: false
weight: 0
---

在使用自定义内网ip的时候突然发现Mac提示我无效的服务器地址。

<!--more-->

思路是这样的：先关闭IPv6，然后设置IPv4，再重新开启IPv6。

# 关闭 IPv6

显然 ”高级“ > "TCP/IP" 下 IPv6 没有提供关闭选项，所以需要用终端命令

```
networksetup -setv6off Ethernet
```

这时候系统会弹窗要求输入密码，搞定后你会发现 ”高级“ > "TCP/IP" 下 IPv6 多了个关闭选项

此时再重新填写信息即可

