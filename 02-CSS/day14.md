# day14-2d与3d变换

## 1.**transform**

**注意：transform 的四个功能函数 skew scale rotate translatez 如果==书写顺序不同值相同最终效果不同==**

### 1.1 **旋转rotate	值度数deg**

```css
         旋转 rotate 
        .box1:hover {
            transform: rotate(45deg);

            /* 单独设置 */
            transform: rotateX(45deg);
            transform: rotateY(45deg);
        }
        
            rotate相当于rotatez
            rotatex 上下      相当于围绕'一'旋转  上下
            rotatey 左右      相当于围绕'|'旋转    左右
            rotatez 前后      相当于围绕'O'旋转    平面顺时针
                transform: rotatex(30deg) rotatey(30deg) rotatez(30deg);
                                    这种写法一起旋转
            或者:rotate3d(x,y,z,角度) xyz 0代表不旋转 1代表顺时针旋转 -1代表反方向  
                                    transform: rotate3d(1, 1, 1, 30deg);  
                                    这种写法是分步骤进行旋转，在原来的基础上旋转
        
            注意：旋转时内容会跟着旋转 不会占据空间 不影响其他元素
```

### 1.2 **斜切skew	值度数deg**

```css
          斜切 skew
        .box2:hover {
            transform: skew(30deg);

            /* 单独设置 */
            transform: skewX(50deg);
            transform: skewy(60deg);
        }
```

### 1.3 **缩放scale	值 默认值1**

```css
        缩放scale    单位倍数
        .box3:hover {
            transform: scale(2);

            /* 单独设置 */
            transform: scaleX(2);
            transform: scaleY(1.5);
        }
        
            值 比1大就是放大相应倍数   比1小就是缩小相应倍数   如果是负数就是翻转加缩放
            值为0  也会隐藏
            值为-1 会进行反转
        默认情况下 写一个值 代表宽度高度缩放的倍数
        
        可以设置两个值 中间用逗号隔开 第一个代表宽度 第二个代表高度
            transform: scale(1,0.5);
            
        可以三个一起设置 scale3d(x,y,z)
            transform: scale3d(1,0.5,2);
                    
        可以单独设置 scalex 宽度 scaley 高度  scalez 厚度
            transform: scaleX(1) scaleY(0.5) scaleZ(2);
```

### 1.4 **移动translate	值px**

- 第一个值 水平方向

- 第二个值 垂直方向

- - **关于translate  绝对定位居中问题**

```css
    .box {
        width: 100px;
        height: 100px;
        background: green;
        
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50px, -50px);
    }
```

```css
    移动translate
    .box4:hover {
        /* 第一个值     水平方向 */
        /* 第二个值     垂直方向 */
        transform: translate(100px, 200px);
  
        /* 单独设置 */
        transform: translateX(100px);
        transform: translateY(200px);
    }
    
            可以单独设置某一个方向 
    translatex 水平 正值向右 负值向左 
    translatey 垂直 正值向下 负值向上
    translatez 前后 正值向前 负值向后
        transform: translatex(100px) translatey(50px) translatez(100px);
      或translate3d(x,y,z)
  
    值 可以是具体的数值 还可以是百分比(值是自身宽度高度的百分比)
        transform: translate(50%,50%);
```

## 2. 其他属性

### 2.1 **景深属性 prespective** 

​	**值为800-1200视距   添加父级**

```css
    景深 perspective 你距离多远的位置去观察事物
    
    使用方式：
             写给父元素直接写 perspective：值
    
             写给元素本身 transform：perspective(值)
```

### 2.2 **3d空间透视效果**

```css
/* 3d空间透视效果 */
transform-style: preserve-3d;
```

### 2.3 **旋转基点 origin**

```css
        旋转默认从中心旋转 可以通过transform-origin更改旋转的基点
        
        默认值是center 可以设置具体的方向值 
                      还可以设置具体的数值
                      百分比
            transform-origin:right center;
            transform-origin: 0px 0px;
                            上下  左右
```

### 2.4 **隐藏元素的背面**

```css
backface-visibility: hidden;
```

### 2.5 **屏蔽鼠标悬停事件**

```css
解决当鼠标划入子元素也显示的效果问题：两种不同效果
//屏蔽鼠标悬停事件     (从初始位置加载)
pointer-events: none;
```

### 2.6 **模糊 filter: blur**

```css
        /*      filter: blur(num)     单位值px  默认值0px
                blur是一个函数 接收一个参数是像素单位 值越大越模糊 
         */
        img {
            transition: all 1s;
            transform: scale(1.2);
            filter: blur(2px);
        }
```