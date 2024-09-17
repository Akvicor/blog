---
title: "Usbfs"
date: 2022-08-09 13:01:01 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

赋予用户组操作usb设备的权限

## 创建用户组并添加用户到用户组

```bash
# 创建用户组
groupadd usbfs
# 查看usbfs用户组的id
#   usbfs:x:1001:
cat /dev/group | grep usbfs
# 把akvicor添加到用户组
useradd -G usbfs akvicor
  # 或
vim /etc/group
  # 修改 usbfs:x:1001:
  # 为   usbfs:x:1001:akvicor
```

## 添加访问权限

```bash
# 进入目录
cd /etc/udev/rules.d
# 创建配置文件
vim xxx.rules
# 写入信息到配置文件
```

允许用户组`usbfs`访问usb设备

- `GROUP` 设置usb设备的所属用户组为`usbfs`
- `MODE` 设置usb设备的访问权限为`0777`

```
SUBSYSTEMS=="usb",GROUP="usbfs",MODE="0777"
SUBSYSTEMS=="usb",GROUP="usbfs"
```

允许用户`akvicor`访问usb设备

- `OWNER` 设置usb设备的所有者为`akvicor`
- `MODE` 设置usb设备的访问权限为`0777`

```
SUBSYSTEMS=="usb",OWNER="akvicor",MODE="0777"
```

