---
title: "PPTP VPN"
date: 2023-07-22 09:30:37 +08:00
draft: false
categories: [OpenWrt]
tags: []
card: false
weight: 0
---


## 服务端（家庭宽带路由器架设

1. 设置PPT服务器IP为路由器IP `172.16.1.1`
2. 客户端分配的ip范围 `172.16.1.200-209`
3. 设置DNS `172.16.1.1`
4. 设置用户名和密码
5. 修改防火墙常规设置，**转发**由**拒绝**改为**接受**，否则客户端连接后无法通过服务端访问外部网络。


## 客户端

### 安卓

1. 服务器地址 `home.akvicor.com`
2. 选中 PPP encryption (MPPE)
3. 填写用户名和密码

**若无网络可以尝试设置以下选项**

方法2会导致本地ip范围以外的访问请求不再经过vpn

1. DNS servers `172.16.1.1`
2. Forwarding routes `172.16.1.0/24` （转发到VPN的目标ip地址范围

### windows

1. 服务器地址 `home.akvicor.com`
2. 选中 PPP encryption (MPPE)
3. 填写用户名和密码


**若无网络可以尝试设置以下方式**

此种方法会导致本地ip范围以外的访问请求不再经过vpn

打开控制面板的网络连接，找到VPN创建的网络适配器，右键打开属性，点击网络选项，点开IPv4属性，取消勾选在远程网络上使用默认网关.


