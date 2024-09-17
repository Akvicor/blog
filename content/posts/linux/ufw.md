---
title: "Ufw"
date: 2023-02-15 12:07:54 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

[https://wiki.ubuntu.org.cn/Ufw%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97](https://wiki.ubuntu.org.cn/Ufw%E4%BD%BF%E7%94%A8%E6%8C%87%E5%8D%97)

## 安装

默认UFW的规则是放通全部端口

```shell
apt-get install ufw
```

## 设置默认规则

首先设置拒绝所有传入并允许所有传出。

请勿运行以下命令后直接应用，否则会直接锁定你的服务器。确保在应用默认规则前放通了SSH和其他关键服务的端口。


```shell
sudo ufw default allow outgoing
sudo ufw default deny incoming
```

## 添加基本规则

有两种方式添加规则: 端口号或服务名。

例如要允许 SSH 的22 端口的传入传出连接，你可以运行:

```shell
ufw allow ssh
```

或者是:

```shell
ufw allow 22
```

同样，如果你要阻止特定端口上的流量，例如1234，你可运行:

```shell
ufw deny 1234
```

为了适应不同的需求，你还可以设置基于TCP或UPD的规则，例如允许80端口的TCP传入传出连接:

```shell
ufw allow 80/tcp
ufw allow http/tcp
```

下面的例子会允许来自 2000 端口上的 UDP 包:

```shell
ufw allow 2000/udp
```

## 添加高级规则

除了简单的基于端口或协议的规则，UFW 还允许按照IP地址、子网、端口、协议的不同组合来设置高级规则。

例如允许从一个IP连接:

```shell
ufw allow from 192.168.1.1
```

允许特定子网的连接:

```shell
ufw allow from 192.168.1.0/24
```

允许 IP + 端口 + 协议的组合:

```shell
ufw allow from 192.168.1.1 to any port 80 proto tcp
```

允许来自192.168.0.0-192.168.255.255的数据通过eth0网卡进入主机

```shell
ufw allow in on eth0 from 192.168.0.0/16
```

允许指向10.0.0.0-10.255.255.255的数据通过eth1网卡从本机发出

```shell
ufw allow out on eth1 to 10.0.0.0/8
```

允许来自192.168.0.0-192.168.255.255通过eth0网卡收入的数据且指向10.0.0.0-10.255.255.255通过eth1网卡发出的数据经本机路由

```shell
ufw route allow in on eth0 out on eth1 to 10.0.0.0/8 from 192.168.0.0/16
```

所有例子的allow都可以根据需求改为deny，proto tcp也可以根据你的需求改为proto udp


## 删除规则

要删除某一条规则，就在相应的规则前加入delete。例如你想要拒绝 HTTP 的流量你可以运行:

```shell
ufw delete allow 80
```

同样的，可以使用服务名删除规则。

还有另一种方法删除规则，首先运行命令:

```shell
ufw status numbered
```

它会将所有正在使用的规则列出来，并且前面标上了序号

如果你想要删除某一个规则，输入`ufw delete [规则号码]`即可，例如:

```shell
ufw delete 2
```

## 编辑 UFW 的配置文件

虽然可以直接通过命令行添加规则，但是如果你有需要添加更高级或特殊的规则可以编辑配置文件，UFW 有三个配置文件。

### before.rules

```shell
/etc/ufw/before.rules
```

在运行你通过命令行设置的规则 **【前】** 运行的任何规则。同目录中的 `before6.rules` 文件用于 IPv6 。

### after.rules

```shell
/etc/ufw/after.rules
```

在运行你通过命令行设置的规则 **【后】** 运行的任何规则。同目录中的 `after6.rules` 文件用于 IPv6 。

### 默认配置文件

```shell
/etc/default/ufw
```

从此处可以设置是否启用 IPv6，可以设置默认规则，并可以设置 UFW 以管理内置防火墙链。

### 自定义应用

所有配置都在 `/etc/ufw/applications.d/`，同一文件中可配置多个应用，如需修改，前往此目录修改即可。

例如以下文件内容，同时设置了`OpenSSH` 和 `WWW Full`。

```text
[OpenSSH]
title=Secure shell server, an rshd replacement
description=OpenSSH is a free implementation of the Secure Shell protocol.
ports=22/tcp

[WWW Full]
title=Web Server (HTTP,HTTPS)
description=Web Server (HTTP,HTTPS)
ports=80,443/tcp
```

如果你的 SSH 端口自定义了，也可以直接修改。**如果 UFW 运行中修改文件，修改保存后，需要执行 `ufw reload` 应用规则。**

#### 查看应用程序配置文件

```shell
ufw app list 
```

#### 查看 app 具体信息

```shell
ufw app info OpenSSH 
```

#### 快捷开放某个程序需要的防火墙设置

```shell
ufw allow OpenSSH
```

## 查看 UFW 状态

该命令会显示所有规则列表，以及当前是否激活。

```shell
ufw status
```

## 启用关闭防火墙

当你设定好规则后，第一次运行`ufw status`可能会提示Status: inactive。这时候就要使用以下命令激活防火墙规则了。

```shell
ufw enable
```

若要禁用防火墙规则使用以下命令:

```shell
ufw disable
```

## 日志

如需启用日志记录，使用以下命令:

```shell
ufw logging on
```

启用日志后可以使用 `ufw logging low|medium|high` 设置日志级别，默认是low。

日志文件储存在 `/var/logs/ufw`

每个值的意思分别是:

- `[UFW BLOCK]`: 这是记录事件的描述开始的位置。在此例中，它表示阻止了连接。
- `IN`: 如果它包含一个值，那么代表该事件是传入事件
- `OUT`: 如果它包含一个值，那么代表事件是传出事件
- `MAC`: 目的地和源 MAC 地址的组合
- `SRC`: 包源的 IP
- `DST`: 包目的地的 IP
- `LEN`: 数据包长度
- `TTL`: 数据包 TTL，或称为 time to live。 在找到目的地之前，它将在路由器之间跳跃，直到它过期。
- `PROTO`: 数据包的协议
- `SPT`: 包的源端口
- `DPT`: 包的目标端口
- `WINDOW`: 发送方可以接收的数据包的大小
- `SYN URGP`: 指示是否需要三次握手。 0 表示不需要。


