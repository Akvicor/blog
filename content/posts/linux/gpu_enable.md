---
title: "Linux服务器开启GPU/Emby启用硬件解码"
date: 2025-03-03 21:51:20 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

Emby的硬件解码需要订阅才能使用


## 安装驱动

```shell
# 安装显卡驱动
apt install intel-media-va-driver-non-free
# 或开源
apt install intel-media-va-driver

# 如果上述没有生效, 安装这个(不确定这个起作用没有)
apt install i965-va-driver
```

## 配置grub

```shell
vim /etc/default/grub
```

查看 `GRUB_CMDLINE_LINUX` 中是否有 `nomodeset` 项, 如果有需要去掉. 去掉之后使grub生效

```shell
# 更新grub
update-grub
# 重启
reboot
```

**查看GPU使用率**

```shell
apt install intel-gpu-tools
```

用法

```shell
intel_gpu_top
```

## 配置Emby

注意emby需要有权限访问`/dev/dri`及其内部设备文件

```yml
services:
  vie:
    container_name: emby
    image: emby/embyserver
    restart: on-failure
    ports:
      - 8096:8096
    environment:
      - UID=0
      - GID=0
      - GIDLIST=0
    volumes:
      - /path/to/config:/config
      - /path/to/media:/mnt/media
    devices:
      - /dev/dri:/dev/dri # VAAPI/NVDEC/NVENC render nodes
#      - /dev/vchiq:/dev/vchiq # Raspberry pi

```