---
title: "Linux虚拟串口 Serial port"
date: 2024-09-22 21:29:29 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

首先安装socat

```shell
sudo apt-get install socat
```

启动虚拟串口

```shell
socat -d -d pty,raw,echo=0 pty,raw,echo=0
```

成功后返回以下信息

```text
2024/09/22 21:32:10 socat[1997612] N PTY is /dev/pts/13
2024/09/22 21:32:10 socat[1997612] N PTY is /dev/pts/14
2024/09/22 21:32:10 socat[1997612] N starting data transfer loop with FDs [5,5] and [7,7]
```

- （终端1）监听其中一个串口 `cat < /dev/pts/2`
- （终端2）另一个串口写入数据 `echo "Hello World!" > /dev/pts/3`

现在，从终端1可以看到“”Hello World!“”打印信息，证明串口创建并连接成功。
