---
title: "树莓派 串口 UART"
date: 2024-04-09 17:12:08 +08:00
draft: false
categories: [Raspberry]
tags: []
card: false
weight: 0
---


## 引脚定义

![Raspberry-UART.png](https://img.akvicor.com/i/2024/09/17/66e9a3662324f.png)

## 展示所有串口

```shell
dtoverlay -a | grep uart
```

## 查看特定串口信息

```shell
dtoverlay -h uart2
```

## 开启串口 UART2-5

```shell
vim /boot/config.txt
# 或 (新版本系统中路径发生变化)
vim /boot/firmware/config.txt
```

在文件结尾添加如下：

```txt
dtoverlay=uart2
dtoverlay=uart3
dtoverlay=uart4
dtoverlay=uart5
```

重启后查看是否生效：

```shell
# UART1
ls /dev/ttyS0
# UART2-5
ls /dev/ttyAMA*
```

## GPIO 对应关系

```txt
GPIO14 = TXD0 -> ttyAMA0
GPIO15 = RXD0 -> ttyAMA0

GPIO0  = TXD2 -> ttyAMA2
GPIO1  = RXD2 -> ttyAMA2

GPIO4  = TXD3 -> ttyAMA3
GPIO5  = RXD3 -> ttyAMA3

GPIO8  = TXD4 -> ttyAMA4
GPIO9  = RXD4 -> ttyAMA$
 
GPIO12 = TXD5 -> ttyAMA5
GPIO13 = RXD5 -> ttyAMA5
```


