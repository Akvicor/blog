---
title: "Tag"
date: 2023-07-09 01:34:32 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

## 编写

编写`tag.html`或`tag_?.html`放在`layouts/shortcodes/`文件夹中

```
<!-- layouts/shortcodes/tag.html -->
<strong style= "background: {{ .Get "colour" }};border: 1px solid {{ .Get "colour" }};border-radius: 5px;">{{ .Get "tag" }}</strong>
```

```
<!-- layouts/shortcodes/tag_academic.html -->
<strong style= "background: #33BBFF;border: 1px solid #33BBFF;border-radius: 5px;">学术</strong>
```

## 使用

```
{{</* tag colour="green" tag="自定义" */>}}

{{</* tag_academic */>}}
{{</* tag_read */>}}
{{</* tag_sport */>}}
{{</* tag_kegel */>}}
{{</* tag_relax */>}}
{{</* tag_bathe */>}}
{{</* tag_play */>}}
{{</* tag_go_out */>}}
{{</* tag_hentai */>}}
```

{{< tag colour="green" tag="自定义" >}}

{{< tag_academic >}}
{{< tag_read >}}
{{< tag_sport >}}
{{< tag_kegel >}}
{{< tag_relax >}}
{{< tag_bathe >}}
{{< tag_play >}}
{{< tag_go_out >}}
{{< tag_hentai >}}

