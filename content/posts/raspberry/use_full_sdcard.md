---
title: "修复树莓派等无法正常使用SD卡全部空间"
date: 2019-02-11T17:44:01Z
draft: false
categories: [Raspberry]
tags: []
card: false
weight: 0
---

在编译opencv的时候，出现内存不够的情况，/root使用率100%，导致编译错误，所以需要拓展SD卡容量

google了一下，发现树莓派在默认情况下，仅仅使用了SD卡的4G容量，剩下的空间，属于空白分区，完全没有利用起来 所以，我们可以通过`df`命令，来调整linux分区的size

<!--more-->

1. 重新树莓派，进入命令行页面
2. 登陆树莓派，用户名`pi`，密码`raspberry`
3. 切换至超级用户`sudo su`
4. 显示出当前分区的状态和使用率`df -h`
5. 输入`fdisk /dev/mmcblk0` 加载SD卡
6. `p`打印当前分区
7. 你应该会看到三个分区(mmcblk0, mmcblk0p1, mmcblk0p2)，现在把分区2的信息写下来（/dev/mmcblk0p2）
8. 我主要记录了开始扇区（122880）和结束扇区（8447999）的数值
9. 按`d`开始删除分区
10. 系统提示输入删除分区号，输入`2`
11. 按`n`新建分区，然后依次输入`p`,` 2`
12. 接下来输入原来记录的2扇区开始号(122880)，记得替换成你自己的数字
13. 按`w`保持配置
14. 输入`reboot`重启树莓派
15. 输入`sudo resize2fs /dev/mmcblk0p2` 更新系统
16. 输入`df -h`看看，是不是已经完全使用了剩余空间
