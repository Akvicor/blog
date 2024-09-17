---
title: "Pake: 打包网页为桌面端应用"
date: 2024-01-04 13:23:19 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

[https://github.com/tw93/Pake/tree/master](https://github.com/tw93/Pake/tree/master)

安装程序

```bash
npm install pake-cli -g 
```

安装依赖

```bash
sudo apt install libwebkit2gtk-4.0-dev \
    build-essential \
    curl \
    wget \
    file \
    libssl-dev \
    libgtk-3-dev \
    libayatana-appindicator3-dev \
    librsvg2-dev
# 安装rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

打包网站

```bash
# 打包为appimage可执行程序
pake https://ha.akvicor.com --name "ha" --targets appimage
```


