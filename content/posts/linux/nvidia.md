---
title: "Nvidia 显卡驱动"
date: 2024-01-03 20:25:07 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

Nvidia 驱动在Linux下, 必须安装 Xorg

## 查看显卡信息

```bash
lspci -k | grep -EA3 'VGA|3D|Display'
lspci | grep VGA
lspci | grep -i nvidia
# 查看指定显卡的详细信息用以下指令
lspci -v -s 00:0f.0
# Nvidia自带一个命令行工具可以查看显存的使用情况：
nvidia-smi
# Fan：显示风扇转速，数值在0到100%之间，是计算机的期望转速，如果计算机不是通过风扇冷却或者风扇坏了，显示出来就是N/A； 
# Temp：显卡内部的温度，单位是摄氏度；
# Perf：表征性能状态，从P0到P12，P0表示最大性能，P12表示状态最小性能；
# Pwr：能耗表示； 
# Bus-Id：涉及GPU总线的相关信息； 
# Disp.A：是Display Active的意思，表示GPU的显示是否初始化； 
# Memory Usage：显存的使用率； 
# Volatile GPU-Util：浮动的GPU利用率；
# Compute M：计算模式； 

# 如果要周期性的输出显卡的使用情况，可以用watch指令实现
watch -n 10 nvidia-smi

# 检测显卡驱动是否正常
sudo apt-get install hwinfo
hwinfo --display
```

## 安装核显驱动

Intel 核显不需要单独安装驱动，系统自带

```bash
# intel 根据驱动官方说法，有功能上的区别。non-free版本是全功能构建版本。
sudo apt install intel-media-va-driver-non-free
sudo apt install intel-media-va-driver
# amd
firmware-amd-graphics
```

## 安装Nvidia驱动

安装前，可以先安装nvidia的工具检测一下：

```bash
sudo apt install nvidia-detect
nvidia-detect
sudo apt install nvidia-driver
# 安装cuda
sudo apt install nvidia-cuda-dev nvidia-cuda-toolkit
# 显卡信息查看工具
sudo apt install nvidia-smi
```

如果出现以下错误是因为Secure Boot，在主板的Bios设置中把这个选项关闭就可以了。

```
NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running.
```




