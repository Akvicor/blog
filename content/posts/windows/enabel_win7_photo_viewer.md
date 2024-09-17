---
title: "在Win10上使用Win7图片查看器"
date: 2019-02-11T17:58:31Z
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

适合喜欢Win7风格图片查看器的用户

![img](https://img.akvicor.com/i/2024/09/17/66e9a63ea895c.png)

<!--more-->

1. Win + R 快捷键调出“运行”对话框，输入“regedit”
2. 进入`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Photo Viewer\Capabilities\FileAssociations`
3. 在右侧窗口中点击右键，选择“新建 - 字符串值”，然后把新建的字符串值重命名为 .jpg ，数值数据设置为`PhotoViewer.FileAssoc.Tiff` 。
4. 然后按同样的方法新建名为 .jpeg .png .gif .bmp .pcx .tiff .ico 等常用图片格式的字符串值，数值数据均设置为 `PhotoViewer.FileAssoc.Tiff` 。

## 或者使用dism++

[Dism++](https://www.chuyu.me/en/index.html)

