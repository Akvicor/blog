---
title: "MySQL Install"
date: 2019-11-06 20:44:19 +08:00
draft: false
categories: [MySQL]
tags: []
card: false
weight: 0
---

## Ubuntu 18.04

安装MySQL8

 - Ubuntu [下载 APT 包](https://dev.mysql.com/downloads/repo/apt/)
 - CentOS [下载 APT 包](https://dev.mysql.com/downloads/repo/yum/)

```bash
sudo dpkg -i mysql-apt-config_0.8.10-1_all.deb
sudo apt update
sudo apt install mysql-server
# 查看mysql是否安装成功
mysql -u root -p
# 查看mysql字符集，mysql8字符集默认为utf-8。
show variables like '%char%'; 
```


