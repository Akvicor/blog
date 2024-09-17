---
title: ".ssh目录说明及密钥"
date: 2019-05-16T18:43:09Z
draft: false
categories: [SSH]
tags: []
card: false
weight: 0
---

SSH是Secure Shell的缩写，`.ssh`目录在用户根目录下

<!--more-->

## 权限

因为sshd为了安全，对属主的目录和文件权限有所要求。如果权限不对，则ssh的免密码登陆不生效。
用户目录权限为 755 或者 700，就是不能是77x、777，需要保障other用户不能有w权限
`.ssh`目录权限一般为`755`或者`700`。
`rsa_id.pub` 及`authorized_keys`权限一般为`644`
`rsa_id权`限必须为`600`

## SSH Key

1. 查看密钥是否存在

```bash
cd ~/.ssh
```

如果没有密钥则不会有此文件夹，有则备份删除,   也可以直接删除

2. 生成新密钥

```bash
ssh-keygen -t rsa -C "youremail@example.com"
```

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，因为这个Key仅仅用于简单的服务，所以也无需设置密码。

如果服务器端需要公钥,  直接把.ssh目录下的id_rsa.pub配置即可, id_rsa为私钥一定要保密!!!!

