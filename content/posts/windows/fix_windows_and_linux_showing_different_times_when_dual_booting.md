---
title: "Fix Windows and Linux Showing Different Times When Dual Booting"
date: 2022-10-04 15:55:43 +08:00
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

## Make Windows Use UTC Time

Filename: `Make Windows Use UTC Time.reg`

```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation]
"RealTimeIsUniversal"=dword:00000001
```

以管理员身份打开 「PowerShell」，输入以下命令

```powershell
Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
```

## Make Windows Use Local Time

Filename: `Make Windows Use Local Time.reg`

```reg
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\TimeZoneInformation]
"RealTimeIsUniversal"=-
```


