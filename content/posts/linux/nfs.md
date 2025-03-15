---
title: "NFS 挂载文件系统"
date: 2025-03-16 00:57:15 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

NFS是一种没有加密的高性能的远程挂载工具, 如果在局域网中可以直接使用, 如果在公网环境下, 可以搭配[Wireguard](https://blog.akvicor.com/posts/linux/wireguard/)使用

## 安装

### NFS服务器

假设IP: 10.0.0.4

```bash
apt install -y nfs-kernel-server
```

### 客户端

假设IP: 10.0.0.2

```bash
apt install -y nfs-common
```

## 创建共享

编辑配置文件

```bash
vim /etc/exports
```

假设`10.0.0.2`是允许是访问的IP(白名单IP)

```ini
/storage 10.0.0.2(rw,sync,no_subtree_check,no_root_squash)
```

应用配置

```bash
exportfs -ra
systemctl restart nfs-server
```

## 挂载

请注意防火墙是否允许, 且NFS传输是未加密的, 如果需要加密可以搭建Wireguard

```bash
mount -t nfs -o proto=tcp 10.0.0.4:/storage /mnt/storage
```

## 配置开机自动挂载

```ini
10.0.0.4:/storage /mnt/storage nfs defaults,_netdev,proto=tcp 0 0
```

