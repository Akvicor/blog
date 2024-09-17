---
title: "X11VNC Srver"
date: 2023-08-09 14:46:18 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## 安装配置

```bash
# 安装x11vnc
apt install x11vnc
# 设置开机自动启动连接密码,将密码储存在/etc/x11vnc.pass 下
x11vnc -storepasswd /etc/x11vnc.pass
```

## 查看认证服务

```bash
ps wwwwaux | grep auth
```

类似以下信息中的`/var/run/lightdm/root/:0`

```
/usr/lib/xorg/Xorg :0 -seat seat0 -auth /var/run/lightdm/root/:0 -nolisten tcp vt7 -novtswitch
```

## 创建服务


```bash
vim /lib/systemd/system/x11vnc.service
```

添加以下内容

```
[Unit]
Description=Start x11vnc at startup
After=multi-user.target

[Service]
Type=simple
ExecStart=/usr/bin/x11vnc -auth /var/run/lightdm/root/:0 -rfbauth /etc/x11vnc.pass -rfbport 5900

[Install]
WantedBy=multi-user.target
```


```bash
systemctl enable x11vnc
systemctl start x11vnc
```

