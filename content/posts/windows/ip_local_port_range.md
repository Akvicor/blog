---
title: "Windows IP Local Port Range"
date: 2022-08-08 14:58:39 +08:00
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

```bat
:: 查看Windows系统TCP端口范围
netsh int ipv4 show dynamicportrange protocol=udp
:: 修改Windows系统TCP端口范围
netsh int ipv4 set dynamicport tcp start=30000 num=35535
:: 查看Windows系统UDP端口范围
netsh int ipv4 show dynamicportrange protocol=udp
:: 修改Windows系统UDP端口范围
netsh int ipv4 set dynamicport udp start=30000 num=35535
```
