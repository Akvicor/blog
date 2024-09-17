---
title: "AlphaSSL"
date: 2023-01-29 08:16:20 +08:00
draft: false
categories: [SSL]
tags: []
card: false
weight: 0
---

已不可用

## SSL2BUY

### 生成CSR

```bash
openssl req -nodes -newkey rsa:2048 -keyout key -out csr
# C = US
# ST = California
# L = San Jose
# O = Kayuwki
# CN = *.kayuwki.com
# emailAddress = admin@kayuwki.com
```

### 补全并安装证书

由于 AlphaSSL 是中级证书商，因此需要把它的中级证书和签发给我的证书合并。

下载 [AlphaSSL](https://support.globalsign.com/ca-certificates/intermediate-certificates/alphassl-intermediate-certificates) 的名为`GlobalSign GCC R6 AlphaSSL CA 2023`的证书

格式如下：

```
-----BEGIN CERTIFICATE-----
你收到的邮件中提供的证书
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
官方提供的 AlphaSSL 中间证书
-----END CERTIFICATE-----
```

## 已失效

该方法基于 [萌咖](https://shop.moeclub.org/index.php) 提供的API。

### 生成 CSR

```bash
openssl req -nodes -newkey rsa:2048 -keyout kayuki.org.key -out kayuki.org.csr
# 域名填写 *.kayuki.org
```

使用openssl生成或使用此网址。**必须加上 `*.` 才可以申请到泛域名证书**

[https://api.moeclub.org/SSL/CSR](https://api.moeclub.org/SSL/CSR)

例如 `*.kayuki.org`

### 获取 Token

打开 [萌咖 杂货店](https://shop.moeclub.org/cart.php) 点击 AlphaSSL Apply Token 的 立即订购。

购买后获得Token

### 确保域名可以接受邮件

需要停用域名的所有CNAME解析，否则无法收到邮件

确保你要申请证书的域名有一个名为admin的邮件账号（如 admin@kayuki.org），若没有可以使用腾讯企业邮箱

### 申请证书

前往 [AlphaSSL](https://api.moeclub.org/SSL) 或 [AlphaSSL2](https://api.libmk.com/SSL) 输入我们在第一步生成的 CSR 和第二步购买的Token，点击 Get AlphaSSL!

### 确认邮件

登录 `admin@kayuki.org` 邮箱，一般在`1-30`分钟内会受到 AlphaSSL 发送的一封邮件，点击其中的链接完成申请。

随后 AlphaSSL 会再发来一封邮件，在这封邮件的底部便是我们申请的SSL证书（的一部分，需要补全）

### 补全并安装证书

由于 AlphaSSL 是中级证书商，因此需要把它的中级证书和签发给我的证书合并。

格式如下：

```
-----BEGIN CERTIFICATE-----
你收到的邮件中提供的证书
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
官方提供的 AlphaSSL 中间证书
-----END CERTIFICATE-----
```

[AlphaSSL 官方网站](https://support.globalsign.com/ca-certificates/intermediate-certificates/alphassl-intermediate-certificates)提供了他们的中级证书，注意有效期，最上面的那个是有效的

### 部署

上面的 key 文件 和 合并好的 crt 文件部署上服务器就可以了

