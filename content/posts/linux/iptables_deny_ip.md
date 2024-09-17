---
title: "Iptables deny ip"
date: 2023-02-15 11:28:58 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## only permit china ip

```shell
systemctl stop firewalld.service
systemctl disable firewalld.service

yum install ipset
yum install iptables-services
```

## 清空之前的规则

```shell
iptables -P INPUT ACCEPT
iptables -F
```

## 创建一个名为cnip的规则

```shell
ipset -N cnip hash:net
ipset save
# 下载国家IP段，这里以中国为例
wget -P . http://www.ipdeny.com/ipblocks/data/countries/cn.zone
# 将IP段添加到cnip规则中
for i in $(cat /root/cn.zone ); do ipset -A cnip $i; done

ipset save cnip -f /etc/ipset.cnip
/sbin/ipset restore -f /etc/ipset.cnip
```

## 放行IP段

```shell
iptables -A INPUT -p tcp -m set --match-set cnip src -j ACCEPT
```

## 关掉指定端口

```shell
iptables -P INPUT DROP
# 关闭指定端口，比如80/443
iptables -A INPUT -p tcp --dport 80 -j DROP
iptables -A INPUT -p tcp --dport 443 -j DROP

# 将参数里的-A改成-D就是删除规则了，如
iptables -D INPUT -p tcp -m set --match-set cnip src -j ACCEPT
iptables -D INPUT -p tcp --dport 443 -j DROP

# save
iptables-save > /etc/sysconfig/iptalbes
iptables-save > /etc/iptables-script
# restore
iptables-restore > /etc/sysconfig/iptables
iptables-restore > /etc/iptables-script
```

## 接受全部中国IP

```shell
#全部接受中国IP
-A INPUT -m set --match-set china src -j ACCEPT
#接受中国IP访问本机特定端口特定协议（例如5060UDP协议），freeswitch一般要用这条，直接具体到端口协议
-A INPUT -m set --match-set china src -p udp -m udp --dport 5060 -j ACCEPT
#接受中国IP的ping响应
-A INPUT -m set --match-set china src -p icmp -j ACCEPT
```

```shell
#!/bin/bash
ipset create china hash:net hashsize 1024 maxelem 65536
rm -f cn.zone
wget http://www.ipdeny.com/ipblocks/data/countries/cn.zone
for i in `cat cn.zone`
do
ipset add china $i
done
```

```shell
#!/bin/bash
ipset create hongkong hash:net hashsize 1024 maxelem 65536
rm -f hk.zone
wget http://www.ipdeny.com/ipblocks/data/countries/hk.zone
for i in `cat hk.zone`
do
ipset add hongkong $i
done
```

## 配置文件

```shell
vim /etc/sysconfig/iptables
```

```
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
-A INPUT -m set --match-set cnip src -j ACCEPT
-I INPUT -m set --match-set hongkong src -j DROP
-I INPUT -p tcp --dport 80 --syn -m recent --name SYN_FLOOD --update --seconds 60 --hitcount 10 -j REJECT
-I INPUT -p tcp --syn -m limit --limit 1/s -j ACCEPT
-A INPUT -j DROP
COMMIT
# Completed on Sun Mar 27 20:29:24 2022
```

```shell
ipset save > /etc/ipset.cn.hk
/sbin/ipset restore -f /etc/ipset.cn.hk
systemctl restart iptables
```

```shell
iptables -L -n --line-number
iptables -vnL
netstat -an | awk '/^tcp/ {++s[$NF]} END {for(a in s ) print a,s[a]}'
```

## 限制 syn 并发数为每秒 1 次

```shell
iptables -A INPUT -p tcp --syn -m limit --limit 1/s -j ACCEPT
```

## 限制单个 IP 在 60 秒新建立的连接数为 10

```shell
iptables -I INPUT -p tcp --dport 80 --syn -m recent --name SYN_FLOOD --update --seconds 60 --hitcount 10 -j REJECT
```





