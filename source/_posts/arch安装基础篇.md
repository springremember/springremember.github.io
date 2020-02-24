---
title: arch安装基础篇
date: 2019-01-26 20:59:43
tags:
	- arch
categories:
	- Linux
---
# 安装基础篇


archLinux安装教程
写于2019年1月26日

> 本人实验了基于BIOS模式下archlinux的安装
> 2019年4月实验了EFI模式下archlinux的安装


参考资料：

1.[安装 Arch Linux 记录——配置](https://www.jianshu.com/p/2c45c5b4c1ab)

2.[ArchLinux(BIOS引导)](https://blog.csdn.net/cristianojason/article/details/80033330)

3.[ArchLinux安装图文教程(EFI模式)](https://blog.csdn.net/r8l8q8/article/details/76516523)

4.[ArchLinux安装、配置、美化和优化(gnome桌面)](https://www.cnblogs.com/bluestorm/p/5929172.html)

5.[安装ArchLinux记录--配置(i3wm)](https://www.jianshu.com/p/2c45c5b4c1ab)

### 1.连接网络

无线网络：`wifi-menu`

拨号：`pppoe-setup`

测试网络：`ping -c 3 www.baidu.com`

### 2.同步时间并编辑国内源

同步时间：`timedatectl set-ntp true`

编辑镜像站文件：`vim /etc/pacman.d/mirrorlist`

将China下网址复制到最开头

更新软件仓库：`pacman -Syy`

### 3.查看启动类型和分区

检查引导方式：

> `ls /sys/firmware/efi/efivars`

若无该文件，则处于BIOS启动模式，反之EFI模式


BIOS模式：

分区：

> /boot分区，至少200M，FAT32文件系统
> swap分区，为内存大小，swap文件系统
> 其余按照Linux分区格式


格式化：

> boot分区：mkfs.fat -F32 /dev/boot分区
> mkswap /dev/swap分区
> swapon /dev/swap分区
> 其余分区自行选择文件系统，例如ext4、xfs等


挂载分区：

> 根目录挂载：mount /dev/根分区 /mnt
> 创建相应目录并挂载分区，例如/boot
> mkdir /mnt/boot
> mount /dev/boot分区


EFI模式：

分区：

> /boot/EFI分区，至少800M，文件系统FAT32
> /swap分区，和内存一样大小，文件系统swap
> 其余按照Linux分区格式


格式化：

> EFI分区：mkfs.fat -F32 /dev/EFI分区
> mkswap /dev/swap分区
> swapon /dev/swap分区
> 其余Linux，自行选择文件系统，例如ext4、xfs等


挂载分区：

> 挂载根分区：mount /dev/根分区
> 创建efi目录：mkdir -p /boot/EFI
> 挂载EFI：mount /dev/EFI分区  /boot/EFI


### 4.安装基本系统并切换

安装基本系统：`pacstrap -i /mnt base base-devel`

解释：/mnt对应根目录，也就是之前挂载的根分区。base和base-devel是基本系统包

配置开机挂载文件(fstab)：`genfstab -U /mnt >> /mnt/etc/fstab`

查看配置文件：`cat /mnt/etc/fstab`

切换新系统：`arch-chroot /mnt`

### 5. vim和语系、时区设置并设置同步时间

安装vim：`pacman -S vim`

编辑语系：`vim /etc/locale.gen`

将en_US.UTF-8和zh_CN.UTF-8,前面#删除

使其有效：`locale-gen`

新建`/etc/locale.conf`文件，添加内容：`LANG=en_US.UTF-8`

时区设置(使用hwclock同步硬件时间)：

`ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime`

`hwclock --systohc`

### 6.引导设置

BIOS模式：

> 安装检测启动项软件：`pacman -S os-prober`
> 安装grub：`pacman -S grub`
> 部署grub：`grub-install --target=i386-pc /dev/磁盘名(sda、sdb一类，不带数字)`
> 生成配置文件：`grub-mkconfig -o /boot/grub/grub.cfg`


EFI模式：

> 安装引导程序：`pacman -S dosfstools grub efibootmgr os-prober`
> 部署grub：`grub-install --target=x86_64-efi --efi-directory=/boot/EFI --recheck`
> 生成配置文件：`grub-mkconfig -o /boot/grub/grub.cfg`


### 7.用户设置和配置sudo

设置root密码：`passwd`

安装sudo：`pacman -S sudo`

新建用户：`useradd -m 你的用户名`

设置新用户密码：`passwd 设置的用户名`

配置sudo：

> `visudo`

找到root ALL=(ALL)ALL

添加一行：你的用户名的群组(一般就是你的用户名)  ALL=(ALL)ALL


### 8.开机联网(若使用deepin桌面可以跳过)

网络管理安装：`pacman -S networkmanager`

网络管理开机启动：`systemctl enable NetworkManager`

有线连接开机启动：`systemctl enable dhcpcd`

无线连接检测：`wifi-menu`，并按照提示安装需要软件包

### 9.自行设置swap文件(可选)

设置交换文件(可选)：

生成交换文件：`fallocate -l 大小(4G) /swapfile`

更改权限：`chmod 600 /swapfile`

格式化为swap：`mkswap /swapfile`

启用交换文件：`swapon /swapfile`

设置自动挂载：`vim /etc/fstab`

最后一行添加：`/swapfile none swap defaults 0 0`

至此，基本系统安装完毕，下面是桌面系统安装和常用软件配置。

请重启后继续。

