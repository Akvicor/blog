---
title: "Create Systemd Service Unit"
date: 2023-02-27 07:01:47 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## 文件所在路径

```shell
vim /usr/lib/systemd/system/SERVICE_NAME.service
```

## 服务模板

```
[Unit]
Description=Viry Service
After=network.target auditd.service

[Service]
User=root
Type=oneshot
RemainAfterExit=true
ExecStart=/viry/serv/serv.sh
ExecStop=/bin/true

[Install]
WantedBy=multi-user.target
Alias=viry.service
```

## serv.sh

```shell
#!/bin/bash

echo "Sync Time"
ntpdate 172.16.1.1
hwclock -w

TIME=$(TZ=UTC-8 date "+%Y-%m-%d %H:%M:%S")
LOG="/viry/serv/serv.log"

echo "Ready"
echo "" >> $LOG
echo $TIME >> $LOG

echo "Start DEMO"
echo "Start DEMO" >> $LOG
sh /viry/serv/demo/demo.sh

echo "Finished"
echo "Finished" >> $LOG
```

## demo.sh

```shell
#!/bin/bash

cd /viry/serv/demo/exec/

screen_name="demo"

screen -s /usr/bin/bash -dmS $screen_name

cmd1=""
cmd2="./demo"

screen -x -S $screen_name -p 0 -X stuff "$cmd1\n"
screen -x -S $screen_name -p 0 -X stuff "$cmd2\n"

echo "Demo Started"

exit 0
```



