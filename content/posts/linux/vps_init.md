---
title: "VPS Init"
date: 2024-08-29 21:04:35 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## 修改时区

```bash
ln -sf /usr/share/zoneinfo/Etc/GMT-8 /etc/localtime
```

## 配置bash命令

```bash
alias l='ls -Al --color=auto'
alias ll='ls -Alh --color=auto'
```

## 配置apt源

## 删除普通用户

```bash
userdel -r admin
```

## 安装工具

```bash
apt update
apt install -y systemd-timesyncd vim curl wget gcc g++ git make screen telnet jq bc tcptrack
```

## 修改主机名

```bash
vim /etc/hosts
vim /etc/hostname
```

## 配置ssh key

```bash
mkdir .ssh
chmod 600 .ssh

vim .ssh/authorized_keys
chmod 600 .ssh/authorized_keys
```

## 配置sshd

```bash
vim /etc/ssh/sshd_config

PermitRootLogin prohibit-password
PubkeyAuthentication yes
PasswordAuthentication no
ClientAliveInterval 15
ClientAliveCountMax 3
```

## 更改密码

```bash
passwd
```

## 配置git

```bash
git config --global user.name "Akvicor"
git config --global user.email akvicor@kayuki.org
git config --global core.editor vim
```


## motd脚本

```bash
vim /etc/motd
vim /etc/update-motd.d/99-custom
chmod +x /etc/update-motd.d/99-custom
```

内容

```bash
#!/bin/bash

# 获取系统运行时间（天数）
UPTIME_DAYS=$(awk '{print int($1/86400)}' /proc/uptime)

# 获取IPv4地址
IPV4=$(curl -s --max-time 2 ip.sb -4 || echo "未知")

# 获取IPv6地址
IPV6=$(curl -s --max-time 2 ip.sb -6 || echo "未知")

# CPU数量
CPU_NUM=$(cat /proc/cpuinfo| grep "processor"| wc -l)

# 获取内存占用
MEM_USAGE=$(free -m | sed -n '2p' | awk '{printf "%dM / %dM\n",$3,$2}')

# 获取硬盘使用情况
DISK_USAGE=$(df -h / | awk 'NR==2 {print $3 " / " $2}')



echo "-------------------------------------"
echo "  运行时间:  $UPTIME_DAYS 天         "
echo "  网络地址:  $IPV4 / $IPV6           "
echo "  CPU数量 :  ${CPU_NUM}C             "
echo "  内存占用:  $MEM_USAGE              "
echo "  硬盘容量:  $DISK_USAGE             "
echo "-------------------------------------"
```

## 安装Caddy

```bash
apt install -y debian-keyring debian-archive-keyring apt-transport-https curl
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | tee /etc/apt/sources.list.d/caddy-stable.list
apt update
apt install caddy
```

## 安装Docker

```bash
apt-get update
apt-get install ca-certificates curl
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
chmod a+r /etc/apt/keyrings/docker.asc
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
apt-get update
apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```