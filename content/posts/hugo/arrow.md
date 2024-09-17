---
title: "Arrow"
date: 2023-07-10 06:43:32 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

## 编写

编写`arrow.html`、`arrow_colour.html`、`arrow_?.html`或`arrow_?_colour.html`放在`layouts/shortcodes/`文件夹中

```
<!-- layouts/shortcodes/arrow.html -->
<font color="#9B59B6">➤</font>
```

```
<!-- layouts/shortcodes/arrow_colour.html -->
<font color="{{ .Inner }}">➤</font>
```

## 使用

```
{{</*arrow>}} {{<arrow_up>}} {{<arrow_down>}} {{<arrow_left>}} {{<arrow_right*/>}}

{{</*arrow_colour>}}#7D3C98{{</arrow_colour>}} {{<arrow_up_colour>}}#7D3C98{{</arrow_up_colour>}} {{<arrow_down_colour>}}#7D3C98{{</arrow_down_colour>}} {{<arrow_left_colour>}}#7D3C98{{</arrow_left_colour>}} {{<arrow_right_colour>}}#7D3C98{{</arrow_right_colour*/>}}
```

{{<arrow>}} {{<arrow_up>}} {{<arrow_down>}} {{<arrow_left>}} {{<arrow_right>}}

{{<arrow_colour>}}#7D3C98{{</arrow_colour>}} {{<arrow_up_colour>}}#7D3C98{{</arrow_up_colour>}} {{<arrow_down_colour>}}#7D3C98{{</arrow_down_colour>}} {{<arrow_left_colour>}}#7D3C98{{</arrow_left_colour>}} {{<arrow_right_colour>}}#7D3C98{{</arrow_right_colour>}}

