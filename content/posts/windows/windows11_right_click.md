---
title: "还原鼠标右键 Restore old Right-click Context menu in Windows 11"
date: 2024-01-18 00:02:56 +08:00
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

## 右键菜单改回Win10(展开)

```bat
reg add "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}\InprocServer32" /f /ve
taskkill /f /im explorer.exe & start explorer.exe
```

## 右键菜单改回Win11(折叠)

```bat
reg delete "HKCU\Software\Classes\CLSID\{86ca1aa0-34aa-4e8b-a509-50c905bae2a2}" /f
taskkill /f /im explorer.exe & start explorer.exe
```
