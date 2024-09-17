---
title: "添加评论功能"
date: 2024-03-02 17:48:55 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

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
        	{{- partial "partials/comment.html" . -}}
    	{{ end }}
```

在`layouts/partials`下添加`comment.html`文件


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

