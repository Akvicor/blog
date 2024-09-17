---
title: "Funny Title"
date: 2023-08-05 08:31:20 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

当页面失去焦点时改变标题，恢复焦点时恢复标题

将js文件放在主题的static/js文件夹中

在layouts/partials/head.html中添加

```
    {{ if .Site.Params.funnyTitle | default false }}
    <script src="/js/funny_title.js"></script>
    {{- end -}}
```

此时在hugo.toml中添加以下内容开启

```toml
[params]
    # 动态标题
    funnyTitle = true
```

## funny_title.js

```js
( function (global) {
	var vendorPrefix=getBrowserPrefix();
	var eventName=visibilityEvent(vendorPrefix);
	document.addEventListener(eventName,visibilityEventCallback);

	var titleTime;
	var oldTitle=document.title;
	function visibilityEventCallback(){
		if(document.hidden){

			document.title="╭(°A°`)╮ 页面崩溃啦 ~ 快回来看看~ | "+oldTitle;
            clearTimeout(titleTime);
		}else{
			document.title="(ฅ>ω<*ฅ) 噫又好了~";
            titleTime = setTimeout(function() {
                document.title = oldTitle;
            }, 2000);
		}
	}
// Get Browser-Specifc Prefix
	function getBrowserPrefix() {
	
	  	// Check for the unprefixed property.  
	  	if ('hidden' in document) {
	    	return null;
		}
	
		// All the possible prefixes.  
		var browserPrefixes = ['moz', 'ms', 'o', 'webkit'];
		 
		for (var i = 0; i < browserPrefixes.length; i++) {
		    var prefix = browserPrefixes[i] + 'Hidden';
		    if (prefix in document) {
		      return browserPrefixes[i];
		    }  
		}
	
	 	// The API is not supported in browser.  
	 	return null;
	}
	
	// Get Browser Specific Hidden Property
	function hiddenProperty(prefix) {
		if (prefix) {
			return prefix + 'Hidden';
		} else {
			return 'hidden';
		}
	}
	
	// Get Browser Specific Visibility State
	function visibilityState(prefix) {
	  if (prefix) {
	    return prefix + 'VisibilityState';
	  } else {
	    return 'visibilityState';
	  }
	}
	
	// Get Browser Specific Event
	function visibilityEvent(prefix) {
	  if (prefix) {
	    return prefix + 'visibilitychange';
	  } else {
	    return 'visibilitychange';
	  }
	}
})(this);

```
