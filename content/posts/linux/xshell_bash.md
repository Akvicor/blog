---
title: "xshell登录服务器提示符显示-bash-4.2#解决方法"
date: 2019-09-29 19:21:57 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

突然发现root登录的xshell的终端提示符显示的是-bash-4.2# 而不是root@主机名 + 路径的显示方式。搞了半天也不知道为什么出现这种情况。今天终于搞定这个问题， 

<!--more-->

原因是root在/root下面的几个配置文件丢失，丢失文件如下： 

```
.bash_profile 
.bashrc 
```

以上这些文件是每个用户都必备的文件。 

使用以下命令从主默认文件重新拷贝一份配置信息到/root目录下

```bash
cp /etc/skel/.bashrc /root/
cp /etc/skel/.bash_profile /root/
```

关掉终端，重新打开就可以了

