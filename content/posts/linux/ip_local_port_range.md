---
title: "Linux IP Local Port Range"
date: 2022-08-08 14:55:22 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

```bash
# 查看当前临时端口的范围
sysctl net.ipv4.ip_local_port_range
cat /proc/sys/net/ipv4/ip_local_port_range
# 暂时性修改临时端口的范围
echo 1024 65535 > /proc/sys/net/ipv4/ip_local_port_range
sysctl -w net.ipv4.ip_local_port_range="1024 64000"
# 永久性修改临时端口范围
vim /etc/sysctl.conf # 编辑此文件并添加下面一行内容
net.ipv4.ip_local_port_range = 1024 65535
```

