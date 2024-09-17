---
title: "拉取私有Go Module"
date: 2023-11-30 08:53:24 +08:00
draft: false
categories: [Golang]
tags: []
card: false
weight: 0
---

若拉取时提示无法验证包，可以添加以下环境变量以信任此地址

## 关闭指定域名的包验证

```
export GONOSUMDB=git.akvicor.com
```

## 关闭所有域名的包验证

```
export GOSUMDB=off
```


