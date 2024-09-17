---
title: "Alsa"
date: 2023-02-15 10:46:31 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

声音控制

```shell
apt install alsa-tools
amixer set 'Master' unmute
```

## 更换默认card

找到对应card序号

```shell
cat /proc/asound/cards
```

创建此文件 `/etc/asound.conf` 添加以下内容

```shell
# 1 应改为对应card序号
defaults.pcm.card 1
defaults.ctl.card 1
```

## 查看cardID或名字


```shell
# 列出映射设备
aplay -l
# 查看card信息
amixer -c Generic_1 scontrols
amixer -c 1 scontrols
```