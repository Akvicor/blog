---
title: "在U盘中建立自己的私有Git云"
date: 2019-02-11T17:44:58Z
draft: false
categories: [Git]
tags: []
card: false
weight: 0
---

安装好的git环境 https://git-scm.com/

在U盘上创建空仓库test.git(我的U盘盘符在PC端是I盘)。

<!--more-->

```
$ cd /I
$ mkdir test
$ cd test
$ git init --bare
```

得到如下显示即表明git仓库创建成功

`Initialized empty Git repository in I:/test/`

现有git工程与U盘仓库关联(在git工程根目录下执行)

`$ git remote add usb /i/test`

版本管理，仓库有了，剩下就是推送和拉取文件了

```
$ git push usb master
$ git pull usb master
```

## Git命令

（先进入项目文件夹）通过命令 git init 把这个目录变成git可以管理的仓库

`git init`

把文件添加到版本库中，使用命令 git add .添加到暂存区里面去，不要忘记后面的小数点“.”，意为添加文件夹下的所有文件

```
git add .
git config --global user.name "yourname"
git config --global user.email "your@email.com"
```

用命令 git commit告诉Git，把文件提交到仓库。引号内为提交说明

`git commit -m 'first commit'`

你的远程库地址`关联到远程库

`git remote add origin`

获取远程库与本地同步合并（如果远程库不为空必须做这一步，否则后面的提交会失败）

`git pull --rebase origin master`

把本地库的内容推送到远程，使用 git push命令，实际上是把当前分支master推送到远程。执行此命令后会要求输入用户名、密码，验证通过后即开始上传。

`git push -u origin master`

状态查询命令

`git status`

## 使用git时出现：warning: LF will be replaced by CRLF

如果项目已经创建，则需要先删除之前创建的.git 文件后添加上面的设置

删除.git

`rm -rf .git`

禁用自动转换

```
$ git config --global core.autocrlf false
$ git init
$ git add .
```

