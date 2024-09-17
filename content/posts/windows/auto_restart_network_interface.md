---
title: "Auto Restart Network Interface"
date: 2022-09-26 17:21:15 +08:00
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

每隔30秒钟检查一次网络状态，如果网络异常则重启网卡，然后刷新DNS缓存。

```bat
@echo off
echo "正在监听网络状态...."

cd C:\opt\netcheck

:begin
ping akvicor.com > ping.txt
rem echo %errorlevel%
if %ERRORLEVEL% == 1 goto ping2
goto loop

:ping2
ping baidu.com > ping.txt

if %ERRORLEVEL% == 1 goto reboot
goto loop

:reboot
echo %date% %time% "网络异常" >> errlog.log
echo %date% %time% "正在停用网卡"
netsh interface set interface "WLAN" disabled
timeout /T 3 /NOBREAK
echo %date% %time% "正在启用网卡"
netsh interface set interface "WLAN" enable
echo %date% %time% "网卡已重新启动...."
echo %date% %time% "网卡已重新启动" >> errlog.log
goto loop

:loop
ipconfig /flushdns > nul
timeout /T 30 > nul
goto begin
```

