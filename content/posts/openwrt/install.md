---
title: "Install OpenWrt/QWRT"
date: 2024-03-24 13:41:31 +08:00
draft: false
categories: [OpenWrt]
tags: []
card: false
weight: 0
---

## 扩充固件

将.img文件中的分区使用fdisk扩充7G

## 写入固件

## 启用魔法

```shell
echo 0xDEADBEEF > /etc/config/google_fu_mode
```

## SSRP 添加Socks5

```shell
opkg update
opkg install ipt2socks
```
