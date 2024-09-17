---
title: "LNMP安装"
date: 2019-07-09 23:23:26Z
draft: false
categories: [PHP]
tags: []
card: false
weight: 0
---

LNMP一键安装包是一个用Linux Shell编写的可以为CentOS/RHEL/Fedora/Aliyun/Amazon、Debian/Ubuntu/Raspbian/Deepin/Mint Linux VPS或独立主机安装LNMP(Nginx/MySQL/PHP)、LNMPA(Nginx/MySQL/PHP/Apache)、LAMP(Apache/MySQL/PHP)生产环境的Shell程序。

<!--more-->

## 系统需求:

- CentOS/RHEL/Fedora/Debian/Ubuntu/Raspbian/Deepin/Aliyun/Amazon/Mint Linux发行版
- 需要5GB以上硬盘剩余空间，MySQL 5.7,MariaDB 10至少9GB剩余空间
- 需要128MB以上内存(128MB小内存VPS,Xen需有SWAP,OpenVZ至少要有128MB以上的vSWAP或突发内存)，注意小内存请勿使用64位系统！
- 安装MySQL 5.6或5.7及MariaDB 10必须1G以上内存，更高版本至少要2G内存!。
- 安装PHP 7及以上版本必须1G以上内存!。
- VPS或服务器必须已经联网且必须设置的是网络源不能是光盘源，同时VPS/服务器DNS要正常！
- Linux下区分大小写，输入命令时请注意！
- 如有通过yum或apt-get安装的MySQL/MariaDB请自行备份数据等相关文件！
- CentOS 5,Debian 6及之前版本其官网已经结束支持无法使用！
- Ubuntu 18+,Debian 9+,Mint 19+,Deepin 15.7+及所有新的Linux发行版只能使用1.6进行安装！
- 低于PHP 7.1.*版本不支持Ubuntu 19+等非常新的Linux发行版！

## 安装步骤:

### 使用putty或类似的SSH工具登陆VPS或服务器；

登陆后运行：`screen -S lnmp`

如果提示screen: command not found 命令不存在可以执行：`yum install screen` 或 `apt-get install screen`安装，详细内容参考screen教程。

### 下载并安装LNMP一键安装包：

```shell
wget http://soft.vpser.net/lnmp/lnmp1.6.tar.gz -cO lnmp1.6.tar.gz && tar zxf lnmp1.6.tar.gz && cd lnmp1.6 && ./install.sh lnmp
```

如提示wget: command not found ，使用`yum install wget` 或 `apt-get install wget` 命令安装。

## PHP模块/扩展

安装前
安装前建议先执行 `/usr/local/php/bin/php -m` (此命令显示目前已经安装好的PHP模块)看一下，要安装的模块是否已安装。然后下载当前PHP版本的源码并解压。

大部分php扩展/模块的安装就是三个步骤，在源码目录下执行：

```shell
/usr/local/php/bin/phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make && make install
```

有些模块可能会稍微有差异，具体看模块的安装文件就可以。

本文以imap和exif模块为例，进入php源码目录下ext，里面会有大部分模块的源码，这里都是php自带模块，第三方模块的话需要自己找第三方模块的源码。

### 安装imap模块

1、安装imap模块前需要先安装imap所需的库：

CentOS ：`yum install libc-client-devel`

Debian：`apt-get install libc-client-dev`

2、首先进入php安装目录的ext目录

比如php的源码目录为：`/root/lnmp1.3-full/src/php-5.4.45/`

则执行：`cd /root/lnmp1.3-full/src/php-5.4.45/ext/` 一般安装完LNMP php源码都是自动删除了的，需要自己进入src目录下解压。

我们要安装imap模块，执行`cd imap/`

再执行 `/usr/local/php/bin/phpize` 会返回如下信息：

```
Configuring for:
PHP Api Version:         20041225
Zend Module Api No:      20060613
Zend Extension Api No:   220060519
```

再执行以下命令：

```shell
./configure --with-php-config=/usr/local/php/bin/php-config --with-kerberos --with-imap-ssl
make && make install
```

执行完返回：

```
Build complete.
Don't forget to run 'make test'.

Installing shared extensions:     /usr/local/php/lib/php/extensions/no-debug-non-zts-20060613/
```

表示已经成功，再修改`/usr/local/php/etc/php.ini`

查找：`extension_dir` 再下面一行添加上`extension = "imap.so"`

保存，执行`/etc/init.d/php-fpm restart` 重启。

### 安装exif模块

安装exif不需要另外安装库，所以省略掉了安装库的步骤。

比如php的源码目录为：`/root/lnmp1.3-full/src/php-5.4.45/`

则执行：`cd /root/lnmp1.3-full/src/php-5.4.45/ext/`

我们要安装exif模块，执行`cd exif/`

再执行 `/usr/local/php/bin/phpize` 会返回如下信息：

```
Configuring for:
PHP Api Version:         20041225
Zend Module Api No:      20060613
Zend Extension Api No:   220060519
```

再执行以下命令：

```shell
./configure --with-php-config=/usr/local/php/bin/php-config
make && make install
```

执行完返回：

```
Build complete.
Don't forget to run 'make test'.

Installing shared extensions:     /usr/local/php/lib/php/extensions/no-debug-non-zts-20060613/
```

表示已经成功，再修改`/usr/local/php/etc/php.ini`

查找：`extension = `再最后一个`extension= `后面添加上`extension = "exif.so"`

保存，执行`/etc/init.d/php-fpm restart` 重启。

**使用：**在`/home/wwwroot/`下面创建一个`exif.php`的文件，内容如下：

```php
<?php
$exif = read_exif_data ('IMG_0001.JPG');
while(list($k,$v)=each($exif)) {
    echo "$k: $v<br>\n";
}
?>
```

## 安装位置信息

##LNMP相关软件安装目录

```
Nginx 目录: /usr/local/nginx/
MySQL 目录 : /usr/local/mysql/
MySQL数据库所在目录：/usr/local/mysql/var/
MariaDB 目录 : /usr/local/mariadb/
MariaDB数据库所在目录：/usr/local/mariadb/var/
PHP目录 : /usr/local/php/
多PHP版本目录 : /usr/local/php5.5/ 其他版本前面5.5的版本号换成其他即可
PHPMyAdmin目录 : 0.9版本为/home/wwwroot/phpmyadmin/ 1.0及以后版本为 /home/wwwroot/default/phpmyadmin/ 强烈建议将此目录重命名为其不容易猜到的名字。phpmyadmin可自己从官网下载新版替换。
默认网站目录 : 0.9版本为 /home/wwwroot/ 1.0及以后版本为 /home/wwwroot/default/
Nginx日志目录：/home/wwwlogs/
/root/vhost.sh添加的虚拟主机配置文件所在目录：/usr/local/nginx/conf/vhost/
PureFtpd 目录：/usr/local/pureftpd/
PureFtpd web管理目录： 0.9版为/home/wwwroot/default/ftp/ 1.0版为 /home/wwwroot/default/ftp/
Proftpd 目录：/usr/local/proftpd/
Redis 目录：/usr/local/redis/
```

### LNMP相关配置文件位置

```
Nginx主配置(默认虚拟主机)文件：/usr/local/nginx/conf/nginx.conf
添加的虚拟主机配置文件：/usr/local/nginx/conf/vhost/域名.conf
MySQL配置文件：/etc/my.cnf
PHP配置文件：/usr/local/php/etc/php.ini
php-fpm配置文件：/usr/local/php/etc/php-fpm.conf
PureFtpd配置文件：/usr/local/pureftpd/pure-ftpd.conf 1.3及更高版本：/usr/local/pureftpd/etc/pure-ftpd.conf
PureFtpd MySQL配置文件：/usr/local/pureftpd/pureftpd-mysql.conf
Proftpd配置文件：/usr/local/proftpd/etc/proftpd.conf 1.2及之前版本为/usr/local/proftpd/proftpd.conf
Proftpd 用户配置文件：/usr/local/proftpd/etc/vhost/用户名.conf
Redis 配置文件：/usr/local/redis/etc/redis.conf
```

### LNMPA相关目录文件位置

```
Apache目录：/usr/local/apache/
Apache配置文件：/usr/local/apache/conf/httpd.conf
Apache虚拟主机配置文件目录：/usr/local/apache/conf/vhost/
Apache默认虚拟主机配置文件：/usr/local/apache/conf/extra/httpd-vhosts.conf
虚拟主机配置文件名称：/usr/local/apache/conf/vhost/域名.conf
```

## 安装PHP模块/拓展

安装前建议先执行 `/usr/local/php/bin/php -m` (此命令显示目前已经安装好的PHP模块)看一下，要安装的模块是否已安装。然后下载当前PHP版本的源码并解压。

大部分php扩展/模块的安装就是三个步骤，在源码目录下执行：

```bash
/usr/local/php/bin/phpize
./configure --with-php-config=/usr/local/php/bin/php-config
make && make install
```

有些模块可能会稍微有差异，具体看模块的安装文件就可以。

本文以imap和exif模块为例，进入php源码目录下ext，里面会有大部分模块的源码，这里都是php自带模块，第三方模块的话需要自己找第三方模块的源码。

### 安装imap模块

1、安装imap模块前需要先安装imap所需的库：

CentOS ：`yum install libc-client-devel`

Debian：`apt-get install libc-client-dev`

2、首先进入php安装目录的ext目录

比如php的源码目录为：`/root/lnmp1.3-full/src/php-5.4.45/`

则执行：`cd /root/lnmp1.3-full/src/php-5.4.45/ext/` 一般安装完LNMP php源码都是自动删除了的，需要自己进入src目录下解压。

我们要安装imap模块，执行`cd imap/`

再执行 `/usr/local/php/bin/phpize` 会返回如下信息：

```
Configuring for:
PHP Api Version:         20041225
Zend Module Api No:      20060613
Zend Extension Api No:   220060519
```

再执行以下命令：

```
[root@vpser imap]# ./configure --with-php-config=/usr/local/php/bin/php-config --with-kerberos --with-imap-ssl
[root@vpser imap]# make && make install
```

执行完返回：

```
Build complete.
Don't forget to run 'make test'.

Installing shared extensions:     /usr/local/php/lib/php/extensions/no-debug-non-zts-20060613/
```

表示已经成功，再修改 `/usr/local/php/etc/php.ini`

查找：`extension_dir` 再下面一行添加上 `extension = "imap.so"`

保存，执行 `/etc/init.d/php-fpm restart` 重启。

在浏览器里面输入http://ip/p.php，打开探针，安装IMAP模块前：

![image](https://lsky.kayuki.org/i/2023/08/05/64cded54a3574.jpg)

安装IMAP模块后：

![image](https://lsky.kayuki.org/i/2023/08/05/64cdedbaef887.jpg)

### 安装exif模块

安装exif不需要另外安装库，所以省略掉了安装库的步骤。

比如php的源码目录为：`/root/lnmp1.3-full/src/php-5.4.45/`

则执行：`cd /root/lnmp1.3-full/src/php-5.4.45/ext/`

我们要安装exif模块，执行 `cd exif/`

再执行 `/usr/local/php/bin/phpize` 会返回如下信息：

```
Configuring for:
PHP Api Version:         20041225
Zend Module Api No:      20060613
Zend Extension Api No:   220060519
```

再执行以下命令：

```
[root@vpser imap]# ./configure --with-php-config=/usr/local/php/bin/php-config
[root@vpser imap]# make && make install
```

执行完返回：

```
Build complete.
Don't forget to run 'make test'.

Installing shared extensions:     /usr/local/php/lib/php/extensions/no-debug-non-zts-20060613/
```

表示已经成功，再修改 `/usr/local/php/etc/php.ini`

查找：`extension = `再最后一个`extension= `后面添加上`extension = "exif.so"`

保存，执行`/etc/init.d/php-fpm restart` 重启。

在`/home/wwwroot/`下面创建一个`exif.php`的文件，内容如下：

```
<?php
$exif = read_exif_data ('IMG_0001.JPG');
while(list($k,$v)=each($exif)) {
echo "$k: $v<br>\n";
}
?>
```

其中IMG_0001.JPG为照片文件。

未安装exif模块前：

![image](https://lsky.kayuki.org/i/2023/08/05/64cdeea9d269d.jpg)

安装exif模块后：

![image](https://lsky.kayuki.org/i/2023/08/05/64cdeeb091098.jpg)

可以读出照片的exif信息。

安装其他模块也基本上都是这两种方式，当 `./configure --with-php-config=/usr/local/php/bin/php-config` 执行这个的时候是会检查系统上库是否安装上，如果没有安装上就会报错，按错误提示安装相关的库就行。

