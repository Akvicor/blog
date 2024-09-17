---
title: "Abbr"
date: 2022-07-14 17:53:41 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

## 编写

编写`abbr.html`放在`layouts/shortcodes/`文件夹中

```
<!-- layouts/shortcodes/abbr.html -->
<abbr title="{{ .Get "title" }}">{{ .Get "text" }}</abbr>
```

## 使用

```
{{</* abbr title="Ocean University of China" text="OUC" */>}}
```

{{< abbr title="Ocean University of China" text="OUC" >}}
