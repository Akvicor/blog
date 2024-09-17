---
title: "增加SWAP分区"
date: 2023-05-04 09:22:13 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

一般来说可以按照如下规则设置swap大小：

- 4G以内的物理内存，SWAP 设置为内存的2倍。
- 4-8G的物理内存，SWAP 等于内存大小。
- 8-64G 的物理内存，SWAP 设置为8G。
- 64-256G物理内存，SWAP 设置为16G。

实际上，系统中交换分区的大小并不取决于物理内存的量，而是取决于系统中内存的负荷，所以在安装系统时要根据具体的业务来设置SWAP的值。

一般Linux桌面系统的SWAP设置的会相对大一点，而Linux服务器，特别是生产环境，SWAP可能只有一点点，1-2G，很多甚至都没有SWAP。


## 添加swap交换分区

1. 查看当前内存和swap空间大小


```shell
free -mh
```

2. 创建swap交换分区文件/swap/swapfile，大小为8G

```shell
mkdir /swap
dd if=/dev/zero of=/swap/swapfile bs=1G count=8
```

3. 格式化swap分区：

```shell
mkswap /swap/swapfile
```

4.设置交换分区：

```shell
mkswap -f /swap/swapfile
```

5. 修改权限：

```shell
chmod 600 /swap/swapfile
```

6. 激活swap分区：

```shell
swapon /swap/swapfile
```

7. 设为开机自动启用：

```shell
vim /etc/fstab
```

在该文件底部添加如下内容：

```shell
/swap/swapfile swap swap default 0 0
```

## 删除swap交换分区

1. 停止正在使用的swap分区：

```shell
swapoff /swap/swapfile
```

2. 删除swap分区文件：

```shell
rm /swap/swapfile
```

3. 删除或注释在/etc/fstab文件中的以下开机自动挂载内容：

```shell
/swap/swapfile swap swap default 0 0
```

