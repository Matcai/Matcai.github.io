---
layout: post
title: "Jekyll _config.yml"
date: 2015-12-16
backgrounds:
    - /assets/images/posts/the-desk.jpg
thumb: /assets/images/posts/the-bridge.jpg
categories: Jekyll
tags: yml 2015 jekyll config
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

