---
title: arch安装DDE桌面环境
date: 2019-01-30 21:01:40
tags:
	- arch
categories:
	- Linux
---
# 安装DDE桌面环境



#### 1.安装显卡驱动和X服务器

intel显卡驱动：`pacman -S xf86-video-intel`

nvidia显卡驱动：`pacman -S nvida`

安装xorg：`pacman -S xorg-server`

#### 2.DDE环境

基本环境：`pacman -S deepin deepin-extra lightdm`

相关软件：`pacman -S file-roller evince unrar unzip p7zip`

色温调节：`pacman -S redshift`

grub主题：`pacman -S grub-theme-vimix`

修改lightdm配置文件：`/etc/lightdm/lightdm.conf`

```shell
[Seat:*]
...
greeter-session=lightdm-deepin-greeter
```

设置lightdm开机启动：`systemctl enable lightdm`

设置网络管理器开机启动：`systemctl enable NetworkManager`

修改grub主题配置文件：

exfat文件系统支持：`pacman -S exfat-utils`

#### 3.安装字体

`pacman -S wqy-microhei wqy-zenhei ttf-dejavu ttf-inconsolata`

#### 4.包管理器设置(添加中国社区)

添加中文社区：

`echo "[archlinuxcn]" | sudo tee -a /etc/pacman.conf`

`echo "Server = https://mirrors.ustc.edu.cn/archlinuxcn/\$arch" | sudo tee -a /etc/pacman.conf`

更新软件仓库并安装社区密钥：`sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring`

#### 5.配置输入法

安装fcitx：`pacman -S fcitx-im fcitx-configtool`

安装谷歌拼音(搜狗由于fcitx-qt4被移除无法使用)：`pacman -S fcitx-googlepinyin`

配置输入法：`~/.xprofile`

> export GTK_IM_MODULE=fcitx
> export QT_IM_MODULE=fcitx
> export XMODIFIERS=@im=fcitx


注意：请运行fcitx-configtool配置输入法，否则无法正常使用

#### 6.常用软件

浏览器：`pacman -S google-chrome`

WPS：`pacman -S wps-office ttf-wps-fonts`

系统信息：`pacman -S neofetch`

安装oh-my-zsh：`pacman -S git wget zsh`，之后百度oh-my-zsh安装

