---
title: "Placeholder"
date: 2022-10-11 15:50:27 +08:00
draft: false
categories: [HTML, CSS]
tags: []
card: false
weight: 0
---

## input输入框占位符变化：输入框处于聚焦状态时，输入框的占位符内容以动画形式移动到左上角作为标题

```html
<div class="input-box"> 
    <input class="input-control input-outline" placeholder="账号">
    <label class="input-label">账号</label>
</div>
```

首先：让浏览器默认的`placeholder`效果不可见

```css
.input-control:placeholder-shown::placeholder { 
    color: transparent; 
}
```

其次：使用`.input-label`元素代替浏览器原声的占位符

```css
.input-box{
  position: relative;
}
.input-label {
  position: absolute;
  left: 16px; top: 14px;
  pointer-events: none;
}
```

最后，在输入框聚焦以及占位符不显示的时候对`<label>`元素进行重定位，效果是缩小并移动到上方

```css
.input-control:not(:placeholder-shown) ~ .input-label,
.input-control:focus ~ .input-label {
  color: #2486ff;
  transform: scale(0.75) translate(-2px, -32px);
}
```
