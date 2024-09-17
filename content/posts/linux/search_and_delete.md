---
title: "使用find和rm搜索并删除文件"
date: 2023-09-19 14:27:37 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

删除指定目录下指定文件

`find /path -name .svn | xargs rm -rf`

删除指定名称的文件或文件夹: find -type d | grep .svn$ | xargs rm -r

分析：

`find -type d | grep .svn$` 通过此命令查找文件夹 过滤正则表达式中的目录
`| xargs rm -r` 执行删除指令

删除目录下所有exe文件

`find . -name '*.exe' -type f -print -exec rm -rf {} \;`

1. `.` 表示从当前目录开始递归查找
2. ` -name '*.exe' ` 根据名称来查找，要查找所有以.exe结尾的文件夹或者文件
3. ` -type f ` 查找的类型为文件
4. `-print` 输出查找的文件目录名
5. 最主要的是是`-exec`了，-exec选项后边跟着一个所要执行的命令，表示将find出来的文件或目录执行该命令。
exec选项后面跟随着所要执行的命令或脚本，然后是一对儿{}，一个空格和一个，最后是一个分号.
