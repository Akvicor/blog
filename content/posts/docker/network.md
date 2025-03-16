---
title: "修改Docker容器占用的IP段"
date: 2025-03-16 04:41:53 +08:00
draft: false
categories: [Docker]
tags: []
card: false
weight: 0
---

docker默认情况下会使用整个172.16.0.0/12网段, 很明显对于个人用户, docker占用了太多的内网IP, 容易与其他内网网段冲突

## 1. 停用所有镜像

```bash
# 停止
docker stop $(docker ps -aq)
# 删除
docker rm $(docker ps -aq)
```

## 2. 清楚已经分配的网络

```bash
docker network prune
```

## 3. 编辑配置文件

```bash
# 创建文件
vim /etc/docker/daemon.json
```

- 我将`172.18.0.1/16`分配给默认网段, 一般情况下`/24`也够用
- 我将`172.19.0.0/16`分配给docker默认的ip段分配器, `/26`限制每次分配的网段大小

```json
{
  "bip": "172.18.0.1/16",
  "default-address-pools": [
    {"base": "172.19.0.0/16", "size": 26}
  ]
}
```

## 4. 应用配置

```bash
systemctl daemon-reload
systemctl restart docker
```