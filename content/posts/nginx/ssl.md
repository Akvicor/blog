---
title: "配置SSL证书"
date: 2019-11-07 21:10:28 +08:00
draft: false
categories: [Nginx]
tags: []
card: false
weight: 0
---

Nginx 配置SSL证书

<!--more-->

- `.crt`文件：是证书文件，crt是pem文件的扩展名。
- `.key`文件：证书的私钥文件（申请证书时如果没有选择自动创建CSR，则没有该文件）。

**友情提示：** `.pem`扩展名的证书文件采用Base64-encoded的PEM格式文本文件，可根据需要修改扩展名。

以Nginx标准配置为例，假如证书文件名是a.pem，私钥文件是a.key。

- 在Nginx的安装目录下创建cert目录，并且将下载的全部文件拷贝到cert目录中。如果申请证书时是自己创建的CSR文件，请将对应的私钥文件放到cert目录下并且命名为a.key；
- 打开 Nginx 安装目录下 conf 目录中的 nginx.conf 文件，将其修改为 (以下属性中ssl开头的属性与证书配置有直接关系，其它属性请结合自己的实际情况复制或调整) :

```
server {
    listen 443;
    server_name localhost;
    ssl on;
    root html;
    index index.html index.htm;
    ssl_certificate   cert/a.pem;
    ssl_certificate_key  cert/a.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    location / {
        root html;
        index index.html index.htm;
    }
}
```

添加完成后大概是这个样子

```
server{
    listen 443;

    root /var/www/akvicor.com/html;
    index index.php index.html index.htm index.nginx-debian.html;
 
    server_name akvicor.com www.akvicor.com;
    ssl on;

    ssl_certificate /var/www/cert/akvicor.pem;
    ssl_certificate_key /var/www/cert/akvicor.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    location ~ \.php$ {
        try_files $uri =404;

        include fastcgi.conf;
        fastcgi_pass 127.0.0.1:9000;
    }
}

server {
    listen 80;
    listen [::]:80;
 
    root /var/www/akvicor.com/html;
    index index.php index.html index.htm index.nginx-debian.html;
 
    server_name akvicor.com www.akvicor.com;

    location / {
        try_files $uri $uri/ /index.php$is_args$args;
    }

    location ~ \.php$ {
        try_files $uri =404;

        include fastcgi.conf;
        fastcgi_pass 127.0.0.1:9000;
    }
}
```

保存退出，重启 Nginx。

```bash
# 确保您的Nginx文件中没有语法错误
sudo nginx -t
# 重启Nginx
sudo systemctl restart nginx
```

