---
title: "配置iptables防火墙"
date: 2019-08-12T19:45:17Z
draft: false
categories: [IPTables]
tags: []
card: false
weight: 0
---

## whereis iptables #查看系统是否安装防火墙可以看到:

```shell
iptables: /sbin/iptables /usr/share/iptables /usr/share/man/man8/iptables.8.gz #表示已经安装iptables
apt-get install iptables #如果默认没有安装，请运行此命令安装防火墙
```

## iptables -L #查看防火墙配置信息，显示如下:

```shell
Chain INPUT (policy ACCEPT)
target prot opt source destination
Chain FORWARD (policy ACCEPT)
target prot opt source destination
Chain OUTPUT (policy ACCEPT)
target prot opt source destination
```

## vi /etc/iptables/rules.v4

添加以下内容(备注:80是指web服务器端口,3306是指MySQL数据库链接端口,22是指SSH远程管理端口.)

```shell
*filter
:INPUT DROP [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:syn-flood - [0:0]
-A INPUT -i lo -j ACCEPT
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 22 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 80 -j ACCEPT
-A INPUT -p tcp -m state --state NEW -m tcp --dport 443 -j ACCEPT
-A INPUT -p icmp -m limit --limit 100/sec --limit-burst 100 -j ACCEPT
-A INPUT -p icmp -m limit --limit 1/s --limit-burst 10 -j ACCEPT
-A INPUT -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -j syn-flood
-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A syn-flood -p tcp -m limit --limit 3/sec --limit-burst 6 -j RETURN
-A syn-flood -j REJECT --reject-with icmp-port-unreachable
COMMIT
```

## iptables-restore < /etc/iptables/rules.v4 #使防火墙规则生效

## vi /etc/network/if-pre-up.d/iptables #创建文件，添加以下内容，使防火墙开机启动

## chmod +x /etc/network/if-pre-up.d/iptables #添加执行权限

## iptables -L -n查看规则是否生效.

## 临时修改

```shell
# 列出所有规则
iptables -nL --line-number
# 将第三条规则改为ACCEPT
iptables -R INPUT 3 -j ACCEPT
```

