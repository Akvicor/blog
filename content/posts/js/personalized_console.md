---
title: "网站添加个性的Console"
date: 2019-02-12T10:46:27Z
draft: false
categories: [JavaScript]
tags: []
card: false
weight: 0
---

![image](https://img.akvicor.com/i/2024/09/17/66e9a23e24352.png)

使用Chrome浏览器访问百度，在页面上右键点击“检查”–>Console，可以看到上面这样的信息，如何在自己的网站上实现这样的功能？

<!--more-->

其实上面一段是利用console.log输出的，此功能前端调试方面很实用。

只要在header中加入以下代码即可

```html
<script>
	try{
		if(window.console&&window.console.log) {
        	console.log("不谢：%chttps://akvicor.com/","color:blue");
		}
	}
	catch(e){};
</script>
```

%c的作用是为后面的字符串加上CSS样式，与后面的css代码对应

