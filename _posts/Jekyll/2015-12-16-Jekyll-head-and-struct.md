---
layout: post
title: "Jekyll 结构和头信息"
date: '2015-12-16 21:37:00'
backgrounds:
    - /assets/images/posts/the-desk.jpg
thumb: /assets/images/posts/shuimo.jpg
categories: Jekyll
tags: Jekyll Code 2015 Github 
---

# Jekyll 目录结构

##### **Jekyll 核心是一个文本转换引擎。可以通过Markdown、Textile、html等实现目的**

- Jekyll 网站的目录结构

<pre>
	_config.yml
	_drafts
		some-drafts.markdown
	_includes
		header.html
		footer.html
		context.html
	_layouts
		default.html
		post.html
		page.html
	_posts
		2015-10-12-how-to-use-markdown.md
		2015-12-10-hello-world.textile
	_data
		socil.yml
	_site
	index.html
	about.md
</pre>

1. _config.yml  
	 保存主要配置数据，同时可通过配置命令行中的设置。  
	 如读取指定目录，编码，时区，排除文件等.

2. _drafts  
	 drafts 是保存未发布的文章，没有日期。相当于草稿文件  
	 drafts 草稿文件当运行`jekyll server` 或 `jekyll build --drafts`  
		将添加自动添加时间生成最新文章。

3. _includes  
	 保存一些包文件用于在布局或文章中加载使用  
	 通**&#123;% include file.ext %}** 把文件 `_includes/file.ext`包含进去

4. _layouts  
	 存放文章的模板文件，文章布局可以根据YAML头信息声明进行选择。  
	 **&#123;{ content }}** 可将content 插入页面

5. _posts  
	 存放文章的地方，格式必须符合year-month-day-title.markup  
	 (markup 可以使md,markdown,textile等)
	
6. _data  
	 存放一些数据，添加额外功能

7. _site  
	 存放Jekyll 转换完成的页面文件

8. index.html  
	 通过包含YAML 头信息，Jekyll自动识别并进行转换。

9. Other  
	 可添加一些其他文件，如css，images 到根文件夹  
	 Jekyll会根据需求自动拷贝。

## Jekyll YAML 头信息使用方法


##### **`Jekyll YAML的格式必须写在文件的开头,用三个虚线包含在中间。`**


<pre>
---
layout: post
title: "Hello world"
---
</pre>

- 头信息中可以设置预定义的变量 或者 创建一个 自己定义的变量。
- 这样就可以通过Liquid 标签来访问这些变量


#### 预定义的全局变量

- layout   
	> 指定_layouts 目录下的模板文件

- permalink   
	> 配置访问该文件的指定路径，相当路径映射

- published  
	> 设置为 `false` 将展示不具体博文

- category | categories  
	> 指定分类属性，使博文能够根据分类属性来阅读  
	> 多个类别通过 YAML list 来指定，或者用空格隔开

- tags  
	> 类似分类，给文章添加标签  
	> 多个标签通过 YAML list 来指定，或者用空格隔开

----

#### **自定义变量**

- 自定义变量可通过Liquid 模板调用

- 调用方法：

![Yaml head example](/assets/images/article/Yaml_head_example.png)

- 通过文章预定义`date`变量
- 会覆盖文件名的日期，达到更新时间分类的效果


