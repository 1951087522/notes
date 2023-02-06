# day06 上午

## 样式表

1：行内样式表（行间 内嵌）

​    写在html标签的内部 style="属性：属性值；"

​    工作中不常用:代码冗余多，耦合性高，不利于后期的维护

```html
     <div style="color:red;">div</div>
```

2：内部样式表

​    写在head之间用style包裹

​    工作中不常用:仅限于当前页面

```html
<style>css选择器</style>
```

3: 外部样式表

​    写在html外部 新建xxx.css文件

​    外部样式表需要引入才能生效

*   引入方式一（常用）：

```html
    <link rel="stylesheet" href="路径">
```

​    **rel="stylesheet" 必须要有 作用是建立文件关联性**

*   引入方式二（不常用）：

```html
<style>
@import url(路径)  必须是内部样式表中的第一行代码
</style>
```

==【link与@import的区别 两种外部样式表引入方式的区别】==

​    1：老祖宗不同：link是html提供的引入方式 可以引入css文件还可以引入其他文件(例如：引入更改页面图标)

​        					   @import是css提供的引入方式，只能引入css文件

​    2：加载顺序不同： link与html同时被加载

​									@import是当所有的html加载完成之后再加载对应的css

​    3：兼容性不同： link没有浏览器版本要求

​								@import需要在ie6以上支持

​    4：控制dom：link支持

​							@import不支持

==【三种样式表的优先级】==

​    行内样式表优先级最高，内部与外部和属性顺序有个 谁在后边谁生效（就近原则）

## 文字相关属性

1:**文字的颜色** color

2:**文字的大小** font-size

​      单位 px（常用） pt(磅) rem em

​      浏览器默认的文字大小是16px  默认情况下 1em=1rem=16px  9pt=12px

3:**文字的字体** font-family

​      如果是汉字的字体需要添加“”引号

​				例如 font-family: "楷体";

​      如果是一个英文单词的字体不需要加引号

​				例如  font-family: Arial;

​      如果是由一组英文单词组成的字体需要添加引号

​				例如 font-family: 'Times New Roman';

​      可以给一个元素同时设置多个字体，中间用逗号隔开,浏览器会优先解析第一个字体，如果第一个字体我们的电脑中不存在则向后顺延。如果设置的字体电脑中都不存在则按照系统默认字体(微软雅黑)去解析。

4:**文字是否加粗** font-weight

​							bold 加粗 bolder 更粗  lighter 细体  normal 正常

5:**文字是否倾斜** font-style

​							italic 倾斜 oblique 倾斜度大一些  normal 正常

6:**将小写字母变成小型的大小字母**

​							font-variant\:small-caps;  normal 正常

7:**行高**（行间距）

​							line-height  直接写数字不加单位代表文字大小的倍数  加单位就是具体数值

​            				如果行高与元素的高度一致可以实现单行文本垂直方向的居中

​    ==缩写：font：font-weight   font-style   font-variant   font-size/line-height font-family==

​    工作中常用缩写：font\:font-size/line-height font-family(三者缺一不可,顺序不可以更改)

# day 06下午

## 文本相关属性

*   1:文本修饰text-decoration （通常用于去除a标签下划线）

    ​     none 去掉下划线

    ​     underline 下划线

    ​     overline 上划线

    ​     line-through 中划线

    ​     blink 闪烁 （目前为止没有）

```HTML
span:hover {
            /* none 无 去除下划线 */
            text-decoration: none;
            /* undeline 下划线 */
            text-decoration: underline;
            /* overline 上划线 */
            text-decoration: overline;
            /* line-through 中划线*/
            text-decoration: line-through;
      		  }
```

可以都进行显示

```html
div {
            text-decoration: underline overline line-through;
      }
```

*   2:首行缩进 text-indent

​      首行缩进两个字的固定写法 text-indent：2em  此属性可以设置负值

> span标签设置text-indent不起作用，是因为text-indent只能给块级元素设置，而span标签是行内元素；
>
> 解决方法：使用“span{display\:inline-block;}”样式将span标签转换为行内块级元素即可。

```html
div {
  	 text-indent: 2em;
      }
```

*   3:字间距 letter-spacing 	此属性可以设置负值

*   4:词间距 word-spacing     此属性可以设置负值

*   5:英文字母大小写 text-transform

​      capitalize 首字母大写

​      uppercase 全部大写

​      lowercase 全部小写

*   6:水平对齐方式 text-align

          				   left 默认		center居中		 right居右 			justify 两端对齐（针对英文）

*   7:垂直对齐方式 vertical-align (目前只有图片支持)

          				    top 上 		middle 中 			bottom下			 baseline基线 默认值

## 边框属性

边框属性 border 是一个复合属性

​    border:边框的粗细		 边框的线型 		边框的颜色 (值无顺序要求)

​    border\:border-width	 border-style 	border-color

​    可以单独设置某一个方向 	border-方向	left、right、top、bottom

​    线型：

​			实线solid 		 虚线 dashed		 双实线 double  		点状线 dotted		  没有线none

```html
.top {
            width: 500px;
            height: 200px;
            /* border 粗细 线型 实线solid 颜色 */
            border-top: 20px solid red;
            /* dashed 虚线 */
            border-left: 10px dashed blue;
            /* double 双实线 */
            border-bottom: 9px double #000;
            /* dotted 点状线 */
            border-right: 5px dotted #f999;
        }
```

​    transparent 透明  或者rgba进行设置透明

```html
.top1 {
      width: 0px;
      border: 100px solid transparent;
      border-top-color: #f00;
        }
```

```html
.top2 {
            width: 0;
            border-top: 10px solid rgba(255, 255, 255, 0);
            border-left: 10px solid rgba(255, 255, 255, 0);
            border-right: 10px solid rgba(255, 255, 255, 0);
            border-bottom: 100px solid lawngreen;
        }
```

