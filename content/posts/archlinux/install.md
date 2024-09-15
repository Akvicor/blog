---
title: "Arch Linux Installation"
date: 2020-02-14 21:19:14 +08:00
draft: false
categories: [Arch Linux]
tags: []
card: false
weight: 0
---

## Download ISO file

[https://www.archlinux.org/download/](https://www.archlinux.org/download/)

[https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/](https://mirrors.tuna.tsinghua.edu.cn/archlinux/iso/)

## Prepare an installation medium

```shell
dd bs=4M if=path/to/archlinux.iso of=/dev/sdb status=progress oflag=sync
```

## Boot the live environment

## Connect Network

```shell
iwctl
device list
station [device] scan
station [device] get-networks
station [device] connect SSID

# Check
ip address

# Update the system clock
timedatectl set-ntp true
timedatectl status
```

## Partition the disks

```shell
lsblk -l

cgdisk /dev/sda

# Format
# EFI `ef00`
# Linux `8300`
# SWAP `8200`
mkfs.ext4
mkfs.vfat
mkswap

# Mount
mount /dev/sda3 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
swapon /dev/sda2
```

## Edit Mirrorlist

```shell
vim /etc/pacman.d/mirrorlist
```

```shell
Setver = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
Setver = https://mirrors.ustc.edu.cn/archlinux/$repo/os/$arch
Setver = https://mirrors.xjtu.edu.cn/archlinux/$repo/os/$arch
Setver = https://mirrors.163.com/archlinux/$repo/os/$arch
```

```shell
pacman -Syy
```

## Install essential packages

```shell
pacstrap /mnt base linux-lts linux-firmware vim
```

## Configure the system

```shell
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt /bin/bash
```

### TimeZone

```shell
ln -s /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc --utc
```

### Locale

```shell
vim /etc/locale.gen
```

```shell
en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8
```

```shell
locale-gen
echo LANG=en_US.UTF-8 > /etc/locale.conf
echo lap > /etc/hostname
```

### Boot Loader

```shell
pacman -S efibootmgr grub
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id="Arch Linux" --recheck
grub-mkconfig -o /boot/grub/grub.cfg
```

### User

```shell
visudo /etc/sudoers
```

```shell
%wheel ALL=(ALL) ALL
```

```shell
# root password
passwd
# user
useradd -m -G wheel -s /bin/zsh akvicor
passwd akvicor
```

### Network

```shell
vim /etc/systemd/network/20-wired.network
```

#### Wired DHCP

```shell
[Match]
Name=enp1s0

[Network]
DHCP=yes
```

#### Wired Static

```shell
[Match]
Name=enp1s0

[Network]
Address=10.1.10.9/24
Gateway=10.1.10.1
DNS=10.1.10.1
DNS=8.8.8.8
```

```shell
vim /etc/systemd/network/25-wireless.network
```

#### Wired DHCP

```shell
[Match]
Name=wlan0

[Network]
DHCP=yes
```

```shell
systemctl enable systemd-networkd
systemctl start systemd-networkd
systemctl enable systemd-resolved
systemctl start systemd-resolved
systemctl enable iwd
systemctl start iwd
```

### Extra Package

```shell
pacman -S base-devel iwd zsh
# Desktop Environment
pacman -S xf86-video-intel xorg
pacman -S noto-fonts noto-fonts-cjk noto-fonts-emoji noto-fonts-extra ttf-font-awesome terminus-font
pacman -S i3-gaps i3blocks i3status xorg-xinit alsa-utils
pacman -S rofi dunst libnotify ranger feh xclip
```

if use xorg-xinit

```shell
cp /etc/X11/xinit/xinitrc ~/.xinitrc
```

Edit ~/.xinitrc and DELETE

```shell
#twm &
#xclock -geometry 50x50-1+1 &
#xterm -geometry 80x50+494+51 &
#xterm -geometry 80x20+494-0 &
#exec xterm -geometry 80x66+0+0 -name login
```

ADD

```shell
exec i3
```

Auto start

```shell
vim ~/.bash_profile or ~/.zprofile
```

```shell
if [[ ~ $DISPLAY && $XDG_VTNR -eq 1 ]]; then
    exec startx
fi

```

## Files Location

### .desktop

```shell
cd ~/.local/share/applications
cd /usr/share/applications
```

## Systemd

```shell
vim /etc/systemd/system/rc-local.service
```

```shell
[Unit]
Description="/etc/rc.local Compatibility"

[Service]
Type=oneshot
ExecStart=/etc/rc.local start
TimeoutSec=0
StandardInput=tty
RemainAfterExit=yes
SysVStartPriority=99

[Install]
WantedBy=multi-user.target
```

```shell
vim /etc/rc.local
```

```shell
#!/bin/sh
# /etc/rc.local
if test -d /etc/rc.local.d; then
    for rcscript in /etc/rc.local.d/*.sh; do
        test -r "${rcscript}" && sh ${rcscript}
    done
    unset rcscript
fi
```

```shell
chmod a+x /etc/rc.local
mkdir /etc/rc.local.d
systenctl enable rc-local.service
```

## Problems

### Multiple Monitor

```shell
xrandr --output eDP1 --primary --auto --output HDMI1 --left-of eDP1 --rotate left --auto
```


### Audio

```shell
pacman -S alsa alsa-utils

amixer sset Master unmute

alsamixer # 'm' to unmute/mute
```

