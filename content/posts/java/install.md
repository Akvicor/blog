---
title: "Java Install"
date: 2019-02-11T17:25:46Z
draft: false
categories: [Java]
tags: []
card: false
weight: 0
---

在Windows环境下配置Java开发环境变量，并配置多版本Java

本文使用java8和java11

<!--more-->

## 下载jdk

http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html

## 打开环境变量变量

![img](https://img.akvicor.com/i/2024/09/17/66e99a78732f1.png)

在**系统变量**下新建

JAVA_HOME

`C:\language\Java\jdk1.8.0_171`

CLASSPATH

`.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\tools.jar`

在**Path**下添加

```
%JAVA_HOME%\bin
%JAVA_HOME%\jre\bin
```

## 检查是否配置正确

打开CMD 输入命令以下命令检查是否配置正确
均能得到返回信息则配置正确
```
javac
java
java -version
```

## 多版本配置
在**系统变量**下新建

```
JAVA8_HOME
	C:\language\Java\jdk1.8.0_171
JAVA11_HOME
	C:\language\Java\jdk-11.0.2
```

修改`JAVA_HOME`为

```
%JAVA8_HOME%
```

若需要Java11只需要将`JAVA_HOME`中的`8`改为`11`即可



