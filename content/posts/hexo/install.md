---
title: "Hexo Install"
date: 2019-02-10T17:21:19Z
draft: false
categories: [Hexo]
tags: []
card: false
weight: 0
---

搭建Hexo并安装Next主题

## 本地

1. 安装[Node js](https://nodejs.org/en/)

2. 安装Hexo命令行工具

```bash
npm install hexo-cli -g
```

3. 生成一个本地的Hexo项目

```bash
# 创建blog目录，并初始化hexo项目
hexo init blog
cd blog
# 安装hexo依赖，hexo是基于nodejs开发的，npm是nodejs的包管理工具
npm install
# 启动本地服务，打开localhost:4000测试是否成功
hexo server
```

<!--more-->

4. 新建一篇文章

```bash
hexo new "My First Post"
# 这个命令是用于在 source/_posts 下生成一个命名的.md 文件，效果等同于新建.md文件并拷贝至该处。
hexo generate
# 将这个新建的博文生成静态文件并部署到本地来预览效果, 简写为hexo g
hexo server
# 启动本地服务，查看效果, 简写为hexo s
```

有时会出现 4000 端口被其他进程占用的问题，这里有两种解决方案：1. 解除其他进程端口占用，2. 使用 hexo server -p 5000 来修改端口并预览

上述命令完成后，就可以打开浏览器，输入 [localhost:4000](http://localhost:4000/) 就可以预览你刚刚生成的博文啦。

### 执行 hexo deploy出错

在执行 hexo deploy 后,出现 error deployer not found:git 的错误处理

输入代码：
`npm install hexo-deployer-git --save`

## 远程（服务端）

- 服务器端安装配置Git、Nodejs、Nginx或Apache。
- 创建git用户，建立git裸库，配置git-hooks。
- 配置本地Hexo，完成git自动化部署。

本教程默认已经配置好**网站环境和域名解析**

### 环境搭建

以下均需要通过SSH工具连接VPS进行操作

#### 安装Git和Nodejs

```bash
# 安装git
apt-get install git
# 安装Nodejs
curl --silent --location https://rpm.nodesource.com/setup_10.x | bash -
```

使用`git --version`和`node --version`查看，显示版本号则安装成功，

#### 创建git用户

1. 创建一个git用户，并git根据提示设置密码，用来专门运行git服务

```bash
# 命令一：这种命令会在登录界面显示用户名
sudo useradd -m XXX -d /home/XXX -s /bin/bash

# 命令二：这种命令会在登录界面隐藏用户名
sudo useradd -r -m -s /bin/bash XXX  
# XX指代创建的用户名

sudo passwd XXX	
# XXX指创建的用户名
```

2. 赋予git用户sudo权限

```bash
# chmod 740 /etc/sudoers
sudo chmod +w /etc/sudoers
vim /etc/sudoers
```

找到以下内容：

```bash
## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
```

在下面添加一行：`git ALL=(ALL) ALL`

保存退出后改回权限：`chmod 400 /etc/sudoers` 或 `sudo chmod -w /etc/sudoers`

#### 切换用户，配置SSH

使用`su git`切换到git用户，再执行下列操作：

```bash
# 切换到git用户目录
cd /home/git
# 创建.ssh文件夹
mkdir ~/.ssh
# 创建authorized_keys文件并编辑
vim ~/.ssh/authorized_keys
# 如果你还没有生成公钥，那么首先在本地电脑中执行生成公钥, 步骤在后面
# 再将公钥复制粘贴到authorized_keys
# 保存关闭authorized_keys后，修改相应权限
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

##### Git SSH 创建Key的步骤

```bash
# 打开Git Bash
cd ~/.ssh/
# 进入ssh文件夹，如果没有就执行 mkdir ~/.ssh 创建一个
# 配置全局的name和email，这里是的你github或者bitbucket的name和email，如果配置了就跳过
git config --global user.name "your name"
git config --global user.email "your email"
# 生成key
ssh-keygen -t rsa -C "your email"
# 最后得到了两个文件：id_rsa和id_rsa.pub
cat ~/.ssh/id_rsa.pub
```



然后可以通过本地Git Bash执行ssh命令测试是否可以免密登录

```bash
ssh -v git@服务器ip地址
```

这样git用户就添加好了。

#### 建立git裸库

```bash
# 回到git目录
cd /home/git
# 使用git用户创建git裸仓库，以blog.git为例
git init --bare blog.git
```

#### 检查用户组权限

我们的git裸仓库已经建立好了，离成功又近了一步。为了以防万一，我们要检查一下之前的blog.git、.ssh、blog目录的用户组权限是否都为git:git

```bash
# 网站目录
ll -a /home/wwwroot/
# git目录
ll -a /home/git/
```

![per](https://img.akvicor.com/i/2024/09/17/66e9999c0cde1.jpg)

如果有哪个不是，执行下面相应的命令后再查看

```bash
sudo chown git:git -R /home/wwwroot/blog.akvicor.com
sudo chown git:git -R /home/git/blog.git
```

#### 使用git-hooks同步网站根目录

简单来说，我们使用一个钩子文件：`post-receive`，每当git仓库接收到内容的时候，就会自动调用这个钩子，把内容同步到网站根目录。

在git用户下执行：

```bash
# 新建一个post-receive文件并编辑
vim ~/blog.git/hooks/post-receive
```

在里面输入以下内容，注意修改为自己的设置：

```bash
#!/bin/bash
GIT_REPO=/home/git/blog.git
TMP_GIT_CLONE=/tmp/blog
PUBLIC_WWW=/home/wwwroot/blog.akvicor.com
rm -rf ${TMP_GIT_CLONE}
git clone $GIT_REPO $TMP_GIT_CLONE
rm -rf ${PUBLIC_WWW}/*
cp -rf ${TMP_GIT_CLONE}/* ${PUBLIC_WWW}
```

保存退出后，执行：`chmod +x ~/blog.git/hooks/post-receive`赋予这个文件可执行权限。

## 配置本地Hexo的_config.yml

非常简单，只需要找到本地Hexo博客的站点配置文件_config.yml，找到以下内容并修改：

```yaml
deploy: 
  type: git
  repo: git@你的服务器IP:/home/git/blog.git
  branch: master
```

保存后，剩下的就是Hexo的日常操作了，这里就不赘述了，写完文章后，在你的本地博客根目录执行以下命令：

```bash
hexo clean
hexo g -d
```

就可以实现线上博客的自动更新了！一切搞定！

## 添加添加关于、分类及标签

```bash
# 执行以下命令添加页面
hexo new page about
hexo new page tag
hexo new page categories
```

```
---
title: This is me
date: 2019-02-09 21:24:37
---
```

```
---
title: categories
date: 2019-02-09 21:26:13
type: "categories"
---
```

```
---
title: tags
date: 2019-02-09 21:27:50
type: "tags"
---
```

## 让hexo的首页只显示文章的部分内容而不是全部

### 法1

用文本编辑器打开 themes/ 目录下的对应的主题的theme文件夹下的 _config.yml 文件，找到这段代码，如果没有则新建，可能不同的主题会不支持这种方法：

```yaml
# Automatically Excerpt. Not recommend.
# Please use <!-- more --> in the post to control excerpt accurately.
auto_excerpt:
  enable: false
  length: 150
```

把 enable 的 false 改成 true 就行了，然后 length 是设定文章预览的文本长度。

修改后重启 hexo 就ok了。

### 法2

在你写 md 文章的时候，可以在内容中加上 `<!--more-->`，这样首页和列表页展示的文章内容就是 `<!--more-->` 之前的文字，而之后的就不会显示了。

效果

上面两种方式展示出来的效果是不一样的。

第一种修改 _config.yml 文件的效果是会格式化你文章的样式，直接把文字挤在一起显示，最后会有 …。

而第二种加上 `<!--more-->`展示出来的就是你原本文章的样式，最后不会有…。

### 法3

在文章的 `front-matter` 中添加 `description`，并提供文章摘录

```markdown
---
title: 让hexo的首页只显示文章的部分内容而不是全部
id: set-hexo-show-more-button-on-index
categories:
  - WEB开发
date: 2017-09-30 11:01:40
tags:
    - blog
    - hexo
description: Hexo 的 Next 主题默认是首页显示你每篇文章的全文内容，那么要如何设置只显示部分呢？正如你现在看到的本篇文章，只显示到这里。
---
```

但是使用这种方式生成的描述信息在文章的详情页是不再显示的。

## 插件

### hexo-abbrlink

```bash
npm install hexo-abbrlink --save
```

修改配置文件

```yaml
permalink: :category/:year/:month/:day/:title/

abbrlink:
  alg: crc32  #support crc16(default) and crc32  
  rep: hex    #support dec(default) and hex
```



## 创建站点地图

### 安装插件

在 Hexo 根目录下，执行如下命令以安装插件：

- Google 版本

  ```bash
  npm install hexo-generator-sitemap --save
  ```

- Baidu 版本

  ```bash
  npm install hexo-generator-baidu-sitemap --save
  ```

### 开始生成站点地图文件

安装好插件后，插件会在每次` hexo g` 命令将 markdown 文件转化为 html 文件时执行

执行结果为，在存放 html 文件根目录下，即` [hexo install location]/public `下生成一 ‘sitemap.xml’/‘baidusitemap.xml’

紧接着在你执行` hexo d `将网站文件部署到 github 仓库上之后，准备工作算是完成大半了

### 添加站点地图 url

最后，你只需将该站点地图文件的 url 添加至搜索引擎的 search console ，旋即完成了整个过程

## 添加搜索功能

1. 要使用搜索，必须先生成博客索引数据，Hexo 可以通过下面的这个插件生成：

```
npm install hexo-generator-search --save
npm install hexo-generator-searchdb --save
```

2. 在 Hexo 站点 _config.yml 中添加如下配置即可：

```
# 搜索
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```

3. 在主题配置文件中开启

```
# Local search
local_search:
  enable: true
```

