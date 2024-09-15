---
title: "Caddy Install"
date: 2023-02-12 06:10:02 +08:00
draft: false
categories: [Caddy]
tags: []
card: false
weight: 0
---

官方安装文档[https://caddyserver.com/docs/install](https://caddyserver.com/docs/install)

# Debian, Ubuntu, Raspbian

```bash
apt install -y debian-keyring debian-archive-keyring apt-transport-https curl
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg
curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | tee /etc/apt/sources.list.d/caddy-stable.list
apt update
apt install caddy
```

## Compile

```shell
go install github.com/caddyserver/xcaddy/cmd/xcaddy@latest
~/go/bin/xcaddy build --with github.com/mholt/caddy-webdav --with github.com/aksdb/caddy-cgi/v2
```

## Module

### cgi

cgi模块：CGI能够让浏览者与服务器进行交互

在官网下载（或自行编译）可执行文件，替换掉bin目录下原有的caddy可执行文件 执行systemctl restart caddy

```
{
  order cgi last
}

akvicor.com {
  encode gzip
  tls pem key
  reverse_proxy  localhost:8080 {
    header_up Host {host}
    header_up X-Real-IP {remote}
    header_up X-Forwarded-For {remote}
    header_up X-Forwarded-Proto {scheme}
  }
}

akvicor.com {
    cgi /cgi-bin/mathtex.cgi /www/cgi-bin/mathtex.cgi
    root * /www/wiki/mediawiki
    encode gzip
    php_fastcgi unix//run/php-fpm/www.sock
    file_server
}
```


