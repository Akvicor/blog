---
title: "exe文件图标变成了白色无图标"
date: 2019-09-29 16:43:06 +08:00
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

exe文件图标变成了白色无图标

<!--more-->

按键 “WIN+R” 输入即可cmd
然后输入分别输入 ：

```cmd
taskkill /im explorer.exe /f
cd /d %userprofile%\appdata\local
del iconcache.db /a
start explorer.exe
exit
```

看了看这段代码，应该就是把图标缓存的数据库给删除了，然后再启动 explorer.exe

