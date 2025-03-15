---
title: "Wireguard Mesh 组网"
date: 2025-03-14 17:07:53 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

有时候服务器之间需要加密传输数据(例如[NFS挂载](https://blog.akvicor.com/posts/linux/nfs/))，为了平衡延迟和速度，可以采用Wireguard来组建一个内网，这样业务数据可以通过内网IP高速和安全的传输

## 安装

```bash
apt install -y wireguard
```

## 生成密钥

```bash
wg genkey | tee privatekey | wg pubkey > publickey
```

## 启动

```bash
# 启动
wg-quick up wg0
# 开机启动
systemctl enable wg-quick@wg0
# 查看状态
wg show
```

## 开放防火墙(如果没有启用防火墙则跳过)

如果想通过内网IP来访问, 那么需要在防火墙中配置允许这个内网IP段访问

```bash
# 通过ufw开放
ufw allow from 10.0.0.0/24 to any comment wg0
```

## 测试

```bash
ping 10.0.0.1
ping 10.0.0.2
ping 10.0.0.3
ping 10.0.0.4
```


## 配置文件

**/etc/wireguard/wg0.conf**

### 1. 均有公网IP

| 服务器 | 公网 IP   | 内网 WireGuard IP |
|--------|-----------|------------------|
| A      | X.X.X.A   | 10.0.0.1         |
| B      | X.X.X.B   | 10.0.0.2         |
| C      | X.X.X.C   | 10.0.0.3         |
| D      | X.X.X.D   | 10.0.0.4         |

#### A

```bash
vim /etc/wireguard/wg0.conf
```

内容

```ini
[Interface]
PrivateKey = <ServerA私钥>
Address = 10.0.0.1/24
ListenPort = 51820

[Peer]
PublicKey = <ServerB公钥>
Endpoint = X.X.X.B:51820
AllowedIPs = 10.0.0.2/32
PersistentKeepalive = 25

[Peer]
PublicKey = <ServerC公钥>
Endpoint = X.X.X.C:51820
AllowedIPs = 10.0.0.3/32
PersistentKeepalive = 25

[Peer]
PublicKey = <ServerD公钥>
Endpoint = X.X.X.D:51820
AllowedIPs = 10.0.0.4/32
PersistentKeepalive = 25
```

#### B

```bash
vim /etc/wireguard/wg0.conf
```

内容

```ini
[Interface]
PrivateKey = <ServerB私钥>
Address = 10.0.0.2/24
ListenPort = 51820

[Peer]
PublicKey = <ServerA公钥>
Endpoint = X.X.X.A:51820
AllowedIPs = 10.0.0.1/32
PersistentKeepalive = 25

[Peer]
PublicKey = <ServerC公钥>
Endpoint = X.X.X.C:51820
AllowedIPs = 10.0.0.3/32
PersistentKeepalive = 25

[Peer]
PublicKey = <ServerD公钥>
Endpoint = X.X.X.D:51820
AllowedIPs = 10.0.0.4/32
PersistentKeepalive = 25
```

#### C

```bash
vim /etc/wireguard/wg0.conf
```

内容

```ini
[Interface]
PrivateKey = <ServerC私钥>
Address = 10.0.0.3/24
ListenPort = 51820

[Peer]
PublicKey = <ServerA公钥>
Endpoint = X.X.X.A:51820
AllowedIPs = 10.0.0.1/32
PersistentKeepalive = 25

[Peer]
PublicKey = <ServerB公钥>
Endpoint = X.X.X.B:51820
AllowedIPs = 10.0.0.2/32
PersistentKeepalive = 25

[Peer]
PublicKey = <ServerD公钥>
Endpoint = X.X.X.D:51820
AllowedIPs = 10.0.0.4/32
PersistentKeepalive = 25
```

#### D

```bash
vim /etc/wireguard/wg0.conf
```

内容

```ini
[Interface]
PrivateKey = <ServerD私钥>
Address = 10.0.0.4/24
ListenPort = 51820

[Peer]
PublicKey = <ServerA公钥>
Endpoint = X.X.X.A:51820
AllowedIPs = 10.0.0.1/32
PersistentKeepalive = 25

[Peer]
PublicKey = <ServerB公钥>
Endpoint = X.X.X.B:51820
AllowedIPs = 10.0.0.2/32
PersistentKeepalive = 25

[Peer]
PublicKey = <ServerC公钥>
Endpoint = X.X.X.C:51820
AllowedIPs = 10.0.0.3/32
PersistentKeepalive = 25
```

### 2. 部分服务器处于NAT

| 服务器 | 公网 IP   | 内网 WireGuard IP |
|--------|-----------|------------------|
| E      | NAT       | 10.0.0.5         |

在本示例中通过A中继访问

#### A

```bash
vim /etc/wireguard/wg0.conf
```

内容

```ini
[Interface]
PrivateKey = <ServerA私钥>
Address = 10.0.0.1/24
ListenPort = 51820

# 允许 IP 转发
PostUp = sysctl -w net.ipv4.ip_forward=1
PostDown = sysctl -w net.ipv4.ip_forward=0

# B 服务器
[Peer]
PublicKey = <ServerB公钥>
Endpoint = X.X.X.B:51820
AllowedIPs = 10.0.0.2/32
PersistentKeepalive = 25

# C 服务器
[Peer]
PublicKey = <ServerC公钥>
Endpoint = X.X.X.C:51820
AllowedIPs = 10.0.0.3/32
PersistentKeepalive = 25

# D 服务器
[Peer]
PublicKey = <ServerD公钥>
Endpoint = X.X.X.D:51820
AllowedIPs = 10.0.0.4/32
PersistentKeepalive = 25

# E 服务器（无公网 IP，需要 A 作为中转）
[Peer]
PublicKey = <ServerE公钥>
AllowedIPs = 10.0.0.5/32
PersistentKeepalive = 25
```

#### E

```bash
vim /etc/wireguard/wg0.conf
```

内容

```ini
[Interface]
PrivateKey = <ServerE私钥>
Address = 10.0.0.5/24

# 连接 A（E 无公网 IP，必须主动连接）
[Peer]
PublicKey = <ServerA公钥>
Endpoint = X.X.X.A:51820
AllowedIPs = 10.0.0.0/24
PersistentKeepalive = 25
```

