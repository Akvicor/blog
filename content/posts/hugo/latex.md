---
title: "LaTeX"
date: 2021-07-14 17:29:28 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

## 如果要启用Mathjax和KaTeX

在header模板中添加以下内容

```html
	<!-- Mathjax support -->
	{{ with .Site.Params.mathjax }}
		<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.1/MathJax.js?config=TeX-AMS-MML_HTMLorMML"> </script>
	
		<!-- inline Mathjax -->
		<script type="text/x-mathjax-config">
		MathJax.Hub.Config({
			tex2jax: {
				inlineMath: [['$math_inline$','$math_inline$'], ['\\(','\\)']],
				displayMath: [['$math_noinline$','$math_noinline$'], ['\[','\]']],
				processEscapes: true,
				processEnvironments: true,
				skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
				TeX: { equationNumbers: { autoNumber: "AMS" },
						 extensions: ["AMSmath.js", "AMSsymbols.js"] }
			}
		});
		</script>
	{{ end }}

	<!-- KaTeX support -->
	{{ with .Site.Params.katex }}
		<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.15.2/dist/katex.min.css">
		<script defer src="https://cdn.jsdelivr.net/npm/katex@0.15.2/dist/katex.min.js"></script>
		<script defer src="https://cdn.jsdelivr.net/npm/katex@0.15.2/dist/contrib/auto-render.min.js" onload="renderMathInElement(document.body);"></script>
		
		<!-- inline KaTeX -->
		<script>
			document.addEventListener("DOMContentLoaded", function() {
					renderMathInElement(document.body, {
							delimiters: [
									{left: "$math_noinline$", right: "$math_noinline$", display: true},
									{left: "$math_inline$", right: "$math_inline$", display: false}
							]
					});
			});
			</script>
	{{ end }}
```

## 在layouts/shortcodes下添加latex.html

```text
<!-- layouts/shortcodes/latex.html -->
{{ if .Site.Params.viryLatex }}
    {{ if ne (.Get "inline") ("false") }}
      {{ $url := printf "<img style='border: none;' src=\"https://latex.akvicor.com/?base=math&key=mTFPk34&crop=1&type=png&transp=1&latex=%s\">" (urlquery .Inner) }}
      {{ replace $url "+" "%20" | safeHTML }}
    {{ else }}
        {{ $url := printf "<p><img style='border: none;' src=\"https://latex.akvicor.com/?base=math&key=mTFPk34&crop=1&type=png&transp=1&latex=%s\"></p>" (urlquery .Inner) }}
        {{ replace $url "+" "%20" | safeHTML }}
    {{ end }}
{{ else }}
    {{ if ne (.Get "inline") ("false") }}
        $math_inline${{ .Inner }}$math_inline$
    {{ else }}
        $math_noinline${{ .Inner }}$math_noinline$
    {{ end }}
{{ end }}
```

## 启用

在配置文件中添加

```toml
[params]
	# 数学公式支持
	viryLatex = false # 使用自建latex渲染服务
    mathjax = true # enable MathJax support
	katex = true # enable KaTeX support
```

## 在Markdown中使用

```text
行内{{</*latex*/>}}\sqrt{x^2+1}{{</*/latex*/>}}

单独一行{{</*latex inline="false"*/>}}\sqrt{x^2+1}{{</*/latex*/>}}
```

**效果**

行内{{<latex>}}\sqrt{x^2+1}{{</latex>}}

单独一行{{<latex inline="false">}}\sqrt{x^2+1}{{</latex>}}

