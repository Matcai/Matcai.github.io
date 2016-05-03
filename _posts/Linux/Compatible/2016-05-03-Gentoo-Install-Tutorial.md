---
layout: post
title: "Gentoo Linux 安装教程"
date: '2016-05-03 11:54:00'
thumb: /assets/images/posts/coding.jpg
categories: Linux
tags: Linux Gentoo 2016
---

# gentoo的安装手册

Gentoo Linux 是一种Linux 操作系统，基于Portage 包管理系统，  
适应性强大，被称为元发行版（Meta-distribution）  
支持多大10种以上的电脑系统结构平台。  
此项目和它的产品以**巴布亚企鹅**命名。

Gentoo Linux 的软件包从源代码中构建  
为了方便，也提供一些大型软件包在多种架构的预编译二进制文件  
用户亦可自建或使用第三方二进制包镜像来直接安装二进制包。


Gentoo Linux 采用滚动式更新，每个半年发行新版本。

#### gentoo 的包管理工具
1. ebuild  
   ebuild 是Portage 包管理程序的根本。它是一个纯文本文件，  
   而每个ebuild都会对应一个包（软件包）。  
   ebuild会记录portage 要下载的文件、该包运行的平台  
   如何编译、它所依赖的ebuild 和一些修补代码的patch。  
   Portage 内有一个ebuild 的 Portage tree，  
   是由gentoo 网站提供的ebuild。包含大部分常用的包，不定时更新。
   
2. USE 标志  
   USE标志主要用于定制软件包，是的emerge 处理依赖关系的时候可以  
   做到不安装或安装指定的软件包（例如gnome 不需要依赖kde 和qt，只需qtk+）  
   做到软件包的最简化。
   
#### Gentoo 安装准备工作

- stage3-amd64-<release>.tar.bz2  amd64架构使用
- portage-latest.tar  portage tree
- install-amd64-minimal-<date>.iso 引导使用

#### Gentoo 的编译安装

- install-amd64-minimal-<date>.iso 制作启动引导盘

- 配置网络环境。  
  <pre>
	  root # ifconfig
	  root # ip addr show
	  root # export http_proxy="http://proxy.gentoo.org:8080"
	  root # export ftp_proxy="ftp://proxy.gentoo.org:8080"
	  root # export RSYNC_PROXY="proxy.gentoo.org:8080"
	  root # ping -c 3 www.gentoo.org
	  root # net-setup eth0                  自动配置网络
	  root # pppoe-setup
	  root # pppoe-start                     pppoe 网络环境
	  root # nano -w /etc/ppp/chap-secrets   
	  root # nano -w /etc/ppp/options.pptp   pppoe 账户以及密码等配置
	  root # dhcpcd eth0                     dhcp
	  root # nano -w /etc/resolv.conf        配置dns
	  root # ip route add default ...        配置网关
	  root # route add default gw $(GATEWAY) 配置网关
	  
	  以上为可选的网络环境配置命令
	  安装Gentoo 的前提是要使其能够上网下载ebuild 包的源代码编译
  </pre>
  
- 磁盘分区。  
  MBR分区可以使用fdisk 或 parted  
  GPT分区使用parted 分区工具  
  <pre>
	  以gpt分区为例，分区如下（可选）
	  Partition    Filesystem    Size    Description
	  ------------------------------------------------------
	  /dev/sda1    (bootloader)  2M      BIOS boot partition
	  /dev/sda2    ext2(or vfat) 128M    Boot partition
	  /dev/sda3    (swap)        512M~   Swap partition
	  /dev/sda4    ext4          ~~~     Root partition
	  ------------------------------------------------------
	  
	  2M 的BIOS分区与MBR分区进行兼容。使得gpt分区可以兼容引导MBR分区方式
  </pre>
  
- parted 分区方法  
  <pre>
	  parted -a optimal /dev/sda
	  (parted) mklabel gpt
	  (parted) rm ~  清空分区
	  (parted) unit mib  以mib 为默认大小单位
	  (parted) mkpart primary 1 3
	  (parted) name 1 grub
	  (parted) set 1 bios_grub on
	  (parted) print
	  (parted) mkpart primary 3 131
	  (parted) name 2 boot
	  (parted) set 2 boot on
	  ....
	  (parted) q
	  
	  格式化分区：
	  mkfs.ext2 /dev/sda2 or mkfs.vfat /dev/sda2
	  mkswap /dev/sda3
	  swapon /dev/sda3
	  mkfs.ext4 /dev/sda4
	  ...
  </pre>
  
- Mounting 挂载分区  
  <pre>
	  root # mount /dev/sda4 /mnt/gentoo
	  root # mkdir /mnt/gentoo/boot
	  root # mount /dev/sda2 /mnt/gentoo/boot
  </pre>
  
- 安装Gentoo files  
    1. 配置时间同步  
	 root # date  
	 root # date MMDDhhmmYYYY  
	 root # tar xvjpf stage3-*.tar.bz2 --xattrs  解压缩  
    2. 配置编译选项  
	   nano -w /mnt/gentoo/etc/portage/make.conf  
	   <pre>
		   CFLAGS="-march=native -O2 -pipe"
		   CXXFLAGS="$(CFLAGS)"
		   MAKEOPTS="-j2"    配置编译器调用cpu核心数量
		   USE="aac aalib acpi alsa acl audit amd64 bash-completion\
			    bindist bootstrap branding bzip2 curl cli ffmpeg gif\
				dri icu jpeg kde libressl mp3 mp4 mpeg mmx minimal \
				networkmanager nptl opengl pam posix python pulseaudio \
				qt5 sound suid static-libs systemd sse sse2 selinux \
				udev wifi wayland vaapi xml zsh-completion gtk -gnome"

				针对kde 桌面使用的USE flag
	       VIDEO_CARDS="intel nvidia" 双显卡
		   INPUT_DEVICES="evdev synaptics" evdev 集成了keyboard 和 mouse
		   LINGUAS="en en_US zh_CN" 配置编译软件包的语言
		   
	   </pre>
	3. Selecting mirrors 选择网络源  
	   mirrorselect -i -o >> /mnt/gentoo/etc/portage/make.conf  
	4. 配置Gentoo 软件仓库（可选）  
	   mkdir /mnt/gentoo/etc/portage/repos.conf  
	   cp /mnt/gentoo/usr/share/portage/config/repos.conf \  
	      /mnt/gentoo/etc/portage/repos.conf/gentoo.conf  
	  <pre>
		  /mnt/gentoo/etc/portage/repos.conf/gentoo.conf 
		  [gentoo]
		  location = /usr/portage
		  sync-type = rsync
		  sync-uri = rsync://rsync.gentoo.org/gentoo-portage
		  auto-sync = yes
	  </pre>
	5. copy dns info (拷贝dns 信息)  
	   root # cp -L /etc/resolv.conf /mnt/gentoo/etc/
	   
	6. 挂载必要的文件系统  
	   <pre>
		   root # mount -t proc proc /mnt/gentoo/proc
		   root # mount --rbind /sys /mnt/gentoo/sys
		   root # mount --make-rslave /mnt/gentoo/sys
		   root # mount --rbind /dev /mnt/gentoo/dev
		   root # mount --make-rslave /mnt/gentoo/dev
	   </pre>
	7. chroot new environment(切换到新的环境)  
	   <pre>
		   root # chroot /mnt/gentoo /bin/bash
		   root # source /etc/profile
		   root # export PS1="(chroot) $PS1"
	   </pre>
	8. Configuring Portage（配置portage更新）  
	   <pre> 
		   root # emerge-webrsync  全局更新最新的portage 快照
		   root # emerge --sync 如果已有离线的portage 解压到/usr/portage 目录下  
			      可以使用该命令进行同步最新的portage tree  
	       root # emerge --sync --quiet  
		   如果使用的是一个慢速终端比如一些帧缓冲或者是串口的控制台
		   添加--quiet选项来加速这个过程
		   
	   </pre>
	9. 配置profile  
	   <pre>
		   root # eselect profile list  查看可用profile
		   Available profile symlink targets:
		   [1]  default/linux/amd64/13.0 *
		   [2]  default/linux/amd64/13.0/desktop
		   [3]  default/linux/amd64/13.0/desktop/gnome
		   [4]  default/linux/amd64/13.0/desktop/kde
		   ....
		   
		   root # eselect profile set 2 or 4   配置桌面环境
		   root # eselect profile set default/linux/amd64/13.0/systemd  选择systemd模式
	   </pre>
	10. 更新系统全局配置@world  
		<pre> 
			root # emerge --ask --update --deep --newuse @world
			USE flag 所有flags 可以在 /usr/portage/profiles/use.desc 中看到
		</pre>
	11. 配置timezone  
		<pre> 
			root # echo "Asia/Shanghai" > /etc/timezone
			root # emerge --config sys-libs/timezone-data
		</pre>
	12. 配置locales  
		<pre> 
			root # nano -w /etc/locale.gen
			root # locale-gen
			root # eselect locale list
			root # eselect locale set NUM
		</pre>
	13. 更新环境  
		`root # env-update && source /etc/profile && export PS1="(chroot) $PS1"`
		

	
#### Gentoo 内核的编译

Gentoo 内核编译有两种方式，一种是手动编译；

一种则是使用genkernel 软件进行自动编译；

手动编译需使用genkernel --install initramfs 安装initrd 文件。

- 下载gentoo 源码  
  <pre> 
	  root # emerge --ask sys-kernel/gentoo-sources
	  root # ls -l /usr/src/linux
  </pre>
- 编译源码  
  <pre> 
	  root # emerge --ask sys-apps/pciutils  硬件查看工具lspci
	  root # cd /usr/src/linux
	  root # make menuconfig  定义配置文件
	  root # make bzImage && make modules && make modules_install
	  root # make install
	  root # emerge --ask sys-kernel/genkernel  安装genkernel
	  root # emerge --ask sys-kernel/genkernel-next systemd genkernel安装方式
	  root # genkernel --install initramfs
	  root # genkernel --lvm --mdadm --install initramfs 添加lvm 以及mdadm支持
   </pre>
- 使用genkernel 方法编译内核  
  <pre>
	  root # emerge --ask sys-kernel/genkernel
	  root # nano -w /etc/fstab 配置boot分区挂载
	  root # genkernel all
  </pre>
  
- 安装linux firmware  
  <pre>
	  root # emerge --ask sys-kernel/linux-firmware
  </pre>
  
#### 配置信息

- 配置主机名  
  `root # nano -w /etc/conf.d/hostname`

- 配置网卡ip地址  
  <pre>
	  root # nano -w /etc/conf.d/net
	  dns_domain_lo="homenetwork"
	  nis_domain_lo="my-nisdomain"
	  config_eth0="192.168.1.1 netmask 255.255.255.0 brd 192.168.1.255"
	  routes_eth0="default via 192.168.1.254"
	  -----------------------------------------------------------------
	  config_eth0="dhcp"  配置dhcp方式获取ip地址
	  -----------------------------------------------------------------
  </pre>

- 配置开机自动启动  
  <pre>
	  root # cd /etc/init.d
	  root # ln -s net.lo net.eth0
	  root # rc-update add net.eth0 default
	  
  </pre>
  
- 配置hosts文件  

- 配置PCMCIA使得笔记本支持内存卡  
  `root # emerge --ask sys-apps/pcmciautils`
  
- 配置root password

- 配置时钟信息  
  `root # nano -w /etc/conf.d/hwclock`

#### 安装常用系统管理工具

- app-admin/sysklogd
- sys-process/cronie
- sys-apps/mlocate
- net-misc/dhcpcd
- net-dialup/ppp
- bash-completion

#### 配置bootloader 引导系统
- Grub2  
  <pre>
	  root # emerge --ask sys-boot/grub:2
	  root # echo GRUB_PLATFORMS="efi-64" >> /etc/portage/make.conf
	  uefi 引导方式使用
	  root # emerge --ask --update --newuse --verbose sys-boot/grub:2
	  root # grub2-install /dev/sda BIOS 引导方式使用
	  root # grub2-install --target=x86_64-efi --efi-directory=/boot 
	  uefi 引导方式使用
	  
	  root # grub2-mkconfig -o /boot/grub/grub.cfg 
	  cmdline 添加quiet 指令系统在引导时开启静默模式，不显示详细的引导信息。
  </pre>
  
- efibootmgr  
  uefi 引导安装  
  <pre>
	  root # emerge --ask sys-boot/efibootmgr
	  root # efibootmgr --create --disk /dev/sda --part 2 --label "Gentoo" --loader "\efi\boot\bootx64.efi"
	  root # efibootmgr -c -d /dev/sda -p 2 -L "Gentoo" -l "\efi\boot\bootx64.efi" \  
		     initrd='\initramfs-genkernel-amd64-<release>-gentoo'
		
  </pre>
  

- 卸载系统重启  
  <pre>
	  root # exit
	  cdimage # umount -l /mnt/gentoo/dev{/sdm,/pts,}
	  cdimage # umount /mnt/gentoo{/boot,/sys,/proc,}
	  cdimage # reboot
  </pre>
  
  
#### 配置普通用户 

- 安装sudo
- 添加用户及指定用户组  
  <pre>
	  useradd -m -G users,wheel,audio,video,usb,portage,games,floppy,cdrom  ....
	  wheel 组使用户能够执行su 提权
	  sudo  添加sudo组
	  
  </pre>


