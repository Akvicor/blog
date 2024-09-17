---
title: "Hugo Install"
date: 2022-07-17 10:38:41 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

## GitHub

[https://github.com/gohugoio/hugo.git](https://github.com/gohugoio/hugo.git)

## Build

1. 使用官方编译版本
2. 使用源码编译

[http://pi.akvicor.com:7021/?p=199](http://pi.akvicor.com:7021/?p=199)

```bash
wget http://pi.akvicor.com:7021/file?f=1233 -O hugo_build.tgz
tar -zxvf hugo_build.tgz
cd hugo
./build.sh
```

## Install

```bash
mv gopath/bin/hugo /usr/bin/hugo
hugo version
```

## Usage

```bash
# 版本和环境详细信息
hugo env

# 创建新站点
hugo new site siteName

# 创建文章
hugo new index.md

# 编译生成静态文件并输出到public目录
hugo

# 编译生成静态文件并启动web服务
hugo server
```

## 主题

[https://github.com/athul/archie](https://github.com/athul/archie)

```bash
mkdir themes
cd themes
git clone https://github.com/athul/archie.git
```

## 配置

```toml
baseURL = "https://blog.akvicor.com/"
languageCode = "en-us"
title = "Akvicor's Blog"
theme = "archie"
copyright = "© Akvicor"
# Code Highlight
pygmentsstyle = "monokai"
pygmentscodefences = true
pygmentscodefencesguesssyntax = true
paginate = 7 # articles per page

[params]
	favicon = "https://paste.akvicor.com/file/1657810677_8674665223082153551_1656915732559.jpg"
	mode = "toggle" # color-mode → light,dark,toggle or auto
	useCDN = false # don't use CDNs for fonts and icons, instead serve them locally.
	subtitle = "{{</* kidding title=\"完全没理解\" text=\"太棒了,我逐渐理解一切\" */>}}"
	customcss = ["css/heimu.css"]

[[menu.main]]
name = "Home"
url = "/"
weight = 1

[[menu.main]]
name = "All posts"
url = "/post"
weight = 2

[[menu.main]]
name = "About"
url = "/about"
weight = 3

[[menu.main]]
name = "Tags"
url = "/tags"
weight = 4
```

