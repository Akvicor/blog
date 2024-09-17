---
title: "开启vps服务器的ssh密码登录"
date: 2019-02-11T17:33:07Z
draft: false
categories: [SSH]
tags: []
card: false
weight: 0
---

由于部分服务器提供商出于安全考虑默认关闭了服务器的ssh密码登录，例如Google Cloud Platform

<!--more-->

1. **登录服务器**

使用网页登录或手机客户端连接到服务器，或者使用key登录

2. **切换到root用户**

```bash
# 执行
sudo su
# 编辑文件
vi /etc/ssh/sshd_config
```

找到以下部分

```bash
# Authentication:
LoginGraceTime 120
PermitRootLogin yes //默认为no，需要开启root用户访问改为yes
StrictModes yes
# Change to no to disable tunnelled clear text passwords
PasswordAuthentication yes //默认为no，改为yes开启密码登陆
```

重启SSH服务使修改生效

`/etc/init.d/ssh restart`


