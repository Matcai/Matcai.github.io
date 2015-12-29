---
layout: post
title: "使用Liquid 语法搭建Jekyll"
date: "2015-12-15"
backgrounds:
    - /assets/images/posts/the-desk.jpg
thumb: 
    - /assets/images/posts/code-screen.jpg
categories: Liquid
tags: Jekyll Liquid Github
---

## Liquid 语法使用


> **Liquid 因为通过Jekyll 建设博客涉及 Liquid 语法，所以不得不学习Liquid 使用。  
	于是就开始了另一段折腾史了。**

> 学习参考 http://docs.shopify.com 

----

### Liquid 简介

- 引用官网介绍：  
	Liquid is an open-source, Ruby-based template language created by Shopify. It is the backbone of Shopify themes and is used to load dynamic content on storefronts.  
	`Liquid 由Shopify 创建，开源并且基于Ruby 的一种模板语言。`

	`Liquid 使用Tags、Objects、Filters 来动态加载内容`

	- Tags
		* 通过标签实现逻辑编程，有效识别出语法内容  
		  &#123;% if user.name == 'elvis' %&#125;  
		    Hey Elvis  
		  &#123;% endif %}
	- Objects
		* 通过对象将动态内容显示  
		  &#123;{ post.title }}  
		  `Output: How to use Liquid`
	- Filters
		* 通过管道过滤字符串、数字、变量和对象的输出  
		  &#123;{ 'sales' | append: '.jpg' }}  
		  `Output: sales.jpg`

#### 常用过滤器
- date  
	格式化时间
- capitalize  
	将句子或字符串首字母大写
- downcase  
	转换小写
- upcase  
	转换大写
- first  
	获取数组第一个对象
- last  
	获取数组最后一个对象
- join  
	指定字符添加进数组
- sort  
	数组排序
- map  
	将数组全部值拼接一起
- size  
	返回数组大小
- escape  
	转义字符串
- strip_html  
	去除html标签
- strip_newlines  
	去除字符串中回车符
- newline_to_br  
	将回车符转换成html中br
- replace  
	替换内容
- replace_first  
	替换第一个内容
- remove  
	删除指定内容
- remove_first  
	删除第一个内容
- prepend  
	字符串前面添加指定内容
- append  
	字符串后面添加指定内容
- minus  
	减法运算
- plus  
	加法运算
- times  
	乘法运算
- divided_by  
	除法运算
- split  
	分割字符串
- modulo  
	取余运算
- uniq  
	删除重复字符串
- highlight  
	编程高亮使用

#### 常用Tags（标签）
- if / elsif / else  
	逻辑运算
- case/when  
- unless  
	判断不到达要求的内容进行处理
