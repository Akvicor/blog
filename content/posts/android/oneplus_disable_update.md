---
title: "关闭一加氧系统更新"
date: 2024-04-20 03:57:22 +08:00
draft: false
categories: [Android]
tags: []
card: false
weight: 0
---

通过ADB停用系统更新和去除更新统治

```shell
# 屏蔽更新
adb shell pm disable-user com.oneplus.opbackup
# 清除更新通知
adb shell pm clear com.oneplus.opbackup
# 恢复更新
adb shell pm enable com.oneplus.opbackup
```
