---
layout: post
title: "Linux 内核的编译"
date: '2016-04-28 19:54:00'
thumb: /assets/images/posts/coding.jpg
categories: Linux
tags: Linux Centos Debian 2016
---


### Linux 引导使用到的文件

  * vmlinux
  * initrd



#### **vmlinux**

1. vmlinux 是一个linux 所编译出来的内核文件，当系统引导时，  
需要加载它开启相对应的功能进行支持内核的功能支持特性有两种，  
一种是内置的特性，意思是说，当使用内置的特性，内核一旦加载后  
该系统就具备了对应的内置功能。

2. 还有一种特性是模块化的功能，内核通过编译模块化特性，  
让它具备处理该功能的能力。但是它的功能是不会在linux 启动后就具备该功能，  
而需要通过编译对应的模块并通过加载启动对该功能的支持

#### **initrd**

  initrd 是一个内核外的驱动工具的集合，比如，文件系统的支持，  
当内核初始化后进行挂载文件系统的时候  
就需要通过initrd 中的集成的文件系统的挂载支持。  
用于临时引导硬件到实际的内核vmlinuz 使之能够接管并继续引导  
主要用于加载ext等文件系统以及scsi设备的驱动等。

#### **vmlinuz**

vmlinuz 是跟vmlinux 一样的内核文件，只不过vmlinuz 是经过压缩的。

#### **mkinitcpio**

mkinitcpio 是一个用来创建初始化内存盘  
（initial ramdisk，简称initrd）的bash脚本。  
默认创建两个内存盘镜像，/boot/initramfs-linux.img  
和 fallback 镜像 /boot/initramfs-linux-fallback.img  
两者的区别是创建是跳过了autodetect 钩子的扩展，  
所以fallback镜像包含了更多的内核模块。  
autodetect扩展会探测硬件信息，针对硬件向镜像添加需要的模块，因此缩小了镜像。

#### make 的一些用法

- make menuconfig  
	  用于配置linux kernel 需要启动的功能，  
	  以文本图形的形式提供配置功能。  
      配置完成后生成一个.config 的配置文件  
      用于编译的配置。
- make all  
      make all 用于 build（建立）带有* 号 的选项vmlinux  
	  和 modules 两个，通过make help 可以看到对应的说明。
- make vmlinux  
      make vmlinux 用于编译系统内核
- make modules  
      make modules 用于编译系统的模块文件，  
      通过make modules_install 并进行安装到对应的目录
- make install  
      将编译出来对应的内核文件cp 到/boot 目录下。


#### make kernel 的步骤：


    1. 如果/boot 下有 config 的内核配置文件，可以cp 到  
	   内核源码下 .config 文件。这样可以方便编译
    2. make menuconfig  
       配置需要编译的内核选项。
    3. make bzImage  
       编译内核文件，bzImage 是经过压缩的内核映像，  
	   bzImage **不是用bz2** 压缩的文件  
       bz表示“big zImage”的意思。  
	   是通过gzip压缩的文件，该文件内嵌gzip 的压缩代码，  
       不能用gunzip或gzip -dc进行解包vmlinuz 的内核文件
    4. make modules  
       编译所需要使用的内核模块，用于系统模块加载的支持
    5. make modules_install  
       将编译完成的模块安装到对应的目录，  
	   即拷贝到对应的目录/lib/modules/(kernel-release)
    6. make install  
       将内核文件拷贝到/boot 目录下。
    7. mkinitcpio 或 mkinitramfs 创建initrd 镜像文件  
       有些系统make install 会自动生成initramfs 的文件，该步骤视情况而定。


#### mkinitcpio 的使用方法

  * `mkinitcpio -p linux`
  * `mkinitcpio -c /etc/mkinitcpio-custom.conf -g /boot/linux-custom.img`
  * `mkinitcpio -g /boot/linux.img -k 4.1.5-linux`



#### mkinitramfs 的使用方法

  * `mkinitramfs -o /boot/initramfs-$(uname -r)`
  * `mkinitramfs -k -o /boot/initramfs-4.1.5-x86_64 4.1.5-x86_64`



#### 只编译需要使用的modules

  * 一般编译模块不需要在编译内核文件，步骤是：
  * 配置config 文件（make menuconfig）开启需要使用的模块功能
  * 编译模块（make modules）
  * 安装模块（make modules_install）
  * 为模块生成依赖（depmod）  
	/lib/modules/$(uname -r)/modules.dep



