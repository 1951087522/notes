# day08 上午

## 列表相关属性

列表属性针对的是ul ol

​    列表符号 list-style-type

​    使用图片作为列表符号 list-style-image\:url(路径)

​    列表符号的位置 list-style-position

​    **工作中常用：list-style\:none 去掉列表符号**

```html
ol {
            list-style-type: none;
            list-style-image: url(./1.png);
            list-style-position: inside;
            background: red;
        } 
```

## 列表布局

```html
/* 
    列表布局公式
        总宽度W = 每个成员宽度iw * 成员个数n + 成员间隙m * (成员个数n - 1)
        W = iw * n + m * (n - 1)
        
    已知其三可求另一个
    
    已知： 容器总宽度1200， 成员个数4个，间距是20.  求成员宽度
        iw = (W - m * (n - 1)) / n
        iw = (1200 - 20 * 3) / 4 = 285
    已知： 容器总宽度1200， 成员个数4个，成员宽度 280.  求间距
        m = (W - iw * n) / (n - 1)
        m = (1200 - 280 * 4) / 3 = 26.66666667
*/
```

## 背景相关属性

背景相关属性 无顺序要求

    	background:
      background-color background-image background-repeat background-position background-attachment

​    **background-color 背景颜色**

​    **background-image 背景图片** url(路径)

​        如果背景图片与元素宽度高度小 则会默认水平垂直平铺 直到铺满整个区域

​        如果背景图片与元素的宽度高度大 则会显示元素宽度高度的背景图 默认从图片的左上角开始加载

​        背景颜色和背景图片可以同时存在

​    **background-repeat 背景图片的平铺方式**

​			repeat默认值平铺 repeat-x 水平平铺 repeat-y 垂直平铺 no-repeat 不平铺

​    **background-position 背景图片的位置** 默认是在左上角

​      值：background-position-x

​			backround-position-y

​      可以是具体的方向值 left center right

​										top center bottom  如果水平垂直都是剧中的可以缩写成一个

​      可以是具体的数字 （先左右后上下）正值向右向下 负值向左向上

​      可以是百分比值 （元素的宽度高度-图片的宽度高度）\*百分比值

​    **background-attachment 背景图片的固定** scroll滚动  fixed固定
==使用fixed固定时，position图片位置相对于视口页面的距离==

```html
.top1 {
             background-color: blue; 

             background-image: url(./1.png);

             background-repeat: no-repeat; 

             background-position: right bottom; 
             background-position: center; 
             background-position-x: right;
             background-position-y: bottom; 
             background-position: 40px 10px; 
						 background-position: 30% 20%; 

             background: 40px 10px blue url(./1.png) no-repeat ;

					   background-attachment: fixed;
        }
```

## 网页中所支持的图片格式

​    jpg 普通图片

​    png 支持背景透明

​    gif  支持背景透明且支持动画

​    svg  支持背景透明 放大缩小不失真

​

​    通过img标签插入的图片	可以右击图片另存为次图片上不能写文字

​    通过css加载的背景图片 	必须有指定宽高才可以显示	不可以右击另存为  背景图片上面可以添加文字或图片

# day08 下午

## logo优化

​    使用h1标签 以背景图片的形式添加 h1标签里面添加文字信息

​    如何隐藏文字

​    1： font-size:0; 设置文字大小为0

​    2:	line-height  overflow\:hidden; 设置大于行高 溢出裁切

​    3:	text-indent 负值  overflow\:hidden; 设置首行缩进 溢出裁切

​    4:	height:0; padding-top\:logo的高度  overflow\:hidden;

​    5:	color 设置文字的颜色透明度为0

## 精灵图：雪碧图  图片整合

​    将网站上一些有规则的小图标整合在一张图片上，利用背景图片的可移动性去加载

​    优势：

​    减少向服务器的请求次数，减少网络带宽占有，提高页面加载速度，提高用户体验，减小图片体积

​    **精灵图的分类：**

*   ​    水平精灵图 宽度固定
*   ​    垂直精灵图 高度固定
*   ​    定点精灵图 宽度高度都固定

​    注意：不是所有的图片都可以整合 只能是以背景图出现的才可以整合

​       整合的图片不要过大 最好不要超过100kb

​       整合的时候注意使用方向问题

```html
.top1 {
            width: 20px;
            background: url(../../../../images/精灵图.png) no-repeat;
            padding-left: 30px;
            background-position-y: -10px;
        }
```

## 居中

        /* 单行文本 */
        /* 水平居中 */
        /* text-align: center; */
        /* 垂直居中 */
        /* 1行高与高度一致(建议) */
        /* height: 500px;
        line-height: 500px; */
        /* 2padding挤压 */
        /* padding: 100px 0; */
        
        /* 多行文本 */
        /* 水平 */
        /* text-align: center; */
        /* 垂直 */
        /* padding: 100px 0; */
        
        /* 行内元素(与文本特征类似) */
        /* text-align: center; */
        /* 垂直居中 */
        /* 1height: 500px;
        line-height: 500px; */
        /* 2padding: 100px 0; */
        
        /* 行内块 */
        /* 水平 */
        /* text-align: center; */
        /* 注意:要将行内块的基线对齐改成中心对齐 */
        /* height: 800px;
        line-height: 800px;  */
        /* padding: 100px 0; */
        /* vertical-align: middle; */
        
        /* 块元素 */
        /* 垂直 */
        padding: 100px 0;
        /* 块元素 */
        /* 水平 */
        margin: 0 auto;
        
        总结                水平            垂直
            单行文本    text-align          height/line-height,  padding
            多行文本    text-align          padding
            行内元素    text-align          height/line-height,  padding
            行内块元素  text-align          height/line-height (行内块基线对齐改成居中对齐),  padding
            块元素      margin              padding 
        padding挤压问题: 父元素高度不确定

文本、行内元素、行内块元素

​	**水平居中:** text-align：center ;**垂直居中:** 高度固定 **单行**line-height 高度不固定是padding  **多行**使用padding计算

**单行文本、行内元素、行内块:**

​	高度固定:

```html
text-align: center; 水平居中   line-height: 100px;(s行高)垂直居中
```

​	高度不固定:

```html
text-align: center;		padding: 50px 0;(向上下没下撑大50像素)
```

**多行文本、行内元素:**

​	高度固定:

```html
.duo1 {
            height: 100px;
            text-align: center;  水平垂直居中
            padding-top: 34px;  （行高-行数*默认文字大小16px）/上下2
            box-sizing: border-box;
            line-height: 1;
        }
```

​	**多行内、行内块元素高度固定**：

```html
.dhang1 {
            height: 100px;
            text-align: center;
            padding-top: 29px;  （行高-行数*默认span文字大小21px）/上下2
            box-sizing: border-box;
        }
```

高度不固定:

```html
text-align: center;		padding: 50px 0;(向上下没下撑大50像素)
```

**注意：行内块如果高度不一致添加  vertical-align\:middle**

**块级元素：**

​	高度固定：

​	水平居中 margin：0 auto  垂直居中 padding或margin计算

```html
.kuai1 p {
            margin: 0 auto;
            margin-top:25px; (行高/2)造成塌陷overflow：hidden解决
        }
```

​	高度不固定：

```html
.kuai2 {
            padding: 50px 0;
        }
.kuai2 p {
            margin: 0 auto;
        }
```

### ​    使用表格很方便实现水平垂直居中

**display\:table-cell\
水平居中text-align\:center
垂直居中vertical-align\:middl
注意：块元素需要设置margin:0 auto**

```html
.biao {
            width: 400px;
            height: 300px;
            display: table-cell;
            text-align: center;
            vertical-align: middle;
        }
        p {
            margin: 0 auto;
        }
```

