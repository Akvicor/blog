---
title: "Hide And Seek"
date: 2023-08-05 08:37:20 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

# 编写

将js文件放在主题的static/js文件夹中，css文件放在主题的static/css文件夹中

在layouts/partials/head.html中添加

```
    <script src="/js/hide_and_seek.js"></script>
    <link rel="stylesheet" href="/css/hide_and_seek.css">
```

编写`hide_and_seek.html`放在`layouts/shortcodes/`文件夹中

```
<!-- layouts/shortcodes/hide_and_seek.html -->
<hide_and_seek>{{ .Inner }}</hide_and_seek>
```

# 使用

```
{{</*hide_and_seek*/>}}
普通信息
{{</*/hide_and_seek*/>}}

{{</*hide_and_seek*/>}}
<strong>Hello</strong> {{<arrow_right_colour>}}#7D3C98{{</arrow_right_colour>}}
{{</*/hide_and_seek*/>}}
```

隐藏的内容在两条分割线之间，若想查看请按照以下方法操作

- 电脑：`上上下下左右左右BABA` （上下左右请使用方向键
- 手机：`上上下下左右左右` （上下左右请使用触屏

------

{{<hide_and_seek>}}
普通信息
{{</hide_and_seek>}}

{{<hide_and_seek>}}
<strong>Hello</strong> {{<arrow_right_colour>}}#7D3C98{{</arrow_right_colour>}}
{{</hide_and_seek>}}

------


