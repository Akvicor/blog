---
title: "Ubuntu Error: ENOSPC:System limit for number of file watchers reached"
date: 2020-01-03 09:43:51 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

Ubuntu Error: ENOSPC:System limit for number of file watchers reached

<!--more-->

这个错误的意思时系统对文件监控的数量已经到达限制数量了


解决方法：修改系统监控文件数量

```bash
sudo vim /etc/sysctl.conf
```

在最后一行增加

```
fs.inotify.max_user_watches=524288
```

保存退出 
执行

```bash
sudo sysctl -p
```

