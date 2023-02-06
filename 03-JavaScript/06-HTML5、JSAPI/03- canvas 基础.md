## **1.canvas 基础**

### **1.1- canvas 介绍**

canvas是HTML5新增的标签，用于提供画布，它的标准属性有width和height，例如（class、id这些都是通用标准属性）

width：表示canvas的宽

height: 表示canvas的高

行内块标签

canvas 元素的默认尺寸为 300 * 150

canvas 支持2D图形绘制和3D图形绘制，通过 getContext() 方法来获取绘制画笔，要绘制2D图形，需要传递参数"2D"，绘制3D图形传递参数"webgl"或"webgl2"。

目前所有浏览器都实现了2D图形绘制，3D图形绘制还在实验阶段。

```js
<body>
    <!--  默认300*150的透明行内块标签-->
    <canvas width="500" height="400"></canvas>
    <script>
        var canvas = document.querySelector('canvas')
        // 返回一个可以绘制2d图像的对象
        var ctx = canvas.getContext('2d')
    </script>
</body>
```

### **1.2.- 图形绘制**

- Canvas 使用路径来区分不同的图形。

beginPath(): 开启新路径。

closePath(): 闭合当前路径，将路径的起点与终点使用直线封闭。

- 绘制线段

moveTo(x, y) 指定线段的起点坐标

lineTo(x, y) 指定线段的终点坐标

多条线段可以绘制出不同的几何图形，比如：三角形，四边形，五边形等。

使用线段绘制几何图形，应该使用 closePath() 进行闭合。

- 路径的描边与填充

moveTo() 和 lineTo() 只是规划路径，canvas 并不会直接绘制路径到画布中。

canvas 提供了两种方式来绘制路径，分别是 描边 与 填充：

stroke() : 以描边的方式绘制路径

fill() : 以填充的方式绘制路径

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        canvas {
            box-shadow: 0 0 10px #333;
        }
    </style>
</head>

<body>
    <canvas width="500" height="400"></canvas>
    <script>
        var canvas = document.querySelector('canvas')
        // 返回一个可以绘制2d图像的对象
        var ctx = canvas.getContext('2d')

        // canvas 使用路径来分层 ，同一个路径 中的所有的图形共享同一个设置
        // beginPath()  开启一条新路径
        ctx.beginPath()

        // 1移动画笔到指定位置
        ctx.moveTo(50, 50)
        // 2指定从起点开始，直线绘制的终点
        ctx.lineTo(250, 50)

        // 画一条水平线
        ctx.moveTo(50, 100)
        ctx.lineTo(250, 100)

        // 设置描边颜色
        ctx.strokeStyle = 'red'
        // 3stroke() : 以描边的方式绘制路径
        ctx.stroke()

        // 画三角形
        ctx.beginPath()
        ctx.strokeStyle = 'gold'
        ctx.moveTo(50, 150)
        ctx.lineTo(250, 150)
        ctx.lineTo(150, 250)
        ctx.lineTo(50, 150)
        ctx.stroke()

        // 开启一条新路径
        ctx.beginPath()
        // 填充效果
        ctx.fillStyle = 'gold'
        // 描边效果
        ctx.strokeStyle = 'green'
        // 线条宽度
        ctx.lineWidth = 10
        // 画实心三角形
        ctx.moveTo(300, 150)
        ctx.lineTo(460, 150)
        ctx.lineTo(380, 250)
        ctx.lineTo(300, 150)
        // 闭合
        ctx.closePath()
        ctx.fill()
        ctx.stroke()
    </script>
</body>

</html>
```



- 绘制矩形

ctx.rect(x,y,w,h);

x,y : 矩形的起始坐标。

w,h：矩形的宽高尺寸。

```js
<body>
    <canvas width="500" height="400"></canvas>
    <script>
        var canvas = document.querySelector('canvas');
        var ctx = canvas.getContext('2d');

        // 绘制矩形
        // 前两个参数 起点坐标
        // 后两个参数 矩形的大小
        ctx.rect(100, 100, 150, 150)
        ctx.strokeStyle = 'red'
        // 空心矩形
        ctx.stroke()
        // 开新路径
        ctx.beginPath()
        ctx.rect(300, 100, 150, 150)
        ctx.fillStyle = 'gold'
        // 实心矩形
        ctx.fill()

        ctx.strokeStyle = 'blue'
        ctx.strokeRect(100, 300, 50, 50)
        ctx.fillStyle = 'green'
        ctx.fillRect(200, 300, 50, 50)

        /*
            rect()  绘制矩形
            strokRect() 绘制描边矩形
            fillRect()  绘制填充矩形

            rect()
                1.不会直接绘制 需要调用 stroke() 或 fill()才会绘制图形
                2.不会创建新路径
            strokeRect() / fillRect()
                1.直接在画布中绘制图形
                2.会创建新路径
        */
    </script>
</body>
```



- 绘制圆形

ctx.arc(x,y,r,startAngle, endAngle, bool)

x,y: 圆心坐标

r: 圆的半径

startAngle: 起点弧度

endAngle: 终点弧度

bool: 是否逆时针绘制，布尔值，false 表示顺时针，true 表示顺时针，默认值为false。

圆形的弧度计算： 1弧度 = Math.PI / 180。

```js
<body>
    <canvas width="500" height="400"></canvas>
    <script>
        var canvas = document.querySelector('canvas');
        var ctx = canvas.getContext('2d');

        /* 
            arc(x, y, r, startAngel, endAngel, bool);
            x y 圆心 坐标
            r   圆的半径
            startAngel  起点的弧度
            endAngel    终点的弧度
            bool        是否逆时针绘制  true 逆时针
                                        false 顺时针
                1.不会直接绘制 需要调用stroke() 或 fill()方法绘制
                2.如果使用 moveTo() 指定绘制的起点(圆心坐标) 默认以开始弧度作为起点
                3.不会创建路径
         */

        // 满圆 (360度)
        // ctx.arc(250, 200, 50, 0, 2 * Math.PI)

        // 半圆 (180度)
        // ctx.arc(250, 200, 50, 0, Math.PI)

        // 四分之一圆
        // ctx.arc(250, 200, 50, 0, Math.PI / 2)

        // 四分之三圆
        // ctx.arc(250, 200, 50, 0, Math.PI * 1.5)

        // 315度角的圆
        ctx.arc(250, 200, 50, 0, Math.PI / 180 * 315)

        // closePath()  将起点和终点使用直线连接
        ctx.closePath()
        
        ctx.stroke()
        // ctx.fill()
    </script>
</body>
```



- 设置图形样式

fillStyle: 设置填充颜色

strokeStyle: 设置描边颜色

lineWidth: 设置线段粗细

lineCap: 设置线段末端样式，允许的值： butt (默认), round, square。

lineJoin：设置两条线段相交的夹角样式，允许的值：round, bevel, miter(默认)。

```js
    <script>
        var canvas = document.querySelector('canvas');
        var ctx = canvas.getContext('2d');

        ctx.moveTo(150, 120)
        ctx.lineTo(300, 120)
        ctx.lineWidth = 10
        ctx.stroke()

        // 线段末端样式 butt默认
        // ctx.lineCap = 'butt'
        // round 圆角包边效果
        // ctx.lineCap = 'round'
        // square 方块包边效果

        ctx.moveTo(150, 180);
        ctx.lineTo(300, 180);
        ctx.lineTo(225, 260);
        // ctx.moveTo(225, 260);
        ctx.lineTo(150, 180);

        ctx.lineWidth = 10;

        // lineJoin 两条线段夹角的外角样式
        // miter 锐角
        ctx.lineJoin = 'miter'
        // bevel 折角
        ctx.lineJoin = 'bevel'
        //  round   圆角
        ctx.lineJoin = 'round'
        ctx.stroke()
    </script>
```



- 自动描边或填充的矩形绘制

fillRect(x, y, w, h)：填充矩形

x: 当前坐标系的x点

y：当前坐标系的y点

w: 矩形的宽

h：矩形的高

strokeRect(x, y, w, h)：描边矩形

x: 当前坐标系的x点

y: 当前坐标系的y点

w: 矩形的宽

h: 矩形的高

- 擦除画布内容

clearRect(x, y, w, h): 擦除画面指定范围的内容

x: 当前坐标系的x点

y: 当前坐标系的y点

w：要清除的区域的宽

h: 要清除的区域的高

```js
<body>
    <canvas width="500" height="400"></canvas>
    <script>
        var canvas = document.querySelector('canvas');
        var ctx = canvas.getContext('2d');

        ctx.fillStyle = 'gold'
        ctx.fillRect(100, 100, 100, 100)
        ctx.beginPath()
        ctx.fillStyle = 'blue'
        ctx.moveTo(250, 150)
        ctx.lineTo(400, 100)
        ctx.lineTo(335, 200)
        ctx.closePath()
        ctx.fill()

        ctx.beginPath()
        ctx.arc(265, 215, 50, 0, 2 * Math.PI)
        ctx.fillStyle = 'red'
        ctx.fill()

        // 清除画布的指定区域   clearRect()
        ctx.clearRect(200, 120, 170, 100)

        // 清除整个画布
        ctx.clearRect(0, 0, canvas.width, canvas.height)
    </script>
</body>
```



- 绘制文字

fillText(text, x, y, [maxWidth]): 绘制填充文字

strokeText(text, x, y [, maxWidth]): 绘制描边文字

text: 要绘制的内容

x,y : 文字的绘制坐标

maxWidth：绘制的最大宽度

font: 设置文字样式 (ctx.font = "font-size font-family";)

```js
<body>
    <canvas width="500" height="400"></canvas>
    <script>
        var canvas = document.querySelector('canvas')
        var ctx = canvas.getContext('2d')
        ctx.font = '60px 楷体'

        // strokeText(text,x,y,maxWidth)    绘制描边文字
        // Text 要绘制的文字
        // x , y 文字的起点左边
        // maxWidth 限制文字绘制的最大宽度

        // ctx.strokeText('你好', 100, 100)

        // fillText()   绘制填充文字
        // ctx.fillText('你好', 100, 100)
    </script>
</body>
```



### **1.3.- 图像绘制**

- 必须等待图片加载完成 才能绘制

- drawImage 该方法用于绘制图片，使用方式有三种：

- - 第一种使用方式：可以以原尺寸来绘制图片

 		ctx.drawImage(img, x, y)

​		 img: 要绘制的图片

​		 x: 以原尺寸将图片放在canvas中的x点

​    y: 以原尺寸将图片放在canvas中的y点

- - 第二种使用方式： 可以缩放图片

 ctx.drawImage(img, x, y, w, h)

 img: 要绘制的图片

 x: 将缩放后的图片放在canvas中的x点

 y：将缩放后的图片放在canvas中的y点

 w: 缩放后的图片的宽 

 h: 缩放后的图片的高

- - 第三种方式: 截取图片中的某一部分

 	ctx.drawImage(img, img_x, img_y, img_w, img_h, canvas_x, canvas_y, canvas_w, canvas_h)

 img: 要绘制的图片

 img_x: 要截取的图片的x点

 mg_y: 要截取的图片的y点

 img_w: 要截取的图片的宽

 img_h: 要截取的图片的高

 canvas_x: 将截取后的图片放在canvas中的x点

 canvas_y: 将截取后的图片放在canvas中的y点

 canvas_w: 将截取后的图片放在canvas中的宽

 canvas_h: 将截取后的图片放在canvas中的高

```js
<body>
    <canvas width="500" height="400"></canvas>
    <script>
        var cvs = document.querySelector('canvas')
        var ctx = cvs.getContext('2d')
        var img = new Image()

        // 必须等待图片加载完成 才能控制
        img.onload = function () {

            // 以原始尺寸绘制图片
            // ctx.drawImage(img, 10, 10)

            // 按比例进行缩放绘制
            // ctx.drawImage(img, 10, 10, 300, 260)

            // 第三种方式 截取图片中的某一部分
            ctx.drawImage(img, 1608, 1587, 370, 260, 20, 20, 370 / 2, 260 / 2);
        }
        img.src = "./images/3.jpg";
    </script>
</body>
```



## **2.- canvas 进阶**

### **2.1- 坐标系变换**

canvas中允许我们对坐标系做变换，类似css中的transorm

translate(x, y) 移动坐标系

x 表示水平移动

y 表示垂直方向移动

rotate(deg) 渲染坐标系

deg 表示旋转角度 单位是弧度（PI）

scale(x, y) 缩放坐标系

x 表示水平方向缩放比例

y 表示垂直方向缩放比例

```js
<body>

    <canvas width="300" height="300"></canvas>

    <div class="box">
        <div id="div1"></div>
        <div id="div2"></div>
    </div>
    <script>
        var cvs = document.querySelector('canvas');
        var ctx = cvs.getContext('2d');

        ctx.fillStyle = 'red'
        // 绘制之前进行位移 
        ctx.translate(50, 50)
        // 以画布左上角为基点 按弧度旋转
        ctx.rotate(Math.PI / 180 * 15)
        // 以左上角为基点 对整个路径进行缩放 x y 参数不能简写 如果省略y值 默认为0
        ctx.scale(0.5, 0.5)
        ctx.fillRect(100, 200, 100, 50)
        ctx.scale(2, 2)
        ctx.rotate(Math.PI / 180 * -15)
        ctx.translate(-50, -50)
        ctx.fillStyle = 'blue'
        ctx.fillRect(100, 100, 100, 50)
    </script>
</body>
```



### **2.2- 状态的存储与恢复**

在canvas中很多时候要用到之前的状态，所以提供了相应的API用于保存canvas上的状态

save() 存储当前的画笔设置

restore() 将画笔设置恢复到上一次 save()。

save() 可以多次使用来保存不同的画笔设置，但每一次save() 保存的设置只能被 restore() 使用一次，并且只能按 save() 时的相反顺序依次使用。

```js
<body>

    <canvas width="300" height="300"></canvas>

    <div class="box">
        <div id="div1"></div>
        <div id="div2"></div>
    </div>
    <script>
        var cvs = document.querySelector('canvas');
        var ctx = cvs.getContext('2d');
        
        // 存储当前所有画笔状态
        ctx.save()
        
        ctx.fillStyle = 'red'
        // 绘制之前进行位移 
        ctx.translate(50, 50)
        // 以画布左上角为基点 按弧度旋转
        ctx.rotate(Math.PI / 180 * 15)
        // 以左上角为基点 对整个路径进行缩放 x y 参数不能简写 如果省略y值 默认为0
        ctx.scale(0.5, 0.5)
        ctx.fillRect(100, 200, 100, 50)

        // 恢复上一次save() 保存的状态
        ctx.restore()

        ctx.scale(0.5, 0.5)
        ctx.rotate(Math.PI / 180 * 15)
        ctx.translate(50, 50)
        ctx.fillStyle = 'blue'
        ctx.fillRect(100, 100, 100, 50)
    </script>
</body>
```

