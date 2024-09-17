---
title: "Disable IPV6"
date: 2024-08-03 22:33:47 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

永久关闭IPV6

编辑grub配置文件`/etc/default/grub`

修改增加`ipv6.disable=1`属性

```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash ipv6.disable=1"
GRUB_CMDLINE_LINUX="ipv6.disable=1"
```

使设置生效

```
update-grub
```
