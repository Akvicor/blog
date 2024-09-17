---
title: "插入音乐/视频"
date: 2019-11-27 18:44:30 +08:00
draft: false
categories: [Hexo]
tags: []
card: false
weight: 0
---

Hexo 插件插入音乐/视频

<!--more-->

两个好用的hexo插件：

- 音乐：[hexo-tag-aplayer](https://github.com/grzhan/hexo-tag-aplayer)
- 视频：[hexo-tag-dplayer](https://github.com/NextMoe/hexo-tag-dplayer)

# 播放音乐的aplayer

```
npm install hexo-tag-aplayer
```

Markdown 中使用下列代码嵌入音乐

```
{% aplayer "她的睫毛" "周杰伦" "http://home.ustc.edu.cn/~mmmwhy/%d6%dc%bd%dc%c2%d7%20-%20%cb%fd%b5%c4%bd%de%c3%ab.mp3"  "http://home.ustc.edu.cn/~mmmwhy/jay.jpg" "autoplay=false" %}
```

# 播放视频的dplayer

```
npm install hexo-tag-dplayer
```

Markdown 中使用下列代码嵌入音乐

```
{% dplayer "url=http://home.ustc.edu.cn/~mmmwhy/GEM.mp4"  "pic=http://home.ustc.edu.cn/~mmmwhy/GEM.jpg" "loop=yes" "theme=#FADFA3" "autoplay=false" "token=tokendemo" %}
```


