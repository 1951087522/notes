# day09

## 浮动

==会给盒子让出位置，不会给文字让出位置（所以造成文字环绕效果）==

==浮动的元素不能通过text-align\:center或者margin:0 auto==

float:	left	 right	 none

​    作用：让竖着的东西横过来

​    效果：

​    1：让竖着的东西横过来

​    2：如果浮动的元素的总宽度超过了父元素的总宽度 那么会自动换行

​    3：浮动的元素后边一个紧跟着前边一个去显示（如果高度不一致会导致换行时会有一定的空隙）

​    4:   浮动的元素后边一个紧跟着前边一个去显示 如果float：right 会导致顺序颠倒

​      **解决方式：给在一排的元素添加float\:left，保证顺序不颠倒。**

​					**给所有的float\:left的元素添加一个父元素，让这个父元素float\:right**·

```html
.top {
		float: right;
     }
.top div {
     float: left;
         }
```

​    5:   float的元素浮向自身父元素的边界

​    6：float的元素浮向自身行的边界

​    7：float的元素会脱离正常的文档流 	导致后面的上来，会占据自身的空间造成文字环绕效果

​      **解决方式 给下面的元素添加clear\:left	 right	 both 	none（默认值）**

清除浮动 左浮动 clear\:left

清除浮动 左浮动 clear\:right

清除两边浮动 	clear\:both

```html
.top3 {
            width: 300px;
            height: 100px;
            background: pink;
            clear: both;
        }
```

​    8:   float的元素如果你设置了宽度高度那么就是你设置的宽高，如果没有设置都那么就是内容的宽高

```html
.main {	
            height: 100px;		设置了宽度高度那么就是你设置的宽高
            background: orange;		
            float: left;
        }
<div class="main">111</div>		如果没有设置都那么就是内容的宽高
```

​    9：如果元素添加了float属性那么相当于给元素添加了display\:block

## inline-block与float

inline-block：

​	中间存在空隙

​	高度不一致，不会导致顺序错乱

float：

​	中间不存在空隙

​	高度不一致，会导致顺序错乱

## PS相关操作

​	1如何打开图片

​    **文件————（找到图片所在位置）打开**

​    2取色

​    **左侧色块单机————调出拾色器面板————鼠标点击你想要知道颜色的位置**

​    3测量

​    **左侧矩形选框工具————鼠标框选你要知道尺寸的位置————w代表宽度h代表高度**

​    如果ps版本比较低没有wh的提示——————去上面菜单栏中找到窗口——————信息

​    如果你的尺寸不是整数 说明单位不对 信息面板左侧加号位置将单位更改成像素

​    **ctrl+r显示或隐藏标尺 在标尺中右击将单位更改成像素**

​    放大 alt+滚轮向上 ctrl++

​    缩小 alt+滚轮向下 ctrl+-

​    抓手工具 按照空格键不放

​    如何测量文字的大小 测量文字的高度

​    测量文字的行高  从一行文字的开始到另一行文字的开始

​    4截图

​    一个个截图

​    **矩形选框工具————选中你要截取的图片————ctrl+c————ctrl+n————enter————ctrl+v————ctrl+alt+shift+s**

​    批量截图

​    使用切片工具————将你要截取的图片一次选择————ctrl+alt+shift+s————存储（选择所有用户切片）

## 图片的分类

```html
<!--  
        图片的分类
        
            矢量图  通过点和线绘制出来
                优势    文件小  放大不失真
                缺点    无法绘制复杂的图像
                应用    网站的图标
            位图    由一个个像素块构成  放大失真
                psd     设计师提供的源文件
                    优势    保留了图片的细节
                    缺点    文件大
                    应用    参考做网站页面
                jpg     存储内容绚丽的图片
                    优势    压缩效率高
                    缺点    不能设置透明
                    应用    网站图片
                GIF     通常由256种颜色构成
                    优势    动画效果    可以设置透明
                    劣势    色彩少  不支持半透明
                    应用    网站动图
                png-    用来替代GIF
                    优势    支持半透明和透明
                    劣势    不支持动画    图片大
                    应用    除了jpg和格式之外的图片，建议使用png8
                        全透明和不透明使用png8
                        半透明使用png24
                
                总结
                    动画    GIF
                    色彩绚丽    JPG
                    半透明  png-24
                    全透明和不透明和其余的png-8
-->
```

## 高度塌陷

######

        <!-- 浮动的特征
                        1浮动的子元素会按照浮动的方向   挤在一行显示
                        2浮动的元素 可以设置宽高内外边距    宽高默认由内容撑开  设置宽高后由宽高撑开
                        3浮动的元素 外边距不会合并
                        4浮动的元素 脱离正常文档流  后面的元素会移动上来 需要设置清除浮动   但是父元素盒子依然没有高度塌陷
                        5浮动的元素 父盒子没有高度会塌陷
                        6浮动的元素 脱离正常文档流设置display无效
                        7浮动的元素 会改变属性  不改变属性设置同一个方向浮动-->

        <!-- 清除浮动   
                    方案一  父盒子设置高度  适应性不强  内容过多不行
                    方案二  外墙法清除浮动  后面的元素后正常渲染    问题但是父盒子高度依然没有高度塌陷
                    方案三  内墙法清除浮动  （必须是块级元素 占一行） 父盒子的HTML结构破坏-->

        <!-- 清除浮动
                    方案四  父盒子浮动  后面的元素清除浮动  问题破坏了父盒子的布局  无法实现居中
                    方案五  父盒子设置行内块   问题破坏了父盒子的布局   无法实现居中
                    方案六  父盒子设置overflow属性  问题不能设置高度    设置会溢出的内容进行裁切-->

        <!-- 清除浮动
                    方案七  伪元素清除浮动
                                    1设置content内容为空
                                    2设置转换块级元素
                                    3清除浮动
                    
                    存在问题    其他元素无法复用
                    封装一个万能清除类  .clearfix            
    -->

***

父元素没有设置高度，子元素float 会造成父元素高度的塌陷

​    1:   外墙法 (外边添加一个div,设置clear\:both) 可以解决下面元素上来的问题 **但是父元素依然没有高度**

```html
.waiqiang {
            clear: both;
        }
		<div class="top">
        <div class="left"></div>
        <div class="right"></div>
    </div>
		添加一个元素 div  设置 clear：both
    <div class="waiqiang"></div>
```

​    2:   内墙法 （里边添加一个div,设置clear\:both） 父元素可以有高度

```html
.waiqiang {
            clear: both;
        }
		<div class="top">
        <div class="left"></div>
        <div class="right"></div>
      在里边设置一个元素 添加clear：both
        <div class="waiqiang"></div>
    </div>
```

​    3：给父元素设置溢出裁切overflow\:hidden 	auto	 scroll(不建议，无论是否溢出都会显示滚动条，宽度不够自动换行)

```html
.top {
            overflow: hidden;
        }
```

​    4：万能清除法   (存在局限性 仅限目前的父元素)

​      父元素\:after {content:"";display\:block;clear\:both;height:0;overflow\:hidden;visibility\:hidden;}

```html
.top:after {
            content: "";
            display: block;
            clear: both;
            /* 高度设置0 防止在content里写内容 */
            height: 0;
            /* content 里的溢出裁切 */
            overflow: hidden;
            /* 隐藏 */
            visibility: hidden;
        }
```

​    \*\*5:   封装万能清除法  （class引入父元素） \*\*

```html
.clear:after {
            content: "";
            display: block;
            clear:both;
						/* 高度设置0 防止在content里写内容 */
            height: 0;
						/* content 里的溢出裁切 */
            overflow: hidden;
						/* 隐藏 */
            visibility: hidden;
        } 
```

​    6:给父元素设置高度
