---
title: "Golang Install"
date: 2022-07-14 16:49:29 +08:00
draft: false
categories: [Golang]
tags: []
card: false
weight: 0
---

## 选择要下载的版本

[https://go.dev/dl/](https://go.dev/dl/)

## 下载安装

```bash
# 下载
wget https://go.dev/dl/go1.18.4.linux-amd64.tar.gz
# 解压
mv go1.18.4.linux-amd64.tar.gz go.tar.gz
tar -C /usr/local -xzf go.tar.gz
```

## 配置环境变量

```bash
# 编辑.bashrc
vim /root/.bashrc
# 添加如下内容
export GOPATH=/root/go
export PATH=/usr/local/go/bin:$PATH:$GOPATH/bin
# 设置代理
# Set the GOPROXY environment variable
export GOPROXY=https://goproxy.io,direct
# Set environment variable allow bypassing the proxy for specified repos (optional)
export GOPRIVATE=git.mycompany.com,github.com/my/private
```

## 使环境变量生效

```bash
source /root/.bashrc
```

## 安装完成

查看安装的go版本

```bash
go version
```


