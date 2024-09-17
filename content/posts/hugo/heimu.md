---
title: "Heimu"
date: 2022-07-29 19:06:40 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

# 编写

将`heimu.css`文件放在`themes/archie/assets/css/`文件夹中

```css
/* heimu.css */

.heimu, .heimu a, a .heimu, .heimu a.new {
    background-color: #252525;
    color: #252525;
    text-shadow: none;
}

.heimu:hover, .heimu:active,
.heimu:hover .heimu, .heimu:active .heimu {
    color: white !important;
}

.heimu:hover a, a:hover .heimu,
.heimu:active a, a:active .heimu {
    color: lightblue !important;
}

.heimu:hover .new, .heimu .new:hover, .new:hover .heimu,
.heimu:active .new, .heimu .new:active, .new:active .heimu {
    color: #BA0000 !important;
}
```

编写`heimu.html`放在`layouts/shortcodes/`文件夹中

```
<!-- layouts/shortcodes/heimu.html -->
<span class="heimu" title="{{ .Get "title" }}">{{ .Get "text" }}</span>
```

在layouts/partials/head.html中添加

```
    <link rel="stylesheet" href="/css/heimu.css">
```


# 使用

```
{{</* heimu title="完全没理解" text="太棒了,我逐渐理解一切" */>}}
```

{{< heimu title="完全没理解" text="太棒了,我逐渐理解一切" >}}
