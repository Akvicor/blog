---
title: "Enable HTTPS"
date: 2023-01-29 08:09:34 +08:00
draft: false
categories: [OpenWrt]
tags: []
card: false
weight: 0
---

修改uhttpd配置文件 `/etc/config/uhttpd`

```
option cert /viry/cert/akvicor.com.crt
option key /viry/cert/akvicor.com.key
```

重启uhttpd服务 `/etc/init.d/uhttpd restart`

