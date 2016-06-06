# sass 样式的语法

> 参考网址[w3cplus](http://www.w3cplus.com)

#### 文件的后缀名

> 有两种后缀名：sass 和 scss

- **差别**
> sass 后缀的文件在内容书写时不使用**大括号**包围内容  
> 同时，每一语句结束不需要使用**分号**申明。

> scss 后缀的文件在内容书写时必须使用**大括号**包围内容  
> 同时，每一语句结束必须使用**分号** 申明。

<pre>
	//文件后缀为sass 的语法示例
	body
		background: #eee
		font-size: 12px
	p
		background: #000cc1
		
	//文件后缀为scss 的语法示例
	body {
		background: #eee;
		font-size: 12px;	
	}
	p {
		background: #000cc1;
	}
</pre>

#### 导入

> sass 的导入（@import） 会在编译的时候将`@import`的scss文件合并进来生成一个css文件。  
> 如果导入的是css 文件`@import 'test.css'`,编译后不会合并文件，而是以`@import` 的命令方式存在。  
> sass 所有导入的文件都可以忽略后缀名`.scss`  
> 一般规范的基础文件命名方法以 _ 开头，这种文件在导入的时候可以忽略下划线进行导入。

#### 注释

> 两种注释方式 `/* */` 和 `//` 的方式，`//` 为单行注释，编译后不会出现在css文件中

#### 变量

> sass 的变量必须以`$` 开头定义，变量值与变量名之间用 `：` 分隔开。
> 值的后面加 `!default` 表示设置为默认值。

> 覆盖默认值的方式是，在默认变量之前在声明变量即可

- 特殊变量

> 一般定义的变量都为属性值，可以直接使用  
> 但是如果变量作为属性或在某些特殊情况下等则必须使用 `#{$variables}` 的形式使用。  
> 例如：

<pre>
	//sass style
	$borderDirection: top !default;
	$baseFontSize: 12px !default;
	$baseLineHeight: 1.5 !default;
	
	// 应用class 和 属性
	.border-#{$borderDirection}{
		border-#{$borderDirection}: 1px solid #ccc;
	}

	// 应用与复杂的属性值
	body{
		font:#{$baseFontSize}/#{$baseLineHeight};
	}
	
</pre>

- 多值变量 （可以使用函数处理list 或 map）

> 多值变量分为list 类型和map 类型  
> **list** 数据可以通过空格，逗号或者小括号 分割多个值。  
> list 可以使用`nth($var,$index)` 进行取值。

<pre>
	//一维数组
	$px: 5px 10px 20px 30px;
	
	//二维数组
	$px: 5px 10px, 20px 30px;
	$px: (5px 10px) (20px 30px);
	
	//使用方法
	
	$linkColor: #08c #333 !default; //第一个值默认值，第二个为鼠标滑过值
	a {
		color:nth($linkColor,1);
		
		&:hover {
			color:nth($linkColor,2);
		}
	}
</pre>

> **map** 类型  
> map 数据以key 和 value 成对出现，其中 value 可以是list  
> 格式为 `$map: (key1: value1, key2: value2);`  
> 通过map-get($map,$key) 进行取值。

- **全局变量**

> 通过在变量值后面加上`!global` 即为全局变量。  
> global 目前还无法使用。

#### 嵌套（Nesting）

> 嵌套包括 选择器的嵌套 和 属性的嵌套  
> 一般用到的是选择器的嵌套。  

- **选择器的嵌套**

选择器的嵌套指的是一个选择器中嵌套另一个选择器来实现继承。  
可以使用`&` 表示父元素选择器。

<pre>
	//sass style
	
	#top_nav{
		line-height: 40px;
		text-transform: capitalize;
		background-color: #333;
		li {
			float: left;
		}
		a {
			display: block;
			padding:0 10px;
			color: #fff;
			
			&:hover {
				color: #ddd;
			}
		}
	}
</pre>

- **属性的嵌套**

属性的嵌套指的是有些属性拥有同一个开始单词，如border-width,border-color等都是border 开头。

<pre>
	//sass style
	
	.fakeshadow {
		border: {
			style: solid;
			left: {
				width: 4px;
				color: #888;
				
			}
			right: {
				width: 2px;
				color: #ccc;
			}
		}
	}
	
	//编译出css 的结果
	
	.fakeshadow {
		border-style: solid;
		border-left-width: 4px;
		border-left-color: #888;
		border-right-width: 2px;
		border-right-color: #ccc;
	}
</pre>


#### 混合（mixin）

> sass 中使用`@mixin` 声明混合，可以传递参数，参数名以$符号开始，可以多个参数以逗号分开  
> 也可以使用默认值。  
> 声明的`@mixin` 通过 `@include` 来调用。

<pre>

	//sass style
	// 没有参数的mixin
	@mixin center-block {
		margin-left: auto;
		margin-right: auto;
	}
	.demo {
		@include center-block;
	}
	
	
	//有参数的mixin
	@mixin opacity($opacity:50){ //$opacity 默认值为50
		opacity: $opacity / 100;
		filter: alpha(opacity=$opacity);
	}
	.opacity {
		@include opacity; //参数使用默认值
	}
	.opacity-80 {
		@include opacity(80); //传递参数
	}


	//多个参数的mixin
	@mixin horizontal-line($border:1px dashed #ccc, $padding:10px){
		border-bottom:$border;
		padding-top:$padding;
		padding-bottom:$padding;
	}
	.imgtext-h li {
		@include horizontal-line(1px solid #ccc); //传递参数
	}
	.imgtext-h--product li {
		#include horizontal-line($padding:15px); //传递参数
	}
	
	//多组值参数mixin
	//--------------------------------
	//如果一个参数可以有多组值，如box-shadow 等，那么参数在变量后加三个点表示
	// 如 $variables...
	//多组参数之间用逗号分开。
	@mixin box-shadow($shadow...){
		-webkit-box-shadow:$shadow;
		box-shadow:$shadow;
	}
	.box {
		border: 1px solid #ccc;
		@include box-shadow(0 2px 2px rgba(0,0,0,.3),0 3px 3px rgba(0,0,0,.3));
	}
</pre>

#### 占位选择器 `%`

> 占位选择器 类似@mixin  的功能  
> 区别是占位器如果没有去调用到的话，在编译的时候便不会将样式添加进入  
> 占位选择器以`%` 表示定义，通过`@extend` 进行调用


#### 函数

> sass 定义了很多函数提供使用，也可以自己定义函数使用，以`@function` 开始  
> 常使用的颜色函数，lighten减淡 和 darken 加深  
> 调用方法 `lighten($color,$amount)` 和 `darken($color,$amount)`  
> 它们第一个是颜色值，第二个是百分比。

<pre>
	//sass style
	$baseFontSize: 10px !default;
	$gray: #ccc !default;
	
	// pixels to rems
	@function pxToRem($px){
		@return $px / $baseFontSize * 1rem;
	}
	.test {
		font-size:pxToRem(16px);
		color:darken($gray,10%);
	}
	
	//css 结果
	.test {
		font-size:1.6rem;
		color:#B3B3B3;
			
	}
</pre>


#### 运算

> sass 具有运算的特性，可以对数值型的value 进行加减乘除。  
> 运算符前后必须留一个空格。

#### `@if` 判断

> `@if` 可以单独使用，也可以结合`@else` 使用

<pre>
	//sass style
	//--------------------------------
	$lte7: true;
	$type: monster;
	.ib {
		display:inline-block;
		@if $lte7 {
			*display:inline;
			*zoom:1;
		}
	}
	
	p {
		@if $type == ocean {
			color: blue;
		} 
		@else if $type == matador {
			color: red;
		}
		@else {
			color: black;
		}
		
	}
</pre>

> **三目判断** `@if($condition, $if_true, $if_false)`  
> if(true, 1px, 2px) => 1px

#### for 循环

> for 两种形式  
> @for $var from <start> through <end>  
> @for $var from <start> to <end>  

> through 方式包括end 的值  
> to 方式不包括end 的值

<pre>
	//sass style
	//--------------------------------
	@for $i from 1 through 3 {
		.item-#{$i} { width: 2em * $i; }
	}
</pre>


#### each 循环

> @each $var in <list or map>  
