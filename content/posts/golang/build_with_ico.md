---
title: "go编译文件带上图标"
date: 2023-06-11 01:07:09 +08:00
draft: false
categories: [Golang]
tags: []
card: false
weight: 0
---

默认的`go build -o xxx.exe`这样是没有图标的，不怎么好看。

首先下载文件：

```shell
git clone  https://github.com/akavel/rsrc.git
```

进入目录,把上面的代码编译一下

```shell
go build rsrc.go # 然后有个rsrc.exe文件
```

就在rsrc的目录下创建个`ico.manifest`，内容如下：

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
<assemblyIdentity
    version="1.0.0.0"
    processorArchitecture="x86"
    name="controls"
    type="win32"
></assemblyIdentity>
<dependency>
    <dependentAssembly>
        <assemblyIdentity
            type="win32"
            name="Microsoft.Windows.Common-Controls"
            version="6.0.0.0"
            processorArchitecture="*"
            publicKeyToken="6595b64144ccf1df"
            language="*"
        ></assemblyIdentity>
    </dependentAssembly>
</dependency>
</assembly>
```

然后把你想要编译带上的ico文件复制到rsrc文件夹，例如 rc.ico

执行命令生成：xxx.syso文件

```shell
rsrc.exe  -manifest ico.manifest -ico rc.ico -o xxx.syso
```

把xxx.syso文件复制到你要编译的项目

执行编译 `go build -o main.exe` 正常编译即可

在这里编译需要注意的是：不能指定编译某个go文件如：go build -o main.exe main.go 这样是不会带上图标的，直接这样编译  go build -o main.exe

