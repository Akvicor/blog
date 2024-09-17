---
title: "通过proxychain在命令行实现魔法"
date: 2020-01-22 13:47:14 +08:00
draft: false
categories: [Proxy]
tags: []
card: false
weight: 0
---

在魔法环境下运行命令行命令，例如 `apt-get` `wget` `git` `curl`

<!--more-->

项目地址 : [https://github.com/haad/proxychains](https://github.com/haad/proxychains)

```bash
sudo apt-get install proxychains
# 配置文件：
vim /etc/proxychains.conf
# 修改最后一行，并保存
socks5 127.0.0.1 1080
```

使用

```bash
proxychains apt-get xxxx
proxychains wget xxxx
```

