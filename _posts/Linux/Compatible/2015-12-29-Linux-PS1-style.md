---
layout: post
title: "Linux PS1 个性化"
date: '2015-12-29 10:55:00'
thumb: /assets/images/posts/coding.jpg
categories: Linux
tags: Linux Centos Debian 2015
---


## Linux PS1 使用

- 通过 export PS1 配置可修改终端提示符样式。  
	达到个性化效果。  

- 常用的配置选项：  
	\d :  代表日期，格式为：“Mon Aug 1”  
	\H :  完整的主机名称  
	\h :  仅取主机名的第一个名字  
	\t :  显示时间24小时格式
	\T :  显示时间12小时格式  
	\A :  显示时间24小时格式：HH:MM  
	\u :  当前用户的账户名称  
	\v :  BASH的版本信息  
	\w :  完整的工作目录名称。家目录以~代替  
	\W :  利用basename取得工作目录名称，所以只会列出最后一个目录。  
	\\# :  下达的第几个命令  
	\\$ :  提示符，如果是root用户，提示符为# ，普通用户为$

- 颜色配置方式：  
	`基本格式`  
	\[\e[F;Bm\]  
	\e[ 与 m 之间定义背景颜色和前景颜色。  
	F 指代字体颜色，编号30-37；  
	B 指代背景色，编号 40-47；  
	通过\e[0m 关闭颜色的输出；  

- 颜色配置表：  
	前景色 背景色 颜色对应  
	--30-----40-----黑色--  
	--31-----41-----红色--  
	--32-----42-----绿色--  
	--33-----43-----黄色--  
	--34-----44-----蓝色--  
	--35-----45-----紫色--  
	--36-----46-----青色--  
	--37-----47-----白色--  
	关闭代码 意义  
	--0-----结束颜色输出--  
	--1-----高亮显示------  
	--4-----下划线--------  
	--5-----闪烁----------  
	--7-----反白显示------  
	--8-----不可见--------

- 问题：  
	所有PS1 中的非打印输出字符都必须通过“\\[  \\]” 包围起来区分  
	否则无法正确换行，出现了回到行首的情况。  
	同时Ctrl+A 也无法正确回到行首。

