---
title: "/usr/bin/ld: cannot find -lpthread"
date: 2022-11-17 08:46:36 +08:00
draft: false
categories: [OpenWrt]
tags: []
card: false
weight: 0
---

使用golang编译程序遇到的问题

```
# runtime/cgo
/usr/bin/ld: cannot find -lpthread
collect2: error: ld returned 1 exit status
```

pthread primitives are part of the core libc.so library. to satisfy `-lpthread`, create an empty AR archive somewhere in the library search path.

```shell
ar -rc /usr/lib/libpthread.a
```

