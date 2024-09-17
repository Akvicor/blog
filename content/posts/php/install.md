---
title: "PHP Install"
date: 2019-11-06 21:25:26 +08:00
draft: false
categories: [PHP]
tags: []
card: false
weight: 0
---

## Ubuntu 18.04

安装PHP7并配置Nginx

### 安装

```bash
sudo apt-get install software-properties-common python-software-properties
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get -y install php7.3

# 安装常用扩展
sudo -y apt-get install php7.3-fpm php7.3-mysql php7.3-curl php7.3-json php7.3-mbstring php7.3-xml php7.3-intl

# 安装其他扩展（按需安装）
sudo apt-get install php7.3-gd
sudo apt-get install php7.3-soap
sudo apt-get install php7.3-gmp
sudo apt-get install php7.3-odbc
sudo apt-get install php7.3-pspell
sudo apt-get install php7.3-bcmath
sudo apt-get install php7.3-enchant
sudo apt-get install php7.3-ldap
sudo apt-get install php7.3-opcache
sudo apt-get install php7.3-readline
sudo apt-get install php7.3-sqlite3
sudo apt-get install php7.3-xmlrpc
sudo apt-get install php7.3-bz2
sudo apt-get install php7.3-interbase
sudo apt-get install php7.3-pgsql
sudo apt-get install php7.3-recode
sudo apt-get install php7.3-sybase
sudo apt-get install php7.3-xsl
sudo apt-get install php7.3-cgi
sudo apt-get install php7.3-dba
sudo apt-get install php7.3-phpdbg
sudo apt-get install php7.3-snmp
sudo apt-get install php7.3-tidy
sudo apt-get install php7.3-zip
```

### 配置Nginx+PHP

```
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

## 编译安装PHP7

### 下载源码

[PHP](https://www.php.net/downloads.php)

```bash
wget -c https://www.php.net/distributions/php-7.3.12.tar.gz
tar -zxvf php-7.3.12.tar.gz
cd php-7.3.12
```

### 安装需要的依赖

```bash
sudo apt-get install openssl
# configure: error: xml2-config not found. Please check your libxml2 installation.
sudo apt-get install libxml2-dev
# configure: error: Please reinstall the BZip2 distribution
sudo apt-get install libbz2-dev
# configure: error: Cannot find OpenSSL's libraries  or  ln -s /usr/lib/x86_64-linux-gnu/libssl.so /usr/lib
sudo apt-get install libcurl4-openssl-dev 
# libxml2 not found. Please check your libxml2 installation
apt-get install libxml2-dev
# configure: error: Please reinstall the libcurl distribution -easy.h should be in /include/curl/
sudo apt-get install libcurl4-gnutls-dev
# configure: error: jpeglib.h not found.
sudo apt-get install libjpeg-dev
# configure: error: png.h not found.
sudo apt-get install libpng-dev
# configure: error: libXpm.(a|so) not found.
sudo apt-get install libxpm-dev
# configure: error: freetype.h not found.
sudo apt-get install libfreetype6-dev
# configure: error: Your t1lib distribution is not installed correctly. Please reinstall it.
sudo apt-get install libt1-dev
# configure: error: mcrypt.h not found. Please reinstall libmcrypt.
sudo apt-get install libmcrypt-dev
# configure: error: Cannot find MySQL header files under yes.
# Note that the MySQL client library is not bundled anymore!
sudo apt-get install libmysql++-dev
# configure: error: xslt-config not found. Please reinstall the libxslt >= 1.1.0 distribution
sudo apt-get install libxslt1-dev
# configure: error: Please reinstall the libzip distribution
sudo apt-get install libzip-dev
# configure: error: DBA: Could not find necessary header file(s).
sudo apt-get install libgdbm-dev
# configure: error: Cannot find OpenSSL's <evp.h>
sudo apt-get install libssl-dev
```

### 编译配置

```bash
./configure --prefix=/usr/local/php \
--with-curl \
--with-freetype-dir \
--with-gd \
--with-gettext \
--with-iconv-dir \
--with-kerberos \
--with-libdir=lib64 \
--with-libxml-dir \
--with-mysqli \
--with-openssl \
--with-pcre-regex \
--with-pdo-mysql \
--with-pdo-sqlite \
--with-pear \
--with-png-dir \
--with-jpeg-dir \
--with-xmlrpc \
--with-xsl \
--with-zlib \
--with-bz2 \
--with-mhash \
--enable-fpm \
--enable-bcmath \
--enable-libxml \
--enable-inline-optimization \
--enable-mbregex \
--enable-mbstring \
--enable-opcache \
--enable-pcntl \
--enable-shmop \
--enable-soap \
--enable-sockets \
--enable-sysvsem \
--enable-sysvshm \
--enable-xml \
--enable-zip
```

编译成功显示：

```
| License:                                                           |
| This software is subject to the PHP License, available in this     |
| distribution in the file LICENSE.  By continuing this installation |
| process, you are bound by the terms of this license agreement.     |
| If you do not agree with the terms of this license, you must abort |
| the installation process at this point.                            |
+--------------------------------------------------------------------+

Thank you for using PHP.

config.status: creating php7.spec
config.status: creating main/build-defs.h
config.status: creating scripts/phpize
config.status: creating scripts/man1/phpize.1
config.status: creating scripts/php-config
config.status: creating scripts/man1/php-config.1
config.status: creating sapi/cli/php.1
config.status: creating sapi/fpm/php-fpm.conf
config.status: creating sapi/fpm/www.conf
config.status: creating sapi/fpm/init.d.php-fpm
config.status: creating sapi/fpm/php-fpm.service
config.status: creating sapi/fpm/php-fpm.8
config.status: creating sapi/fpm/status.html
config.status: creating sapi/phpdbg/phpdbg.1
config.status: creating sapi/cgi/php-cgi.1
config.status: creating ext/phar/phar.1
config.status: creating ext/phar/phar.phar.1
config.status: creating main/php_config.h
config.status: main/php_config.h is unchanged
config.status: executing default commands
```

### 编译并安装

```bash
make && make install
```

### 配置相应文件

```bash
cp php.ini-development /usr/local/php/lib/php.ini 
cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
cp sapi/fpm/php-fpm /usr/local/bin
cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf
```

然后设置php.ini，使用：`vim /usr/local/php/lib/php.ini` 打开php配置文件找到 `cgi.fix_pathinfo` 配置项，这一项默认被注释并且值为1，根据官方文档的说明，这里为了当文件不存在时，阻止Nginx将请求发送到后端的PHP-FPM模块，从而避免恶意脚本注入的攻击，所以此项应该去掉注释并设置为0，设置完毕保存并且退出。

### 创建web用户

```bash
groupadd www-data
useradd-g www-data www-data

// 打开www.conf文件，配置用户权限
vim /usr/local/php/etc/php-fpm.d/www.conf
默认user和group的设置为nobody，将其改为www-data
```

### 启动php-fpm服务

```bash
/usr/local/bin/php-fpm
```

php-fpm服务默认使用9000端口，使用 `netstat -tln |grep 9000` 可以查看端口使用情况，9000端口正常使用，说明php-fpm服务启动成功。

### 配置mginx
打开nginx.conf文件
首先找到user 设置为：user www-data www-data;
然后在service{}层中 添加php location段

```
       location / {
              root  /var/www/test;
              index index.php index.html; # 添加index.php
        }


        location ~ \.php$ {
           root           /var/www/test; # 项目文件根目录
           fastcgi_pass   127.0.0.1:9000; # 对应的php-fpm地址和端口
           fastcgi_index  index.php; # 默认访问文件
           fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name; # 将 /scripts改为$document_root
           include        fastcgi_params;
        }

```

修改完之后保存nginx.conf文件 并且重启nginx

```bash
/usr/local/nginx/sbin/nginx -s reload
```

然后在项目路径中创建一个index.php文件，内容可以如下：

```php
<?php 
    echo phpinfo(); 
?>
```

然后打开浏览器输入对应的地址进行访问，看到输出页面，说明nginx和php都配置成功。
