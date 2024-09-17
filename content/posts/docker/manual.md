---
title: "Manual"
date: 2019-09-08T16:40:24Z
draft: false
categories: [Docker]
tags: []
card: false
weight: 0
---

## 上传镜像

```
docker images
docker login
docker tag hello-world etanqil/test
docker push etanqil/test
```

## 停止、删除所有的docker容器和镜像

```bash
# 列出所有的容器 ID
docker ps -aq
# 停止所有的容器
docker stop $(docker ps -aq)
# 删除所有的容器
docker rm $(docker ps -aq)
# 删除所有的镜像
docker rmi $(docker images -q)
```

现在的docker有了专门清理资源(container、image、网络)的命令。 docker 1.13 中增加了 docker system prune的命令，针对container、image可以使用 `docker container prune`、`docker image prune` 命令。

`docker image prune --force --all` 或者 `docker image prune -f -a` : #删除所有不使用的镜像

`docker container prune -f` : #删除所有停止的容器


