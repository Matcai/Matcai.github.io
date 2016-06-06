# lvm 加密磁盘错误修复

Debian 在安装系统格式化磁盘中由于失误，选择了lvm encrypt  
加密磁盘后返回又选择了lvm 不加密磁盘，原来的分区加密结果没有取消  
导致安装系统后，initrd 没有装载lvm 加密解密工具cryptsetup及相关的模块。


出现不能正常进入系统。

#### 解决方法。
思路：  
	为initramfs 文件加入lvm 解密磁盘必要工具。  
	更新/etc/crypttab文件lvm 封装表。
	

- 通过u盘 或光盘等引导进入 rescue mode（拯救模式）
- 查看分区表`fdisk -l /dev/sda`
- 获取uuid `blkid /dev/sda5`
- 获取lvm crypt 名 `ls /dev/mapper/`
- 更新/etc/crypttab `blkid /dev/sda5 >> /etc/crypttab`  
  并修改格式可以用一个空格 或 tab 分开各行，多个空格出现错误
- 备份原来initramfs 文件  
  `mv /boot/initramfs-$(uname -r).img /boot/initramfs-$(uname -r).img.bak`
- 创建initramfs 文件  
  `mkinitramfs -o /boot/initramfs-$(uname -r).img`
- 更新grub  
  `update-grub2 ` 或 `grub2-mkconfig -o /boot/grub/grub.cfg`
