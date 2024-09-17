---
title: "SSH/Git-连接远程主机错误"
date: 2019-02-10T16:47:57Z
draft: false
categories: [SSH,Git]
tags: []
card: false
weight: 0
---

在git bash链接远程主机时连接失败

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
51:82:00:1c:7e:6f:ac:ac:de:f1:53:08:1c:7d:55:68.
Please contact your system administrator.
Add correct host key in /Users/isaacalves/.ssh/known_hosts to get rid of this message.
Offending RSA key in /Users/isaacalves/.ssh/known_hosts:12
RSA host key for 104.131.16.158 has changed and you have requested strict checking.
Host key verification failed.
```

这种情况下并非是远程主机配置出现问题，而是曾经连接过一次此主机，而此次连接时缓存密钥不匹配

```bash
# 编辑此文件，删除缓存密钥即可
vim ~/.ssh/known_hosts
```
