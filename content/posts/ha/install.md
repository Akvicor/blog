---
title: "Home Assistant Install"
date: 2023-12-12 10:21:21 +08:00
draft: false
categories: [Home Assistant]
tags: []
card: false
weight: 0
---

官方指导链接 [https://github.com/home-assistant/supervised-installer](https://github.com/home-assistant/supervised-installer)

## 安装基础工具

```bash
apt update
apt upgrade
apt install vim screen gcc g++ golang ca-certificates curl gnupg
```

## 安装Docker

```bash
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 安装 Home Assistant Supervised

运行于root用户下

```bash
# 安装依赖
apt install \
apparmor \
cifs-utils \
curl \
dbus \
jq \
libglib2.0-bin \
lsb-release \
network-manager \
nfs-common \
systemd-journal-remote \
systemd-resolved \
udisks2 \
wget -y
```

```bash
# 安装Docker
curl -fsSL get.docker.com | sh
```

```bash
# OS-Agent
# 教程 https://github.com/home-assistant/os-agent/tree/main#using-home-assistant-supervised-on-debian
# 下载 https://github.com/home-assistant/os-agent/releases/latest
sudo dpkg -i os-agent_1.0.0_linux_x86_64.deb
# 测试是否安装成功（没有报错表示成功
gdbus introspect --system --dest io.hass.os --object-path /io/hass/os
```

```bash
# 安装 Home Assistant Supervised
wget -O homeassistant-supervised.deb https://github.com/home-assistant/supervised-installer/releases/latest/download/homeassistant-supervised.deb
apt install ./homeassistant-supervised.deb
# 默认 $DATA_SHARE 是 /usr/share/hassio，可以通过以下命令更改
DATA_SHARE=/my/own/homeassistant dpkg --force-confdef --force-confold -i homeassistant-supervised.deb
# 如果报错可以通过以下命令查看报错信息
journalctl -f
```

```bash
# 安装HACS
# 在ha中安装SSH & Web Terminal并执行（注意修改默认22端口为随机临时端口
wget -O - https://get.hacs.xyz | bash -
```

