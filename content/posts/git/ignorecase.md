---
title: "git 默认对大小写不敏感"
date: 2019-04-14T21:26:29Z
draft: false
categories: [Git]
tags: []
card: false
weight: 0
---

git 默认不区分文件名大小写

当你创建一个文件后,叫 readme.md 写入内容后 提交到线上代码仓库.

然后你在本地修改文件名为 Readme.md 接着你去提交,发现代码没有变化.

```bash
git status
```


无任何提示信息.

其实 git 默认对于文件名大小写是不敏感的,所以上面你修改了首字母大写,但是git 并没有发现代码任何改动.

那么如何才能让 git 识别文件名大小写变化.

<!--more-->

好的约定其实比技术本身更重要，所以尽可能统一规范大小写，从而避免修改默认的配置。

## 配置git使其对文件名大小写敏感

在git库文件夹中执行

```bash
git config core.ignorecase false

git config --global core.ignorecase false // 全局设置
```

