---
title: "AdGuard Home"
date: 2023-06-26 12:17:55 +08:00
draft: false
categories: [OpenWrt]
tags: []
card: false
weight: 0
---

## 配置

- 端口：`11004`
- 5553重定向：使用53端口替换dnsmasq
- 执行文件路径：`/viry/serv/AdGuardHome/exec/AdGuardHome`
- 配置文件路径：`/viry/serv/AdGuardHome/data/AdGuardHome.yaml`
- 工作目录：`/viry/serv/AdGuardHome`
- 运行日志路径：`/viry/serv/AdGuardHome/data/AdGuardHome.log`

## 无法更新内核

问题出在默认执行文件路径不对，将执行文件路径修改为 `/usr/bin/AdGuardHome/AdGuardHome`

## 将AdGuard Home作为dns服务器时无网络问题。

53重定向：使用53端口替换dnsmasq

问题出在端口无法为客户端提供dns服务器，所以需要手动在dhcp选项中添加。

- 接口 -> LAN -> DHCP 服务器 -> 高级设置 -> `6,172.16.1.1`
- 接口 -> PLAN -> DHCP 服务器 -> 高级设置 -> `6,192.168.1.1`

