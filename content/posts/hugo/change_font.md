---
title: "修改字体"
date: 2023-07-08 16:28:44 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

## 1. 下载字体

添加到`static/fonts`文件夹下，若使用了主题则可以放在主题的fonts文件夹下，如`themes/archie/static/fonts`。

## 2. 添加字体

在`fonts.css`中添加字体，如`themes/archie/assets/css/fonts.css`

otf目前可以在大多数浏览器中使用。

```css
/* NotoSansSC Black */
@font-face {
  font-family: 'NotoSansSC Black';
  font-style: black;
  font-weight: 400;
  src: url('../fonts/NotoSansSC-Black.otf') format('opentype');
}
/* NotoSansSC Bold */
@font-face {
  font-family: 'NotoSansSC Bold';
  font-style: bold;
  font-weight: 400;
  src: url('../fonts/NotoSansSC-Bold.otf') format('opentype');
}
/* NotoSansSC Regular */
@font-face {
  font-family: 'NotoSansSC Regular';
  font-style: normal;
  font-weight: 400;
  src: url('../fonts/NotoSansSC-Regular.otf') format('opentype');
}
```

但如果想支持多种浏览器，建议切换到WOFF和TTF字体。

可以通过以下工具转换。[https://transfonter.org/](https://transfonter.org/)

```css
@font-face {
    font-family: GraublauWeb;
    src: url("path/GraublauWebBold.woff") format("woff"), url("path/GraublauWebBold.ttf")  format("truetype");
}
```

## 3. 引用

在archive主题中，可以通过修改`themes/archie/assets/css/main.css`来修改某个标签的字体或全局字体

```css
strong {
    font-family: 'NotoSansSC Bold', sans-serif;
    line-height: 1.5;
}
```

