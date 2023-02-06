# day17 布局/单位

## **1. 多列属性**

| 创建多列	     创建几个写几个           | column-count        |
| :---------------------------------------- | ------------------- |
| 设置多列间的空隙                          | column-gap          |
| 多列间的边框                              | column-rule         |
| 多列在哪里中断 （子元素）(设置子元素)     | break-inside：avoid |
| 多列标题的跨越列数（默认1）（设置给标题） | column-span：all    |

```css
        div {
            width: 1000px;
            margin: 0 auto;
            background: red;
            /* 创建多列 创建几个写几个*/
            column-count: 4;
            /* 设置多列间的空隙 */
            column-gap: 50px;
            /* 多列间的边框  缩写*/
            /* column-rule: 10px dashed #000; */
            column-rule-color: #000;
            column-rule-width: 10px;
            column-rule-style: dashed;
        }

        p {
            background: yellow;
            /* 多列在哪里中断 */
            break-inside: avoid;
        }

        h3 {
            /* 多列标题跨越的列数 */
            column-span: all;
        }
```

### **瀑布流**

1. 利用多列属性制作瀑布流
2. 其他三种布局方式 会造成中间存在空隙问题
3. 多列布局	空隙会在最后显示

```css
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        body {
            background: rgba(226, 233, 233, 0.9);
        }

        section {
            width: 95%;
            margin: 0 auto;
            /* 设置页面几列显示 */
            column-count: 5;
            /* 多列之间的空隙 */
            column-gap: 10px;
            margin-bottom: 20px;
        }

        img {
            /* 图片继承父元素的宽度 解决过大问题 */
            width: 100%;
            border-radius: 10px;
        }

        figure {
            padding: 5px;
            /* 解决中断问题 */
            break-inside: avoid;
        }

        figcaption {
            text-align: center;
        }
    </style>
</head>

<body>
    <section>
        <!-- 快捷键 -->
        <!-- figure{<img src="./images/$.jpg" alt=""><figcaption>人生不言弃</figcaption>}*10 -->
        <figure><img src="./images/1.jpg" alt="">
            <figcaption>纵有疾风起,人生不言弃</figcaption>
        </figure>
        <figure><img src="./images/2.jpg" alt="">
            <figcaption>纵有疾风起,人生不言弃</figcaption>
        </figure>
        <figure><img src="./images/3.jpg" alt="">
            <figcaption>纵有疾风起,人生不言弃</figcaption>
        </figure>
        <figure><img src="./images/4.jpg" alt="">
            <figcaption>纵有疾风起,人生不言弃</figcaption>
        </figure>
        <figure><img src="./images/5.jpg" alt="">
            <figcaption>纵有疾风起,人生不言弃</figcaption>
        </figure>
        <figure><img src="./images/6.jpg" alt="">
            <figcaption>纵有疾风起,人生不言弃</figcaption>
        </figure>
        <figure><img src="./images/7.jpg" alt="">
            <figcaption>纵有疾风起,人生不言弃</figcaption>
        </figure>
        <figure><img src="./images/8.jpg" alt="">
            <figcaption>纵有疾风起,人生不言弃</figcaption>
        </figure>
        <figure><img src="./images/9.jpg" alt="">
            <figcaption>纵有疾风起,人生不言弃</figcaption>
        </figure>
        <figure><img src="./images/10.jpg" alt="">
            <figcaption>纵有疾风起,人生不言弃</figcaption>
        </figure>
    </section>
</body>

</html>
```

## **2. 网格属性**

|                           设置网格                           | grid 块级网格  inline-grid 行内网格                          |
| :----------------------------------------------------------: | ------------------------------------------------------------ |
|                     设置网格每一列的宽度                     | grid-template-columns: 100px 200px 300px 400px;缩写       grid-template-columns: repeat(4, 100px); |
|                分配父元素的宽度 每一列占几份                 | grid-template-columns: 1fr 2fr 3fr;缩写       grid-template-columns: repeat(3,1fr); |
|                       设置网格行的高度                       | grid-template-rows: repeat(4, 100px);                        |
| 设置网格的空隙 一个值代表水平垂直  两个值 第一个代表水平 第二个代表垂直 | grid-gap: 10px 20px;                                         |
|                        划分网格的区域                        | grid-template-areas:指定grid-area;                           |

| 网格的对齐方式     | place-content: align-content justify-content 的缩写 |
| ------------------ | :-------------------------------------------------: |
| 网格内容的对齐方式 |    place-items：align-items justify-items 的缩写    |

```css
            1 /* 设置网格 grid 块级网格  inline-grid 行内网格*/
            display: grid;

            2 /* 设置网格每一列的宽度 */
            /* grid-template-columns: 100px 200px 300px 400px; */
            /* 缩写 */
            grid-template-columns: repeat(4, 100px);

                3 /* 分配父元素的宽度 每一列占几份 */
                /* grid-template-columns: 1fr 2fr 3fr; */
                     grid-template-columns: repeat(3,1fr);  

            4 /* 设置网格行的高度 */
            grid-template-rows: repeat(4, 100px);

            5 /* 设置网格的空隙 一个值代表水平垂直  两个值 第一个代表水平 第二个代表垂直 */
            grid-gap: 10px 20px;
            
            6/* 网格的对齐方式   place-content: align-content justify-content 的缩写 */
            place-content: center space-evenly;
            
            7/* 网格内容的对齐方式    place-items：align-items justify-items 的缩写 */
            place-items: center;
            
            6/* 划分网格的区域 */
            grid-template-areas:
                "a1 a1 a2 a3"
                "a1 a1 a4 a3"
                "a5 a6 a4 a3"
                "a7 a8 a8 a3"
                
                /* 分配网格的区域 */
                  p:nth-child(1) {
                    grid-area: a1;
                    }
```

```css
nav {
            width: 400px;
            height: 400px;
            background: pink;
            /* 设置网格 */
            display: grid;
            /* 设置网格的每一列的宽度 */
            grid-template-columns: repeat(3, 100px);
            /* 设置网格的高度 */
            grid-template-rows: repeat(3, 100px);


            /* 网格的对齐方式   place-content: align-content justify-content 的缩写 */
            place-content: center space-evenly;
            /* 网格内容的对齐方式    place-items：align-items justify-items 的缩写 */
            place-items: center;
        }
```

## 3. **布局**

```css
1：列表布局
       /* 
        布局公式：容器的宽度 w，盒子的宽度是 iw，边距 m，放置盒子的数量 n
            w = n * iw + (n - 1) * m
            1000 = 4 * iw + (4 - 1) * 20
            iw = (1200 - (4 - 1) * 20) / 4
       */
        实现方式：1 float方式 设置每一个距离右边的margin-right 然后父元素添加margin-right:负值
        
                 2 float方式 设置每一个距离右边的margin-right 最后一个起一个类名 设置margin-right:0;
                 不便于后期维护
                 
                 3 结构伪类选择器 (css3.0新增)
                 nth-child（）
                 
                 4 弹性布局    注意父级元素设置弹性盒
                 display:flex
                 
                 5 多列布局
                 column-count
                 
                 6 网格布局
```

- /* 实现方式1：float方式 设置每一个元素margin-right 父元素设置margin-right负值 */

```css
/* 实现方式1：float方式 设置每一个元素margin-right 父元素设置margin-right负值 */
        .top ul {
            margin-right: -20px;
        }
        .top ul li{
            float: left;
            width: 235px;
            height: 50px;
            margin-right: 20px;
            margin-bottom: 20px;
            background: pink;
        }
```

- /* 实现方式2 float方式 给最后一个元素设置类名 设置margin-right：0 */

```css
/* 实现方式2 float方式 给最后一个元素设置类名 设置margin-right：0 */
        .top .last {
            margin-right: 0;
        }
```

- /* 实现方式3 结构伪类选择器 :nth-child()*/

```css
/* 实现方式3 结构伪类选择器 :nth-child()*/
        .top ul li:nth-child(4n) {
            margin-right: 0;
        }
```

- /* 实现方式4 弹性布局 注意父级元素设置弹性盒 */

```css
/* 实现方式4 弹性布局 注意父级元素设置弹性盒 */
        .top ul {
            display: flex;
            flex-wrap: wrap;
            justify-content: space-between;
        }
```

- /* 实现方式5 多列布局 */

```css
/* 实现方式5 多列布局 
            不设置宽高 就是内容的宽高   需要子元素设置高
            排列方式    纵向    */
        .top2 {
            width: 1000px;
            height: 300px;
            background: blue;
            margin: 0 auto;
            column-count: 4;
            column-gap: 20px;
        }

        .top2 ul li {
            background: red;
            margin-bottom: 20px;
        }
```

## 4. **双飞翼布局/单飞翼**

```html
    <!-- 双飞翼布局 先加载中间  后加载左右两边
                    单飞翼删除右侧元素即可-->
    <div class="center">
        <div class="main">中间</div>
    </div>
    <div class="left">左边</div>
    <div class="right">右边</div>
```

```css
<style>
        * {
            margin: 0;
            padding: 0;
        }
        .center {
            /* 百分百宽度   占全屏 */
            width: 100%;
            height: 300px;
            background: red;
            /* 浮动后 所有元素在一行 */
            float: left;
        }
        .left {
            width: 200px;
            height: 200px;
            background: pink;
            float: left;
            /* 向左移动中间宽度100% 的距离  负值 */
            margin-left: -100%;
        }
        .right {
            width: 200px;
            height: 200px;
            background: orange;
            float: left;
            /* 向左移动 自身的宽度 到最右边 */
            margin-left: -200px;
        }
        
        .main {
            height: 300px;
            background: skyblue;
            /* 向左右   移动元素的宽    实现双飞翼布局
                            单飞翼布局  删除右侧元素即可 */
            margin-left: 200px;
            margin-right: 200px;
        }
    </style>
```

## 5. **圣杯布局**

```html
    <div class="center">
        <div class="main">中间</div>
        <div class="left">左边</div>
        <div class="right">右边</div>
    </div>
```

```css
<style>
        * {
            margin: 0;
            padding: 0;
        }

        .center {
            height: 500px;
            background: skyblue;
            /* 内边距   左右元素的宽度     */
            padding: 0 200px;
        }

        .main {
            width: 100%;
            height: 300px;
            background: red;
            /* 三个元素都浮动 在一排显示 */
            float: left;
        }

        .left {
            width: 200px;
            height: 200px;
            background: pink;
            float: left;
            /* 向左偏移 中间元素的负值 */
            margin-left: -100%;
            /* 相对定位 向左偏移自身的宽度 */
            position: relative;
            left: -200px;
        }

        .right {
            width: 200px;
            height: 200px;
            background: orange;
            float: left;
            margin-left: -200px;
            position: relative;
            right: -200px;
        }
    </style>
```

## 6. **其他方式**

#### 1.

```html
<div class="main">中间</div>
<div class="left">左边</div>
<div class="right">右边</div>
```

```css
<style>
        * {
            margin: 0;
            padding: 0;
        }
        .main {
            height: 500px;
            background: pink;
            margin-left: 200px;
            margin-right: 200px;
        }
        .left {
            width: 200px;
            height: 200px;
            background: red;
            /* 根据父元素浏览器窗口定位 */
            position: absolute;
            top: 0;
            left: 0;
        }
        .right {
            width: 200px;
            height: 200px;
            background: red;
            /* 根据父元素浏览器窗口定位 */
            position: absolute;
            top: 0;
            right: 0;
        }
    </style>
```

#### 2. 弹性盒

```html
<body>
    <div class="main">中间</div>
    <div class="left">左边</div>
    <div class="right">右边</div>
</body>
```

```css
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        body {
            display: flex;
        }

        .left {
            width: 200px;
            height: 200px;
            background: red;
            /* 更改子元素的顺序 */
            order: -1;
        }

        .right {
            width: 200px;
            height: 200px;
            background: red;
        }

        .main {
            /* 分配父元素的宽度 */
            flex: 1;
            height: 300px;
            background: pink;
        }
</style>
```

## 7. **百分比布局**

```html
<!-- 
        百分百布局
                默认情况下  margin padding width 都是父元素宽度width的百分比 默认不受border padding的影响
                                height 是父元素高度height高度的百分比
                存在定位    margin padding  width height会受父元素padding的影响
-->
```

## 8. **单位**

**等比缩放**

-  px 固定单位

  ​    em 根据自身的字号计算 自身没有 就根据离其最近的父元素的字号计算

  ​    rem 根据html-根标签计算

-  vw  视窗宽度的百分之一   假设屏幕宽度320px- 	1vw=3.2px

  ​    vh  视窗高度的百分之一   假设屏幕高度500px- 	1vh=5px

  ​    vmin   视窗宽高百分比较小的那个

  ​    vmax   视窗宽高百分比较大的那个

-   如果将HTML的字号设置为1px 

    那么1rem=1px

    如果是320的屏幕 1vw=3.2px

    1px=多少vw

    1/3.2=0.3125vw

**注意 将body的字号设置16px**

```css
html {
    /* 响应式单位 */
     /* 375px的设计图 */
    /* 375px = 100vw */
    
    /* 320px的设计图 */
    /* 320px = 100vw */
    /* 1px = 0.3125vw */
    /* 字号设置为设计图中的1px */
    font-size: 0.3125vw;
}

body {
    font-size: 16px;
}
```

### 8.1 **响应式单位**

```css
        1不用百分比的原因是，盒子嵌套，百分比计算结果不同

        2不用vw作为单位原因是：设计图像素单位与vw之间转换麻烦

            需要的单位
                可以响应式变化
                不受盒子嵌套关系影响
                设计图的px单位好转换

            可以借助rem实现这样的单位
                响应式变化：根元素rem使用响应式单位
                不受盒子嵌套关系影响：rem都相对于根元素
                设计图px单位与rem单位等价  1rem = 1px
                    屏幕宽度 390px
                    响应式单位：100vw
                        100vw = 320vw
                        1px = 100/320vw = 0.3125vw;
```

