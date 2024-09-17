---
title: "升级vim至最新版"
date: 2019-04-21T10:36:18Z
draft: false
categories: [Mac]
tags: []
card: false
weight: 0
---

macOS 默认已经安装了 Vim，可执行程序是 `/usr/bin/vim`，当前的系统 Vim 版本有一个问题是不支持与系统剪贴板的集成，另外由于是系统集成版本，使用一段时间后往往会出现版本低于当前 Vim 最新版的情况。

<!--more-->

## 安装包管理器Homebrew

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## 使用Homebrew安装vim

```bash
brew install vim
```

可以看到最新版的vim安装到了 `/usr/local/Cellar/vim/8.0.0946` 路径下了

```bash
cd /usr/local/Cellar/vim/8.0.0946
ls
cd bin/
ls
```

那么，最新版vim程序的所在位置路径就是：`/usr/local/Cellar/vim/8.0.0946/bin/vim`了！

## 修改bash_profile文件内容

接下来要做到就是把最新版vim的安装路径添加到环境变量PATH中了

打开 `~/.bash_profile`，添加一句话：

```bash
export PATH=/usr/local/Cellar/vim/8.0.0946:$PATH
```

或者

```bash
alias vim='/usr/local/Cellar/vim/8.0.0946/bin/vim'
```

两者选一

再source一下文件

```bash
source ~/.bash_profile
```

