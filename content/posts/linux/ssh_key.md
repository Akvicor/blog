---
title: "SSH 密钥"
date: 2024-08-17 21:11:12 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## Ed25519算法

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

## 旧算法

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

## 设置文件权限

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```
