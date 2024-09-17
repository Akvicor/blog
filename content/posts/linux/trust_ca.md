---
title: "Trust CA certificates"
date: 2023-01-26 13:42:28 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## Debian

```bash
# Debian/Ubuntu/Gentoo
# - 安装
sudo cp root_ca.crt /usr/local/share/ca-certificates/root_ca.crt
# update-ca-certificates 会添加 /etc/ca-certificates.conf 配置文件中指定的证书
#   另外所有 /usr/local/share/ca-certificates/*.crt 会被列为隐式信任
sudo update-ca-certificates

# - 删除
sudo rm /usr/local/share/ca-certificates/root_ca.crt
sudo update-ca-certificates --fresh
```

## RHEL

```bash
# CentOS/Fedora/RHEL
yum install ca-certificates
# 启用动态 CA 配置功能：
update-ca-trust force-enable
cp root_ca.crt /etc/pki/ca-trust/source/anchors/
update-ca-trust
```

## MacOS

```bash
# 安装 root_ca.crt
sudo security add-trusted-cert -d -r trustRoot -k /Library/Keychains/System.keychain ~/root_ca.crt

# 删除指定的证书
sudo security delete-certificate -c "<name of existing certificate>"
```

## Windows


1. 在根证书文件点鼠标右键，选择“安装证书”
2. 选择“当前用户”或者“本地计算机”，下一步
3. “将所有的证书都放入下列存储”，“浏览”，“受信任的根证书颁发机构”，“确定”，下一步
4. 完成，“是”，确定



