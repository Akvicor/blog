---
title: "form表单提交后不刷新不跳转的实现方法"
date: 2019-09-29 17:09:57 +08:00
draft: false
categories: [HTML]
tags: []
card: false
weight: 0
---

html form表单提交后不刷新不跳转的实现方法

<!--more-->

```html
<html>  
    <body>  

        <form action="" method="post" target="nm_iframe">  
            <input type="text" id="id_input_text" name="nm_input_text" />  
            <input type="submit" id="id_submit" name="nm_submit" value="提交" />  
        </form>  
      
        <iframe id="id_iframe" name="nm_iframe" style="display:none;"></iframe>  

    </body>  
</html>  
```
