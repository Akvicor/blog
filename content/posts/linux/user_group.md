---
title: "Linux User/Group"
date: 2022-08-09 13:13:57 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## 概念和相关文件

在 Linux 用户系统中存在两类组。第一类是主要用户组（主组），第二类是附加用户组（附属组）。

- 主组：也被称为primary group、first group或initial login group，用户的默认组，用户的gid所标识的组。
- 附属组：也被称为Secondary group或supplementary group，用户的附加组。

## 存储文件

- 用户帐户及相关信息都存储在`/etc/passwd`文件中，
- 用户组信息存储在`/etc/shadow`和`/etc/group`文件。

## id命令

通过`id`命令查看用户的主组和附属组

```bash
id root
 # uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel)
id gg
 # uid=503(gg) gid=503(gg) groups=503(gg)
id mm
 # uid=502(mm) gid=500(jww) groups=500(jww)
```

gid标识主组，groups表示用户所属的全部组（主组和附属组）

用户必须有且只能有一个主组，可以有0个、1个或多个附属组

## 新增一个用户并添加到指定用户组

```bash
#检查用户组是否存在，如果组存在则会输出组信息，否则没有任何输出
grep <用户组名称> /etc/group
#如果用户组不存在则使用如下命令新建用户组：
groupadd <用户组名称>
 
#新建用户并将其加入指定用户组,作为其主用户组（每个用户有且只有一个主用户组）
useradd -g <用户组名称> <用户名称>
#或者 新建用户并将其加入指定附属用户组，附属用户组可以有多个，多个附属组名称用逗号分隔即可
useradd -G <用户组名称> <用户名称>
 
#设置用户密码
passwd <用户名称>
#查看用户属性，检查是否添加到正确的用户组
id <用户名称>
```

常用添加用户命令（添加用户并添加到主组）：`useradd -g <用户组名称> <用户名称>`

## 将已有用户添加到指定用户组

```bash
#将已有用户添加到指定用户组，作为其附属用户组
# -a 代表append，和 -G 一起使用，将用户添加到新用户组中而不必来开原有的其他用户组
usermod -a -G <用户组名称> <用户名称>
 
#将已有用户的主用户组改为新的用户组
usermod -g <新的用户组名称> <用户名称>
```

## 添加用户，并创建家目录、登录shell等信息

```bash
# -m 自动建立用户家目录
# -M 不自动创建家目录
# -d <dir> 使用指定目录作为家目录
# -g <gid> 指定用户所在的主组
# -U 自动创建同名主组
# -G <gid1>,<gid2> 指定用户所属的组
# -s <shell> 指定用户登录的shell
useradd -m -s /bin/bash <用户名称>
```

## 将一个用户从某个用户组删除

```bash
#将用户从该用户的附属组中删除
gpasswd -d <用户名称> <用户组名称>
```

## 删除用户

```bash
#永久性删除用户账号
userdel <用户名称>
```

