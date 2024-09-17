---
title: "搭建 Hoppscotch"
date: 2024-04-13 18:19:04 +08:00
draft: false
categories: [Open Source]
tags: []
card: false
weight: 0
---

官方安装教程 [https://docs.hoppscotch.io/documentation/self-host/community-edition/install-and-build](https://docs.hoppscotch.io/documentation/self-host/community-edition/install-and-build)

## Chrome 插件

[https://chromewebstore.google.com/detail/hoppscotch-browser-extens/amknoiejhlmhancpahfcfcfhllgkpbld](https://chromewebstore.google.com/detail/hoppscotch-browser-extens/amknoiejhlmhancpahfcfcfhllgkpbld)

安装后在插件中添加域名`https://post.akvicor.com`来启用插件

## 初始化数据库

在安装成功且访问之前, 需要先初始化数据库, 否则容器会崩溃(如果容器已经崩溃, 需要删除容器后重新安装)

```shell
# 进入aio或backend执行
pnpx prisma migrate deploy
```
