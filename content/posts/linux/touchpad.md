---
title: "Debian 触摸板 Touchpad"
date: 2024-02-06 07:59:59 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

使触摸板敲击（不是按压）时也产生左键事件

```bash
sudo apt install xserver-xorg-input-synaptics
sudo vim /etc/X11/xorg.conf.d/50-synaptics.conf
```

在文件中添加以下内容

```conf
Section "InputClass"
    Identifier "touchpad catchall"
    Driver "synaptics"
    MatchIsTouchpad "on"

    Option "TapButton1" "1"            #单指敲击产生左键事件
    Option "TapButton2" "2"            #双指敲击产生中键事件
    Option "TapButton3" "3"            #三指敲击产生右键事件

    Option "VertEdgeScroll" "on"       #滚动操作：横向、纵向、环形
    Option "VertTwoFingerScroll" "on"
    Option "HorizEdgeScroll" "on"
    Option "HorizTwoFingerScroll" "on"
    Option "CircularScrolling" "on"
    Option "CircScrollTrigger" "2"

    Option "EmulateTwoFingerMinZ" "40" #精确度
    Option "EmulateTwoFingerMinW" "8"
    Option "CoastingSpeed" "20"        #触发快速滚动的滚动速度

    Option "PalmDetect" "1"            #避免手掌触发触摸板
    Option "PalmMinWidth" "3"          #认定为手掌的最小宽度
    Option "PalmMinZ" "200"            #认定为手掌的最小压力值
EndSection
```

## 键入时禁止触摸板

键入时禁止触摸板可以避免焦点变化，影响当前的输入。 对于使用 startx 来启动的桌面系统，可以修改其 .xinitrc 初始化配置文件来完成：

```bash
syndaemon -t -k -i 2 -d &
```

其中的 `-i 2` 表示两秒空闲，即键盘事件后的两秒内不允许响应触摸板 Tap。更多信息请参照手册页：

```
man syndaemon
```

## 外接鼠标时禁用触摸板

在 arch linux 中，使用 udev 监测硬件的热拔插，通过修改其规则文件，来响应外接鼠标事件，从而禁用和启用触摸板。如下的规则文件，调用了 synclient。

```
#file: /etc/udev/rules.d/01-touchpad.rules
ACTION=="add", SUBSYSTEM=="input", KERNEL=="mouse[0-9]", ENV{DISPLAY}=":0.0", ENV{XAUTHORITY}="/home/harttle/.Xauthority", ENV{ID_CLASS}="mouse", RUN+="/usr/bin/synclient TouchpadOff=1"
ACTION=="remove", SUBSYSTEM=="input", KERNEL=="mouse[0-9]", ENV{DISPLAY}=":0.0", ENV{XAUTHORITY}="/home/harttle/.Xauthority", ENV{ID_CLASS}="mouse", RUN+="/usr/bin/synclient TouchpadOff=0"
```

注意：该文件中每个操作必须单独一行，可以使用 `\` 来折行；`SUBSYSTEM` 与 `KERNEL` 指定了设备 `/dev/input/mouse[0-9]`（archwiki的中文页面中此处有误，我会找时间去修改）。了解更多 `udev rules` 语法：https://wiki.archlinux.org/index.php/Udev

开机时鼠标检测

`PS/2` 鼠标在开机时不会触发 `udev` 规则。我们做一个桌面环境的启动脚本，在 `.xinitrc`，`profile` 中调用，或者放在 `KDE` 的 `Autostart` 中：

```bash
#!/bin/bash
ids=`ls /dev/input/by-id | grep -E '.*-mouse'`
[ "$ids" ] && synclient TouchpadOff=1
```
