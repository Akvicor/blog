---
title: "LibreSpeed"
date: 2022-11-17 08:52:54 +08:00
draft: false
categories: [Open Source]
tags: []
card: false
weight: 0
---

## Compile from source

```shell
git clone github.com/librespeed/speedtest-go
cd speedtest-go
go build -ldflags "-w -s" -trimpath -o speedtest main.go
```

## Setting

将`settings.toml`文件中的`assets_path`设置为`web/assets`目录（也可以将`assets`目录和可执行文件移动到其他位置）

## Modify

- 生成图片的水印定义在`results/telemetry.go`中的`watermark`变量
- 主页标题在`web/assets/index.html`文件中。

