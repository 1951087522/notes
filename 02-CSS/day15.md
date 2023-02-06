# day15-动画与弹性盒

## 1. **关键帧 @keyframes**

可以用于鼠标悬停时 动画暂停

```css
//动画是否执行 running 执行 paused 暂停
div::hover {
    animation-play-state: paused;
    }
```

```css
<!-- 
    关键帧动画
    创建关键帧动画 在所有css的最下面创建
--------------------------------------------------------------------------------
        方法一：@keyframes 名称 {
                from {初始状态}  可以省略
                to {结束状态}
            }
            
                @keyframes play {
                     from {width: 100px;}   
                     to {
                           width: 200px;
                        }
                    }
--------------------------------------------------------------------------------
        方法二：@keyframse 名称 {
                0% {初始状态} 可以省略
                10% {状态1}
                20% {状态2}
                .....
                100% {结束状态}
--------------------------------------------------------------------------------
    引入关键帧动画 animation 
        div:nth-child(1) {
            animation: play 1s 1s 3;
                // 名称 执行时间 延迟执行时间 执行次数
                }
                
                /* 调用动画 */
                /* 第一个参数: 表示动画名称 */
                /* 第二个参数: 表示完成动画的时间 */
                /* 第三个参数: 表示缓冲描述 linear ease steps(步长)*/
                /* 第四个参数: 表示动画延迟时间 */
                /* 第五个参数: 表示动画次数  infinite无限次 (可选)*/
                /* 第六个参数: 自动补全反方向的动画。alternate (可选)*/
                /* 第七个参数: 保持最后一帧的状态 forwards(可选) */
            /* animation: donghua 2s linear 0s infinite alternate forwards; */
        }
--------------------------------------------------------------------------------
        动画的名称  必有
        动画的执行时间 必有
        延迟执行时间
        执行次数 默认是1次 想要执行几次就写阿拉伯数字几 infinite无限次循环
--------------------------------------------------------------------------------
        动画如何执行 默认是从开始到结束
            alternate 从头开始到结束 再从结束到开始
            alternate-reverse 从结束到开始在从开始到结束
--------------------------------------------------------------------------------
        动画执行完成之后保留的状态
             默认是恢复到初始状态 forwards保留在最后一个状态
--------------------------------------------------------------------------------
        动画的运动曲线
         ease默认     ease-in     ease-out     ease-in-out     linear     steps()
                div:nth-child(10) {
                    animation: play 1s ease-in;
                }
                div:nth-child(11) {
                    animation: play 1s ease-out;
                }
                div:nth-child(12) {
                    animation: play 1s ease-in-out;
                }
                div:nth-child(13) {
                    animation: play 1s linear;
                }
                div:nth-child(14) {
                    animation: play 1s steps(4);
                }
        动画是否执行 running 执行 paused 暂停
            div:nth-child(15) {
            animation: play 1s running;
        }
            div:nth-child(15):hover {
            animation: play 1s paused;
        }
            可以用于鼠标悬停时 动画暂停
                div:nth-child(16):hover {
            animation-play-state: paused;
        }
-->
--------------------------------------------------------------------------------
            /* 动画的名称 */
            animation-name: play;
            /* 延迟执行时间 */
            animation-delay: 1s;
            /* 动画的执行时间 */
            animation-duration: 1s;
            /* 动画的执行次数 */
            animation-iteration-count: infinite;
            /* 动画的运动曲线 */
            animation-timing-function: linear;
            /* 动画如何执行 */
            animation-direction: alternate;
            /* 动画执行完成之后保留的状态 */
            animation-fill-mode: forwards;
            /* 动画是否执行 */
            animation-play-state: running;
```

动画的运动曲线：

- linear	匀速		规定以相同的速度开始和结束；
- ease	相对于匀速，中间快，开始和结束慢		规定慢速开始，然后变快，最后慢速结束；
- ease-in	相对于匀速，开始慢，结束快			规定以慢速开始
- ease-out	相对于匀速，开始快，结束慢			规定以慢速结束
- ease-in-out	相对于匀速，开始和结束两头慢	规定以慢速开始和慢速结束

###  **text 轮播图**

1. 创建骨架

2. 1. 外层容器

   2. 1. 图片容器

      2. 1. 图片

3. 定义样式

4. 1. 外层容器等于每一张图片的宽高	溢出隐藏
   2. 图片容器等于图片总宽度
   3. 图片相对定位	浮动
   4. 定义动画

```css
        /* 定义外层盒子容器 */
        main {
            width: 800px;
            height: 400px;
            border: 2px solid #999;
            box-shadow: 5px 5px 5px #999;
            overflow: hidden;
            /* 窗口居中 */
            position: absolute;
            left: 50%;
            top: 50%;
            margin-top: -200px;
            margin-left: -400px;
        }

        /* 定义图片容器 */
        .box {
            width: 4000px;
        }

        p {
            /* 模糊 */
            filter: blur(2px);
            position: relative;
            width: 800px;
            height: 400px;
            float: left;
            animation: play 10s linear 0s infinite forwards;
            transition: all 1s;
        }


        p:nth-child(1) {
            background: url(../../../../images/1.jpg);
            background-size: cover;
        }

        p:nth-child(2) {
            background: url(../../../../images/2.jpg);
            background-size: cover;
        }

        p:nth-child(3) {
            background: url(../../../../images/3.jpg);
            background-size: cover;
        }

        p:nth-child(4) {
            background: url(../../../../images/4.jpg);
            background-size: cover;
        }

        p:nth-child(5) {
            background: url(../../../../images/1.jpg);
            background-size: cover;
        }

        div:hover p {
            /* 鼠标悬停 暂停动画 */
            animation-play-state: paused;
            /* 恢复模糊 */
            filter: blur(0px);
        }

        @keyframes play {
            0% {
                left: 0px;
            }

            20% {
                left: -640px;
            }

            40% {
                left: -1280px;
            }

            60% {
                left: -1920px;
            }

            80% {
                left: -2560px;
            }

            100% {
                left: -3200px;
            }
        }
    </style>
</head>

<body>
    <main>
        <div class="box">
            <p></p>
            <p></p>
            <p></p>
            <p></p>
            <p></p>
        </div>
    </main>
</body>
```

## 2. **弹性父元素的属性**

切记不要存在margin: 0 auto;

否则后三个属性不生效。

### 2.1 **容器属性**

```css
<!-- 
    1:设置弹性盒 父级添加
            display：flex 块级弹性盒、inline-flex 行内弹性盒
            display: flex;
           默认弹性子元素都在一排显示，如果宽度或高度不够不会换行；
           会自动收缩弹性子元素，直到不能再收缩为止那么会溢出
--------------------------------------------------------------------------------
    2:是否允许弹性子元素进行换行显示 flex-wrap    (是否换行)
            flex-wrap: wrap;
        nowrap 不允许 默认值
        wrap 允许
        wrap-reverse 允许换行并且翻转（内容不会翻转）
--------------------------------------------------------------------------------
    3:弹性子元素的排列方式 flex-direction (决定主轴方向)
            flex-direction: column;
        row 默认值 自左向右在一排显示
        row-reverse 自左向右在一排显示并且翻转
        column 自上而下
        column-reverse 自上而下翻转
--------------------------------------------------------------------------------
    4:flex-flow：flex-direction flex-wrap的缩写
        /* flex-wrap: wrap;
        flex-direction: column; */
             flex-flow: column wrap;
```

### 2.2 **水平对齐/垂直对齐/多行对齐**

```css
    5:主轴对齐方式 justify-content    (水平对齐)
            justify-content: center;
            如果自左向右排列   row     x轴水平方向是主轴  
               自上而下排列  coloumn   y轴 垂直方式是主轴
               
        flex-start 开始 默认 (居左)
        flex-end  结束    (居右)
        center  居中
        space-around  中间是两端的倍数
        space-between  两端对齐
        space-evenly   平均分配
--------------------------------------------------------------------------------
    6:侧轴（交叉轴）对齐方式 align-items    (垂直对齐)
            align-items: center;
            
        如果弹性子元素没有设置宽度高度
        设置了侧轴（交叉轴）的对齐方式那么弹性子元素的宽度高度就是内容的宽高

        如果自左向右排列 y轴 是侧轴  
        如果自上而下排列 x轴 是侧轴
        
        flex-start 开始 默认
        flex-end 结束
        center 居中
        baseline 基线对齐 如果子元素的设置都一样与flex-start没有区别
        stretch
                如果弹性子元素没有设置宽度高度
                添加此属性可以让弹性子元素的宽度或高度与父元素一致，占满整个容器的高度
--------------------------------------------------------------------------------
    7:弹性子元素行的对齐方式 align-content (弹性子元素必须存在换行)        (多行对齐方式)
        flex-start 开始 默认  （居上）
        flex-end  结束  （居下）
        center  居中
        space-around  中间是两端的倍数
        space-between  两端对齐
        space-evenly   平均分配
        stretch    占满
-->
```

### 2.3 **弹性子元素的属性/项目属性**

```css
<!-- 
    弹性子元素如果没有设置高度宽度 父元素也没有设置align-items 属性 
        那么 如果是自左向右排列 高度是父元素的高度  
             如果自上而下排列 宽度是父元素的宽度
--------------------------------------------------------------------------------
    1:flex-basis 设置弹性子元素的宽度或高度 可以用width、height代替
        flex-basis: 100px;
            如果是自左向右排列此值代表宽度
            如果自上而下排列此值代表高度 auto
--------------------------------------------------------------------------------
    2:flex-grow 分配父元素的剩余空间（扩展弹性子元素） 默认值0
        /* flex-grow计算公式: (容器的宽度 - 项目宽度之和) / 所有项目flex-grow之和 * 当前项的flex-grow */
        flex-grow: 1;
        把剩余的空间分配给子元素
            （如剩余的空间100px 有5个子元素 100/5 每个元素120px）
        指定某个元素    （剩余的空间除以总共的份数，其中第二个占两份）
            p:nth-child(2) {
            flex-grow: 2;
                    }
--------------------------------------------------------------------------------
    3:flex-shrink  
     如果父容器的空间不足情况下 每一个项目如何缩放比例，是否允许弹性子元素进行收缩
        /* flex-shrink计算公式: (项目宽度之和 - 容器的宽度) / 所有项目的flex-shrink之和 * 当前项的felx-shrink */
     默认是允许 1   0不允许
        flex-shrink: 0;
--------------------------------------------------------------------------------
    4:flex：flex-grow flex-shrink flex-basis的缩写  默认值 0 1 auto
        flex:1;
        一个值 平分父元素的宽度或高度
--------------------------------------------------------------------------------
    5:order：弹性子元素的顺序
        order:-3;
         值越小当前项越靠前显示 可以为负值 必须是整数
--------------------------------------------------------------------------------
    6:align-self 弹性子元素的对齐方式
        flex-start
        flex-end
        center
        baseline
        stretch
-->
```

