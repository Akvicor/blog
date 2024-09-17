---
title: "nav 右侧导航标题显示不全"
date: 2024-03-16 13:55:06 +08:00
draft: false
categories: [Hugo]
tags: []
card: false
weight: 0
---

由于css中限制了标签的宽高, 如果标题过长会无法显示, 因此需要修改css中的属性

在`virgo/assets/scss/partials/content/nav.scss`中的`.container-nav/.toc/li`中的`width`
