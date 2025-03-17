---
title: "Linux UFW端口转发"
date: 2025-03-17 22:47:12 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## 编辑 `/etc/default/ufw`

找到 18行左右的 `DEFAULT_FORWARD_POLICY`，将值改为`true`

```bash
vim /etc/default/ufw
```

## 编辑 `/etc/sysctl.conf`

将 `net.ipv4.ip_forward=1` 注释去掉，并确保值为1

```bash
vim /etc/sysctl.conf
sysctl -p
```

## 编辑 `/etc/ufw/before.rules`

在 `*filter` 上方，一般在最顶部，添加如下命令

如果要支持`UDP`的话就再复制一份, 然后将`tcp`改成`udp`即可

```conf
*nat
:POSTROUTING ACCEPT [0:0]
-A PREROUTING -p tcp --dport 监听端口 -j DNAT --to-destination 目标转发IP地址:目标转发端口
-A POSTROUTING -j MASQUERADE
COMMIT
```

重启ufw即可

```bash
ufw reload
```

