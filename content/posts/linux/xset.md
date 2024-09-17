---
title: "Xset"
date: 2022-08-01 14:33:25 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

`xset`:用于设置显示器的各种用户属性选项。如果想要持久化设置，可以将设置写入`.xinitrc`文件中。

dpms（Display Power Manage System）和屏保是互相作用的，这两个值谁设的小谁生效。

```bash
# 查看设置
xset -q
# 设置屏保时间为300秒，时间单位为秒
xset s 300
# 关闭屏幕保护
xset s 0
xset s 0 0
# 设置DPMS，三个数值分别为Standby、Suspend、Off，时间单位为秒
xset dpms 600 900 1200
# 关闭DPMS
xset dpms 0 0 0
xset -dpms
```

