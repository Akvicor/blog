---
title: "Debian Install"
date: 2022-08-09 12:14:48 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

Debian安装说明

## Debian 源

```
wget https://paste.akvicor.com/api/106
```

## Debian 12 + i3

### 修改时区

```bash
# 修改时区
ln -sf /usr/share/zoneinfo/Etc/GMT-8 /etc/localtime
# 同步硬件时间
hwclock -w
```

### 配置源

```bash
curl https://paste.akvicor.com/api/101 > /etc/apt/sources.list
# 注释掉不需要用到的源，如 bookworm-proposed-updates(介于stable和testing之间), bookworm-backports(新特性移植到旧版本)

apt update
apt upgrade
reboot
```

### 安装基础工具

```bash
apt install apt-transport-https ca-certificates
apt install vim curl wget git gcc g++ make screen bc jq zsh
```

### 添加基础bash命令

```bash
# 添加ll命令.bashrc
alias l='ls -al --color=auto'
alias ll='ls -alh --color=auto'
```

### 笔记本电源操作

```bash
vim /etc/systemd/logind.conf
 HandleLidSwitch=ignore # 合上屏幕
 HandlePowerKey=ignore # 电源按键
```

### 修改启动展示信息

**ssh登录信息**

```bash
# 修改登录显示信息
vim /etc/update-motd.d/10-uname
 # 注释掉原有内容，添加以下内容
 printf "\033c"
# 清空motd内容
vim /etc/motd
```

**关闭grub引导界面**

```bash
# 关闭grub引导界面
vim /etc/default/grub
 修改GRUB_TIMEOUT为0
 GRUB_TIMEOUT=0
# 更新
update-grub
reboot
```

### 为用户添加sudo权限

```bash
# 为用户添加sudo权限
apt install sudo
cd /etc/sudoers.d
vim user
 # 填入以下内容
 akvicor ALL=(ALL)NOPASSWD:ALL
```

### 配置root用户的git

#### 修改git用户信息

```bash
git config --global user.name "Akvicor"
git config --global user.email akvicor@akvicor.com
```

或

```bash
git config --global --edit
```

#### 修改git默认编辑器

```bash
git config --global core.editor vim
```

### 支持SMB挂载

```bash
apt install cifs-utils
mkdir /smb/HHDx
mount -t cifs -o username=root //172.16.1.1/HHDx /smb/HHDx
```

### 安装桌面环境

```bash
apt install xorg i3 i3blocks terminator libnotify-bin notify-osd
```

### 配置sddm刷新率

```bash
vim /usr/share/sddm/scripts/Xsetup
```

增加下面内容

```bash
xset r rate 200 30
```

### 配置桌面环境

```bash
# 配置桌面环境
cp /etc/X11/xinit/xinitrc ~/.xinitrc
 # 添加以下内容到 ~/.xinitrc
 exec i3
```

### 添加快捷命令

```bash
# 添加以下内容到 .bashrc / .zshrc
alias l='ls -al --color=auto'
alias ll='ls -alh --color=auto'
alias ui='startx'
```

### 安装字体

#### 从备份恢复

```bash
# 下载fonts.tgz https://via.akvicor.com/file?f=1581
mv fonts.tgz /usr/local/share/fonts
cd /usr/local/share/fonts
tar -zxvf fonts.tgz
rm fonts.tgz
 # 更新字体缓存
fc-cache -fv
```

#### 字体配置文件

将以下文件放在`~/.config/fontconfig`中，以改变字体生效顺序，防止出现不必要的乱码或识别错误

```xml
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "urn:fontconfig:fonts.dtd">
<fontconfig>

<!-- Default system-ui fonts -->
<match target="pattern">
  <test name="family">
    <string>system-ui</string>
  </test>
  <edit name="family" mode="prepend" binding="strong">
    <string>sans-serif</string>
  </edit>
</match>

<!-- Default sans-serif fonts-->
<match target="pattern">
  <test name="family">
    <string>sans-serif</string>
  </test>
  <edit name="family" mode="prepend" binding="strong">
    <string>Noto Sans SC</string>
    <string>Font Awesome 6 Pro</string>
  </edit>
</match>

<!-- Default serif fonts-->
<match target="pattern">
  <test name="family">
    <string>serif</string>
  </test>
  <edit name="family" mode="prepend" binding="strong">
    <string>Noto Serif SC</string>
    <string>Noto Serif</string>
  </edit>
</match>

<!-- Default monospace fonts-->
<match target="pattern">
  <test name="family">
    <string>monospace</string>
  </test>
  <edit name="family" mode="prepend" binding="strong">
    <string>JetBrainsMono Nerd Font Mono</string>
    <string>Hack Nerd Font Mono</string>
  </edit>
</match>

</fontconfig>
```

#### Awesome字体

在这里面搜索字体并使用

[https://fontawesome.com/search](https://fontawesome.com/search)

```shell
# 执行以下命令更新字体缓存
fc-cache -fv
# 若字体文件未生效，切换至用户，执行以下命令删除旧缓存
rm -rf ~/.cache/fontconfig/
# fontawesome-pro_v6.4.0_package.zip
# 将otf字体放到系统指定目录中使用
https://via.akvicor.com/file?f=1496
# 全部字体，可自选放入系统中，配合fontconfig设置每个字体的顺序
https://via.akvicor.com/file?f=1497
# 修改i3默认字体为Google Noto Sans
```

#### 常用命令

```bash
#lists fonts
fc-list
# show an ordered list of fonts matching a certain name or pattern
fc-match -s helvetica
# rebuilds cached list of fonts (in `~/.cache/fontconfig`, older caches may also be in `~/.fontconfig`)
fc-cache -fv
```

### 配置User用户的git

#### 修改git用户信息

```bash
git config --global user.name "Akvicor"
git config --global user.email akvicor@akvicor.com
```

或

```bash
git config --global --edit
```

#### 修改git默认编辑器

```bash
git config --global core.editor vim
```

### 安装中文输入法

```bash
apt install fcitx5 fcitx5-pinyin
fcitx5-configtool # 添加pinyin输入法
im-config # 修改默认输入法，如果报错安装 zenity
```

### 安装环境

#### Golang

#### Node

#### Rust

#### Python

### 安装拓展工具

#### rdesktop

Windows远程桌面

**安装**

```bash
apt install rdesktop
```

#### xclip

剪切板

**安装**

```bash
apt install xclip
```

#### xbacklight

屏幕背光亮度

**安装**

```bash
apt install xbacklight
```

#### picom

窗口背景透明

**安装**

```bash
apt install picom
```

#### !废弃 compton

窗口背景透明

**安装**

```bash
apt install compton
```

#### light

屏幕背光亮度

**安装**

```bash
apt install light
```

#### rofi

应用程序窗口选择器，运行对话框

**安装**

```bash
apt install rofi
```

#### feh

图片查看器

**安装**

```bash
apt install feh
```

#### poppler-utils

PDF工具集

**安装**

```bash
apt install poppler-utils
```

#### caca-utils

命令行图形库

**安装**

```bash
apt install caca-utils
```

#### highlight

高亮显示源代码的命令行工具

**安装**

```bash
apt install highlight
```

#### atool

归档文件管理工具

**安装**

```bash
apt install atool
```

#### imagemagick

图像处理工具集

**安装**

```bash
apt install imagemagick
```

#### ethtool

网络管理工具

**安装**

```bash
apt install ethtool
```

#### scrot

截图工具

**安装**

```bash
apt install scrot
```

#### ranger

命令行文件管理工具

**安装**

```bash
apt install ranger
```

#### alsa-utils

声音控制

**安装**

```bash
apt install alsa-utils
```

#### !废弃 pulseaudio, pavucontrol

声音控制

**安装**

```bash
apt install pulseaudio
apt install pavucontrol
```

#### smartmontools

硬盘管理工具

**安装**

```bash
apt install smartmontools
```

**用法**

```bash
sudo smartctl -A /dev/sda
sudo smartctl -H /dev/sda
```

#### tcptrack

Monitor TCP connections on the network

**安装**

```bash
sudo apt-get install tcptrack
```

**用法**

```bash
tcptrack -i wlp2s0
```

#### proxychains

通过代理运行程序

**安装**

```bash
apt-get install proxychains

sudo find / -name "libproxychains.so.3" # get path
sudo vim /usr/bin/proxychains # change
```

**用法**

```bash
sudo proxychains apt-get update
proxychains google-chrome
```

#### privoxy

Convert socks to http proxy

**安装**

```bash
sudo apt-get install privoxy

sudo vim /etc/privoxy/config
# port: 8118
# forward-socks5 / 127.0.0.1:1080 .
```

**用法**

```bash
sudo systemctl start privoxy
```

#### uGet + aria2

多线程下载工具

**安装**

```bash
sudo apt-get install uget aria2
# config uget: plug-in use aria2
```

#### ufw

Firewall 防火墙

**安装**

```bash
sudo apt-get install ufw
sudo ufw enable
sudo systemctl enable ufw
sudo systemctl restart ufw
sudo ufw status
```

**用法**

```bash
sudo ufw allow 22
sudo ufw deny 80
```

#### mesa-utils

OpenGL工具包

**安装**

```bash
apt install mesa-utils
```

**用法**

```bash
glxgears # 显示一个旋转的齿轮动画, 用于测试OpenGL性能, 在控制台输出帧率
glxheads # 显示所有当前连接到的X服务器的OpenGL应用程序信息
```

#### rsync

文件同步工具

**安装**

```bash
apt install rsync
```

**用法**

```bash
rsync -arvz --exclude="*/.idea" --exclude="*/node_modules" --delete /akvicor/mvq/workspace/sync/ work:/viry/workspace/sync
rsync -arvz --exclude="*/.idea" --exclude="*/node_modules" --delete work:/viry/workspace/sync/ /akvicor/mvq/workspace/sync
```

### 安装应用程序

#### Chrome

Web浏览器

```bash
apt install gnupg
curl https://paste.akvicor.com/api/102 | bash
apt update
apt install google-stable-chrome
```

### Firefox

非Chromium内核Web浏览器

```bash
apt install firefox-esr
```

### RustDesk

远程桌面

```bash
apt install -fy ./rustdesk-<version>.deb
```

### GIMP

图像处理程序

```bash
apt install gimp
```

### LibreOffice

文档处理程序

```bash
apt install libreoffice
```

### i3lock-color

支持毛玻璃效果的i3lock

#### i3lock-color基础包

```bash
apt install autoconf gcc make autoconf pkg-config libpam0g-dev libcairo2-dev libfontconfig1-dev libxcb-composite0-dev libev-dev libx11-xcb-dev libxcb-xkb-dev libxcb-xinerama0-dev libxcb-randr0-dev libxcb-image0-dev libxcb-util0-dev libxcb-xrm-dev libxkbcommon-dev libxkbcommon-x11-dev libjpeg-dev
git clone https://github.com/Raymo111/i3lock-color.git
cd i3lock-color
git tag -f "git-$(git rev-parse --short HEAD)" # add a tag with the short commit ID, which will be used for the version info
./build.sh
./install-i3lock-color.sh
```

#### i3lock-color美化

```bash
wget https://github.com/betterlockscreen/betterlockscreen/archive/refs/heads/main.zip
unzip main.zip
cd betterlockscreen-main/
chmod u+x betterlockscreen
cp betterlockscreen /usr/local/bin/
cp system/betterlockscreen@.service /usr/lib/systemd/system/
systemctl enable betterlockscreen@$USER
```

### Fix

#### amd驱动

正常情况下不需要手动安装, 因为安装xorg时已经自动安装了

[https://www.amd.com/en/support/linux-drivers](https://www.amd.com/en/support/linux-drivers)

下载安装后执行

```shell
sudo amdgpu-install
```

#### `firmware load for amdgpu/gc_11_0_1_mes_2.bin failed with error -2`

开机提示缺少firmware

前往内核firmware网站

[https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/amdgpu](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/amdgpu)

搜索`gc_11_0_1_mes_2.bin`

也可以下载此驱动包, 包含了大量可能缺失的驱动: [firmware_amdgpu.tgz](https://via.akvicor.com/d/HHD4/Patchouli/Asset/Driver/AMD%20Debian%2012%20GPU%20Firmware/firmware_amdgpu.tgz?sign=qhVIItRYjB0jVj1szf4_SA9khEdjjWALF08b6LO9TkQ=:0)

下载后移动到`/usr/lib/firmware/amdgpu`或`/lib/firmware/amdgpu/`下, 重启电脑

