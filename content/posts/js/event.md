---
title: "事件响应"
date: 2019-09-29 19:00:32 +08:00
draft: false
categories: [JavaScript]
tags: []
card: false
weight: 0
---

JavaScript 创建动态页面。事件是可以被 JavaScript 侦测到的行为。 网页中的每个元素都可以产生某些可以触发 JavaScript 函数或程序的事件。

比如说，当用户单击按钮或者提交表单数据时，就发生一个鼠标单击（onclick）事件，需要浏览器做出处理，返回给用户一个结果。

![](https://img.akvicor.com/i/2024/09/17/66e99f925f195.jpg)

<!--more-->

## 鼠标单击事件( onclick ）

onclick是鼠标单击事件，当在网页上单击鼠标时，就会发生该事件。同时onclick事件调用的程序块就会被执行，通常与按钮一起使用。

```javascript
onclick="document.getElementById('sign_in_form').submit();"
```

## 鼠标经过事件（onmouseover）

鼠标经过事件，当鼠标移到一个对象上时，该对象就触发onmouseover事件，并执行onmouseover事件调用的程序。

现实鼠标经过"确定"按钮时，触发onmouseover事件，调用函数info()，弹出消息框，代码如下:

![](https://img.akvicor.com/i/2024/09/17/66e9a1b606fe0.jpg)

## 鼠标移开事件（onmouseout）

鼠标移开事件，当鼠标移开当前对象时，执行onmouseout调用的程序。

当把鼠标移动到"登录"按钮上，然后再移开时，触发onmouseout事件，调用函数message()，代码如下:

![](https://img.akvicor.com/i/2024/09/17/66e9a1c2d3501.jpg)

## 光标聚焦事件（onfocus）

当网页中的对象获得聚点时，执行onfocus调用的程序就会被执行。

如下代码, 当将光标移到文本框内时，即焦点在文本框内，触发onfocus 事件，并调用函数message()。

![](https://img.akvicor.com/i/2024/09/17/66e9a1d2dc0aa.jpg)

## 失焦事件（onblur）

onblur事件与onfocus是相对事件，当光标离开当前获得聚焦对象的时候，触发onblur事件，同时执行被调用的程序。

如下代码, 网页中有用户和密码两个文本框。当前光标在用户文本框内时（即焦点在文本框），在光标离开该文本框后（即失焦时），触发onblur事件，并调用函数message()。

![](https://img.akvicor.com/i/2024/09/17/66e9a1e3a85db.jpg)

## 内容选中事件（onselect）

选中事件，当文本框或者文本域中的文字被选中时，触发onselect事件，同时调用的程序就会被执行。

如下代码,当选中用户文本框内的文字时，触发onselect 事件，并调用函数message()。

![](https://img.akvicor.com/i/2024/09/17/66e9a1f13ace1.jpg)

## 文本框内容改变事件（onchange）

通过改变文本框的内容来触发onchange事件，同时执行被调用的程序。

如下代码,当用户将文本框内的文字改变后，弹出对话框“您改变了文本内容！”。

![](https://img.akvicor.com/i/2024/09/17/66e9a2002eefa.jpg)

## 加载事件（onload）

事件会在页面加载完成后，立即发生，同时执行被调用的程序。
注意：

1. 加载页面时，触发onload事件，事件写在`<body>`标签内。
2. 此节的加载页面，可理解为打开一个新页面时。

如下代码,当加载一个新页面时，弹出对话框“加载中，请稍等…”。

![](https://img.akvicor.com/i/2024/09/17/66e9a21b0f42e.jpg)

## 卸载事件（onunload）

当用户退出页面时（页面关闭、页面刷新等），触发onUnload事件，同时执行被调用的程序。

注意：不同浏览器对onunload事件支持不同。

如下代码,当退出页面时，弹出对话框“您确定离开该网页吗？”。

![](https://img.akvicor.com/i/2024/09/17/66e9a22949d1b.jpg)

