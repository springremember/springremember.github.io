---
title: grub2制作启动盘
date: 2019-02-01 21:05:41
tags:
	- grub
categories:
	- Linux
---
# grub2启动盘


### 安装必要软件

debian系：

> 安装必要包：`sudo apt-get install grub-efi-amd64-bin grub-pc-bin`
> 安装bios启动：`sudo grub-install --target=i386-pc --recheck --boot-directory=/tmp/sdb2/boot /dev/sdb`
> 安装uefi启动：`sudo grub-install --target=x86_64-efi --efi-directory=/tmp/sdb2 --boot-directory=/tmp/sdb2/boot --removable`


arch系：

`pacman -S dosfstools grub efibootmgr os-prober`

### 添加微PE

修改grub.cfg：

```bash
insmod all_video

menuentry "WePE_bios" {
	ntldr /bootmgr
	boot
}

menuentry "WePE_uefi" {
	chainloader /EFI/BOOT/wEbootx64.efi
}
```

将wepe.iso解压到分区，并重命名efi文件(文件名冲突)

### 添加arch启动项

将arch的iso文件拷贝到启动分区中：

参考archwiki中arch的启动参数修改grub.cfg：

```shell
menuentry "Archlinux-2013.05.01-dual.iso" --class iso {
  set isofile="/archives/archlinux-2013.05.01-dual.iso"
  set partition="6"
  loopback loop (hd0,$partition)/$isofile
  linux (loop)/arch/boot/x86_64/vmlinuz archisolabel=ARCH_201305 img_dev=/dev/sda$partition img_loop=$isofile earlymodules=loop
  initrd (loop)/arch/boot/x86_64/archiso.img
}
```

注意：1.启动时archisolabel		2.启动分区位置

### 添加deepin启动项

deepin的iso不能直接通过grub2启动，需要将deepin的iso完整解压到启动分区，注意冲突文件的处理。

### 参考资料

[基于 GRUB2 制作滋瓷 BIOS/UEFI 双模式 启动的 Linux/Windows 安装U盘](https://blog.nanpuyue.com/2016/033.html)

[grub2各种手动命令引导教程](https://blog.csdn.net/qq_42748849/article/details/81273703)

[grub2详解(翻译和整理官方手册)](https://www.cnblogs.com/f-ck-need-u/p/7094693.html)

### 最终文件内容

```shell
insmod all_video

menuentry "WePE_BIOS" {
    ntldr /bootmgr
    boot
}

menuentry "WePE_UEFI" {
    chainloader /EFI/BOOT/wEbootx64.efi
}

menuentry "Archlinux-201905" --class iso {
  set isofile="/archlinux-2019.05.02-x86_64.iso"
  loopback loop (hd0,msdos2)/$isofile
  linux (loop)/arch/boot/x86_64/vmlinuz archisolabel=ARCH_201905 img_dev=/dev/sdb2 img_loop=$isofile earlymodules=loop
  initrd (loop)/arch/boot/x86_64/archiso.img
}
```

