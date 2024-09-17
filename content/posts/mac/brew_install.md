---
title: "安装Brew"
date: 2019-04-28T19:32:14Z
draft: false
categories: [Mac]
tags: []
card: false
weight: 0
---

[Homebrew](https://link.jianshu.com?t=http%3A%2F%2Fbrew.sh%2Findex_zh-cn.html)是一款Mac OS上的软件包管理工具，通过它可以很方便的安装/卸载软件工具等，类似于Linux下的apt-get，node的npm等包管理工具。

Homebrew将工具安装在自己创建的/usr/local/Cellar目录下，并在/usr/local/bin建立这些工具的符号链接。

<!--more-->

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

命令详解：

1.先用 shell命令curl，将文件下载本地，文件名为 install，文件地址：https://raw.githubusercontent.com/Homebrew/install/master/install

2.执行 ruby -e 文件install。

但是ruby命令里面的内容，是下载github上的Homebrew库，但是这个下载是连接国外的网络的，所以比较慢，经常网络断导致安装错误。如果有vpn或者网络较好的情况鼓励使用这种方式，比较方便。

**关闭每次运行时的自动更新**

```bash
	# 临时关闭  在Terminal中运行
	# 永久关闭  在Terminal配置文件中加入下面这一行 .bash_profile
export HOMEBREW_NO_AUTO_UPDATE=true
```



