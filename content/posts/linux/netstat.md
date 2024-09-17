---
title: "Netstat"
date: 2023-02-15 12:01:26 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

```shell
# 查看系统tcp连接中各个状态的连接数。
netstat -an|awk '/^tcp/ {++s[$NF]} END {for(a in s ) print a,s[a]}'

# 查看和本机23端口建立连接并状态在established的所有ip
netstat -an|grep 80|grep ESTA|awk '{print $5}'|awk 'BEGIN {FS=":"} {print $1 "\n"}'|sort|uniq -c
```


