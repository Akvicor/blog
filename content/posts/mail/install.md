---
title: "搭建邮件系统"
date: 2024-06-26 21:22:50 +08:00
draft: false
categories: [Mail]
tags: []
card: false
weight: 0
---

官方docker-compose文件生成

[https://setup.mailu.io/2024.06/](https://setup.mailu.io/2024.06/)

参考博客

[https://www.ywbj.cc/?p=929](https://www.ywbj.cc/?p=929)

## 测试服务器是否开启25端口

```shell
telnet smtp.google.com 25 #谷歌邮箱地址
# 或者
telnet smtp.qq.com 25 #腾讯qq邮箱
```

如果已经开启则会显示Connected

```shell
root@mail:~# telnet smtp.qq.com 25
Trying 157.148.54.34...
Connected to smtp.qq.com.
Escape character is '^]'.
220 newxmesmtplogicsvrsza9.qq.com XMail Esmtp QQ Mail Server.
```

如果未开启会显示一直在连接

```shell
root@mail:~$ telnet smtp.qq.com 25
Trying 157.148.54.34...
```

## 设置DNS

将 `mail.akvicor.com` 解析到服务器IP

## 安装常用工具

```shell
apt-get update
apt-get upgrade
apt-get install vim gcc g++ nasm make screen git jq bc curl wget
```

重启

```shell
reboot
```

## 关闭密码登录

```shell
vim /etc/ssh/sshd_config
# PasswordAuthentication no
```

## 安装ufw

```shell
apt-get install rsyslog ufw
```

关闭IPV6

```ini
IPV6=disable
```

开放22端口

```shell
ufw allow ssh
```

重启

```shell
reboot
```

## 安装 Caddy

```
apt install -y debian-keyring debian-archive-keyring apt-transport-https curl
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | tee /etc/apt/sources.list.d/caddy-stable.list
apt update
apt install caddy
```

配置

```
https://mail.akvicor.com {
  tls /akvicor/arrow/cert/ssl/akvicor.com/crt /akvicor/arrow/cert/ssl/akvicor.com/key
  encode gzip
  reverse_proxy  https://127.0.0.1:10006 {
    header_up Host {host}
    header_up X-Real-IP {remote}
    header_up X-Forwarded-Port {server_port}
    transport http {
      tls_insecure_skip_verify
    }
  }
}
```

## 安装Docker

```shell
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

## crt key 转 pem

直接将后缀改为pem即可, 内容不变
