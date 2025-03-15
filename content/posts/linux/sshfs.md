---
title: "SSHFS"
date: 2025-03-16 01:14:41 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

远程挂载目录

## 安装

```bash
apt install sshfs
```

## 挂载

```bash
sshfs root@10.0.0.4:/storage /mnt/storage -o _netdev,reconnect,ServerAliveInterval=15,ServerAliveCountMax=3,IdentityFile=/root/.ssh/id_ed25519
```

## 开机自动挂载

```bash
# vim /etc/fstab
10.0.0.4:/storage /mnt/storage fuse.sshfs _netdev,reconnect,ServerAliveInterval=15,ServerAliveCountMax=3,IdentityFile=/root/.ssh/id_ed25519 0 0
```
