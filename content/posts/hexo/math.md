---
title: "支持数学公式"
date: 2019-03-04T20:26:14Z
draft: false
categories: [Hexo]
tags: []
card: false
weight: 0
---


```bash
$ npm install hexo-math --save
```

在站点配置文件` _config.yml `中添加：

```yml
math:
  engine: 'mathjax' # or 'katex'
  mathjax:
    # src: custom_mathjax_source
    config:
      # MathJax config
```

在 next 主题配置文件中 `themes/next-theme/_config.yml` 中将 `mathJax` 设为 `true`:

```yml
# MathJax Support
mathjax:
  enable: true
  per_page: false
  cdn: //cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML
```

也可以在文章的开始集成插件支持，但不建议这么做：

```html
<script type="text/javascript"
   src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
```
