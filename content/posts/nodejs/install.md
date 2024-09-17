---
title: "Node.js Install"
date: 2019-02-10T17:00:21Z
draft: false
categories: [Node.js]
tags: []
card: false
weight: 0
---

## Centos-7

一共有四种方法安装

- 源码安装
- 使用已编译版本安装
- 使用EPEL安装
- 使用NVM安装

<!--more-->

### 源码安装

1. 下载源码（[官网查看最新版本链接](http://nodejs.org/download/)）

```shell
wget http://nodejs.org/dist/v0.10.30/node-v0.10.30.tar.gz
```

2. 解压源码

```shell
tar xzvf node-v* && cd node-v*
```

3. 安装必要的编译软件

```shell
sudo yum install gcc gcc-c++
```

4. 编译

```shell
./configure
make
```

5. 编译&安装

```shell
sudo make install
```

6. 查看版本（测试安装是否成功）

```shell
node --version
```

### 使用已编译版本安装

1. 下载已编译版本 [传送门](http://nodejs.org/download/)

```shell
cd ~
wget http://nodejs.org/dist/v11.7.0/node-v11.7.0-linux-x64.tar.gz
```

2. 解压

```shell
sudo tar --strip-components 1 -xzvf node-v* -C /usr/local
```

3. 老样子，测试安装

```shell
node --version
```

### 使用EPEL安装

1. 下载EPEL

```shell
sudo rpm -i http://download.fedoraproject.org/pub/epel/beta/7/x86_64/epel-release-7-0.2.noarch.rpm
```

2. 安装

```shell
sudo yum install nodejs
```

3. 测试安装

```shell
node --version
```

4. （可选）安装npm管理包

```shell
sudo yum install npm
```

### 通过NVM安装

NVM（Node version manager）顾名思义，就是Node.js的版本管理软件，可以轻松的在Node.js各个版本间切换，项目源码[GitHub](https://github.com/creationix/nvm)

1. 下载并安装NVM脚本

[https://github.com/nvm-sh/nvm#install--update-script](https://github.com/nvm-sh/nvm#install--update-script)

```shell
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
# 安装到指定目录
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | NVM_DIR="/akvicor/vivc/env/nvm" bash
```

2. 列出所需要的版本

```shell
nvm list-remote
```

返回结果如下

```shell
. . .
v0.10.29
v0.10.30
v0.11.0
v0.11.1
v0.11.2
v0.11.3
v0.11.4
v0.11.5
v0.11.6
v0.11.7
v0.11.8
v0.11.9
v0.11.10
v0.11.11
v0.11.12
v0.11.13
```

3.安装相应的版本

```shell
nvm install 14
nvm install 16
nvm install 20
nvm install v20.11.1
```

4.查看已安装的版本

```shell
$ nvm list       
->     v14.21.3
       v16.20.2
       v20.11.1
default -> 14 (-> v14.21.3)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v20.11.1) (default)
stable -> 20.11 (-> v20.11.1) (default)
lts/* -> lts/iron (-> v20.11.1)
lts/argon -> v4.9.1 (-> N/A)
lts/boron -> v6.17.1 (-> N/A)
lts/carbon -> v8.17.0 (-> N/A)
lts/dubnium -> v10.24.1 (-> N/A)
lts/erbium -> v12.22.12 (-> N/A)
lts/fermium -> v14.21.3
lts/gallium -> v16.20.2
lts/hydrogen -> v18.19.1 (-> N/A)
lts/iron -> v20.11.1
```

5.切换版本

```shell
nvm use 16
nvm use v20.11.1
```

6.设置默认版本

```shell
nvm alias default v20.11.1
```

### 配置

```shell
# 方便设置yarn版本
corepack enable
```
