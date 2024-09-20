---
title: "纯净安装Harbor"
date: 2024-09-21 01:02:02 +08:00
draft: false
categories: [Harbor]
tags: []
card: false
weight: 0
---

鉴于Harbor官方的安装无比混乱, 因此记录如何将有用的东西提取出来, 最终形成`data`目录, `config`目录, `docker-compose.yml`文件. 

- 修复因端口问题导致的`docker login`失败
- 删除log容器和网络
- 增加容器名称前缀

## 按照官方教程进行前期操作

下载执行官方配置脚本, 配置`harbor.yml`, 注意配置和记录端口, 密码和`data`目录

执行`install.sh`生成`data`目录, `common`目录和`docker-compose.yml`

## 修改目录位置

data目录最好一开始就填写正确位置, 这样后续就不需要修改, 如果需要更改则需要修改`docker-compose.yml`文件

config目录就在common目录中, 修改方法和data目录修改方法一致, 直接修改`docker-compose.yml`文件即可

## 更改log记录方式

官方将容器运行产生的log都存放在了文件里, 我不喜欢它占用主机1514端口, 且不需要记录log, 因此直接删除

log记录由下面这个容器提供, 直接删除

```yml
  log:
    image: goharbor/harbor-log:v2.11.1
    container_name: harbor-log
    restart: always
    cap_drop:
      - ALL
    cap_add:
      - CHOWN
      - DAC_OVERRIDE
      - SETGID
      - SETUID
    volumes:
      - /var/log/harbor/:/var/log/docker/:z
      - type: bind
        source: ./common/config/log/logrotate.conf
        target: /etc/logrotate.d/logrotate.conf
      - type: bind
        source: ./common/config/log/rsyslog_docker.conf
        target: /etc/rsyslog.d/rsyslog_docker.conf
    ports:
      - 127.0.0.1:1514:10514
    networks:
      - harbor
```

删除容器后, 还需要修改引用到此容器的容器

### 删除依赖

如果depends_on中只有log, 那就整个删除, 如果还有其他的, 则只删除log这行

```yml
    depends_on:
      - log
```

### 删除logging

将下面这个样式的直接删除

```yml
    logging:
      driver: "syslog"
      options:
        syslog-address: "tcp://localhost:1514"
        tag: "registryctl"
```

### 删除网络

由于删除log后, 网络不再需要, 因此可以删除, 也就是下面这个

```yml
    networks:
      - harbor
```

最后删除networks配置

```yml
networks:
  harbor:
    external: false
```

## 修复docker login端口

进入config目录执行如下命令, 查询引用到端口的地方(请将11191端口换成自己定义的端口)

```shell
root@xxx:xxxx/common/config# grep -rn "11191"
core/env:8:EXT_ENDPOINT=https://dockerhub.akvicor.com:11191
nginx/nginx.conf:147:      return 308 https://$host:11191$request_uri;
```

修改后应该是这样

```shell
core/env:8:EXT_ENDPOINT=https://dockerhub.akvicor.com
nginx/nginx.conf:147:      return 308 https://$host$request_uri;
```

## 删除默认的container_name

删除所有容器的container_name, 方便安装时统一添加前缀

## 安装

执行如下命令安装并添加前缀, 这样每个容器的名字都会有个dockerhub前缀, 这样就不会与其他容器冲突了

```shell
docker compose -p dockerhub up -d
```
