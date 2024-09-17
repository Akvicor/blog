---
title: "Music Player Control"
date: 2023-03-22 11:21:42 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

通过命令行控制软件的声音

## 安装

```shell
sudo apt install playerctl
```

## 使用

### 控制当前

```shell
playerctl play
playerctl pause
playerctl play-pause  # toggle
playerctl stop
playerctl next
playerctl previous
playerctl volume
```

### 控制指定软件

获取软件列表

```shell
playerctl status -l
```

控制指定软件，如网易云音乐

```shell
playerctl --player=netease-cloud-music play-pause
```


