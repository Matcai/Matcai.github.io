---
layout: post
title: "How to use Jekyll"
date: 2015-12-14
backgrounds:
    - /assets/images/posts/the-desk.jpg
thumb: /assets/images/posts/jekyll.png
categories: Jekyll 
tags: Jekyll 2015 Linux Ruby Gems
author: "Matthew·Cai"
---

## Jekyll、 一个静态网页生成神器

----

Jekyll 静态网页生成神器。有了它，再也不用担心弄不了博客了啊哈

> 学习参考：http://www.zhanxin.info 和 http://jekyll.bootcss.com

> 感谢上诉网站提供的资料。


### Jekyll 简介

- Jekyll 是一个博客动态生成静态站点的工具。类似WordPress
- Jekyll 不需要数据库的支持，但可以配合第三方服务
- Jekyll 可以免费在Github 上部署。

----

### Jekyll 的安装

1. 首先需要安装ruby 环境
	- Debian 系列通过 apt-get install ruby 
	- Readhat 系列通过 yum install ruby
	- `Ruby 版本不能低于2.0`

2. 安装RubyGems

3. 安装Jekyll
	- gem sources --remove https://rubygems.org/  
		 `#网络通常无响应，更换地址获得更好体验`
	- gem sources -a https://ruby.taobao.org/  
		 `#添加淘宝的服务源`
	- gem sources -l  
		 `#查看地址`
	- gem update --system  
		 `#更新软件，可以跳过。`
	- gem install jekyll  
		 `#安装Jekyll`

4. 如运行报错 Missing dependency: rdiscount
	- gem install rdiscount  
		 `#安装rdiscount依赖包。`


# Jekyll 快速指南

----

###  生成静态页面魔板

	jekyll new myblog
	
	cd myblog

	jekyll server

> 通过浏览 http://127.0.0.1:4000 即可查看效果,就这么简单。

![myblog](/assets/images/article/Yaml_head_example.png)


### **基本用法**

- jekyll build  
	`生成内容到默认./_site 文件夹中`

- jekyll build --destination \<destination\>  
	`指定生成内容到目标文件夹中`

- jekyll build --source \<source\> --destination \<destination\>  
	`指定源文件生成内容到目标文件夹中`

- jekyll build --watch  
	`生成内容到文件夹并监视内容改变自动生成。`

### **服务运行**

- jekyll server  
	`运行服务在 http://localhost:4000/`

- jekyll server --detach  
	`后台运行服务监视 http://localhost:4000/`

- jekyll server --watch  
	`运行服务并自动查看变更生成页面。`

