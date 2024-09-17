---
title: "Mathtex"
date: 2023-02-27 08:05:42 +08:00
draft: false
categories: [Open Source]
tags: []
card: false
weight: 0
---

[https://www.mediawiki.org/wiki/MediaWiki_and_LaTeX_on_a_host_with_shell_access](https://www.mediawiki.org/wiki/MediaWiki_and_LaTeX_on_a_host_with_shell_access)

安装TexLive，使用 which latex 和 which dvipng 检查是否安装

## 按照注释中操作

```php
<?php
# Place this file in extension directory as Mtag.php
# Add the following line to LocalSettings.php:
# include './extensions/Mtag.php';
# Mediawiki will render as LaTeX the code within <m> </m> tags.

$wgExtensionFunctions[] = "wfMtag";

function wfMtag() {
    global $wgParser;
    $wgParser->setHook( "m", "returnMtagged" );
}

function returnMtagged( $code, $argv) {
# if you have mathtex.cgi installed:
    $txt='<img src="/cgi-bin/mathtex.cgi?'.$code.'">';
# OR if you want to temporarily test a public mathtex.cgi:
#    $txt='<img src="http://www.forkosh.com/mathtex.cgi?'.$code.'">';

    return $txt;
}
?>
```

## 下载并解压 Mathtex.zip

[https://via.akvicor.com/file?f=1241](https://via.akvicor.com/file?f=1241)

```shell
# 编译
cc mathtex.c -DLATEX=\"$(which latex)\"  -DDVIPNG=\"$(which dvipng)\" -o mathtex.cgi
# 移动cgi文件
mv mathtex.cgi /var/www/mediawiki/cgi-bin
cd /var/www/mediawiki/cgi-bin
# 更改权限
chmod 775 mathtex.cgi
chown caddy:caddy mathtex.cgi
# 测试
./mathtex.cgi "x^2+y^2" –m 9 –o test
```

## 配置于wiki

LocalSettings.php

```php
include './extensions/Mtag.php';
$wgTrustedMathMimetexUrl = 'https://wiki.akvicor.com/cgi-bin/mathtex.cgi?';
```

