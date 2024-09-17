---
title: "PPTP VPN"
date: 2023-12-29 13:35:12 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

```bash
# 安装PPTP客户端
apt-get install pptp-linux
```

编辑配置文件 `vim /etc/ppp/chap-secrets`

```
# Secrets for authentication using CHAP
# client	server	secret			IP addresses
your_username PPTP your_password home.akvicor.com
```

在 `/etc/ppp/peers/` 下新建一个VPN配置文件，文件名就是VPN的名字 `vim /etc/ppp/peers/PPTP`

```
pty "pptp home.akvicor.com --nolaunchpppd"
name Akvicor
remotename PPTP
require-mppe-128
file /etc/ppp/options.pptp
ipparam PPTP
```

添加网卡 `vim /etc/network/interfaces.d/pptp`

```
auto ppp0
iface ppp0 inet ppp
provider PPTP
```

添加vpn启动脚本，用于修改路由表和DNS `vim /etc/ppp/ip-up.d/pptp-route`

```bash
#!/bin/bash

# 关闭VPN时恢复到之前的设置
echo "#!/bin/bash" > /etc/ppp/ip-down.d/pptp-route
echo "ip route replace $(ip route show | grep default | head -n 1)" >> /etc/ppp/ip-down.d/pptp-route
echo "mv /etc/resolv.conf.viry.bk /etc/resolv.conf" >> /etc/ppp/ip-down.d/pptp-route

# 开启VPN时的DNS和路由
cp /etc/resolv.conf /etc/resolv.conf.viry.bk
echo -e "nameserver 172.16.0.1\nnameserver 172.16.1.1" > /etc/resolv.conf
ip route replace default via 172.16.0.1 dev ppp0
```

为脚本添加执行权限

```bash
chmod +x /etc/ppp/ip-up.d/pptp-route
```

开启和关闭VPN

```bash
sudo pon PPTP # 开启
sudo poff PPTP # 关闭
```


