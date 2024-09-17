---
title: "使用密钥登陆并禁用密码登录"
date: 2019-04-15T00:26:09Z
draft: false
categories: [SSH]
tags: []
card: false
weight: 0
---

使用密钥登陆相对于密码来说安全性要高很多

我们知道SSH登录是用的RSA非对称加密的，所以我们在SSH登录的时候就可以使用RSA密钥登录，SSH有专门创建SSH密钥的工具ssh-keygen，下面就来一睹风采。

<!--more-->

首先进入Linux系统的用户目录下的.ssh目录下，root用户是/root/.ssh，普通用户是/home/您的用户名/.ssh，我们以root用户为例：

```bash
cd /root/.ssh
```

## 生成密钥



```bash
ssh-keygen -t rsa -b 4096 -C "youremain@gmail.com"
```

这里加了-b 参数，指定了长度，也可以不加-b参数

-C 参数是注释



执行密钥生成命令，基本上是一路回车既可以了，但是需要注意的是：执行命令的过程中是会提示呢输入密钥的密码的（如下图中红色箭头处，输入两次相同的，即是又一次确认密码），不需要密码直接回车就行。



密钥生成后会在当前目录下多出两个文件，id_rsa和id_rsa.pub，其中id_rsa是私钥（敲黑板：这个很重要，不能外泄），id_rsa.pub这个是公钥，



进入远程服务器需要SSH登录的用户的目录下，这里仍然用root用户，cd /root/.ssh，执行ls看看目录下是否有authorized_keys文件没有的话则执行以下命令创建：



执行成功会创建空authorized_keys文件，授予600权限（注意：此处权限必须是600）：

```bash
chmod 600 /root/.ssh/authorized_keys
```

如果已经有了authorized_keys文件，这直接执行以下的密钥追加工作。

将上面生成的公钥id_rsa.pub追加到authorized_keys文件中：

```bash
cat /root/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
```

密钥准备好了接下来就可以使用密钥登录了，

```bash
ssh -i ./id_rsa root@192.168.100.39
# or
ssh root@192.168.100.39 -i ./id_rsa
```

注意：id_rsa是私钥，笔者这里是进入私钥的目录下操作的，如果没在私钥的目录下，请写全目录，比如/mnt/id_rsa，也可以是您自定义的目录。执行命令过程中，如果创建密钥对的时候设置了密码，则会提示您输入密码，没有的话会直接登录。



## 禁用服务器密码登录

- 编辑/etc/ssh/sshd_config
- 将PasswordAuthentication参数值修改为no： PasswordAuthentication no
- 重启ssh服务：systemctl restart sshd.service

## 参数

```
ssh-keygen可用的参数选项有：
 
     -a trials
             在使用 -T 对 DH-GEX 候选素数进行安全筛选时需要执行的基本测试数量。
 
     -B      显示指定的公钥/私钥文件的 bubblebabble 摘要。
 
     -b bits
             指定密钥长度。对于RSA密钥，最小要求768位，默认是2048位。DSA密钥必须恰好是1024位(FIPS 186-2 标准的要求)。
 
     -C comment
             提供一个新注释
 
     -c      要求修改私钥和公钥文件中的注释。本选项只支持 RSA1 密钥。
             程序将提示输入私钥文件名、密语(如果存在)、新注释。
 
     -D reader
             下载存储在智能卡 reader 里的 RSA 公钥。
 
     -e      读取OpenSSH的私钥或公钥文件，并以 RFC 4716 SSH 公钥文件格式在 stdout 上显示出来。
             该选项能够为多种商业版本的 SSH 输出密钥。
 
     -F hostname
             在 known_hosts 文件中搜索指定的 hostname ，并列出所有的匹配项。
             这个选项主要用于查找散列过的主机名/ip地址，还可以和 -H 选项联用打印找到的公钥的散列值。
 
     -f filename
             指定密钥文件名。
 
     -G output_file
             为 DH-GEX 产生候选素数。这些素数必须在使用之前使用 -T 选项进行安全筛选。
 
     -g      在使用 -r 打印指纹资源记录的时候使用通用的 DNS 格式。
 
     -H      对 known_hosts 文件进行散列计算。这将把文件中的所有主机名/ip地址替换为相应的散列值。
             原来文件的内容将会添加一个".old"后缀后保存。这些散列值只能被 ssh 和 sshd 使用。
             这个选项不会修改已经经过散列的主机名/ip地址，因此可以在部分公钥已经散列过的文件上安全使用。
 
     -i      读取未加密的SSH-2兼容的私钥/公钥文件，然后在 stdout 显示OpenSSH兼容的私钥/公钥。
             该选项主要用于从多种商业版本的SSH中导入密钥。
 
     -l      显示公钥文件的指纹数据。它也支持 RSA1 的私钥。
             对于RSA和DSA密钥，将会寻找对应的公钥文件，然后显示其指纹数据。
 
     -M memory
             指定在生成 DH-GEXS 候选素数的时候最大内存用量(MB)。
 
     -N new_passphrase
             提供一个新的密语。
 
     -P passphrase
             提供(旧)密语。
 
     -p      要求改变某私钥文件的密语而不重建私钥。程序将提示输入私钥文件名、原来的密语、以及两次输入新密语。
 
     -q      安静模式。用于在 /etc/rc 中创建新密钥的时候。
 
     -R hostname
             从 known_hosts 文件中删除所有属于 hostname 的密钥。
             这个选项主要用于删除经过散列的主机(参见 -H 选项)的密钥。
 
     -r hostname
             打印名为 hostname 的公钥文件的 SSHFP 指纹资源记录。
 
     -S start
             指定在生成 DH-GEX 候选模数时的起始点(16进制)。
 
     -T output_file
             测试 Diffie-Hellman group exchange 候选素数(由 -G 选项生成)的安全性。
 
     -t type
             指定要创建的密钥类型。可以使用："rsa1"(SSH-1) "rsa"(SSH-2) "dsa"(SSH-2)
 
     -U reader
             把现存的RSA私钥上传到智能卡 reader
 
     -v      详细模式。ssh-keygen 将会输出处理过程的详细调试信息。常用于调试模数的产生过程。
             重复使用多个 -v 选项将会增加信息的详细程度(最大3次)。
 
     -W generator
             指定在为 DH-GEX 测试候选模数时想要使用的 generator
 
     -y      读取OpenSSH专有格式的公钥文件，并将OpenSSH公钥显示在 stdout 上。

```

```
ssh-copy-id的参数有：
 
    -i #指定密钥文件
    -p #指定端口，默认端口号是22
    -o <ssh -o options>
    user@]hostname #用户名@主机名
    -f: force mode -- copy keys without trying to check if they are already installed
    -n: dry run    -- no keys are actually copied
    -h|-?: 显示帮助
```



