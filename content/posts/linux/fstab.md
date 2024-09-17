---
title: "Fstab"
date: 2023-03-08 08:21:29 +08:00
draft: false
categories: [Linux]
tags: []
card: false
weight: 0
---

## 系统挂载的一些限制

- 根目录/是必须挂载的，而且一定要先于其他mount point被挂载进来。
- 其他挂载点必须为已新建的目录，可任意指定，但一定要遵守必需的系统目录架构原则
- 所有挂载点在同一时间之内，只能挂载一次
- 所有分区在同一时间内，只能挂载一次
- 如若进行卸载，必须先将工作目录移到挂载点（及其子目录）以外。

## /etc/fstab（file system table）

```shell
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda2 during installation
UUID=0b15699d-6b0c-4c41-91e8-b2a5fe113366 /               ext4    errors=remount-ro 0       1
# /boot/efi was on /dev/sda1 during installation
UUID=DBF9-FECA  /boot/efi       vfat    umask=0077      0       1
# swap was on /dev/sda3 during installation
UUID=2126dcda-b478-4b0f-bfc6-a29189440e46 none            swap    sw              0       0

LABEL=HHD1 /HHD1 ext4 noexec 0 2
LABEL=HHD2 /HHD2 ext4 noexec 0 2
LABEL=PHHD1 /PHHD1 ext4 noexec 0 2
```

## 语法

 `[Device] [Mount Point] [File System Type] [Options] [Dump] [Pass]`

- 设置LABEL `e2label /dev/sdb HHD1`
- 查看UUID `blkid /dev/sdb`

1. `<device>` The device/partition (by /dev location, LABEL or UUID) that contain a file system.
2. `<mount point>` The directory on your root file system (aka mount point) from which it will be possible to access the content of the device/partition (note: swap has no mount point). Mount points should not have spaces in the names.
3. `<file system type>` Type of file system  (auto, vfat, ntfs, ntfs-3g, ext4, ext3, ext2, jfs, iso9660, swap)
4. `<options>` Mount options of access to the device/partition (see the man page for mount).
5. `<dump>` Enable or disable backing up of the device/partition (the command dump). This field is usually set to 0, which disables it.
6. `<pass num>` Controls the order in which fsck checks the device/partition for errors at boot time. The root device should be 1. Other partitions should be 2, or 0 to disable checking.

### options
n the names.
3. `<file system type>` Type of file system  (auto, vfat, ntfs, ntfs-3g, ext4, ext3, ext2, jfs, iso9660, swap)
4. `<options>` Mount options of access to the device/partition (see the man page for mount).
5. `<dump>` Enable or disable backing up of the device/partition (the command dump). This field is usually set to 0, which disables it.
6. `<pass num>` Controls the order in which fsck checks the device/partition for errors at boot time. The root device should be 1. Other partitions should be 2, or 0 to disable checking.

### options
bai
- `sync/async` - All I/O to the file system should be done (a)synchronously.
- `auto` - The filesystem can be mounted automatically (at bootup, or when mount is passed the -a option). This is really unnecessary as this is the default action of mount -a anyway.
- `noauto` - The filesystem will NOT be automatically mounted at startup, or when mount passed -a. You must explicitly mount the filesystem.
- `dev/nodev` - Interpret/Do not interpret character or block special devices on the file system.
- `exec/noexec` - Permit/Prevent the execution of binaries from the filesystem.
- `suid/nosuid` - Permit/Block the operation of suid, and sgid bits.
- `ro` - Mount read-only.
- `rw` - Mount read-write.
- `user` - Permit any user to mount the filesystem. This automatically implies noexec, nosuid,nodev unless overridden.
- `nouser` - Only permit root to mount the filesystem. This is also a default setting.
- `uid` - Set the owner and group of all files. (Default: the uid and gid of the current process.)
- `gid` - Set the owner and group of all files. (Default: the uid and gid of the current process.)
- `umask` - Set the umask (the bitmask of the permissions that are not present). The default is the umask of the current process. The value is given in octal.
- `dmask` - Set the umask applied to directories only. The default is the umask of the current process. The value is given in octal.
- `fmask` - Set the umask applied to regular files only. The default is the umask of the current process. The value is given in octal.
- `defaults` - Use default settings. Equivalent to rw, suid, dev, exec, auto, nouser, async.
- `_netdev` - this is a network device, mount it after bringing up the network. Only valid with fstype nfs.

### dump & pass num

- `/` 一般为 `1 1`
- `swap` 一般为 `0 0`
- `其他分区` 一般为 `1 2`
- `云硬盘` 一般为 `0 2`




