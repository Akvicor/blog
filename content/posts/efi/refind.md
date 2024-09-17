---
title: "UEFI启动管理器rEFInd"
date: 2022-07-28 23:34:28 +08:00
draft: false
categories: [UEFI]
tags: []
card: false
weight: 0
---

## 安装rEFInd

[https://github.com/techysy/rEFInd](https://github.com/techysy/rEFInd)

```bash
sudo apt-get install refind
```

- 输入y继续执行，首次安装时，系统会询问你是否将rEFInd安装到ESP
- 关闭安全启动

## 安装主题

```bash
cd /boot/efi/EFI/refind
mkdir themes
cd themes
git clone https://github.com/kgoettler/ursamajor-rEFInd.git
```

## 屏蔽多余启动项

先注释`include themes/ursamajor-rEFInd/theme.conf`，在选择界面使用`delete`删除不需要的启动项

```bash
cd /boot/efi/EFI/refind
vim refind.conf
```

```text
uefi_deep_legacy_scan

menuentry "Windows 10" {
    icon \EFI\refind\themes\ursamajor-rEFInd\icons\os_win.png
    loader \EFI\Microsoft\Boot\bootmgfw.efi
}

dont_scan_dirs ESP:/EFI/Boot,/EFI/tools,/EFI/debian,/EFI/Microsoft,/Windows
include themes/ursamajor-rEFInd/theme.conf
```


