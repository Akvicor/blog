---
title: "修复sftp无法正常工作"
date: 2019-02-11T17:42:16Z
draft: false
categories: [SSH]
tags: []
card: false
weight: 0
---

在日常使用过程中突然sftp无法连接到服务器

<!--more-->

将`/etc/ssh/sshd_config` 中的

`Subsystem sftp /usr/libexec/openssh/sftp-server`

改为

`Subsystem sftp internal-sftp`

重启 `sshd` 后，`sftp` 正常工作了。
