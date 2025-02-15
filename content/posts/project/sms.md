---
title: "Air780E+树莓派短信收发平台"
date: 2025-02-15 16:59:28 +08:00
draft: false
categories: [project]
tags: ["sms","air780e","go","golang"]
card: false
weight: 0
---

苦于国内的短信平台都需要指定发送模板, 没法发送高度自定义的消息, 且模板审核过于麻烦, 因此想要自己开发一套用于发送短信的平台

受 [此文章](https://www.chenxublog.com/2022/10/28/19-9-sms-forwarding-air780e-esp32c3.html) 的启发, 决定使用Air780E和树莓派制作一个能够**接收**和**发送**短信的程序

[项目代码 https://github.com/Akvicor/sms](https://github.com/Akvicor/sms)

## 功能

- 发送短信
- 接收短信
- Web界面, 支持查看发送和接收历史
- API接口, 直接通过调用API接口发送短信

## 所需材料

- Air780E
- 一张手机卡(最好按照自身需求开通短信包,这样发短信便宜)
- 树莓派

## 实现逻辑

1. 在树莓派上运行编写好的程序, 通过GPIO和Air780E通信, 提供HTTP的API接口来发送或查看收到的短信
2. Air780E收到短信后通过GPIO发送给树莓派, 交由树莓派进一步处理
3. 树莓派通过GPIO将需要发送的短信内容和手机号发送给Air780E, Air780E将短信发送给指定手机号

- 由于我还有一个海外手机卡, 因此你会在代码和配置中看到cn和us两个结尾的配置, 他们分别代表了国内和国外手机卡
- 对于Air780E如何上电自启以及GPIO定义可以看 [这篇博客](https://www.chenxublog.com/2022/10/28/19-9-sms-forwarding-air780e-esp32c3.html)
- 如何刷写固件及代码可以看 [官方文档](https://wiki.luatos.com/chips/air780e/mcu.html)

## 整体预览

由于我用了两张卡, 两个手机号, 因此使用了两个Air780E, 如果你只用一个手机号, 那么可以去掉一个

图中蓝色部分是串口数据线, 红色部分是5v供电线, Air780E上除了供电还有一个用于检测树莓派是否在线(由于本身供电就来自树莓派,因此无意义,不过最好还是连上)

![连线](https://img.akvicor.com/i/2025/02/15/67b07238dae87.jpg)

![Air780E使用的GPIO](https://img.akvicor.com/i/2025/02/15/67b07234bb88e.jpg)

![Air780E使用的GPIO](https://img.akvicor.com/i/2025/02/15/67b072337a9d4.jpg)

![整体](https://img.akvicor.com/i/2025/02/15/67b0723629997.jpg)

Web界面

![界面](https://img.akvicor.com/i/2025/02/15/67b07e3667f86.png)

收到和发出的短信的历史

![短信历史](https://img.akvicor.com/i/2025/02/15/67b07e3594c27.png)

## 注意事项

由于当初开发时没想过要开源出来, 因此很多地方的代码非常简陋, 部分功能需要的配置写死在文件中, 因此如果想获取除收发短信外的功能, 需要一定的代码知识, 手动修改代码

### 首先看配置文件

文件: `config.ini`

- `serial-cn`和`serial-us`这里定义了国内和国外手机卡使用的那个串口和Air780E通信, 具体树莓派中每个串口使用的是那个GPIO需要看自己树莓派版本的GPIO定义
- `server`定义了程序提供服务的端口
- `database`定义了数据库文件存放位置
- `log`定义了是否将log输出到文件, 以及log文件存放位置
- `security`定义了访问Web界面需要输入的用户名和密码, 以及访问API接口时使用的key

### Air780E中收到短信后自身执行的命令

文件: `air780e/main.lua`

这是写入Air780E的文件, 也就是Air780E用的代码

搜索代码中的手机号`12345678900`替换为自己日常使用的手机号(并非插在Air780E的手机号)

例如在收到`help`短信内容时Air780E会直接通过短信返回信息, 不再发送给树莓派处理

```lua
if txt == "help" then
    sms.send("+8612345678900", "[SMS][HELP]\nreboot - Reboot SMS\nstatus - SMS Status\ncstatus - SMS Current Status")
    return
end
```

同时Air780E在上电后也会向这个手机号发送一条短信, 表示程序正常

### 树莓派和Air780E的数据交换部分

文件: `serial/serial_cn.go`

下面代码和`main.lua`中的手机号一样, 都是自己日常使用的手机号

```go
const selfPhoneCN = "12345678900"
```

在`readCallbackCN`函数中有下面代码

主要说明最后一个`else if`, 由于树莓派本身运行了HA(Home Assistant这)是收到特定短信后, 通过HA重启路由器, 这样当家里断网时可以尝试通过短信重启路由器看看能否恢复正常

```go
if sms.Message == "hello" {
    if strings.Contains(sms.Phone, selfPhoneCN) {
        SendCN("sms", model.NewMSG(model.MsgTagSmsSend, model.NewSMSLong(sms.Phone, "Hello Akvicor! here is sms")))
    } else {
        SendCN("sms", model.NewMSG(model.MsgTagSmsSend, model.NewSMSLong(sms.Phone, "Hello! here is sms")))
    }
} else if sms.Message == "你好" {
    if strings.Contains(sms.Phone, selfPhoneCN) {
        SendCN("sms", model.NewMSG(model.MsgTagSmsSend, model.NewSMSLong(sms.Phone, "你好Akvicor！这里是sms")))
    } else {
        SendCN("sms", model.NewMSG(model.MsgTagSmsSend, model.NewSMSLong(sms.Phone, "你好！这里是sms")))
    }
} else if sms.Message == "ha.help" && strings.Contains(sms.Phone, selfPhoneCN) {
    SendCN("sms", model.NewMSG(model.MsgTagSmsSend, model.NewSMSLong(sms.Phone, "[HA][HELP]\nha.op.reboot - Reboot OP")))
} else if sms.Message == "ha.op.reboot" && strings.Contains(sms.Phone, selfPhoneCN) {
    _ = util.HttpPost("http://127.0.0.1/api/services/script/reboot_router", nil, util.HTTPContentTypeJson, map[string]string{"Authorization": "Bearer xxxxxxx"})
    SendCN("sms", model.NewMSG(model.MsgTagSmsSend, model.NewSMSLong(sms.Phone, "Reboot OP")))
}
```

## 编译运行

将代码下载到本地, 进入代码根目录执行

```shell
go mod tidy
go build -trimpath -ldflags "-s -w" -o sms
```

假设执行文件目录: `path/to/exec/sms`
假设数据文件目录: `path/to/data`
假设配置文件目录: `path/to/data/config.ini`

那么可以编写以下systemd脚本`/etc/systemd/system/sms.service`

```service
[Unit]
Description=sms
After=network.target auditd.service
ConditionFileIsExecutable=path/to/exec/sms

[Service]
User=root
StartLimitInterval=5
StartLimitBurst=10
ExecStart=path/to/exec/sms -c path/to/data/config.ini
WorkingDirectory=path/to/data
Restart=always
RestartSec=15
LimitNPROC=32768
LimitNOFILE=1048576
# CAP_NET_ADMIN:允许执行网络管理任务
# CAP_NET_BIND_SERVICE:允许绑定到小于1024的端口
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE

[Install]
WantedBy=multi-user.target
```

启用服务

```
systemctl enable sms
systemctl start sms
```
