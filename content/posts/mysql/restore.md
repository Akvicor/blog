---
title: "恢复MySQL数据库数据"
date: 2019-02-11T17:48:06Z
draft: false
categories: [MySQL]
tags: []
card: false
weight: 0
---

这种方法只有在数据文件ibd存在及知道表结构时才能使用

<!--more-->

连接mysql 切换到对应的数据库：

`use DatabaseName`

执行：

`alter table 表的名字 discard tablespace;`

复制表ibd文件

修改ibd文件权限 ：

`Chown -R mysql.mysql 表的名字.ibd`

执行：

`alter table 表的名字 import tablespace;`


