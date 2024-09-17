---
title: "Compile"
date: 2023-06-26 13:30:51 +08:00
draft: false
categories: [OpenWrt]
tags: []
card: false
weight: 0
---

## 平台

- Ubuntu 20.04.6 LTS

## 编译环境

```shell
sudo apt update -y
sudo apt full-upgrade -y
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
bzip2 ccache cmake cpio curl device-tree-compiler fastjar flex gawk gettext gcc-multilib g++-multilib \
git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libglib2.0-dev libgmp3-dev libltdl-dev \
libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libreadline-dev libssl-dev libtool lrzsz \
mkisofs msmtp nano ninja-build p7zip p7zip-full patch pkgconf python2.7 python3 python3-pyelftools \
libpython3-dev qemu-utils rsync scons squashfs-tools subversion swig texinfo uglifyjs upx-ucl unzip \
vim wget xmlto xxd zlib1g-dev
```

## 编译OpenWrt

```shell
# 下载
git clone https://github.com/coolsnowwolf/lede && cd lede

# 添加其他源
echo "src-git kenzo https://github.com/kenzok8/openwrt-packages" >> feeds.conf.default
echo "src-git small https://github.com/kenzok8/small" >> feeds.conf.default

# 更新源
./scripts/feeds update -a
./scripts/feeds install -a

# 修改默认IP为 10.0.0.2
sed -i 's/192.168.1.1/172.16.1.1/g' package/base-files/files/bin/config_generate

# 修改默认主机名
sed -i '/uci commit system/i\uci set system.@system[0].hostname='op'' package/lean/default-settings/files/zzz-default-settings

# 加入编译者信息
sed -i "s/OpenWrt /Akvicor build $(TZ=UTC-8 date "+%Y.%m.%d") @ OpenWrt /g" package/lean/default-settings/files/zzz-default-settings

# 修改banner
echo -e "-------------------------------------------\n %D %V, %C\n-------------------------------------------" > package/base-files/files/etc/banner

# 修改IPSec VPN账户名
sed -i "s/ 'lean'/ 'akvicor'/g" feeds/luci/applications/luci-app-ipsec-vpnd/root/etc/config/ipsec

# 配置
make menuconfig

# 修改分区大小
Target Images -> Kernel partition size -> 64
Target Images -> Root filesystem partition size -> 4096

# 添加编程语言支持
Languages -> Go -> golang -> on

# 关闭adbyby plus
LuCI -> 3. Applications -> luci-app-adbyby-plus -> off

# 添加AdguardHome
LuCI -> 3. Applications -> luci-app-adguardhome -> on

# 添加EQOS
LuCI -> 3. Applications -> luci-app-eqos -> on

# 添加Passwall代理
LuCI -> 3. Applications -> luci-app-passwall -> on

# 添加方糖气球 serverchan
LuCI -> 3. Applications -> luci-app-serverchan -> on

# 添加watchcat
LuCI -> 3. Applications -> luci-app-watchcat -> on

# 关闭迅雷快鸟
LuCI -> 3. Applications -> luci-app-xlnetacc -> off

# 关闭zerotier (一个开源VPN服务，但是需要通过第三方网站使用
LuCI -> 3. Applications -> luci-app-zerotier -> off

# 下载
make download -j8

# 编译
screen -S build
make V=s -j1
```
## 在esxi测试

```shell
# 上传vmdk文件
# ssh连接esxi
cd /vmfs/volumes/hhd/op
ls *.vmdk
vmkfstools -i openwrt-x86-64-generic-squashfs-combined-efi.vmdk opd.vmdk
vmkfstools -X 5120M opd.vmdk # 需大于源文件大小
# 更换op虚拟机的硬盘文件

# 配置测试网络
vim /etc/config/network
# 修改lan的ip地址
# 重启网络，通过浏览器访问
/etc/init.d/network restart
```
