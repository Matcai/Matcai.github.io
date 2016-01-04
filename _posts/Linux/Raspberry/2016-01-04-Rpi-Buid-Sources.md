---
layout: post
title: "Rasberry Pi 内核编译"
date: '2016-01-04 20:00'
thumb: /assets/images/posts/coding.jpg
categories: Raspberry
tags: Linux Debian Raspberry 2016
---

## Raspberry Pi 编译内核使其支持网卡混杂模式

- 由于从某宝中买回来的无线WIFI 宣称Linux上免驱动安装  
    使用发现网卡无法开启混杂模式  
    无法忍受，以为是网卡问题，网上经过一番查询  
    原来是因为Raspbian 官方rtlwifi 这驱动arm上  
    不稳定，所以编译内核是默认屏蔽了  
    换成realtek 官方的驱动。 该驱动是2013年出的  
    所以有很多功能无法支持。  

- 解决办法是从新编译内核  
    将rtlwifi 驱动加载到内核中就可以了  


##### 具体步骤   
    只针对Raspberry Pi 2，其他型号可以通过  
    https://www.raspberrypi.org/documentation/linux/kernel/building.md  
    查看具体说明

1. uname -a (查看 内核版本)

2. git clone --depth=1 https://github.com/raspberrypi/linux  
    下载默认版本。指定版本可以加 -b 指定  

3. apt-get -y install bc  
    安装bc ，必须

<pre>
        vim linux/drivers/net/wireless/Kconfig
        取消注释 
        #source "drivers/net/wireless/rtlwifi/Kconfig"

        vim linux/drivers/net/wireless/Makefile
        #obj-$(CONFIG_RTLWIFI)  += rtlwifi/

        把# 号去掉

    $ cd linux
    $ KERNEL=kernel7
    $ make bcm2709_defconfig

    copy 前请做好备份工作!
    $ make zImage modules dtbs
    $ sudo make modules_install
    $ sudo cp arch/arm/boot/dts/*.dtb /boot/
    $ sudo cp arch/arm/boot/dts/overlays/*.dtb* /boot/overlays/
    $ sudo cp arch/arm/boot/dts/overlays/README /boot/overlays/
    $ sudo scripts/mkknlimg arch/arm/boot/zImage /boot/$KERNEL.img
    
    make 在Raspberry Pi 2 中可以添加 -j4 充分利用多核心工作加快速度
    如：make -j4 zImage modules dtbs


    $ sudo reboot

    lsmod | grep rtl8192cu   
    查看是否成功加载

</pre>
