---
title: "添加评论功能 utterances和isso"
date: 2024-03-02 17:48:55 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

utterances和isso两种评论系统搭建

## isso

- 自建
- 数据存储在本地, 方便备份迁移
- 无需登陆
- 可配置审核等

### 搭建isso

```yml
services:
  isso:
    container_name: isso
    image: ghcr.io/isso-comments/isso:release
    ports:
      - "8080:8080"
    restart: always
    volumes:
      - "/app/isso/data/config:/config"
      - "/app/isso/data/db:/db"
```

配置: `/app/isso/data/config/isso.cfg`

```conf
# Isso example configuration file
# vim: set filetype=dosini

[general]

# Change dbpath to /db/comments.db if running in docker!
dbpath = /db/comments.db
host =
    https://isso.akvicor.com
    https://blog.akvicor.com
max-age = 5
notify = smtp
gravatar = true
gravatar-url = https://www.gravatar.com/avatar/{}?d=identicon&s=55

[admin]
enabled = true
password = abced

[guard]
# 开启 SPAM 保护。防止一些恶意攻击。比如限制每个钟的新评论个数。
enabled = true
# 每分钟内最多五个新评论
ratelimit = 5
direct-reply = 7
; 是否允许回复自己
reply-to-self = true
; 是否必须填写作者
require-author = false
; 是否必须填写邮箱
require-email = false

; 是否开启审核，true 是开启，30d 表示删除 30 天没有通过审核的评论
[moderation]
enabled = false
approve-if-email-previously-approved = false
purge-after = 30d

[smtp]
host = smtp.xxxx.com
port = 465
security = ssl
username = xxx@xxxx.com
password = xxxxxxx
to = akvicor@xxxx.com
from = "Akvicor's Blog" <xxx@xxxx.com>
timeout = 10

[rss]
base = https://isso.akvicor.com
limit = 100

```

### 配置博客

找到主题的`layouts/_default/single.html`文件

在`{{ define "main" }}`后面添加以下内容

```
{{ $IsNav := eq .Title "Nav"}}
{{ $IsSearch := eq .Title "Search"}}
{{ $IsArchive := eq .Title "Archive"}}
{{ $IsAbout := eq .Title "About"}}
{{ $IsSecrets := eq .Section "secrets"}}
<!-- 那些不需要文章相关功能（如目录层级，文章切换、字数时长统计等）的页面 -->
<!-- {{ $IsPurePage := or $IsNav $IsSearch $IsArchive $IsAbout }} -->
<!-- 正常博文 -->
{{ $IsPostPage := and (not $IsNav) (not $IsSearch) (not $IsArchive) (not $IsAbout) }}
{{ $IsCommentPage := and (not $IsNav) (not $IsSearch) (not $IsArchive) }}
{{ $IS_DEV_ENV := eq .Site.BaseURL "http://localhost:1313/"}}
```

在`<article>`内部的末尾添加

```
{{ if and .IsPage $IsPostPage (not $IsSecrets) .Site.Params.isso.active (not $IS_DEV_ENV) }}
    {{- partial "partials/_isso.html" . -}}
{{ end }}
```

在`layouts/partials`下添加`_isso.html`文件


```html
<div class="container-comment">
    <script data-isso="https://isso.akvicor.com"
    data-isso-css="true"
    data-isso-lang="en"
    data-isso-reply-notifications="true"
    data-isso-reply-to-self="true"
    data-isso-require-author="false"
    data-isso-require-email="false"
    data-isso-max-comments-top="10"
    data-isso-max-comments-nested="5"
    data-isso-reveal-on-click="5"
    data-isso-avatar="true"
    data-isso-gravatar="true"
    data-isso-vote="true"
    data-vote-levels=""
    src="https://isso.akvicor.com/js/embed.min.js"></script>
    <section id="isso-thread"></section>
</div>
```

在配置文件中添加

```toml
[params.isso]
    # 是否启用评论插件
    active = true
```

## utterances

- 借助Github保存评论
- 必须登陆Github

找到主题的`layouts/_default/single.html`文件

在`{{ define "main" }}`后面添加以下内容

```
{{ $IsNav := eq .Title "Nav"}}
{{ $IsSearch := eq .Title "Search"}}
{{ $IsArchive := eq .Title "Archive"}}
{{ $IsAbout := eq .Title "About"}}
{{ $IsSecrets := eq .Section "secrets"}}
<!-- 那些不需要文章相关功能（如目录层级，文章切换、字数时长统计等）的页面 -->
<!-- {{ $IsPurePage := or $IsNav $IsSearch $IsArchive $IsAbout }} -->
<!-- 正常博文 -->
{{ $IsPostPage := and (not $IsNav) (not $IsSearch) (not $IsArchive) (not $IsAbout) }}
{{ $IsCommentPage := and (not $IsNav) (not $IsSearch) (not $IsArchive) }}
{{ $IS_DEV_ENV := eq .Site.BaseURL "http://localhost:1313/"}}
```

在`<article>`内部的末尾添加

```
{{ if and .IsPage $IsCommentPage (not $IsSecrets) .Site.Params.utterances.active (not $IS_DEV_ENV) }}
    {{- partial "partials/_comment.html" . -}}
{{ end }}
```

在`layouts/partials`下添加`_comment.html`文件


```html
<div class="container-comment">
	<script src="https://utteranc.es/client.js"
        repo="{{.Site.Params.utterances.repo}}"
        issue-term="{{.Site.Params.utterances.issueTerm}}"
        theme="{{.Site.Params.utterances.theme}}"
        crossorigin="{{.Site.Params.utterances.crossorigin}}"
        async>
	</script>
</div>
```

在配置文件中添加

```toml
[params]
    # 在开发环境下（http://localhost:1313/），不再启用评论插件，
    # 如果想在开发环境下启用它，修改服务端口即可，如下
    # hugo server -p=1314
    [params.utterances]
        # 是否启用评论插件
        active = true
        # 输入你的仓库名称
        repo = "AkvicorEdwards/blog"
        issueTerm = "pathname"
        theme = "github-light"
        crossorigin = "anonymous"
```

