# day02上午

## 1.DTD

### 1.1 DTD

DocType Definition：文档类型定义（DTD），用来定义文档的规范。可以内部声明也可以外部声明。

- 内部声明：<!DOCTYPE 根元素 [元素声明]>		
  - 例如：<!DOCTYPE html>
- 外部声明：<!DOCTYPE 根元素 类型 文件名>		
  - 例如：<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">

作用：规定了html使用的是哪个版本的html书写规范。

html、css、js规范维护更新的组织W3C组织。

	网址：http://www.w3.org/
	
	中文学习网址：http://www.w3school.com.cn/

### 1.2 DTD版本

==html4.01版本，结构和样式做了分离。==

==Xhtml1.0版本，规范做了拓展升级。==

	代码必须用小写字母，属性必须用双引号包裹，结束标签必须写关闭符号/等等。

==html4.01 和 Xhtml1.0这两个规范分别包含了3个小规范：==

- 	Strict  				严格版：不能使用font/b/u/i等废弃标签，不能使用框架集（Framesets）。
- 	Transitional	 过渡版（通用版）：可以使用font等废弃标签，不能使用框架集（Framesets）
- 	Frameset	     框架集版：可以使用框架集

==严格程度：XHTML1.0 Strict  >  HTML4.01 Strict  >  XHTML1.0 transitionl  > HTML4.01  transitionl==

html5 不基于 SGML规范，不再区分3个小规范，所以不需要引用 DTD。<!DOCTYPE html>

## 2.注释

==注释：就是一段用来解释说明的文档，渲染页面的时候会被浏览器忽略，而不会显示在页面中。==

	语法：<!-- 描述的内容 -->
	
		快捷键： ctrl + /
	
	注：在开始标签中有一个惊叹号，但是结束标签中没有。

浏览器不会显示注释，但是能够说明的 HTML 文档。可以利用注释在 HTML 中定义通知和提醒信息。

## 3.head标签

```<head>``` 标签用于定义文档的头部，它是所有头部元素的容器。<head> 中的元素可以引用脚本、指

示浏览器在哪里找到样式表、提供元信息等等。文档的头部描述了文档的各种属性和信息，包括文档

的标题、在 Web 中的位置以及和其他文档的关系等。绝大多数文档头部包含的数据都不会真正作为

内容显示给读者。

==下面这些标签可用在 head 部分：<meta>,<title>,<link>,<style>,<script>,<base>==

- **meta**		 提供页面相关的元信息（meta-information），标签位于文档的头部，不包含任何内容

  content		定义与 http-equiv 或 name 属性相关的元信息

  http-equiv	把 content 属性关联到 HTTP 头部（Refresh，content-type，expires，set-cookie）

  ==设置网站5秒之后跳转到其他页面  Refresh==

```html
//content="秒数；url=网址"
<!--设置网站5秒之后跳转到其他页面-->
<meta http-equiv="Refresh"content="5;url=http://www.baidu.com">
```

		name		    把 content 属性关联到一个名称（==关键字keywords、描述信息description、作者author==（有利于SEO优化））

```html
<!--关键字keywords有利于SE0-->
<meta name:="keywords"content="关键字">
<!--描述信息description有利于SE0-->
<meta name="description"content="描述内容">
<!--作者author有利于SE0-->
<meta name="author"content="">
```

- **title** 标签内部放的是网页的名字。里面的内容可以帮我们提高搜索引擎优化（SEO）

  ```html
  <title>百度一下，你就知道</title>
  ```

- **link** 定义文档与外部资源的关系。引入外部样式或者==更改网站图标icon shortcut==

```html
<!--1ink引入外部样式表stylesheet创建文档关联性-->
<link rel="stylesheet"href="./wai.css">
<!--1ink设置网页的页签图标icon设置图标--，
<link rel="icon shortcut"href="./1.png">
```

- **style**		  定义内嵌样式 

```html
<style>
		p{
		color:▣blue;
		}
</stvle>
```

- **script**		引入外部脚本，或定义内嵌脚本等 ==alert弹框==

```html
<!--引入外部js文件-->
<script src="./a.js"></script>
<!--添加内部js文件-->
<script>
alert("我是内部5s")
</script>--
```

- **base**  标签为页面上的所有链接规定默认地址或默认目标。

  			==特点：文件从哪里查找，页面打开方式==

    			==影响相对路径==
  
    			href：规定页面中所有相对链接的基准 URL。
  
    			target：在何处打开页面中所有的链接（==在新的窗口打开__blank，默认覆盖当前页面_self,==_parent，_top，framename）

```html
<!--设置网页的超链接href可以让下面没有跳转位置的超链接统一跳转给定位置-->
<!--target设置超链接的打开方式_self默认值在当前窗口打开_blank在新的窗口打开-->
<!--文件从哪里查找，页面打开方式-->
<base href="http://www.baidu.com" target="_blank">
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
<!-- head表示视口外部的内容（网页中看不到的） -->
<!-- 常用标签：<meta>,<title>,<link>,<style>,<script>,<base> -->
  	<!-- meta网页的每一条信息 -->
    <!-- charset定义编码 -->
    <meta charset="UTF-8">
  	<!-- 用户如果使用IE浏览器，用最新版本的，edge -->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
  	<!-- viewport设置视口 -->
    <!-- width=device-width: 网页宽度与屏幕宽度一样, initial-scale=1.0: 
页面打开的时候，页面使用原始比例（没有缩放） -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
    <title>Document</title>
</head>
<!-- 网页主题，视口（显示页面的区域）内部的内容 -->
<body>
    <a href="https://www.icketang.com" target="_self">爱创乐育</a>
</body>
</html>
```

# day02 下午

## 1.标签介绍
标签分为 容器级标签和文本级b

## 1.1 img 标签

img 标签是一个单标签 ，单标签在标签内使用 / 结束，当然目前是可以省略的。

img 标签的属性：

	src  		添加资源文件的路径
	
	alt  		图片加载失败后的文本提示
	
	title 		文本提示信息（鼠标悬停）
	
	width 	设置图片的宽度
	
	height 	设置图片的高度
	
	border 	设置图片的边框厚度（默认黑色实线，且不可修改）

## 1.2标签属性

必有属性 src (如果图片没有src属性不能显示)
可选属性 width
标准属性 style  title
事件属性 js的相关事件
==必有属性和可选属性是元素特有属性
标准属性和事件属性是元素的通用属性==