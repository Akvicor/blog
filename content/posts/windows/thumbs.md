---
title: "禁止Windows系统生成Thumbs.db缩略图缓存"
date: 2023-09-19 14:16:24 +08:00
draft: false
categories: [Windows]
tags: []
card: false
weight: 0
---

1. 在Win10系统下，按住键盘的 **“Win+R”** 组合快捷键，系统将会打开“运行”命令对话窗口。
2. 在打开的运行命令对话框中输入 **“gpedit.msc”** 命令，然后再点击“确定”按钮。
3. 点击确定按钮后，这个时候会打开 **“本地组策略编辑器”** 对话窗口，
4. 在本地组策略编辑器窗口的左侧小窗口中，依次展开 **“用户配置-->管理模版-->Windows组件”** 选项。
5. 在“Windows组件”选项右侧窗口，找到 **“文件资源管理器”** 选项选中并双击鼠标左键将其打开。
6. 进去到文件资源管理器中，找到 **“关闭隐藏的 thumbs.db 文件中的缩略图缓存”** 并双击鼠标左键将其打开。
7. 在打开的“关闭隐藏的 thumbs.db 文件中的缩略图缓存”对话窗口中，将其设置更改为 **“已启用”** 选项，然后在点击“应用-->确定”按钮退出即可。