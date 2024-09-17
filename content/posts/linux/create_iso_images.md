---
title: "Creating ISO images with dd and mkisofs"
date: 2023-02-27 07:10:15 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## IMG file

### Make

```shell
dd if=/dev/zero of=fdimage.img count=2880
# or
dd if=/dev/zero of=fdimage.img bs=1024 count=1440
```

### Format

```shell
mkfs.msdos fdimage.img
```

### Mount

```shell
mount -o loop *.img /mnt
```

### Bootable

因为制作可启动镜像一定会用到虚拟机，推荐用 Virtualbox，先到网上下个 DOS 启动盘来引导。用 DOS 的 sys 命令传递系统。推荐使用 FreeDOS，属自由软件。也可用 dd 命令 来传递引导引导信息，并复制启动启动时所需文件来做启动盘。以 FreeDOS 为例，传递启动信息用以下命令，其中下载的启动盘为balder10.img 文件

```shell
dd if=balder10.img of=fdimage.img bs=512 count=1 conv=notrunc
```

多系统用 grub4dos

1. 用 grub.exe 引导多系统
2. 安装 grub 到MBR，用 grldr 来引导多系统。当然也可用同上面一样的办法用 dd 直接写入引导信息。

```shell
bootlace.com –floppy –chs 0×00
```

注：dd 命令只能从逻辑扇区开始 copy，先前我想可否用 dd 来将 grldr.mbr 写入 u 盘，我用自己的 U 盘试了，结果不能打开了。因为我的 U 盘为 fat16 格式，逻辑扇区开始是OBR，接着是FAT表，结果把 FAT1 表给盖了，那时还没有想到还有 FAT2 呢，就格了，现在想起来郁闷啊，好多东西都没有了。为什么软盘可以呢，因为它就没有前面的63个扇区，直接从逻辑0扇区开始的。

## ISO file

### Make

```shell
mkisofs -r -o cdimage.iso /home/XXX/cddir
```

### Format

用mkiso制作的 iso 已有文件系统 iso9660

### Bootable

无论是引导单系统还是引导多系统都还是用 mkisofs 这个工具，只是加载到光盘的 boot loader 不一样而已。当然也可以将 DOS 的引导器 (也就是它的引导扇区) 或 windows 的引导器 ( XP 系统的是 ntldr ) 放入让光盘引导。下面只讨论 grub4dos 的使用

1. 用 grub.exe 引导多系统 用 DOS 加载 grub.exe 引导多系统
2. 将 grub 安装到光盘 MBR 在制作时可用下面的命令直接生成可启动镜像，其中 grldr, menu.lst 要放在 cddir 目录下，也就是在 cd 根目录。

```shell
mkisofs -R -b grldr -no-emul-boot -boot-load-seg 0×1000 -o cdimage.iso cddir
mkisofs -R -b grldr -no-emul-boot -boot-load-size 4 -o cdimage.iso cddir
```


