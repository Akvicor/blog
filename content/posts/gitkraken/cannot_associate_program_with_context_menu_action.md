---
title: "cannot associate program with context menu action"
date: 2019-09-29 20:00:15 +08:00
draft: false
categories: [GitKraken]
tags: []
card: false
weight: 0
---

I installed a program called GitKraken and it's pretty nice; But one thing it does during installation is to forcefully add a context menu item to open the current folder in it.

Unfortunately, since day 1, this has been broken. I've tried setting the association manually and it doesn't work. Setting an association with the program doesn't yield any different results. Is there anything else that can be done? I've even tried removing it from the registry.

This is all happening on Windows 10 x64.

<!--more-->

![](https://img.akvicor.com/i/2024/09/17/66e99935555b6.jpg)

- Run regedit.exe
- Go to `HKEY_CLASSES_ROOT/Directory/Background/shell/GitKraken/command`
- Change `"%somedir%\gitkraken\update.exe" --processStart=gitkraken.exe --process-start-args="-p %L"`
- to `"%somedir%\gitkraken\update.exe" --processStart=gitkraken.exe --process-start-args="-p %V"`

