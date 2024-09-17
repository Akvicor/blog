---
title: "Kidding"
date: 2022-07-29 19:27:45 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

## 编写

编写`kidding.html`放在`layouts/shortcodes/`文件夹中

```
<!-- layouts/shortcodes/kidding.html -->
<s style="text-decoration: line-through;" title="{{ .Get "title" }}">{{ .Get "text" }}</s>
```

## 使用

```
{{</* kidding title="完全没理解" text="太棒了,我逐渐理解一切" */>}}
```

{{< kidding title="完全没理解" text="太棒了,我逐渐理解一切" >}}



