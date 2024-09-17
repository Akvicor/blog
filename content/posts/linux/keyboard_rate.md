---
title: "Keyboard rate"
date: 2024-01-03 19:20:43 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

修改键盘重复频率和延迟

## XServer startup options

As alternative to this tool, you can set defaults for the X Server at startup.

For the keyboard repeat rate:

`/etc/X11/xinit/xserverrc`

```bash
X -ardelay 200 -arinterval 30  # (interval is 1000/rate_in_hz)
```

For this to configure, you need the privileges to edit X launch properties (probably in your login tool like lightdm).

## XServer configuration file

XServer since version 21.1.0 supports the option AutoRepeat. Basically you need an xorg config section like this (the second value again 1000/rate_in_hz):

`/etc/X11/xorg.conf.d/`

```conf
Section "InputClass"
    Identifier "system-keyboard"
    MatchIsKeyboard "on"
    Option "AutoRepeat" "200 20"
EndSection
```


