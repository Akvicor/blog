---
title: "Wallet 资金管理/订阅管理"
date: 2025-02-15 23:49:00 +08:00
draft: false
categories: [project]
tags: ["wallet","go","golang"]
card: false
weight: 0
---


这是一篇指导如何使用和开发Wallet的博客

{{< kidding title="指挖坑" text="详细的内容以后再说吧, 今天连着开源了三个项目, 写了三篇博客, 燃尽了" >}}

![64ce148cec784.gif](https://img.akvicor.com/i/2025/02/16/67b0c8224bb97.gif)
![64ce146497524.gif](https://img.akvicor.com/i/2025/02/16/67b0c82167416.gif)

## 相关项目

- [https://github.com/Akvicor/wallet](https://github.com/Akvicor/wallet) 项目本体

## 界面预览

- 多用户
- 自定义银行,货币类型,银行卡类型
- 保存银行卡信息
- 自定义汇率
- 自定义交易分类(收入/支出/转账/兑换下的子分类
- 创建钱包, 以及钱包下的划分, 每个划分绑定到某张银行卡
- 创建愿望单, 与钱包功能一致
- 创建债务, 与钱包功能一致
- 创建订阅, 自定义付费周期, 自动计算下次付费时间, 下次付费前通过短信或邮件提醒, 自动计算每天/月/年的订阅开支
- 开支图表, 日历形式展示每天/月的开支, 饼状图展示区间内的开支占比, 折线图展示区间内的开支占比
- 通过短信或邮件发送通知

![2025-02-16-004955_1871x779_scrot.png](https://img.akvicor.com/i/2025/02/16/67b0c60cce4de.png)
![2025-02-16-005049_1873x623_scrot.png](https://img.akvicor.com/i/2025/02/16/67b0c60aaa42f.png)
![2025-02-16-005055_1881x633_scrot.png](https://img.akvicor.com/i/2025/02/16/67b0c609c72e5.png)
![2025-02-16-005102_1885x862_scrot.png](https://img.akvicor.com/i/2025/02/16/67b0c6085955a.png)
![2025-02-16-005106_1874x757_scrot.png](https://img.akvicor.com/i/2025/02/16/67b0c606e0c86.png)
![2025-02-16-005109_1868x803_scrot.png](https://img.akvicor.com/i/2025/02/16/67b0c605c3d71.png)

## 钱包逻辑

### 钱包和交易

创建钱包时需要选择一张银行卡作为第一个划分绑定的银行卡, 后续可以创建其他绑定不用银行卡的划分.

同一张一行卡可以绑定多个划分, 同一个划分只能属于一个钱包.

每次记录交易时, 都需要选择钱包下的划分, 对划分中资金的操作会影响到绑定银行卡的资金(即所有绑定了同一张银行卡的划分的余额总和就是银行卡的余额)

### 愿望单和债务

这两个本质上就是钱包, 将划分的名字命名为购买物品的名字, 通过设置目标金额来表示物品价值, 从其他钱包往此愿望划分中转账, 表示资金进度. 债务同理

### 订阅

支持配置为 每天/每周/每月/每季度/每年/每隔n天/每隔n月/每隔n年

当为某个订阅支付后, 点击更新按钮即可将订阅的提醒时间更新到下次的时间

## 搭建

Docker快速搭建

如需自己编译镜像, 可以直接使用项目中的`Dockerfile`编译, 编译完成后替换后续`docker-compose.yml`中的镜像名称

通过`docker-compose.yml`快速搭建

需要修改数据存储的目录

```yml
services:
  wallet:
    image: akvicor/wallet:v0.2.20
    restart: always
    ports:
      - "3000:3000"
    volumes:
      - "/path/to/data:/data"
```

启动

```
docker compose -p wallet up -d
```

## 配置

### 配置文件

如果`docker-compose.yml`中配置的目录是`/path/to/data`, 那么配置文件应在`/path/to/data/config.toml`

如果需要接收通知提醒, 例如 订阅付费到期提醒, 银行卡账单日和还款日提醒 需要配置`mail`和`sms`, 其中sms是 [这篇博客中搭建的](https://blog.akvicor.com/posts/project/sms/)

### 用户配置

要想收到邮箱或短信, 除了在配置文件中配置, 还需要在网站中为用户添加邮箱地址和手机号
