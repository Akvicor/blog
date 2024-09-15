---
title: "禁止某文件夹内运行PHP脚本、禁止访问文件或目录执行权限的设置方法"
date: 2019-09-29T16:15:07Z
draft: false
categories: [Apache]
tags: []
card: false
weight: 0
---

Apache禁止某文件夹内运行PHP脚本、禁止访问文件或目录执行权限的设置方法

<!--more-->

首先我们来看两段对上传目录设置无权限的列子，配置如下：

```
<Directory "要去掉PHP执行权限的目录路径，如/upload">
    ErrorDocument 404 /404/404.html
    ErrorDocument 403 /404/403.html
    <FilesMatch "\.(?i:php|php3|php4)$"> // ?是尽可能多的匹配.php的字符串,i是不区分大小写,然后冒号后面跟上正则表达式，也可以写成：<FilesMatch "\.(php|php3)$">
        Order allow,deny
        Deny from all
    </FilesMatch>
</Directory>
```

上面的意思就是说，`<Directory “要去掉PHP执行权限的目录路径，例如：/upload”>` 内目录路径下所有php文件不区分大小写，通过order,allow,deny 原则判断拒绝执行php文件，对nginx同样也是可应用。

另外一种方法，是设置在.htaccess里面的，这个方法比较灵活一点，针对那些没有apapche安全操作权限的网站管理员，推荐使用！
Apache环境规则内容如下：Apache限制uploads目录执行php脚本，把规则添加到.htaccess文件中，代码如下:

```
RewriteEngine on RewriteCond % !^$
RewriteRule uploads/(.*).(php)$ – [F]
```

此方法仅限于apache服务器环境，windows环境无效。

apache禁止访问某些文件/目录
增加Files选项来控制，比如要不允许访问 .inc 扩展名的文件，保护php类库：

```
<Files ~ "\.inc$">
    Order allow,deny
    Deny from all
</Files>
```

禁止访问某些指定的目录：（可以用 `<DirectoryMatch>` 来进行正则匹配）

```
<Directory ~ "^/var/www/(.+/)*[0-9]{3}">
    Order allow,deny
    Deny from all
</Directory>
```

通过文件匹配来进行禁止，比如禁止所有针对图片的访问：

```
<FilesMatch \.(?i:gif|jpe?g|png)$>
    Order allow,deny
    Deny from all
</FilesMatch>
```

针对URL相对路径的禁止访问：

```
<Location /dir/>
    Order allow,deny
    Deny from all
</Location>
```

