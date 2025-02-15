---
title: "MSG 信息发送平台攻略"
date: 2025-02-15 20:57:00 +08:00
draft: false
categories: [project]
tags: ["msg","sms","go","golang"]
card: false
weight: 0
---

这是一篇指导如何使用和开发MSG的博客

## 概念介绍

- 平台: 例如SMS,邮箱,Telegram,微信
- Bot: 每个bot都可以支持一个或多个平台, 但每种平台只能绑定一个
- 通知渠道: 将Bot,平台以及发送目标绑定在一起, 同时可以给渠道设置标记(全局唯一), 发送信息时可以通过渠道ID或渠道标记向对应的渠道发送信息

## 相关项目

- [https://github.com/Akvicor/msg](https://github.com/Akvicor/msg) 项目本体
- [https://github.com/Akvicor/gmsg](https://github.com/Akvicor/gmsg) 一个Go Mod, 可以快速调用已经搭建的MSG来发送信息
- [https://github.com/Akvicor/msg-cmd](https://github.com/Akvicor/msg-cmd) 一个命令行应用, 可以快速调用已经搭建的MSG来发送信息
- [https://github.com/Akvicor/sms](https://github.com/Akvicor/sms) SMS平台, 用来发送短信

## 界面介绍

### 用户中心

可以在这里面修改密码和头像什么的

![user-center](https://img.akvicor.com/i/2025/02/15/67b08ef5b0c15.png)

### 访问密钥

通过API访问时需要使用这个

![access-token](https://img.akvicor.com/i/2025/02/15/67b08ef68c0f1.png)

### 发送渠道

实际的发送渠道, 将信息接受者绑定到平台和Bot

![channel](https://img.akvicor.com/i/2025/02/15/67b08ef768bc9.png)

发送渠道的 **目标**

- SMS/短信: 手机号
- Mail/邮件: 邮箱地址
- Telegram: 会话ID(添加对应机器人后, 通过发送`/get_id`获取到的内容)
- 微信: 用户在企业微信中的名称

## 搭建

Docker快速搭建

如需自己编译镜像, 可以直接使用项目中的`Dockerfile`编译, 编译完成后替换后续`docker-compose.yml`中的镜像名称

通过`docker-compose.yml`快速搭建

### Sqlite

需要修改数据存储的目录

```yml
services:
  msg:
    image: akvicor/msg:v0.1.6
    restart: always
    volumes:
      - /path/to/msg/data/msg-data:/data
    ports:
      - 3000:3000
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/api/sys/info/health"]
      interval: 10s
      timeout: 5s
      retries: 5
```

### Postgres

需要修改数据存储的目录, postgres的密码

```yml
services:
  msg:
    image: akvicor/msg:v0.1.6
    restart: always
    volumes:
      - /path/to/msg/data/msg-data:/data
    ports:
      - 3000:3000
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/api/sys/info/health"]
      interval: 10s
      timeout: 5s
      retries: 5
    depends_on:
      db:
        condition: service_healthy

  db:
    image: postgres:17.2
    restart: always
    volumes:
      - /path/to/msg/data/db-data:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=msg
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres -d msg"]
      interval: 10s
      timeout: 5s
      retries: 5
```

启动

```
docker compose -p msg up -d
```

对于使用postgres的用户, 需要在启动后修改`/path/to/msg/data/msg-data/config.toml`

将

```toml
[database]
# sqlite, postgres
type = 'sqlite'
```

改为

```toml
[database]
# sqlite, postgres
type = 'postgres'
```

然后重启docker容器

```
docker compose -p msg restart
```

### 配置

Bot部分放在介绍**默认Bot Maid**时介绍

#### server

这是配置web服务域名和端口的

#### database

配置数据库, 目前仅支持sqlite和postgres两种

**sqlite**: 仅type和file生效
**postgres**: 如果是按照上面的`docker-compose.yml`启动的服务, 那么host可以直接使用`db`

#### encrypt

每次生成配置文件时, 都会随机生成`key`和`iv`

#### log

这是日志配置, 默认不会写入文件

#### cron

这是定时执行某个任务时需要的配置, 但这个功能比较私人, 我在开放的版本中清空了这部分

## 默认Bot Reminder

这个bot仅支持微信企业应用, 当然你也可以自己在代码中添加支持其他平台. {{< kidding text="毕竟我现在用的就是微信企业应用, 懒得继续加平台" >}}

下面这张图就完整的概括了Reminder的功能, 这是一个让MSG定时向你发送信息的功能, 用于提醒自己指定时间该干啥

![Reminder](https://img.akvicor.com/i/2025/02/15/67b0929d8c1e3.jpg)

如需使用Reminder, 请手动添加一个绑定Reminder的通知渠道, 这个通知渠道绑定的发送目标才能使用Reminder功能

## 默认Bot Maid (包含如何配置每种平台的示例)

这个bot支持目前所有平台, 但默认情况下仅支持发送信息, 如果你想支持收到消息后自动进行某些操作, 那你需要去代码的指定接收函数中编写对应功能

### 配置 SMS

这个平台是自己搭建的, 项目地址 [https://github.com/Akvicor/sms](https://github.com/Akvicor/sms)

将这个项目搭建后开放出来的URL以及Token配置到这里后, maid就可以通过sms发送短信了

- `enable-sender`: 是否启用发送短信功能, 默认为`false`, 如需启用需修改为`true`
- `enable-receiver`: 目前并未实现接受短信功能

```toml
[bot.maid.sms]
debug = false
enable-receiver = false
enable-sender = false
api = 'https://example.com'
token = ''
```

### 配置 Mail

如何搭建邮件服务器可以看 [这篇博客](https://blog.akvicor.com/posts/mail/install/)

- `enable-smtp`: 是否启用发送邮件功能, 默认为`false`, 如需启用需修改为`true`
- `enable-imap`: 是否启用接收邮件功能, 默认为`false`, 如需启用需修改为`true`, 如需进一步处理接收到的邮件, 需要自行修改代码

发送邮件需要SMTP, 接收邮件需要IMAP

```toml
[bot.maid.mail]
debug = false
enable-imap = false
host-imap = 'https://imap.example.com'
port-imap = 993
enable-smtp = false
host-smtp = 'https://smtp.example.com'
port-smtp = 465
from = 'MSG Backend <maid@example.com>'
username = 'maid@example.com'
password = ''
```

### 配置 Telegram

- `enable-sender`: 是否启用发送信息功能, 默认为`false`, 如需启用需修改为`true`
- `enable-receiver`: 是否启用接收信息功能, 默认为`false`, 如需启用需修改为`true`, 如需进一步处理接收到的信息, 需要自行修改代码

token处填写机器人的Token, 如何创建机器人请自行Google或咨询AI

当机器人被添加到对话后, 发送`/get_id`可以获取当前会话的ID, 这个ID就是本程序的**发送渠道**中的Telegram的渠道**目标**

```toml
[bot.maid.telegram]
debug = false
enable-receiver = false
enable-sender = false
api = 'https://api.telegram.org/bot%s/%s'
token = ''
```

### 配置 微信

- `enable-sender`: 是否启用发送信息功能, 默认为`false`, 如需启用需修改为`true`
- `enable-receiver`: 是否启用接收信息功能, 默认为`false`, 如需启用需修改为`true`, 如需进一步处理接收到的信息, 需要自行修改代码
- `corp-id`: 企业ID
- `secret`: 企业应用的Secret
- `agent-id`: 企业应用的ID
- `token`: 应用收到消息的回调所需的token (receiver使用)
- `aes-key`: 应用收到消息的回调所需的加密密钥 (receiver使用)

receiver还需要在企业微信中配置应用的回调URL, 例如本应用的域名是`msg.example.com`, bot是`maid`, 那么URL就需要填写`https://msg.example.com/api/bot/maid/wechat/callback`

```toml
[bot.maid.wechat]
debug = false
enable-receiver = false
enable-sender = false
corp-id = ''
secret = ''
agent-id = 0
token = ''
aes-key = ''
```

## API调用发送

通过API可以绕过**发送渠道**直接通过**Bot**发送给**指定的目标**, 无需先添加发送渠道, 再通过发送渠道发送

### `/api/send` 发送

支持`POST/GET`

参数如下

- `id`: 发送渠道的**ID**
- `sign`: 发送渠道的**标记**
- `type`: 消息类型(text/textcard/markdown/html), 由于部分消息类型仅支持特定平台, 因此在不支持的平台会自动变更为其他类型
- `at`: 秒级Unix时间戳, 信息期望的发送时间
- `title`: 信息标题
- `msg`: 信息内容

其中`id`和`sign`提供一个即可, `title`和`msg`至少填写一个

### `/send/cancel` 取消

在消息还未发送出去时 (未达到指定发送时间), 取消发送此消息

- `id`: 消息的**ID**

### 其他API

请在`cmd/app/server/app/server.go`查询

### 示例: maid

每个bot的API请在`cmd/app/server/bot/bot.go`查询, 通过这些API可以直接调用指定bot发送信息

- `/api/bot/maid/sms/send`: `POST/GET` 参数为`phone`,`msg`
- `/api/bot/maid/mail/send`: `POST/GET` 参数为`to`,`subject`,`type`,`msg`
- `/api/bot/maid/telegram/send`: `POST/GET` 参数为`chat`,`mode`,`msg`
- `/api/bot/maid/wechat/send`: `POST/GET` 参数为`touser`,`type`,`title`,`msg`,`url`,`btn`
- `/api/bot/maid/wechat/callback`: 企业微信回调

### 示例: reminder

- `/api/bot/reminder/wechat/send`: `POST/GET` 参数为`touser`,`type`,`title`,`msg`,`url`,`btn`
- `/api/bot/reminder/wechat/callback`: 企业微信回调

## 使用简易教程

以Maid为例, 仅提供创建所需参数介绍

### SMS

在发送渠道这里, 点击创建

如图

- 标记: 我将此渠道的标记配置为`channel_sms_1`, 这样我就可以用此标记替代通知渠道ID来代指某个通知渠道, 这个可以随意填写, 但无法创建已存在的字符串(包括存在于其他人账号中的)
- 名称: 名称随意
- Bot: 这个演示里我用 **Maid** 作为发送信息的机器人
- 类型: 我希望用这个Bot发送 **短信**
- 目标: 我希望用这个Bot给 **10086** 发送短信

![1](https://img.akvicor.com/i/2025/02/15/67b0a573defa0.png)

### 邮件

在发送渠道这里, 点击创建

如图

- 标记: 我将此渠道的标记配置为`mail_1`, 这样我就可以用此标记替代通知渠道ID来代指某个通知渠道, 这个可以随意填写, 但无法创建已存在的字符串(包括存在于其他人账号中的)
- 名称: 名称随意
- Bot: 这个演示里我用 **Maid** 作为发送信息的机器人
- 类型: 我希望用这个Bot发送 **邮件**
- 目标: 我希望用这个Bot给 **akvicor@akvicor.com** 发送邮件

![1](https://img.akvicor.com/i/2025/02/15/67b0a6cb0fab1.png)

### Telegram

在发送渠道这里, 点击创建

如图

- 标记: 我将此渠道的标记配置为`tg`, 这样我就可以用此标记替代通知渠道ID来代指某个通知渠道, 这个可以随意填写, 但无法创建已存在的字符串(包括存在于其他人账号中的)
- 名称: 名称随意
- Bot: 这个演示里我用 **Maid** 作为发送信息的机器人
- 类型: 我希望用这个Bot发送 **Telegram**
- 目标: 我希望用这个Bot给 **-123456789** 发送邮件(通过在会话中发送`\get_id`获取,适用于与bot的私聊,和bot共同的Group和Channel)

![1](https://img.akvicor.com/i/2025/02/15/67b0a73715b76.png)

### 微信

在发送渠道这里, 点击创建

如图

- 标记: 我将此渠道的标记配置为`wechat`, 这样我就可以用此标记替代通知渠道ID来代指某个通知渠道, 这个可以随意填写, 但无法创建已存在的字符串(包括存在于其他人账号中的)
- 名称: 名称随意
- Bot: 这个演示里我用 **Maid** 作为发送信息的机器人
- 类型: 我希望用这个Bot发送 **微信**
- 目标: 我希望用这个Bot给 **Akvicor** 发送邮件(通过在会话中发送`\get_id`获取,适用于与bot的私聊,和bot共同的Group和Channel)

![1](https://img.akvicor.com/i/2025/02/15/67b0a82414eb6.png)

发送目标来源

![1](https://img.akvicor.com/i/2025/02/15/67b0a87f947a3.png)

## 开发

目录`cmd/app/server/bot`下存放各类平台和bot

### 平台

- `cmd/app/server/bot/mail` 收发邮件相关的代码
- `cmd/app/server/bot/sms` 收发短信相关的代码
- `cmd/app/server/bot/telegram` 收发Telegram相关的代码
- `cmd/app/server/bot/wechat` 收发微信相关的代码

### Maid

`cmd/app/server/bot/maid.go`内就是Maid这个bot相关的功能

- 开启接收邮件后: `mailReceiver`函数用于处理接收到的所有邮件, 代码默认不对邮件内容做出任何操作
- 开启接收Telegram后: `telegramReceiver`函数用于处理接收到的所有电报信息, 函数简单的对每种类型信息做了区分, 如果想要对应功能则自行在对应的信息下编写功能, 代码默认不对信息内容做出任何操作
- 开启接收微信后: `wechatReceiver`函数用于处理接收到的所有微信消息, 代码默认不对消息内容做出任何操作

### Reminder

`cmd/app/server/bot/reminder.go`内就是Reminder这个bot相关的功能

开启接收微信后: `wechatReceiver`函数用于处理接收到的所有微信消息, 这个函数提供了Reminder的定时发送信息功能

