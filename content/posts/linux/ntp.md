---
title: "搭建NTP服务器"
date: 2024-05-12 23:14:12 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

为其他设备提供NTP授时服务

```shell
# 安装NTP服务
apt-get install ntp
# 编辑配置文件, pool 后替换为想要的ntp地址
vim /etc/ntpsec/ntp.conf
# 重启NTP服务
systemctl status ntp
systemctl restart ntp
# 检查NTP同步功能
ntpq -p
# 开放UDP的123端口
```


```shell
# 检查与NTP服务器的连接, 使用IP可以跳过dns解析
ntpdate -q 13.113.25.87
```


```shell
# 设置NTP服务器
vim /etc/ntpsec/ntp.conf
```

