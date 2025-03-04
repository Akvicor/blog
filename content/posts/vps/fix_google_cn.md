---
title: "修复Google/Youtube送中"
date: 2025-03-02 14:45:23 +08:00
draft: false
categories: [VPS]
tags: []
card: false
weight: 0
---

如果IP被Google判定为中国, 可使用此方式修复, 视情况可能需要坚持很久

## 临时解决方案

访问 [google.com](https://google.com) 时，地址后加上ncr，即 [google.com/ncr](https://google.com/ncr) （ncr = no country redirect = 无国家/地区重定向）

在谷歌搜索设置里，SafeSearch选项选为Show explicit results

## 修复谷歌定位

用这个插件虚拟定位，在**谷歌搜索**最底部点击 **更新位置信息**

[https://chromewebstore.google.com/detail/location-guard/cfohepagpmnodfdmjliccbbigdkfcgia?pli=1](https://chromewebstore.google.com/detail/location-guard/cfohepagpmnodfdmjliccbbigdkfcgia?pli=1)

1. 关闭/取消登录了谷歌账户的APP定位权限/授权
2. 将常用的一个谷歌账号的位置记录功能打开
3. 在电脑上打开Chrome/谷歌浏览器，登录开了位置记录功能的谷歌账号，安装Location Guard拓展插件
4. 打开Location Guard插件，选择Fixed Location，并在给出的地图上单击，即可标记上你想要IP所处的国家/地区
5. 转换到Options选项，Default level默认设置为Use fixed location
6. 打开谷歌地图google.com/maps，点击右下角定位授权图标，使google maps获取当前“我的GPS位置”
7. 谷歌搜索my ip，即可看到谷歌IP定位到了刚才地图上标记的位置

## 提交归属地报告

[https://support.google.com/websearch/workflow/9308722](https://support.google.com/websearch/workflow/9308722)

