---
title: "添加重定向"
date: 2019-02-11T17:05:18Z
draft: false
categories: [Apache]
tags: []
card: false
weight: 0
---


给使用Appache网站添加重定向

在网站根目录下建立`.htaccess`文件

<!--more-->

开启rewrite
```
RewriteEngine On
```
默认跳转https
```
RewriteCond %{SERVER_PORT} !^443$
RewriteRule ^(.*)?$ https://%{SERVER_NAME}/$1 [L,R]
```
隐藏`.php`后缀
```
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^([^.]+)$ $1.php [NC,L]
```
隐藏`.html`后缀
```
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^([^.]+)$ $1.html [NC,L]
```