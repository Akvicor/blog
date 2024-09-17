---
title: "Debian 笔记本合盖和电源键作用"
date: 2023-02-15 11:23:37 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

取消笔记本合盖后挂起

```shell
vim /etc/systemd/logind.conf
```

修改

```shell
# HandleLidSwitch=suspend
HandleLidSwitch=ignore
# HandlePowerKey=poweroff
HandlePowerKey=ignore
```

修改屏幕保护




