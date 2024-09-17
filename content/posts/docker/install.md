---
title: "Docker Install"
date: 2023-02-14 04:56:40 +08:00
draft: false
categories: [Docker]
tags: []
card: false
weight: 0
---

## Debian

官方安装文档[https://docs.docker.com/engine/install/debian/](https://docs.docker.com/engine/install/debian/)

### Uninstall old versions

```shell
apt-get remove docker docker-engine docker.io containerd runc
```

### Set up the repository

```shell
# Add Docker's official GPG key:
apt-get update
apt-get install ca-certificates curl
install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  tee /etc/apt/sources.list.d/docker.list > /dev/null
apt-get update
```

### Install Docker Engine

```shell
apt-get update
apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### Portainer管理多台服务器的docker

```bash
vim /usr/lib/systemd/system/docker.service
#找到ExecStart这行 在后面加上-H tcp://0.0.0.0:2375  其它方式一会docker就挂了 而且重启无效 
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock -H tcp://0.0.0.0:2375
systemctl daemon-reload
systemctl restart docker
```

