---
title: "开启SSH服务"
date: 2022-07-14 15:30:03 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## 安装SSH服务

```bash
apt-get install openssh-server
```

## 修改SSH配置文件

```bash
vim /etc/ssh/sshd_config
```

配置项

```text
# 监听端口
Port 22

# 监听地址
ListenAddress 0.0.0.0

# 是否允许root用户登录
PermitRootLogin yes

# 允许密码认证
PasswordAuthentication yes

# 允许密钥认证
PubkeyAuthentication yes

# 客户端保活
ClientAliveInterval 60
ClientAliveCountMax 3
```

## Message of the Day

内容放在`vim /etc/motd`，可以用 UpdateMotd 动态生成的内容，也可以直接使用landscape-common

```shell
apt-get install landscape-common
```

## 重启ssh服务

```bash
systemctl restart sshd
```


