---
title: "检测IP地址"
date: 2019-10-07 11:40:20 +08:00
draft: false
categories: [Proxy]
tags: []
card: false
weight: 0
---

- 当使用国外服务器搭建网站的时候, 可能遇到一些网站屏蔽了网站IP, 因此需要检测一下是否被屏蔽
- 当使用国外服务器搭建网站的时候经常会碰到一些 IP 因为被墙导致网站不能访问，所以可以先检测一下 IP 是否被墙。 

<!--more-->

**时区**

```bash
ln -sf /usr/share/zoneinfo/Etc/GMT-8 /etc/localtime
```

**检测ip**

```bash
bash <(curl -sL IP.Check.Place) -f

# 流媒体解锁
bash <(curl -sL Media.Check.Place) -M 4
bash <(curl -L -s check.unlock.media) -M 4 -R 0

# 三网回程
curl https://raw.githubusercontent.com/oneclickvirt/backtrace/main/backtrace_install.sh -sSf | bash

# 磁盘IO
curl -sL yabs.sh | bash
bash <(curl -sL yabs.sh) -i -g
```

**检测端口**

```bash
telnet smtp.google.com 25
```

**同步时间**

```bash
apt-get install systemd-timesyncd
```

## 检测IP是否被墙

### 国内 Ping 测试网页

访问 [https://www.ipip.net/ping.php](https://www.ipip.net/ping.php) 输入IP地址,勾选中国、港澳台，如果如下图所示丢包率 100%，那肯定是被墙了。

### 国内外端口扫描测试

[http://tool.chinaz.com/port](http://tool.chinaz.com/port)

### 国外测试

[https://www.yougetsignal.com/tools/open-ports/](https://www.yougetsignal.com/tools/open-ports/)

如果上一步 22 端口是关闭状态，在这一步检测是 open 状态，说明 IP 肯定是被墙掉了。

如果上一步 22 端口是关闭状态，在这一步检测也是 close 状态，那就要查看是不是服务器的防火墙把端口限制了。

