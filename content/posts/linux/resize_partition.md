---
title: "Resize Partition"
date: 2023-05-04 13:43:24 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## 1.卸载磁盘

```shell
umount  -l  /data
```

若提示磁盘忙，使用fuser找出将正在使用磁盘的程序并结束掉。

```shell
fuser -m -v /data
fuser -m -v -i -k /data
```

## 2.磁盘分区

使用fdisk命令重新调整磁盘分区大小

```shell
fdisk -l
fdisk /dev/sdb
```

```shell
p #查看磁柱号 ，记住，后面要用到
d #删除之前的分区
n #建立新分区
p #主分区
1 #第一个主分区
```

删除之前的分区，然后建立新分区，注意开始的磁柱号要和原来的一致（保证数据不丢失的关键步骤），结束的磁柱号默认回车使用全部磁盘。

```shell
wq #保存分区信息并退出
```

## 3.调整分区

```shell
e2fsck -f /dev/sdb1 #检查分区信息
resize2fs /dev/sdb1 #调整分区大小
```

## 4.重新挂载分区

```shell
mount /data
```

