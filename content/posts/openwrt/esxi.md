---
title: "vmdk for ESXi"
date: 2023-05-05 01:23:51 +08:00
draft: false
categories: [OpenWrt]
tags: []
card: false
weight: 0
---

## 开启SSH

管理 -> 服务 -> TSM-SSH

使用SSH登录到ESXI

## 修改的固件vmdk大小

```shell
cd /vmfs/volumes/datastore1
ls *.vmdk
vmkfstools -i openwrt.vmdk 123.vmdk
vmkfstools -X 1024M 123.vmdk # 需大于源文件大小
```

## 添加硬盘

根据实际情况选择EFI或BIOS引导


