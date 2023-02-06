# day 07上午

## 盒子模型

1:盒子的组成

​     margin 外边距

​	 border 边框

​	 padding 内边距

​	 content 内容（width、height）

2：margin 外边距 外补丁 外补白 外填充

*   ​     	一个值 代表的是 上下左右

```html
div {
            width: 500px;
            height: 400px;
            background: red;
            margin: 10px;
        }
```

*   ​     	两个值 代表 上下-左右        margin:0 auto    可以实现一个块的居中

```html
div {
            width: 500px;
            height: 400px;
            background: red;
            margin: 10px 20px;
        }
```

*   ​     	三个值 代表 上-左右-下

```html
div {
            width: 500px;
            height: 400px;
            background: red;
            margin: 10px 20px 30px;
        }
```

*   ​    	 四个值 代表 上-右-下-左(顺时针)

```html
div {
            width: 500px;
            height: 400px;
            background: red;
            margin: 10px 20px 30px 40px;
        }
```

​     可以单独设置某一个方向 margin-left、right、top、bottom

​     ==**margin区域：**==

​							==**不会出现背景颜色,**==

​							==**可以设置负值,**==

​							==**如果单纯一个小盒子来说 margin不影响盒子的实际大小**==

​     		               ==**如果一个大盒子里面排放n个小盒子 存在margin会影响一排摆放的个数**==

3：padding 内边距 内补丁 内补白 内填充

*   ​     一个值 代表的是上下左右
*   ​     两个值 代表 上下-左右
*   ​     三个值 代表 上-左右-下
*   ​     四个值 代表 上-右-下-左

​     可以单独设置某一个方向 padding-left、right、top、bottom

​     ==**padding区域：**==

​							==**会出现背景颜色,**==

​							==**不可以设置负值,**==

​							==**会影响盒子的实际大小**==

4:盒子实际宽度高度的算法

​     **实际width=(margin-left)+(border-left)+(padding-left)+width+(padding-right)+(border-right)+(margin-right)**

​     **实际height=(margin-top)+(border-top)+(padding-top)+height+(padding-bottom)+(border-bottom)+(margin-bottom)**

5: 盒子模型的分类

​     标准盒模型 计算方式=margin+padding+border+content

​     怪异盒模型 计算方式=margin+content

​     标准与怪异盒模型转换的属性 **box-sizing：border-box**怪异盒 content-box标准盒 默认值

## 使用

设置两个元素之间的空隙

*   ​    如果元素本身没有背景颜色用margin padding都可以

*   ​    如果元素本身有背景颜色 空隙没有背景颜色 用margin

    ​											   空隙有背景颜色  用padding

*   ​    如果元素存在边框 空隙在边框线外部用margin

    ​									空隙在边框线内部用padding

*   ​    如果鼠标悬停时存在背景颜色    必须使用padding

```HTML
a {
						
            background: red;
						
            border: 3px solid rgb(11, 10, 10);
            /* 如果元素没有背景颜色 用margin padding 都可以 */
            padding: 20px;
            /* 如果元素本身有背景颜色 空隙之间存在背景颜色 用padding 反之则用margin */

            /* 如果元素存在边框线 空隙之间有背景颜色 用padding 反之则用margin */

            /* 如果元素鼠标划入存在背景颜色 空隙之间存在背景颜色 则用padding 反之则用margin */
        }

        a:hover {
            background: rgb(11, 6, 88);
        }
```

## margin 塌陷

块级标签

    <!-- 
        margin合并
            相邻的两个同级盒子，同时设置外边距，相邻的外边距会合并，保留大的
        margin塌陷
            父盒子和第一个子盒子同时设置上外边距，外边距会合并，保留最大的
                同时子盒子的上外边距传递到父盒子外部，造成了父盒子高度的塌陷
            父盒子和最后一个子盒子同时设置下外边距，外边距会合并，保留最大的
                同时子盒子的下外边距传递到父盒子外部，造成了父盒子高度的塌陷
        解决方式    
            父子盒子外边距合并与塌陷问题，可以通过给父盒子设置“边界”实现
                如：border，overflow:hidden, padding
        工作中，为了避免父子盒子margin塌陷和合并问题，我们建议给父盒子设置，不要给第一个子盒子设置上外边距，或者最后一个子盒子设置下外边距
    -->

*   两个元素的上下margin默认情况下会发生重叠 取较大值20px

```html
.top {
            height: 100px;
            background: yellow;
            margin-bottom: 10px;
						float: left;
			}

.link {
            background: pink;
            margin-top: 20px;
						float: left;
			}
```

*   如果两个元素都存在float属性上下margin不会发生重叠 取之和30

```html
.top {
            height: 100px;
            background: yellow;
            margin-bottom: 10px;
			}

.link {
            background: pink;
            margin-top: 20px;
			}
```

​    第一个子元素的margin-top会传递给父元素 最后一个子元素的margin-bottom也会传递给父元素 这一现象叫做margin的塌陷

​    	解决方式：

    	 1:不让其是第一个子元素和最后一个子元素（一般不用）

```html
				<span>1</span>
        <div class="link1"></div>
        <div class="link2"></div>
        <div class="link3"></div>
        <div class="link4"></div>
        <span>2</span>
```

    	  2:给父元素添加border  border会影响盒子实际宽高

```html
.link {
        border: 1px solid red;
        background: yellow;
    }
```

    		3:给父元素添加padding  padding会影响盒子实际宽高

```html
.link {
        background: yellow;
        padding: 10px;
    }
```

    	    4: 给父元素添加 overflow:hidden;  内容如果存在溢出会被裁切

```html
.link {
        background: yellow;
        overflow: hidden;
    }
```

    	   5:给父元素添加float属性  会影响下面的元素

```html
.link {
        background: yellow;
        float: left;
    }
```

    	  6:给元素本身添加float属性

```html
.link1 {
        width: 100px;
        height: 100px;
        background: rgb(34, 10, 124);
        margin-top: 10px;
        float: left;
    }
```

    	  7: 给元素本身添加display:inline-block

```html
.link1 {
        width: 100px;
        height: 100px;
        background: rgb(34, 10, 124);
        margin-top: 10px;
        display: inline-block;
    }
```

# day 07下午

## margin负值

margin负值对宽度的影响：

*   如果元素没有设置宽度

​			margin正值会缩小宽度 	负值会增大宽度

*   如果元素设置了宽度

​			margin的正值负值不会影响宽度 	会影响位置

​    **margin-left的优先级要高于margin-right**

```html
.top1 {
            width: 200px;
            height: 50px;
            background: red;
            /* magin的值为正会缩小宽度
                            负值会增大宽度 */
            /* margin-left: 10px; */
            /* margin-left: -10px; */
            /* 如果元素本身有宽度
                        magin 值为正负值 则会影响位置 */
                        margin-left: -10px;
        }
```

## 百分比值

百分比值：

​    width的百分比值是父元素宽度的百分比

​    height的百分比值是父元素高度的百分比

​    margin的百分比值是父元素宽度的百分比

​    padding的百分比值是父元素宽度的百分比

```html
.top {
            width: 400px;
            height: 300px;
            background: red;
        }
        .top1 {
            width: 30%;
            height: 20%;
            background: orange;
            margin: 10%;
            padding: 10%;
        }
```

## overflow 溢出

溢出属性 overflow overflow-x overflow-y

*   ​	visible    默认值溢出
*   ​    hidden  溢出裁切
*   ​    scroll  不管内容是否溢出都会出现滚动条的位置
*   ​    auto   内容溢出显示滚动条/内容不溢出不显示
*   ​    inherit  继承父元素的overflow属性值

## white-space空白空间

空白空间 white-space

           /* normal空白折叠自动换行 */
            white-space: normal;
            /* nowrap空白折叠 没有换行 */
            white-space: nowrap;
            /* pre-line空白折叠换行保留换行符 */
            white-space: pre-line;
            /* pre空白不折叠不换行 */
            white-space: pre;
            /* prewrap空白不折叠换行*/
            white-space: pre-wrap;

​    pre 					将所有的回车换行空格缩进都解析 遇到边界不会换行 遇到br会换行

​    **pre-wrap 		将所有的回车换行空格缩进都解析 遇到边界会换行**

​    pr-line 			只解析回车换行 遇到边界会换行 空白字符解析成一个字符

​    **nowrap 		 强制在一排显示 遇到边界不会换行 遇到br换行 不解析回车换行空格**

​    normal 			默认值

```html
.top1 {
            background: red;
            width: 150px;
            white-space: normal;
            white-space: pre;
            white-space: pre-wrap;
            white-space: pre-line;
            white-space: nowrap;
			}
```

## 单行文本显示省略号

省略号 text-overflow

​    clip 裁切  不用

​    ellipsis 省略号

​    截字（单行文本溢出显示省略号）

必要条件

*   ​    有一定的宽度  width: 400px;
*   ​    强制在一排显示 white-space\:nowrap
*   ​    溢出裁切 overflow: hidden;
*   ​    省略号  text-overflow: ellipsis;

```html
div {
            width: 400px;
            white-space: nowrap;
            text-overflow: ellipsis;
            overflow: hidden;
        }
```

## 元素的分类

###### ==浏览器解析行内块或行内标签的时候，如果标签换行书写会产生一个空格的距离==

***

1：元素的分类

*   块级元素：

​      可以设置宽度高度

​      遵循盒子模型设置 padding border margin content

​      自上而下独占一行

​      块级元素作为其他行内元素的盒子去使用

​      常见的块级元素：div h1 h2 h3 h4 h5 h6 p ul ol li dl dt dd table form

*   行内元素

​      不可以设置宽度高度 他的宽度高度就是其内容

​      遵循盒子模型设置 padding border margin content 但是有些属性不能正常显示

​      自左向右在一排显示

​      常见的行内元素：a span b strong i em sup sub u ins del s br img input select textarea

​    2：元素类型的转换属性 display

​      block 块级元素

​      inline 行内元素

​      inline-block 行内块元素  img input select textarea 此元素类型支持vertical-align 根据最高的去对齐

​      list-item 列表元素

```html
 p {
            display: list-item;
            list-style-position: inside; 
        }
```

​      table-cell 表格的td或th

```html
 p {
            display: table-cell;
        }
```

​      none 隐藏

```html
span {
            display: none;
        }
```

​      大多数块级元素的display属性值默认是block li特殊是list-item table也是特殊

​      大多数行内元素的display属性值默认是inline  img input select textarea特殊是inline-block
