---
title: "H5ai Install"
date: 2019-02-11T17:09:05Z
draft: false
categories: [H5AI]
tags: []
card: false
weight: 0
---

h5ai是一款功能强大 php 文件目录列表程序，由德国开发者 Lars Jung 主导开发，它提供多种文件目录列表呈现方式，支持多种主流 Web 服务器，例如 Nginx、Apache、Cherokee、Lighttpd 等，支持多国语言，可以使用本程序在线预览文本、图片、音频、视频等。

请注意，默认情况下，放到目录下的 .php 文件将会被直接执行，并不以文本显示。

<!--more-->

## web环境

首先需要搭建好 Web 服务器，例如 LNMP（Linux/Nginx/MySQL/Php）组合，本文直接以 LNMP 组合为例。

推荐使用 PHP 7 版本。

## 下载 h5ai 安装包

转至官网下载：[链接](https://larsjung.de/h5ai/)

### 设置好虚拟主机后，编辑虚拟主机配置文件：

`vim /usr/local/nginx/conf/vhost/your_domain.conf`

将 root 一行，改为：

`index index.html index.php /_h5ai/public/index.php;`

去除被禁用的 PHP 函数：

`vim /usr/local/php/etc/php.ini`

搜索 scandir、exec、passthru，将其从被禁用的函数中删除。

重启 web 服务器：

```
service php-fpm restart
service nginx reload
```

将要在网站上显示的目录和 \_h5ai 文件夹放在一起：

到目前为止，h5ai 可以正常使用了，但是我们可以开启 \_h5ai 全部功能。通过 `http(s)://your_domain/_h5ai/public/index.php` 可以查看 \_h5ai 的全部功能开启情况，默认密码是空的。

## 安装 FFmpeg

### debian 8：

编辑软件源文件：

`vim /etc/apt/sources.list`

添加四个软件源

```
deb http://www.deb-multimedia.org jessie main non-free
deb ftp://ftp.deb-multimedia.org jessie main non-free
deb http://www.deb-multimedia.org stable main non-free
deb ftp://ftp.deb-multimedia.org stable main non-free
```

更新软件源

`apt-get -y update`

安装 ffmpeg

`apt-get -y install ffmpeg`

### Ubuntu 16.04+：

直接通过命令安装：

`apt-get -y install ffmpeg`

**注意：**请转至 `http://www.ffmpeg.org/releases/ `查看最新的 FFmpeg 版本。

### 编译安装。

```
apt-get -y install ffmpeg
tar -zxvf ffmpeg-*.*.tar.gz
cd ffmpeg-*.*
./configure
make
make install
```

## 略缩图功能

图片：

将` _h5ai` 中，private 与 public 文件夹中的 cache 目录设置权限为 755。

EXIF：

通过 phpize 安装 PHP 的 exif 模块即可。

视频略缩图：

参考 2.1 安装 FFmpeg 即可。

## 安装 ImageMagick。

可使用如下命令：

Ubuntu/Debian:

`apt-get install -y imagemagick`

CentOS:

`yum install ImageMagick -y`

Shell tar、Shell zip和Shell du

参考 1.4 去除在 php.ini 中被禁用函数 exec与 passthru 即可。

另外去除禁用的 scandir 函数（如果有），不然会导致无法显示目录。

options.json 中的更多功能

位于 `_h5ai/private/conf` 目录下。

打包下载：
搜索 “`download`”

`127` 行，`enabled` 由 `false` 改为 `true`。

文件信息及二维码：

搜索 “`info`”

`185` 行，`enabled` 由 `false` 改为 `true`。

默认简体中文：

搜索 “`l10n`”


`202` 行，`enabled` 由 `false` 改为 `true`。

文件及文件夹多选：

搜索 “`select`”


`323` 行，`enabled` 由 `false` 改为 `true`。

默认密码：

首先生成自定义 `sha512` 密码：[http://md5hashing.net/hashing/sha512](http://md5hashing.net/hashing/sha512)


然后搜索 “`passhash`”，大概第 `10` 行，将其密码改成自己生成的。

上一张完整开启所有功能的截图：

![](https://img.akvicor.com/i/2024/09/17/66e9997b34b4b.png)

在目录头部或尾部显示 HTML 内容

在需要显示自定义 HTML 的目录下，添加 \_h5ai.headers.html 或 \_h5ai.footers.html。这个通常用于对该目录的一些说明。支持 HTML 标签。

