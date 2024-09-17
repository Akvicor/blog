---
title: "Chromium Tips"
date: 2023-08-08 06:44:51 +08:00
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

## 缺少Google API 密钥，因此Chromium 的部分功能将无法使用

### 1. 设置环境变量，屏蔽提示 (会导致无法登录Google)

```bat
setx GOOGLE_API_KEY "no"
setx GOOGLE_DEFAULT_CLIENT_ID "no"
setx GOOGLE_DEFAULT_CLIENT_SECRET "no"
```

### 2. 配置Google API Key

1. https://cloud.google.com/console
2. 创建或选择已有项目 -> 左侧边栏 API和服务 -> 凭证
3. 创建凭证(类型为 “API 密钥”,名称随意, 不使用密钥限制,记住生成的key)
4. 再创建一个凭证(类型为 “OAuth 客户端 ID”, 名称随意, 应用类型选择 “其他”, 记住生成的 “客户端 ID” 和 “客户端密钥”)
5. 格式填写自己的 API Key

```bat
setx GOOGLE_API_KEY 生成的API密钥
setx GOOGLE_DEFAULT_CLIENT_ID 生成的客户端ID
setx GOOGLE_DEFAULT_CLIENT_SECRET 生成的客户端密钥
```

