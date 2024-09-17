---
title: "无法删除网络驱动器：此网络连接不存在"
date: 2023-03-28 11:11:55 +08:00
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

1. 单击 “开始”，指向 “运行”，键入 `regedit`，然后单击 “确定”。
2. 在注册表编辑器中，找到以下注册表子项： `HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\MountPoints2`
3. 右键单击要删除的映射驱动器。 例如，右键单击 ##Server_Name#Share_Name，然后单击 “删除”。

