---
title: "Wiki Install"
date: 2024-02-20T14:07:44Z
draft: false
categories: [Wiki]
tags: []
card: false
weight: 0
---

## Special pages

- Interwiki [(more information)](https://www.mediawiki.org/wiki/Extension:Interwiki)

## Editors

- CodeEditor [(more information)](https://www.mediawiki.org/wiki/Extension:CodeEditor)
- WikiEditor [(more information)](https://www.mediawiki.org/wiki/Extension:WikiEditor)

## Parser hooks

- CategoryTree [(more information)](https://www.mediawiki.org/wiki/Extension:CategoryTree)
- Cite [(more information)](https://www.mediawiki.org/wiki/Extension:Cite)
- ImageMap [(more information)](https://www.mediawiki.org/wiki/Extension:ImageMap)
- InputBox [(more information)](https://www.mediawiki.org/wiki/Extension:InputBox)
- Math [(more information)](https://www.mediawiki.org/wiki/Extension:Math)
- ParserFunctions [(more information)](https://www.mediawiki.org/wiki/Extension:ParserFunctions)
- Poem [(more information)](https://www.mediawiki.org/wiki/Extension:Poem)
- Scribunto [(more information)](https://www.mediawiki.org/wiki/Extension:Scribunto)
- SyntaxHighlight_GeSHi [(more information)](https://www.mediawiki.org/wiki/Extension:SyntaxHighlight)
- TemplateData [(more information)](https://www.mediawiki.org/wiki/Extension:TemplateData)

## API

- PageImages [(more information)](https://www.mediawiki.org/wiki/Extension:PageImages)

## Other

- MultimediaViewer [(more information)](https://www.mediawiki.org/wiki/Extension:MultimediaViewer)
- OATHAuth [(more information)](https://www.mediawiki.org/wiki/Extension:OATHAuth)

## LocalSettings.php

### 网站图标

```php
$wgFavicon = "$wgResourceBasePath/resources/assets/snowflake_128.png";
```

### 短URL

在编译好的docker镜像中，已经配置好了`apache`,因此只要修改wiki的配置文件即可

```php
$wgScriptPath       = "";
$wgArticlePath      = "/$1";
$wgUsePathInfo      = true;
$wgScriptExtension  = ".php";
```

