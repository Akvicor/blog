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

## 注意

- **Wireguard有加密但无混淆，特征较为明显，谨慎用于其他用途**
- **Wireguard使用UDP传输，部分云服务商限制了UDP会导致性能较差**
- 由于海外和国内网络互联较差, 使用了一个带优化线路的中转节点来中转国内和海外服务器, 降低连接延迟, 提高传输速度

## ~~特性~~

### ListenPort 监听和发送端口的问题

当给Interface配置ListenPort之后，这个端口既承担了监听端口的功能，又承担了对外发送端口的功能，这会导致一些问题

当我给家里的各个设备（经过NAT后由同一个公网IP发送）分配好IP, 并且每台设备ListenPort都相同。然后就出问题了

家里的设备互相能访问，那些配置了IP和端口的Peer也能直连访问，但是中转服务器和那些需要中转的服务器就访问不了了（有时候家里部分设备能访问，但我猜可能是他们提前把端口抢过来了）

然后通过`wg show`查看中转，发现他们连接到中转服务器的端口就是ListenPort监听的端口，也就是说他们**接收和发送都用的同一个UDP端口**，这可能会导致端口冲突

因此，要么NAT下设备**不配置ListenPort**, 要么**配置不同的ListenPort**

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
ufw allow from 10.0.0.0/16 to any comment wg0
```

## 测试

```bash
ping 10.0.0.1
ping 10.0.0.2
ping 10.0.0.100
ping 10.0.0.3
ping 10.0.0.4
```

## 配置文件

**/etc/wireguard/wg0.conf**

### 示例服务器

- ABC在公网
- DE在某个NAT下的内网（如家里的设备
- AB互相直连访问
- DE互相直连访问
- AB和DE互相通过C中转访问

| 服务器  | 公网/NAT IP      | 内网 WireGuard IP |
|--------|-----------------|--------------------|
| A      | X.X.X.A         | 10.0.0.1         |
| B      | X.X.X.B         | 10.0.0.2         |
| C      | X.X.X.C         | 10.0.0.100       |
| D      | X.X.X.D         | 10.0.0.3         |
| E      | X.X.X.E         | 10.0.0.4         |

### A - 公网服务器

```bash
vim /etc/wireguard/wg0.conf
```

- 直连C, 同时将`10.0.0.0/16`网段的请求转发到C
- 直连B
- 通过C中转连接DE

```ini
[Interface]
PrivateKey = <A>
Address = 10.0.0.1/16
ListenPort = 51820

#############
# center    #
#############

[Peer]
PublicKey = <C>
Endpoint = X.X.X.C:51820
AllowedIPs = 10.0.0.0/16
PersistentKeepalive = 15

#############
# direct    #
#############

[Peer]
PublicKey = <B>
Endpoint = X.X.X.B:51820
AllowedIPs = 10.0.0.2/32
PersistentKeepalive = 15
```

### B - 公网服务器

```bash
vim /etc/wireguard/wg0.conf
```

- 直连C, 同时将`10.0.0.0/16`网段的请求转发到C
- 直连A
- 通过C中转连接DE

```ini
[Interface]
PrivateKey = <B>
Address = 10.0.0.2/16
ListenPort = 51820

#############
# center    #
#############

[Peer]
PublicKey = <C>
Endpoint = X.X.X.C:51820
AllowedIPs = 10.0.0.0/16
PersistentKeepalive = 15

#############
# direct    #
#############

[Peer]
PublicKey = <A>
Endpoint = X.X.X.A:51820
AllowedIPs = 10.0.0.1/32
PersistentKeepalive = 15
```

### C - 公网服务器 - 中转节点

```bash
vim /etc/wireguard/wg0.conf
```

- 中转ABDE节点

```ini
#############
# center    #
#############

# C
[Interface]
PrivateKey = <C>
Address = 10.0.0.100/16
ListenPort = 51820

PostUp = sysctl -w net.ipv4.ip_forward=1; iptables -A FORWARD -i wg0 -j ACCEPT
PostDown = sysctl -w net.ipv4.ip_forward=0; iptables -D FORWARD -i wg0 -j ACCEPT

#############
# direct    #
#############

#############
# redir     #
#############

# A
[Peer]
PublicKey = <A>
AllowedIPs = 10.0.0.1/32

# B
[Peer]
PublicKey = <B>
AllowedIPs = 10.0.0.2/32

# D
[Peer]
PublicKey = <D>
AllowedIPs = 10.0.0.3/32

# E
[Peer]
PublicKey = <E>
AllowedIPs = 10.0.0.4/32
```

### D - NAT下服务器

```bash
vim /etc/wireguard/wg0.conf
```

- 直连C, 同时将`10.0.0.0/16`网段的请求转发到C
- 直连E
- 通过C中转连接AB

```ini
[Interface]
PrivateKey = <D>
Address = 10.0.0.3/16
ListenPort = 51820

#############
# center    #
#############

[Peer]
PublicKey = <C>
Endpoint = X.X.X.C:51820
AllowedIPs = 10.0.0.0/16
PersistentKeepalive = 15

#############
# direct    #
#############

[Peer]
PublicKey = <E>
Endpoint = X.X.X.E:51821
AllowedIPs = 10.0.0.4/32
PersistentKeepalive = 15
```

### E - NAT下服务器

```bash
vim /etc/wireguard/wg0.conf
```

- 直连C, 同时将`10.0.0.0/16`网段的请求转发到C
- 直连D
- 通过C中转连接AB

```ini
[Interface]
PrivateKey = <E>
Address = 10.0.0.4/16
ListenPort = 51821

#############
# center    #
#############

[Peer]
PublicKey = <C>
Endpoint = X.X.X.C:51820
AllowedIPs = 10.0.0.0/16
PersistentKeepalive = 15

#############
# direct    #
#############

[Peer]
PublicKey = <D>
Endpoint = X.X.X.D:51820
AllowedIPs = 10.0.0.3/32
PersistentKeepalive = 15
```

