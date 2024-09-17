---
title: "CSS-Only Carousel"
date: 2023-02-25 12:31:24 +08:00
draft: false
categories: [CSS]
tags: []
card: false
weight: 0
---

![](https://img.akvicor.com/i/2024/09/17/66e99811091b8.gif)

```html
<div class="wheelPlayer wheelAnimate">
    <div>1</div>
    <div>2</div>
    <div>3</div>
</div>
```

```css
.wheelPlayer{
   padding: 20px 0;
   margin: auto;
   width: 350px;
}
.wheelPlayer div{
   position: relative;
   height: 25px;
   line-height: 26px;
   width: 70px;
   text-align: center;
   border: 1px solid gray;
}
.wheelAnimate div{
   animation: whellPlayer 9s infinite;
   -moz-animation: whellPlayer 9s infinite;
   -webkit-animation: whellPlayer 9s infinite;
}
.wheelPlayer div:nth-of-type(1){
   animation-delay: -6s;
   -moz-animation-delay: -6s;
   -webkit-animation-delay: -6s;
}
.wheelPlayer div:nth-of-type(2){
   animation-delay: -3s;
   -moz-animation-delay: -3s;
   -webkit-animation-delay: -3s;
   margin-top:  -27px;
}
.wheelPlayer div:nth-of-type(3){
   animation-delay: -0s;
   -moz-animation-delay: 0s;
   -webkit-animation-delay: 0s;
   margin-top:  -27px;
}
```



