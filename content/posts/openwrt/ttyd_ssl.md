---
title: "TTYD 开启SSL"
date: 2023-06-28 09:49:34 +08:00
draft: false
categories: [OpenWrt]
tags: []
card: false
weight: 0
---

修改`/etc/config/ttyd`

```
config ttyd
        option interface '@lan'
        option command '/bin/login'
        option ssl 'true'
        option ssl_cert '/viry/cert/akvicor.com.crt'
        option ssl_key '/viry/cert/akvicor.com.key'
```
