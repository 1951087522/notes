# day03上午

## 1. 路径

==绝对路径==：目标文件所在的完成的路径（带盘符）

*   本地文件的绝对路径 D:\素材\其他\a.jpg
*   服务器的绝对路径：<https://www.icketang.com/demo.jpg>

==相对路径==：目标文件相对于引用文件所在的位置

*   /   根目录（绝对路径）
*   ./  当前目录（相对路径）
*   ../  退出当前目录（相对路径）

## 2.标签

### 2.1 标题标签h

h1-h6标题标签

```html
<h1>标题一共六级选,</h1>
<h2>文字加粗一行显。</h2>
<h3>由大到小依次减，</h3>
<h4>从重到轻随之变。</h4>
<h5>语法规范书写后，</h5>
<h6>具体效果刷新见。</h6>
```

给文本添加标题语义用，不能互相嵌套

*   标题标签独占一行
*   默认加粗
*   具有换行符
*   从h1开始，文本逐渐变小

\==**语义：**==

*   **标题**
*   **h1语义最强，h6语义最弱**
*   **一个页面中，一级标题只能出现一次，其它的可以出现多次**

由于 h 标签拥有确切的语义，因此要选择恰当的标签层级来构建文档的结构。

注意：请不要利用标题标签来改变同一行中的字体大小。相反，我们应当使用css层叠样式表定义来达到漂亮的显示效果.

### 2.2 段落标签p

给文本添加段落语义

具有换行符

```html
<p>你好</p>
```

注意：

	p 标签可以嵌套在块级元素内，
	
	但 p 标签不能嵌套其它块元素，如 div，h系列标签，p标签等

**==注意 段落标签和标题标签不能嵌套==**

### 2.3 div 和 span 标签

==div   span  都没有自带的样式
div     	布局块     		独占一行适合做外围标签时使用
span 	布局节点 		一排显示 自左向右在一行显示 适合做某一节点使用==

div ：容器级标签，（“division” 范围、区域、分割）用来划分独立的逻辑块

	是一个容器（大的区域），本身在网页中没有任何的默认样式，可以放置任何标签和内容。
	
	里面可以存放一些相近、相同功能的标签。div会把页面分割成一个小的区域。

span ：文本级的标签，只能放文字、图片、表单元素等，用来修饰部分文本的效果

	注：div作为容器，如果没有内容撑起，或者不设置宽高，是没有默认效果
	
			span 标签需要配合后面学的 css 样式使用
	
	大的内容放在div里，小的内容放在span。
	
	==div盒子内容会独占一行，span的内容会写在一行==

```html
 <div>
       <span>新闻</span>
       <span>视频</span>
       <span>工艺</span>
       <span>军事</span>
       <span>体育</span>
</div>
```

### 2.4 a标签

一个网站中有很多的页面，页面之间通过超链接进行跳转，超链接可以看做是网页的灵魂。

a 标签是一个双标签，有开始标记和结束标记
语法：

```html
<a href="打开地址">超链接</a>
```

默认:超链接是蓝色带下划线
注意：如果超链接有具体的地址显示的是紫色带有下划线，超链接点击过后就会变成紫色
如果没有添加具体地址的是空连接

属性：

*   	href 			设置超链接的地址  #代表链接到当前页面或页面的某个位置

*   	title 			鼠标悬停时文本提示信息

*   	rel			   规定当前文档与被链接文档之间的关系

*   	==download	规定被下载的超链接目标==

        	download="下载文件夹"

    ```html
    <!--download属性值表示下载文件的名称-->
    <a href="../../02 html/movie/01 DTD.mp4"download="QQ">dtd/a>
    ```

*   	media		规定被链接文档是为何种媒介/设备优化的。

*   	type			规定被链接文档的的 MIME 类型。

*   	target         页面的打开方式  属性值

        		\_self			当前页面中打开新的页面（新页面覆盖当前页面）
    
        		\_blank 		在新的页面中打开（会创建一个新的窗口来打开新页面）

***

==iframe 嵌入其它页面==

			==scrolling=”no“  去除滚动条==
	
			==frameborder="no"  去除边框线==

```html
<!-- 被嵌入的网站 注意有些网站不支持 target要与下方name属性值一样 -->
    <a href="http://www.suning.com" target="suning">苏宁</a>

    <!-- 嵌入其他的页面 src 后地址路径  frameborder 边框厚度-->
    <!-- name 属性表示给网站起名字 -->
    		<iframe src="http://www.taobao.com" name="suning" frameborder="1">淘宝</iframe>
```

***

\_parent		页面的父窗口或父框架中打开

\_top			将页面在整个浏览器窗口打开

==首先要创建 top页面（相对于child来说顶层页面）==

```html
<h1>top页面</h1>
<iframe src="./06-框架-parent.html" width="1000" height="200" frameborder="1"></iframe>
```

==其次创建parent页面（相对于child来说 父级窗口页面）==

```html
<h1>parent 页面</h1>
    <iframe src="./07-框架-child.html" width="600" height="300" frameborder="1"></iframe>
```

==创建child页面==

```html
<h1>child 页面</h1>
    <!-- 当前页面打开 -->
    <a href="http://www.taobao.com" target="_self">淘宝默认</a>
    <!-- 新窗口打开 -->
    <a href="http://www.taobao.com" target="_blank">淘宝新标签</a>
    <!-- 父级窗口打开 -->
    <a href="http://www.taobao.com" target="_parent">淘宝父级窗口</a>
    <!-- 顶层窗口打开 -->
    <a href="http://www.taobao.com" target="_top">顶层窗口</a>
```

framename	在指定的框架中打开页面

### 2.5 锚点

锚点作用：实现同一页面不同位置的跳转

锚点的设置

	==给标签设置id属性（要求id是唯一的）==

==通过另外一个a标签href属性去链接这个锚点==

*   	==可以链接到当前页面中的指定位置，==

*       ==还可以链接到其他页面的指定部分。==

语法：

锚点在a标签中的href属性里写#id名称
id名称是你要跳转的位置的id名

==注意：锚点必须使用id href.里的#不能丢==

==任何标签，设置了id都可以作为锚点的标记==

==a标签除了使用id还可以使用name==

```html
		<div>001</div>
    <div>002</div>
    <div id="名称">003</div>
    <div>004</div>
    <div>005</div>
    <div>006</div>

    <div>
        <a href="#id名称">003</a>
    </div>
```

# day03 下午

## 1.列表

### 1.1无序列表：

给文本添加无序的列表语义。项与项之间没有顺序的先后之分（没有先后顺序）。

*   	ul	unordered list 无序列表。
*   	li	list item 列表项。

ul 和 li 这两个标签必须同时出现，不能单独书写。

ul 里可以嵌套一个或者多个 li 标签，但 ul 里面只能放li标签，不能放其他内容。

==li 标签是一个典型的容器级标签，可以放置任何的内容标签。甚至可以放置子级ul 与 li。==

ul 和 li只能添加无序列表的语义，默认 li 的前面小圆点的样式，

==通过 type 属性修改：==

*   		disc 	默认值 实心圆
*   		circle  空心圆
*   		square 实心方块
*           none   无

```html
		<!-- 默认值 disc实心圆 -->
    <ul>
        <li>实心圆 disc</li>
        <li>实心圆 disc</li>
        <li>实心圆 disc</li>
    </ul>
    <!-- circle 空心圆 -->
    <ul type="circle">
        <li>空心圆 circle</li>
        <li>空心圆 circle</li>
        <li>空心圆 circle</li>
    </ul>
    <!-- square 实心方块 -->
    <ul type="square">
        <li>实心方块 square</li>
        <li>实心方块 square</li>
        <li>实心方块 square</li>
    </ul>
    <!-- none 没有 -->
    <ul type="none">
        <li>无</li>
        <li>无</li>
        <li>无</li>
    </ul>
```

### 1.2 有序列表

有序列表：给文本添加有序列表的语义。

*   	ol		ordered list有序的列表
*   	li		list item列表项。

ol 和 li 也是一组标签，必须同时出现。

ol 里只能嵌套 li 标签。li 不能脱离 ol 自己出现。ol 里可以嵌套多个 li 标签。

==li 是一个容器级标签，里面可以放置任何内容，甚至 ol、ul。==

虽然显示会有阿拉伯数字排序，但是我们 ol 的作用仅仅是添加有序列表的语义。数字样式不是 ol 的

作用。

==通过 type 属性修改：==

*   	 type 		属性设置有序列表的项目符号（ol 有且只有5个）

		1    阿拉伯数字 默认值
	
		A   大写英文字母
	
		a   小写英文字母
	
		I    大写罗马数字
	
		i    小写的罗马数字

*   	start 		设置列表符号从第几个开始排列，属性值只能是阿拉伯数字

*   	reversed 		倒序（html5 新增）

		这是一个布尔型的属性，关于这类型的属性有两种写法：
	
			reversed
	
			reversed = “reversed”

```html
<!-- 1 默认值  start 开始  reversed倒序-->
    <ol type="1" start="5" reversed>
        <li></li>
        <li></li>
        <li></li>
    </ol>
    <ol type="A">
        <li></li>
        <li></li>
        <li></li>
    </ol>
    <ol type="a">
        <li></li>
        <li></li>
        <li></li>
    </ol>
    <ol type="i">
        <li></li>
        <li></li>
        <li></li>
    </ol>
    <ol type="I">
        <li></li>
        <li></li>
        <li></li>
    </ol>
```

\==**注：ul、ol 项目符号不能互相使用；但是li标签可以使用它们任何一个**==

### 1.3 自定义列表

dl 定义列表给我们的文本添加定义列表语义。

==自定义列表dl dt(标题)dd(答案)
注意：工作中一个dl只能有一个dt可以同时存在多个dd==

```html
<!--d1表示定义列表整体，dt是其中的标题，dd是其中的说明文案
```

*   	dl		definition list 定义列表
*   	dt		definition title定义标题
*   	dd		definition description定义描述

dl 里面嵌套了 dt 和 dd。dt 和 dd 是同一级的标签。dd 是对 dt 的解释、说明、定义。

dl 里面只能放置 dt、dd。dt 和 dd都是容器级标签，里面的内容是不限制的。

dl 里面可以放置多组的 dt 和 dd。dt 后面的 dd 可以有多个。这些 dd 都是在解释上面的dt。

dt 后面可以没有 dd，表示没有解释说明。

实际工作中，经常将每一组 dt 和 dd 单独放在一个 dl 标签内部。

## 2、表格

### 2.1 表格基础

table：定义表格容器。

*   	tr		table rows		定义行
*   	th 		table head		定义表头
*   	td		table dock		定义单元格

三层嵌套关系：table > tr > th | td。

最简单的表格要求每一行的单元格个数相同。

如果表格有表头的概念：需要将 td 标签变成 th。

属性：

*   border  设置表格的边框

*   bordercolor 		设置边框的颜色

*   height 、 width  	设置表格的宽高

*   ==align  			设置表格的整体水平对齐方式  center 居中  left 默认居左  right居右==

*   **==cellspacing 		设置边框与边框之间的距离==**

*   **==cellpadding 		设置内容与边框之间的距离==**

*   background  		设置表格的背景图片

*   bgcolor 			设置表格的背景颜色

*   summary 		表格隐藏信息，用来提高SEO

### 2.2 tr，td 与 th 标签

**tr：行语义，一对 table 标签中有几对 tr 标签，就代表这个表格中有多少行，属性如下：**

*   	==align 				设置的当前行单元格中文本的水平对齐方式 默认值left  right  center==
*        ==valign              垂直对齐 valign=默认值midll  top   bottom==
*   	bgcolor、background  	设置背景颜色和背景图片

**td(th)：单元格语义，一对tr标签中有几对td(th)标签，就代表当前行有多少个单元格**

	th中的文本具有默认加粗，居中的效果

*   	bgcolor、background  	设置背景颜色和背景图片
*   	colspan  				列合并；左右合并
*   	rowspan 				行合并；上下合并

### 2.3 表格的标题

caption在表格的上边处于居中位置即便'写在下边也会显示在表格的上边

### 2.4 合并表格

td | th 有两个属性，可以合并单元格。

*   rowspan：跨行合并
*   colspan：跨列合并

属性值：是几就合并几个单元格。

```html
<!-- 
        合并单元格
            1 根据几行几列写出表格
            2 找出合并的单元格
            3 对第一个单元设置colspan和rowspan属性
                colspan     表示列合并，横向左右合并
                rowspan     表示行合并，纵向上下合并
            4 将被合并的单元格注释掉
            5 向单元格写入内容
 -->
```

### 2.5 表格的分组

表格的分组：
按照数据行分组   表头thead 表体tbody 表尾tfoot
==注意：一个表格只能有一个thead一个tfoot可以同时存在多个tbody==
顺序：thead tfoot tbody
要使用就一起用，要么就都不用

### 2.6 表格的分割线

分割线rules
==all	横向和竖向
none	默认值
rows	横向
cols	竖向==
groups组	与col和colgroup连用	可以使用span将相邻的几列合并
col没有竖着的线条	colgroup有竖线条

col是单标签	colgroup是双标签

```html
<table rules="all">
```

```html
<table rules="groups">
        <colgroup></colgroup>
        <colgroup bgcolor="red" span="2"></colgroup>
        <!-- <colgroup></colgroup> -->
        <colgroup></colgroup>
        <colgroup></colgroup>
        <colgroup></colgroup>
```