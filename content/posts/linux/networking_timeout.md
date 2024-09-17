---
title: "Networking timeout"
date: 2023-02-15 12:03:23 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## 1

```shell
vim /etc/systemd/system/network-online.target.wants/networking.service

TimeoutStartSec=2sec
```

## 2

```shell
vim /etc/dhcp/dhclient.conf

timeout 15
```

## 1

change `auto eth0` to `allow-hotplug eth0`


