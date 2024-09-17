---
title: "Disable Windows Hibernate"
date: 2023-06-28 13:03:21 +08:00
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

Hiberfil.sys文件位于系统盘的根目录下，这是一个受保护的操作系统文件，它是 win10 休眠功能（Hibernation）中将内存数据与会话数据保存到电脑硬盘、以便于win10计算机断电重新启动后可以快速恢复会话所需的内存镜像文件。在离开计算机一段时间后，计算机将会进入休眠状态。计算机休眠文件就是hiberfil.sys，一般情况它会占用很大存储空间。

打开**管理员**cmd命令窗口输入：

- 关闭 `powercfg -h off`
- 打开 `powercfg -h on`
