---
title: "OpenWrt管理后台使用Caddy代替uhttpd"
date: 2023-06-28 07:38:14 +08:00
draft: false
categories: [OpenWrt]
tags: []
card: false
weight: 0
---

## 暂停uhttpd服务

uhttpd服务占用了80端口，需要先暂停。使用ssh登录openwrt，执行下面命令暂停uhttpd服务：

```shell
/etc/init.d/uhttpd stop
/etc/init.d/uhttpd disable
```

## 下载caddy

额外功能模块选上`aksdb/caddy-cgi/v2`

下载后放到`/viry/bin`目录下，并重命名为caddy，并给予执行权限：

```shell
chmod +x /viry/bin/caddy
```

## 配置caddy

增加下面的配置，并放到`/etc/config/Caddyfile`文件中：

```
{
  order cgi last
}
:80 {
    @notcgi {
        not path /cgi-bin/*
        not path /
    }
    redir / /cgi-bin/luci
    file_server @notcgi {
        root /www
    }
    cgi /cgi-bin/luci*  /www/cgi-bin/luci {
        script_name /cgi-bin/luci
    }
}

https://op.akvicor.com {
  encode gzip
  tls /viry/cert/akvicor.com.crt /viry/cert/akvicor.com.key
  @notcgi {
    not path /cgi-bin/*
    not path /
  }
  redir / /cgi-bin/luci
  file_server @notcgi {
    root /www
  }
  cgi /cgi-bin/luci* /www/cgi-bin/luci {
    script_name /cgi-bin/luci
  }
}

```

## 增加启动脚本

增加自启动脚本，并保存到`/etc/init.d/caddy`中：

```shell
#!/bin/sh /etc/rc.common


START=99

SERVICE_USE_PID=1
SERVICE_WRITE_PID=1
SERVICE_DAEMONIZE=1

start() {
       service_start  /viry/bin/caddy run --config /etc/config/Caddyfile
}

stop() {
        service_stop /viry/bin/caddy
}
```

给予执行权限：

```shell
chmod +x /etc/init.d/caddy
```

## 运行

执行下面脚本运行caddy服务，并加入到自启动中：

```shell
/etc/init.d/caddy enable
/etc/init.d/caddy start
```

成功启动后，就可以正常访问后台了

