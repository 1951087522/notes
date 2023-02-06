# day11

## hack

### html的hack：

注释显示问题：

```html
	//当IE浏览器版本低于IE9时才会显示
	//高于会当成注释


    <!--[if lte IE 9]>
		<h1>亲，版本太低了，需要升级高级浏览器</h1>
	<![endif]-->
```

### css的hack：

### css值的hack

```css
			/* IE6 hack： _ ，  如：-color:#fff; */
            _color: red;

            /* IE 6/7 hack：! $ & * ( ) = % + @ , . / ` [ ] # ~ ? : < > | ， 如：!color:green; */
            $font-size: 50px;

            /* IE 8/9 hack：\0， 如： color: red\0;  */
            /* background-color: pink\0; */
            background-color: orange\0;

            /* IE 6/7/8/9/10 hack：\9，如：color: blue\9; */
            color: green\9;
```

### css选择器的hack

```css
        /* IE6 hack：* html .box {} */
        * html .box {
            color: red;
        }

        /* IE7 hack:	.selector, {}  */
        .box,
        {
        font-size: 20px;
        }

        /* 除了 IE6 hack： html > body .selector， */
        /* > 表示子级选择器：只选择儿子级，后代级不选，IE6 不支持  */
        /* ie6以上版本认识 以下版本也不支持 */
        html>body .box {
            color: orange;
        }
```

## IE6的兼容

```html
    1、<!-- IE6不兼容交集选择器里的类选择器连写（已经修改支持了） -->
    <style>
        .box.title {
            color: red;
        }
    </style>
    <div class="box title">hello</div>

	2、<!-- 如果不写DTD IE6的盒子是内减的。其他浏览器是外扩正常渲染 -->
    <!-- IE5浏览器即使加上DTD 也是内减 -->
    <style>
        .box {
            width: 100px;
            height: 100px;
            margin: 50px;
            padding: 50px;
            background: red;
            border: 50px solid #000;
        }
    </style>
    <div class="box"></div>

	3、<!-- IE6里，两个盒子，一个浮动，一个不浮动，不浮动的盒子不会钻到浮动盒子的下面，1会占用原来标准流的位置 
		2它们之间有3px的距离-->
    <!-- 解决方式 都浮动 -->
	<style>
        div {
            width: 100px;
            height: 100px;
            background: red;
        }

        .box1 {
            float: left;
        }

        .box2 {
            width: 150px;
            height: 150px;
            background: pink;
            /* float: left; */
        }
    </style>
    <div class="box1"></div>
    <div class="box2"></div>

    	4、
		<!-- 
			双倍margin问题，情况：一些元素浮动，有一个与浮动方向相同的方向的margin,第一个元素会出现双倍边距的问题
			解决办法：
				1不能用子级元素去撑开和父级的间距，用父级的padding去挤。
				2设置margin与浮动的方向相反。
				3单独设置第一个子元素的左边距是正常边距的一半。	
			-->

    	5、<!-- 固定定位   IE6不支持 -->
   		 <!-- 解决方法  使用绝对定位 -->
            <style>
                .box {
                    width: 100px;
                    height: 100px;
                    background: red;
                    position: fixed;
                    right: 20px;
                    bottom: 100px;
                    /* 绝对定位 */
                    position: absolute;
                }	
            </style>
            <div class="box"></div>

        6、<!-- IE6/7/8 不支持opacity透明属性 -->
        <!-- 解决方法   filter: alpha(opacity=50);  值0--100的整数 -->
        <style>
            .box {
                width: 100px;
                height: 100px;
                background: red;
                opacity: 0.5;
                filter: alpha(opacity=50);
            }
        </style>
        <div class="box"></div>
```

## css单位

```html
        /* em 相对于自身字号的大小 */
        .box1 {
            font-size: 10px;
            width: 10em;
            background: red;
        }

        /* rem  相对于html根的字号大小 */
        .box2 {
            width: 10rem;
            background: pink;
        }

        /* vw   1vw相对于视口宽度的1% */
        .box3 {
            font-size: 10vw;
            background: orange;
        }

        /* vh   1vh相对于视口高度的1% */
        .box4 {
            font-size: 10vh;
            background: orange;
        }
```

## css兼容：

​        **1: 插入的图片会默认向下撑大3px**

解决方式：

*   ​		给图片添加 vertical-align\:top/middle/bottom
*   ​		或者添加 display\:block （块级元素居中text-algin：center不生效需要margin：0 auto）
*   ​		或者给父元素添加font-size:0; （父元素一旦有文字不会生效）

​        2: 使用超链接出来的图片会在ie浏览器的低版本中会默认存在边框  （ie10以下）

​            解决方式：给图片添加 border\:none;

​        **3: 表单行高不一致**

​            解决方式：给input添加float属性

​        **4: 按钮大小不一致**

​            解决方式：变成怪异盒模型box-sizing\:border-box  或者  单独设置高度，注意边框自己设置

​        5: 鼠标指针变成小手型

​            cursor\:pointer

​        **6: 透明度的兼容 opacity 其他浏览器支持**

​			ie的低版本   filter：alpha(opacity=值) 值:0-100

​        **7: 在属性前边添加**

```html
					*     ie7以下支持
   				\9   ie10以下支持
  				 \0   ie8以上支持
h2 {
            height: 100px;
            background: red;
            background: orange\0;
        }
```

## fc

### bfc 块级格式化上下文

&#x20;解析规则：

1.  自上而下排列
2.  上下margin会发生重叠 取较大值
3.  float的元素上下margin不会发生重叠
4.  元素与包含块的左侧相接触(边框线以内)想要出去 设置负值
5.  属于同一个bfc区域内的元素float不会影响外部的其他元素
6.  bfc区域计算高度的时候float的元素也会参与计算&#x9;

​        可以触发bfc的条件：
​        根元素 html
​        元素添加 float
​        元素添加 overflow\:hidden auto scroll
​        元素添加 position\:absolute fixed
​        元素添加 display\:inline-block table-cell table-caption flex inlin-flex

### ifc 行内格式化上下文

&#x20;解析规则:&#x20;

1.  高度由内容决定
2.  IFC中的元素会因为浮动 ，顺序扰乱
3.  高度不一致取最高的
4.  ifc区域添加一个块级元素 会产生两个匿名的ifc
5.  ifc高度不一致默认基线对齐 设置对齐方式以最高的为准

*   默认情况都是基线对齐

    *   vertical-align解决基线对齐问题

*   水平居中

    *   盒子居中
    *   如果盒子没有文本，盒子是以底边对齐
    *   带有文本，是以基线对齐
    *   如果有块元素，必须转为行内块元素

*   垂直居中

    *   在内部创建IFC
    *   撑开父元素的高度
    *   设置对齐方式 vertical-align

ffc 弹性格式化上下文
gfc 网格格式化上下文

## 继承

继承：子元素拥有父元素的属性及属性值

​        **可以继承的属性：**

​         		文字的相关设置都可以继承:

​          color（超链接除外）  font-size	  font-family 	font-weight	 font-style	 font-variant	 line-height

​				 文本相关          text-indent 	text-transform 	text-align	 word-spacing	 letter-spacing 	 white-space  list-style

​         **不可以继承的:**  盒子的属性都不可以 	border	padding 	margin	 content(width/height)

display	  overflow	 text-overflow  	background 	position 	 vertical-align  	text-decoration
