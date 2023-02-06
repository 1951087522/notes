# day10

## 显示和隐藏

transition过渡属性 作用：可以看到从一个状态到另一个状态的变化过程；

| 属性                   | 值                    | 位置    | 过渡    |
| -------------------- | -------------------- | ----- | ----- |
| 1-display\:none      | none隐藏  	block显示     | 不保留位置 | 不支持过渡 |
| 2-visibility\:hidden | hidden隐藏   visible显示 | 保留位置  | 支持过渡  |
| 3-opacity 透明属性       | 0透明 	1不透明            | 保留位置  | 支持过渡  |

4-宽度 高度的变化 			内容会溢出 		需要做溢出裁切

```html
.top4 {
            width: 0px;
            height: 0px;
            background: rgb(101, 23, 108);
            transition: 1s;
						存在内容 需要做溢出裁切
            overflow: hidden;
        }
body:hover .top4 {
            width: 100px;
            height: 200px;
            background: rgb(21, 63, 102);
            opacity: 1;
        }
<div class="top4">1</div>  存在内容
```

## 透明

| 透明      |         |              |                                       |
| ------- | ------- | ------------ | ------------------------------------- |
| rgba    | 不会影响内容  | IE8低版本浏览器不支持 |                                       |
| opacity | 内容会跟着透明 | IE8低版本浏览器不支持 | IE的写法 filter：alpha(opacity=值) 值:0-100 |

```html
.top1 {
            width: 400px;
            height: 500px;
            background: rgba(255, 0, 0, 0.5);
        }
```

```html
.top2 {
            width: 400px;
            height: 500px;
            background: red;
            opacity: 0.5;
            filter: alpha(opacity=50);			取值范围:0-100
        }
```

## 定位

定位属性position 定位属性一般跟方向值连用	 left 	right 	top 	bottom

| 定位属性position | 定位属性一般跟方向值连用 | left  right    top bottom |                                              |                                                |
| ------------ | ------------ | ------------------------- | -------------------------------------------- | ---------------------------------------------- |
| static       | 默认值   静态     |                           |                                              |                                                |
| relative     | 相对定位         | 不会脱离正常的文档流                | 相对于自身原来位置的偏移                                 | 与margin区别：margin会影响其他元素                        |
| absolute     | 绝对定位         | 脱离正常的文档流，不占据空间            | 根据离其最近的有定位的父元素进行定位，如果父元素没有定位则根据浏览器窗口的第一屏进行定位 | 具有行内块的特征 空间由内容撑开 ==在未设置宽高的情况下，可以通过偏移量撑开盒子的宽高== |
| fixed        | 固定定位(常用于侧边栏) | 脱离正常的文档流，不占据空间            | 根据浏览器窗口进行定位 无论是否出现滚动条都会处在给定位置不动              | 具有行内块的特征 空间由内容撑开==在未设置宽高的情况下，可以通过偏移量撑开盒子的宽高==  |
| sticky       | 粘性定位(常用于吸顶)  | 不会脱离正常的文档流                | 当元素达到指定位置时会粘在那里不动                            | 注意：一定要指定位置 css3.0新增                            |

```html
.top1 {
            width: 200px;
            height: 200px;
            background: red;
            /*相对定位relative; 相对于自身原来位置的偏移  */
            left: 20px;
            top: 100px;
        }
```

```html
.top {
            height: 500px;
            width: 500px;
            position: relative;
            background: blue;
        }
.top2 {
            width: 200px;
            height: 200px;
            background: palegoldenrod;
            /* absolute 绝对定位 根据离其最近有定位的父元素进行定位，若父元素没有定位 根据浏览器窗口的第一屏进行定位 */
            position: absolute;
            left: 20px;
            top: 100px;
        }
```

```html
.top3 {
            width: 200px;
            height: 200px;
            background: url(../../../images/7.jpg);
            /* fixed 固定定位 （通常用于侧边栏 ） 根据浏览器窗口进行定位 无论是否出现滚动条 都会固定在给定位置不动  */
            position: fixed;
            right: 0;
            top: 200px;
        }
```

```html
.top4 {
            height: 50px;
            background: pink;
            /* sticky （通常用于吸顶作用）粘性定位 根据浏览器窗口进行定位 当元素到达指定位置会黏在那里不动 */
            position: sticky;
            top: 100px;
        }
```

定位的时候 left top的优先级大于right bottom

工作中想要子元素根据父元素进行定位，一般**父级相对定位 子级绝对定位**（如果父元素设置绝对定位 后续元素会进行叠加不显示）

*   **定位的元素默认情况下谁在后边谁在上边显示**

​			**可以通过z-index更改显示顺序 值越大越靠上 值可以为负值但是必须是整数**

​			**z-index必须与定位属性连用**

*   当父元素设置z-index	比较父元素得z-index值

### 百分比

> 在标准文档流中以及浮动，百分比是相对于父盒子的宽高 不包括padding
>
> 在绝对定位中 百分比相对于父盒子宽高+padding
> 宽/padding/margin/left/right 相当于父级有定位盒子的宽width+padding
> 高/top/bottom相当于父级有定位盒子的高height+上下padding

## 水平垂直居中

方式一：绝对定位absolute，向左向上离浏览器窗口50%距离，内容会到右下角，margin 设置负值向左向上调整

```html
div {
            width: 400px;
            height: 300px;
            background: pink;
            position: absolute;
            left: 50%;
            top: 50%;
            margin-left: -200px;
            margin-top: -150px;
        }
```

方式二：绝对定位 absolute 先设置margin 水平居中auto   后四个方向为0

```html
div {
            width: 400px;
            height: 300px;
            background: pink;
            margin: auto;
            position: absolute;
            left: 0;
            top: 0;
            right: 0;
            bottom: 0;
        }
```

方式三：四则运算函数方向：calc (50%-宽高一半的值)

```html
/* 四则运算函数 calc(50% - 值)  + - * / */
        div {
            width: 400px;
            height: 300px;
            background: pink;
            position: absolute;
            top: calc(50% - 150px);
            left: calc(50% - 200px);
        }
```

## 图片裁切居中

```html
.banner {
    overflow: hidden;
    position: relative;
    height: 316px;
}

.banner img {
    position: absolute;
    left: 50%;
    margin-left: -950px;
}
```

