---
title: "为树莓派等挂载硬盘"
date: 2019-02-11T17:39:33Z
draft: false
categories: [Raspberry]
tags: []
card: false
weight: 0
---

硬盘首先进行分区和格式化，这里推荐用MiniTool Partition Wizard Home Edition来格式化成ext4，下载地址为`http://www.partitionwizard.com/download.html` 。其实你也可以使用树莓派自带的sudo mkfs -t ext4 /dev/sdb1来进行格式化，这里不推荐啊，因为速度比较慢。：）

用Xshell5软件连接树莓派，输入df -h来查看挂载情况。

接下来就直接挂载到samba上面，为以后方便。

<!--more-->

![img](https://img.akvicor.com/i/2024/09/17/66e9a3462a4f2.png)

新建文件夹

`sudo mkdir /samba`

设置权限

`sudo chmod 0777 /samba`

取消默认挂载

`sudo umount /dev/sda1`

挂载到samba上

`sudo mount /dev/sda1 /samba`

这里再用df -h查看挂载情况，上图画红圈的/dev/sda1表示已经挂载，这里我挂载到了samba上面。

每次树莓派重启或者硬盘插拔都需要对硬盘进行重新挂载，比较麻烦，因此需要自动挂载。这里要修改`/etc/fstab`文件。

这里使用`sudo nano /etc/fstab`来修改挂载情况。按照如下图来修改就好了。

![img](https://img.akvicor.com/i/2024/09/17/66e9a356096b9.png)