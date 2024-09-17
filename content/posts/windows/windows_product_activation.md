---
title: "Windows Product Activation"
date: 2022-10-08 09:34:25 +08:00
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

## KMS激活Windows

1. 首先用管理员权限打开命令提示符
2. 卸载当前系统秘钥：`slmgr.vbs /upk`
3. 写入KMS的GVLK秘钥：`slmgr /ipk XXXXX-XXXXX-XXXXX-XXXXX-XXXXX`
4. 写入KMS服务器地址：`slmgr /skms home.akvicor.com`
5. 连接KMS服务器激活：`slmgr /ato`
6. 查看激活状态：`slmgr.vbs -dlv`

## KMS激活Office

1. 首先用管理员权限打开命令提示符
2. 切换到Office的安装路径，例如：`C:\Program Files\Microsoft Office\Office16`
3. 查看当前密钥最后五位数：`cscript ospp.vbs /dstatus`
4. 卸载当前密钥：`cscript ospp.vbs /unpkey:XXXXX`
5. 安装KMS密钥：`cscript ospp.vbs /inpkey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX`
6. 设置激活KMS服务器：`cscript ospp.vbs /sethst:home.akvicor.com`
7. 执行激活命令：`cscript ospp.vbs /act`

## Download Office

Office Deployment Tool简称ODT，是微软官方提供的Office部署工具。下载地址为：[https://www.microsoft.com/en-us/download/details.aspx?id=49117](https://www.microsoft.com/en-us/download/details.aspx?id=49117)或[http://pi.akvicor.com:7021/file?f=1284](http://pi.akvicor.com:7021/file?f=1284)

运行下载的程序，将内容解压到指定目录，并进入此目录

- configuration-Office365-x64：里面包含了用于安装64位版本的Office 365的默认配置信息。
- configuration-Office365-x86：里面包含了用于安装32位版本的Office 365的默认配置信息。
- configuration-Office2019Enterprise：里面包含了用于安装Office 2019专业增强版、企业版的默认配置信息。
- configuration-Office2021Enterprise：里面包含了用于安装Office 2021专业增强版、企业版的默认配置信息。

可以使用官方配置工具生成配置文件

- [https://config.office.com/](https://config.office.com/)
- [https://config.office.com/deploymentsettings](https://config.office.com/deploymentsettings)

也可以手动编辑configuration-Office2021Enterprise.xml文件

- `OfficeClientEdition="64"`意思是64位版本。
- `Product ID="ProPlus2019Volume"`意思为Office 2019专业增强版、企业版。
- `Product ID="VisioPro2019Volume"`为Visio 2019专业版、企业版。
- `Product ID="ProjectPro2019Volume"`为Project 2019专业版、企业版。
- Visio和Project均为Office家族中的软件。但是Visio和Project均是单独发布的Office产品，不包含在Office 2019专业增强版中，因此配置信息会单独列出。
- `Language ID="en-us"`代表语言版本为美式英语，如果你需要简体中文，则需要将其中的en-us修改为`zh-cn`，你还可以将en-us改为`MatchOS`，意思是让程序自动匹配系统语言版本，如果你的系统为英文版，则安装英文版，如果为中文版，则安装中文版。
- `<ExcludeApp ID="Publisher" />`代表不安装Publisher，同样还可以设置OneDrive、Outlook、Access、Skype、OneNote。

```xml
<!-- Office 2021 enterprise client configuration file sample. To be used for Office 2021 
     enterprise volume licensed products only, including Office 2021 Professional Plus,
     Visio 2021, and Project 2021.

     Do not use this sample to install Office 365 products.

     For detailed information regarding configuration options visit: http://aka.ms/ODT. 
     To use the configuration file be sure to remove the comments

     The following sample allows you to download and install Office 2021 Professional Plus,
     Visio 2021 Professional, and Project 2021 Professional directly from the Office CDN.

     This configuration file will remove all other Click-to-Run products in order to avoid
     product conflicts and ensure successful setup.
 -->



<Configuration>

  <Add OfficeClientEdition="64" Channel="PerpetualVL2021">
    <Product ID="ProPlus2021Volume">
      <Language ID="zh-cn" />
      <Language ID="en-us" />
      <ExcludeApp ID="Lync" />
	  <ExcludeApp ID="Groove"/>
	  <ExcludeApp ID="Bing"/>
      <ExcludeApp ID="Publisher" />
      <ExcludeApp ID="OneDrive" />
      <ExcludeApp ID="Outlook" />
      <ExcludeApp ID="Teams" />
      <ExcludeApp ID="Access" />
      <ExcludeApp ID="Skype" />
      <ExcludeApp ID="OneNote" />
    </Product>
    <Product ID="VisioPro2021Volume">
      <Language ID="zh-cn" />
      <Language ID="en-us" />
      <ExcludeApp ID="Lync" />
	  <ExcludeApp ID="Groove"/>
	  <ExcludeApp ID="Bing"/>
      <ExcludeApp ID="Publisher" />
      <ExcludeApp ID="OneDrive" />
      <ExcludeApp ID="Outlook" />
      <ExcludeApp ID="Teams" />
      <ExcludeApp ID="Access" />
      <ExcludeApp ID="Skype" />
      <ExcludeApp ID="OneNote" />
    </Product>
    <Product ID="LanguagePack">
      <Language ID="zh-cn" />
      <Language ID="en-us" />
    </Product>
  </Add>

  <Remove All="True" />

  <!--  <RemoveMSI All="True" /> -->

  <!--  <Display Level="None" AcceptEULA="TRUE" />  -->

  <!--  <Property Name="AUTOACTIVATE" Value="1" />  -->

</Configuration>
```

在此目录打开PowerShell，执行`cmd`，然后执行`setup.exe /download configuration-Office2021Enterprise.xml`开始下载。

执行`setup.exe /configure configuration-Office2021Enterprise.xml`开始安装

