---
layout: post
title: "Jekyll _config.yml"
date: 2015-12-16
backgrounds:
    - /assets/images/posts/the-desk.jpg
thumb: /assets/images/posts/the-bridge.jpg
categories: Jekyll
tags: yml 2015 jekyll config
author: "Matthew·Cai"
---

#### _config.yml 配置文件用法说明 **/** 后面为Jekyll 命令选项

#### **配置文件中不能使用 `Tab` 制表符，否则解析错误**

### 全局配置

- source: DIR / -s,--source DIR  
	> 设置Jekyll 读取文件的目录

- destination: DIR / -d,--destination DIR  
	> 设置Jekyll 写入文件的目录（默认_site）

- safe: BOOL / --safe  
	> 禁用自定义插件

- exclude: [DIR,FILE, ...]  
	> 排除目录，文件

- include: [DIR,FILE, ...]  
	> 包含指定文件

- timezone: TIMEZONE  
	> 设置时区 如Asia/Shanghai,默认为系统时区

- encoding: ENCODING  
	> 设置文件的编码

### 编译选项

- future: BOOL / --future  
	> 用将来的日期发布文章

- lsi: BOOL / --lsi  
	> 为相关的文章生成索引

- limit_posts: NUM / --limit_posts NUM  
	> 限制文章的数量

- / -w,--watch  
	> 监控文件的修改并自动生成最新网站

- / --config FILE1,[FILE2, ...]  
	> 指定_config.yml 文件

- / --drafts  
	> 处理草稿文件

### 服务选项

- port: PORT / --port PORT  
	> 指定监听端口
- host: HOSTNAME / --host HOSTNAME  
	> 指定监听的主机名
- baseurl: URL / --baseurl URL  
	> 指定网站的跟路径
- detach: BOOL / -B,--detach  
	> 后台运行，从终端命令行中分离出来

### **配置一些复用值**

+ 一种情况，当你文章书写时总是配置一些重复参数，  
	而你又想减少这个麻烦。  
	例如，作者名称。  
	可以通过配置`defaults` 选项全局配置该默认参数，  
	页面加载时会自动使用默认值。

<pre>
defaults:
  -
	scope:
	  path: ""
	values:
	  layout: "default"
  -
  	scope:
	  path: "_posts"
	  type: "posts"
	values:
	  layout: "project"
	  author: "Matthew·Cai"

#defaults : 为声明默认值，有scope/values 定义引用范围用对应值
   -	  : 多个配置范围用- 号隔开声明
 scope	  : 声明应用范围
 path	  : 指定路劲下生效配置，空字符表示所有文件
 type     : 指定生效的类型。
 		    type 值包括 pages，posts，drafts 以及collections
		    type 值是可选项，但是path 选项必须要指定
 values	  : values 指定相应值通过page.{values} 访问
 layout   : 表示应用的布局文件
 ....
</pre>

----

### Collections 集合

> **有时候一个网页并非只有 post 或 page。 或许需要使用各种方式记录展示项目**  
	Collections 允许你这么做!

- 使用 Collections

<pre>
1. 告诉Jekyll 来读取你的Collections，通过_config.yml 添加配置
  
  collections:
  - my_collection # 定义一个my_collection

  collections:
    my_collection:
	  foo: bar
  还可以为my_collection 指定数据，在defaults 属性里面也可以进行配置
  如：
    defaults:
	 - 
	  scope:
	   path: ""
	   type: my_collection
	  values:
	   layout: page
  
2. 为my_collection 添加内容
  根目录下创建对应文件夹（以_ 开头标识）\<source\>/_my_collection 并添加文档
  如果数据存在YAML头信息，将会读取内容赋予content 变量。
  如果没有数据存在YAML 头信息 ，Jekyll不会处理生成Collections。

3. 可将collections 文档独立文件，每个文章生成一个html文件
	通过output: true 声明，如：
	collection:
	 my_collection:
	  output:true
	  permalink: /awesome/:path/ # 可创建虚拟路劲

	  _my_collection/subdir/some_doc.md 输出的页面路径变成
	  \<dest\>/awesome/subdir/some_doc/index.html

</pre>
### Collections 属性访问

- site.albums  
	访问所有collections 数组  
	访问方法就像site.pages 和 site.posts

- site.collections  
	如果对应信息有定义，将返回以下信息   
	  
	`label`  
	collections名称，如my_collection  
	`docs`  
	所有文档的数组集合  
	`files`  
	在collection 静态文件数组  
	`relative_directory`  
	collections 源路径  
	`directory`  
	collections 的完整源路径  
	`output`  
	collections 的每一个文档

----

### 一些默认配置

<pre>
	source:			.
	destination:	./_site
	plugins:		./_plugins
	layouts:		./_layouts
	include:		['.htaccess']
	exclude:		[]
	keep_files:		['.git','.svn']
	gems:			[]
	timezone:		nil
	encoding:		nil

	future:			true
	show_drafts:	nil
	limit_posts:	0
	highlighter:	pygments

	relative_permalinks:	true

	permalink:		date
	paginate-path:	'page:num'
	paginate:		nil

	markdown:		maruku
	markdown_ext:	markdown,mkd,mkdn,md
	textile_ext:	textile

	excerpt_separator:	"\n\n"

	safe:			false
	watch:			false
	server:			false
	host:			0.0.0.0
	port:			4000
	baseurl:		/
	url:			http://localhost:4000
	lsi:			false

	maruku:
		use_tex:	false
		use_divs:	false
		png_engine:	blahtex
		png_dir:	images/latex
		png_url:	/images/latex
		fenced_code_blocks:	true

	rdiscount:
		extensions:	[]
	
	redcarpet:
		extensions: []
	
	kramdown:
		auto_ids:	true
		footnote_nr:1
		entity_output:	as_char
		toc_levels:		1..6
		smart_quotes:	lsquo,rsquo,ldquo,rdquo
		use_coderay:	false

	coderay:
		coderay_wrap:		div
		coderay_line_numbers:	inline
		coderay_line_numbers_start:1
		coderay_tab_width:	4
		coderay_bold_every:	10
		coderay_css:		style

	redcloth:
		hard_breaks:		true
</pre>

