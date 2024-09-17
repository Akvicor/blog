---
title: "屏幕保护、息屏"
date: 2024-01-03 17:29:44 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

可以使用xset命令进行操作

```bash
xset dpms force off # 关闭屏幕
xset s 300 #设置屏保时间为300秒，时间单位为秒
xset s 0 #关闭屏幕保护
xset dpms 600 900 1200 # 三个数值分别为Standby、Suspend、Off，具体什么意思就不多说了，单位秒
xset -dpms #关闭电源管理

xset -q # 查看设置情况。
```

也可以编辑xorg.conf(或者在`/etc/X11/xorg.conf.d/`添加`.conf`结尾的配置文件)，添加如下选项把xscreen saver直接关闭:

```conf
Section "ServerFlags"
   Option "BlankTime" "0"    #关闭黑屏
   Option "StandbyTime" "0"   #关闭待机功能
   Option "SuspendTime" "0"   #关闭睡眠功能
   Option "OffTime" "0"
EndSection
```

修改后重启x即可生效。 
编辑xorg.conf文件和使用xset命令效果一样，可使用xset -q查看设置和当前配置。



