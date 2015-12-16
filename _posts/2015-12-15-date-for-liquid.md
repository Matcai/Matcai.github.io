---
layout: post
title: "Liquid date command"
date: "2015-12-15"
backgrounds:
    - /assets/images/posts/the-desk.jpg
thumb: 
    - /assets/images/posts/bg4.jpg
categories: Liquid
tags: Jekyll Liquid Github Command
---

### Liquid  时间格式化date 的使用方法及选项

##### 具体内容从官方翻译而来。


- date  
	`用于格式化时间`  
	基本用法形如：**&#123;{ date: "%d" }}**  
	Input: &#123;{ article.published_at | date: "%a, %b %d,%y" }}  
	Output: Tue, Apr 22, 14  
	`选项：`
	- `%a`  
		星期的简写  
		`Sat`

	- `%A`  
		星期的全称  
		`Tuesday`

	- `%b`  
		月份的简写  
		`Jan`
		
	- `%B`  
		月份的全称  
		`January`

	- `%c`  
		本地日期和时间显示  
		`Tue Apr 22 11:16:09 2014`
		
	- `%d`  
		表示月份，用0填充  
		`04`
		
	- `%-d`  
		表示月份，不用0 填充  
		`4`
		
	- `%D`  
		日期格式（DD/MM/YY）  
		`04/22/14`
		
	- `%e`  
		表示月份，用空格填充  
		`` 3``
		
	- `%F`  
		ISO 8601格式（yyyy-mm-dd）  
		`2015-12-01`
		
	- `%H`  
		表示小时，24小时制(00-23)  
		`20`
		
	- `%I`  
		表示小时，12小时制  
		`8`
		
	- `%j`  
		表示一年中多少天（001-366）  
		`266`
		
	- `%k`  
		表示小时，24小时制（1-24）  
		`14`
		
	- `%m`  
		表示月份（01-12）  
		`04`
		
	- `%M`  
		表示分钟（00-59）  
		`53`
		
	- `%p`  
		表示上下午（AM/PM）  
		`PM`
		
	- `%r`  
		表示时间（%I:%M:%S %P）集合  
		`03:20:07 PM`
		
	- `%R`  
		24 小时表示（%H:%M）集合  
		`15:21`
		
	- `%T`  
		24小时（%H:%M:%S）集合  
		`15:22:13`
		
	- `%U`  
		以星期日作为第一天，显示本周为一年中第几周  
		`15`
		
	- `%W`  
		以星期一作为第一天，显示本周为一年中第几周  
		`16`
		
	- `%w`  
		显示星期，星期日表示0  
		`2`
		
	- `%x`  
		单独表示日期（mm/dd/yy）  
		`04/22/14`
		
	- `%X`  
		单独表示时间（hh:mm:ss）  
		`13:20:25`
		
	- `%y`  
		年份的简写  
		`15`
		
	- `%Y`  
		年份的全称  
		`2015`
		
	- `%Z`  
		时区名称  
		`CST`
		
