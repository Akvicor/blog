---
title: "Ruby"
date: 2022-07-29 19:34:53 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

## 编写

编写`ruby.html`放在`layouts/shortcodes/`文件夹中

```
<!-- layouts/shortcodes/ruby.html -->
<ruby>
  {{ .Get "text" }}<rp>(</rp><rt>{{ .Get "title" }}</rt><rp>)</rp>
</ruby>
```

## 使用

```
{{</* ruby title="完全没理解" text="太棒了,我逐渐理解一切" */>}}
```

{{< ruby title="完全没理解" text="太棒了,我逐渐理解一切" >}}


