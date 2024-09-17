---
title: "指纹识别 Fingerprint"
date: 2024-04-26 11:35:09 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

Debian指纹识别

```bash
# 安装
sudo apt install fprintd
# 录入 (录入5次后会显示completed)
sudo fprintd-enroll
# 识别
sudo fprintd-verify
```
