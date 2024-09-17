---
title: "Debian Zoneinfo"
date: 2023-02-15 10:21:29 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

```shell
# 使用UTC时间
cp /usr/share/zoneinfo/UTC /etc/localtime
# 使用GMT-8时间，即北京时间
cp /usr/share/zoneinfo/Etc/GMT-8 /etc/localtime
# 查看当前时间
date "+%Y-%m-%d %H:%M:%S"
TZ=UTC-8 date +%c%:::z
# 同步时间到硬件
hwclock -w
```
