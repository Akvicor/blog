---
title: "暴力删除registry镜像文件"
date: 2024-01-29 13:03:30 +08:00
draft: false
categories: [Docker]
tags: []
card: false
weight: 0
---

暴力删除registry镜像文件

直接进入这个目录删除仓库 `/HHD4/docker/docker_hub/docker/registry/v2/repositories` 

进入docker实例执行垃圾回收 `/bin/registry garbage-collect /etc/docker/registry/config.yml`

重启registry

