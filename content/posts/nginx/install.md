---
title: "Nginx Install"
date: 2019-11-06 20:29:08 +08:00
draft: false
categories: [Nginx]
tags: []
card: false
weight: 0
---

## Ubuntu 18.04

搭建Nginx服务

### 安装

Ubuntu可以从源直接安装nginx

```
sudo apt-get update
sudo apt-get install nginx
```

### 调整防火墙，以免出现各种问题

```
sudo ufw app list
```

获得应用程序配置文件的列表：

```
可用应用程序：
CUPS
Nginx Full
Nginx HTTP
Nginx HTTPS
OpenSSH
```

正如你所看到的，Nginx有三个配置文件可用：`Nginx Full`、`Nginx HTTP`、`Nginx HTTPS`

- `Nginx Full` ：此配置文件打开端口80（正常，未加密的网络流量）和端口443（TLS / SSL加密流量）
- `Nginx HTTP` ：此配置文件仅打开端口80（正常，未加密的网络流量）
- `Nginx HTTPS` ：此配置文件仅打开端口443（TLS / SSL加密流量）

```
sudo ufw allow 'Nginx HTTP'
sudo ufw allow 'Nginx HTTPS'
```

输入以下命令以启动防火墙，据知有部分用户是没有启动防火墙的，还是建议开启

```bash
sudo ufw enable
```

输入以下命令以查看防火墙状态：

```bash
sudo ufw status
```

### 检查您的Web服务器是否在运行

```bash
sudo systemctl status nginx
```

### 检查是否可以访问默认网页，在浏览器输入：

```
http://本地IP地址
```

### 管理nginx进程

```bash
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
# 如果您只是简单地进行配置更改，Nginx通常可以重新加载而不会丢失连接。
sudo systemctl reload nginx
# 默认情况下，Nginx配置为在服务器引导时自动启动。 如果这不是您想要的，可以通过输入以下命令来禁用此行为：
sudo systemctl disable nginx
# 要重新启用服务以在启动时启动，您可以键入：sudo systemctl enable nginx
```

### 设置服务器块

Ubuntu 18.04上的`Nginx`默认启用了一个服务器模块，该模块被配置为在 `/var/www/html` 目录下提供文档。 虽然这适用于单个站点，但如果您托管多个站点，它可能会变得很笨重。 我们不必修改`/var/www/html` ，而是在`/var/www`为我们的`example.com`网站创建一个目录结构，并将`/var/www/html`保留为默认目录，如果客户端请求没有匹配任何其他网站。

按如下所示为`example.com`创建目录，使用-p标志创建任何必需的父目录：

```
sudo mkdir -p /var/www/example.com/html
```

接下来，使用$USER环境变量分配目录的所有权：

```
sudo chown -R $USER:$USER /var/www/example.com/html/
```

如果你没有修改你的umask值，你的web根目录的权限应该是正确的，但是你可以通过输入：

```
sudo chmod -R 755 /var/www/example.com/
```

接下来，使用Vim或您最喜欢的编辑器创建一个index.html页面示例：

```bash
vim /var/www/example.com/html/index.html
```

在里面，添加下面的示例HTML：

```html
<html>
    <head>
        <title>Welcome to Example.com!</title>
    </head>
    <body>
        <h1>Success!  The example.com server block is working!</h1>
    </body>
</html>
```

为了让`Nginx`提供这些内容，有必要创建一个具有正确指令的服务器块。 我们不要直接修改默认配置文件，而是在`/etc/nginx/sites-available/example.com`上创建一个新文件：

```bash
sudo vim /etc/nginx/sites-available/example.com
```

粘贴到以下配置块中，该块类似于默认值，但已更新为我们的新目录和域名：

```
server {
        listen 80;
        listen [::]:80;
 
        root /var/www/example.com/html;
        index index.html index.htm index.nginx-debian.html;
 
        server_name example.com www.example.com;
 
        location / {
                try_files $uri $uri/ =404;
        }
}
```

请注意，我们已将root配置更新到我们的新目录，并将`server_name`为我们的域名。 

接下来，让我们通过创建一个链接到启动sites-enabled目录来启用该文件，该目录是Nginx在启动过程中读取的：

```
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/
```

现在启用两个服务器模块并将其配置为基于`listen`和`server_name`指令响应请求（您可以阅读关于Nginx如何处理这些指令的更多信息）：

- example.com ：将响应example.com和www.example.com请求。
- default ：将响应端口80上与其他两个块不匹配的任何请求。

为了避免添加额外的服务器名称可能导致的哈希桶内存问题，有必要调整`/etc/nginx/nginx.conf`文件中的单个值。 

```
sudo vim /etc/nginx/nginx.conf
```

找到`server_names_hash_bucket_size`指令并删除#符号以取消注释该行：

```
...
http {
    ...
    server_names_hash_bucket_size 64;
    ...
}
...
```

接下来，测试以确保您的Nginx文件中没有语法错误：

```
sudo nginx -t

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

如果没有任何问题，请重新启动Nginx以启用您的更改：

```
sudo systemctl restart nginx
```

Nginx现在应该为您的域名提供服务。 你可以通过导航到`http://example.com`来测试它

### 重要的Nginx文件和目录

nginx服务器配置文件：

```bash
/etc/nginx # Nginx配置目录。 所有的Nginx配置文件都驻留在这里。
/etc/nginx/nginx.conf # 主要的Nginx配置文件。 这可以修改，以更改Nginx全局配置。
/etc/nginx/sites-available/ # 可存储每个站点服务器块的目录。 除非将Nginx链接到sites-enabled了sites-enabled目录，否则Nginx不会使用此目录中的配置文件。 通常，所有服务器块配置都在此目录中完成，然后通过链接到其他目录启用。
/etc/nginx/sites-enabled/ # 存储启用的每个站点服务器块的目录。 通常，这些是通过链接到sites-available目录中的配置文件创建的。
/etc/nginx/snippets # 这个目录包含可以包含在Nginx配置其他地方的配置片段。 可重复配置的片段可以重构为片段。
# nginx服务器日志文件：
/var/log/nginx/access.log # 除非Nginx配置为其他方式，否则每个对您的Web服务器的请求都会记录在此日志文件中。
/var/log/nginx/error.log # 任何Nginx错误都会记录在这个日志中。 
```

