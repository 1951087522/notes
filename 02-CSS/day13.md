# day13-背景与阴影属性

## 1.**圆角属性 border-radius**

```css
<!-- 
        圆角属性 border-radius
        最多可以设置8个值 工作中常用一个值或者四个值
        
            一个值代表 四个角 都是这个给定值的弧度
            两个值代表 第一个值是左上和右下    第二个值是右上和左下    '对角'
            三个值代表 第一个值是左上    第二个值是右上和左下    第三个值是右下
            四个值代表 分别是    左上    右上    右下    左下 顺时针
            .top4 {
            background: green;
            border-radius: 5px左上 10px右上 15px右下 20px左下;
            
            '/'后    的值代表垂直方向
            一个值代表 四个角 都是这个给定值垂直方向的弧度
            两个值代表 第一个值是左上和右下    第二个值是右上和左下    '对角'
            三个值代表 第一个值是左上        第二个值是右上和左下    第三个值是右下
            四个值代表 分别是    左上    右上    右下    左下 顺时针
            .top8 {
            background: orchid;
            border-radius: 5px 10px 15px 20px/5px 20px 30px 15px;
                }
                
            拆分表示法
                .box {
                    border-radius:10px 20px 30px 40px;
                    //等价于
                    border--top-left-radius:10px;   
                    border--top-right-radius:20px;
                    border-bottom-right-radius:30px;
                    border-bottom-left-radius:40px;             
                }
        注意：如果圆角的值只设置一个，并且此元素的宽度高度是一致的，
        那么值大于或者等于宽度高度一半 或者写50% 都可以形成一个圆形
     -->
```

## 2.**背景相关属性**

```css
<!-- 
    css3.0支持一个元素同时添加多张背景图片 
            中间用逗号隔开,一一对应
    注意：谁在前边写谁在上边显示 
    如果存在多张背景图同时还有背景颜色，那么背景颜色一定要放在最后的图片位置
        .box {
            width: 300px;
            height: 300px;
            border: 1px solid #000;
            /* 设置背景 */
            background: url(./imgs/1.jpg), url(./imgs/2.jpg), url(./imgs/3.jpg);


            /* 其它的单一属性也是用逗号隔开，按顺序一一对应。 */
            
            /* 调整图片 */
            background-repeat: no-repeat;
            /* 调整位置 */
            background-position: 0 0, 100px 100px, 200px 200px;
            background-size: 100px 100px, 100px 100px, 50px 50px;
        }
--------------------------------------------------------------------------------
    背景相关属性  背景图片默认从padding区域开始加载 
    一起用：
    background-clip 背景的裁切位置  背景颜色的位置  
        默认值border-box  
            padding-box 
            content-box
    background-origin 背景图片的绘制起点 背景图片的位置 
        默认值padding-box 
            border-box  
            content-box
    .top4 {
            width: 200px;
            height: 100px;
            border: 10px dashed #0f0;
            background: url(./pic1.png) no-repeat;
            padding: 20px;
            background-origin: padding-box;
            background-clip: padding-box
        }
--------------------------------------------------------------------------------
    background-size 背景图片的尺寸
        一个值代表设置图片的宽度  等比例缩放
        两个值 第一个代表宽度 第二个代表高度  
        .top5 {
            width: 200px;
            height: 100px;
            background: url(./pic1.png);
            background-size: 200px 100px;
        }
        cover    等比例缩放  宽度或高度有一个方向缩放到不能缩放就停止  
                                                        图片不一定显示全    但是空间可以填满
        contain  等比例缩放  宽度和高度都不能缩放才停止  
                                                        图片可以显示的全    但是空间不一定填满
-->
```

## 3.**背景颜色-渐变**

### 3.1- **线性渐变linear**

```css
<!-- 
    注意：渐变是background-image 或者 background
        兼容前缀 
        -webkit- 谷歌 苹果
        -ms-     IE
        -moz-    火狐
        -o-      欧朋
-->
--------------------------------------------------------------------------------
线性渐变    linear-gradient    默认从上而下渲染
方式一:     background: -webkit-linear-gradient(left top,red,yellow,blue);
                                线性    渐变     初始位置  颜色  颜色 颜色
           background: -webkit-linear-gradient(left top, red 20%, yellow 60%, blue 80%) ;
                                                        红色从20%处开始有渐变，黄色到60%，80%之后没有渐变
                                                        (百分比 占总背景的多少)
方式二:     background: -webkit-gradient(linear,left top,left bottom,from(#f00),to(#00f));
                                 渐变     线性   初始位置    结束位置    开始颜色   结束颜色   （仅限于两种颜色）

方式三:     background: -webkit-linear-gradient(45deg, red, yellow, blue); 
                                                45度                                            
--------------------------------------------------------------------------------
线性渐变重复
        background: -webkit-repeating-linear-gradient(位置,颜色1 初始位置,颜色1 结束位置,颜色2 颜色1的结束位置,颜色2 结束位置)
                            重复
        可以做边框：
            <section></section>
            <footer>
                <div>人生很长，不必自卑，明确缺陷，抓紧行动。</div>
            </footer>
            
            footer {
            width: 200px;
            height: 200px;
            background: -webkit-repeating-linear-gradient(left top, pink 10px, green 30px, blue 30px, red 100px);
                }

            footer div {
            width: 160px;
            height: 160px;
            background: #fff;
            margin-left: 20px;
            margin-top: 20px;
                }
```

### 3.2- **径向渐变radial**

```css
径向渐变         --->默认椭圆   circle圆
方式一:        background:radial-gradient(形状，颜色，颜色，颜色)   

方式二:        background:radial-gradient(形状 位置，颜色，颜色，颜色)
        
                   //background: radial-gradient(circle at 10px 30px, red, yellow, blue)
                                                    水平方向10  垂直方向    30px
                                                    
径向渐变重复
        background: -webkit-repeating-radial-gradient(形状 位置,颜色1 初始位置,颜色1 结束位置,颜色2 颜色1的结束位置,颜色2 结束位置)
                            重复        径向
```

## 4.**蒙版	-webkit-mask**

```css
        .box {
            width: 330px;
            height: 195px;
            /* 设置背景图片 */
            background-image: url(./imgs/1.jpg);
            /* 通过--webkit-mask属性 可以实现渐变 */
            /* 渐变蒙版 */
            -webkit-mask: -webkit-linear-gradient(left, rgba(0, 0, 0, 1), rgba(0, 0, 0, 0));
        }
```

## 5.**倒影box-reflect**

```css
<!-- 
        倒影 box-reflect：位置 距离（可以设置负值）  倒影不占据空间
        -webkit-box-reflect:below 10px;
        
        left 左边
        right 右边
        above 上边
        below 下边
        
        方式一：
            -webkit-box-reflect: below 0px -webkit-gradient(linear, left top, left bottom, from(transparent), to(#0000ff));
        方式二：
            -webkit-box-reflect: below 0px -webkit-linear-gradient(top,rgba(0,0,0,1),egba(0,0,0,0) 60%)
        注意：如果要写遮罩层渐变必须添加距离
    -->
```

## 6.**阴影shadow**

- 注意浏览器兼容 前缀

```css
<!-- 
阴影 不占据空间 都可以同时设置多个中间用逗号隔开 用阴影可以做边框 不影响其他元素区域
        盒子的阴影 box-shadow
            box-shadow: 0px 0px 0px 1px #00f;
            第一个值    水平偏移值 正值向右 负值向左
            第二个值    垂直偏移值 正值向下 负值向上
            第三个值    模糊程度  只能为正值
            第四个值    阴影的扩张:   正值放大 负值缩小  可以不写
                box-shadow: 0px 0px 0px 10px 1px #00f;
        可以设置多个值:
            box-shadow: 0px 2px 0px #00f,2px 2px 0px #0f0,3px 3px 3px #f0f;

            阴影颜色
            阴影的位置 默认是外阴影outside inset内阴影
            box-shadow: 0px 0px 0px 10px 1px #00f inset;
--------------------------------------------------------------------------------
        文字的阴影 text-shadow
            text-shadow: 2px 0px 0px #f00;
                第一个值    水平偏移值 正值向右 负值向左
                第二个值    垂直偏移值 正值向下 负值向上
                第三个值    模糊程度  只能为正值
                可以设置多个值
                    text-shadow: 2px 0px 0px #f00,2px 2px 0px #0f0,3px 3px 3px #f0f;
            阴影颜色
     -->
```

## 7.**过渡属性transition**

```css
<!-- 
        transition 过渡属性        注意：display不支持过渡

            /* 第一个参数表示要参与过度的属性 */
                /* 如果参数过度的属性是多个 可以使用all表示所有的属性都将参与  */
                /* 如果只是某一个属性要过度 可以直接书写 */
                
            /* 第二个参数过度完成的时间  单位是s 或者是ms 不能省略 */
            
            /* 第三个参数表示缓冲描述: linear匀速  ease 非匀速  (可选)*/
                            运动曲线 
                                ease默认值 
                                ease-in         慢速进入
                                ease-out        慢速结束
                                ease-in-out     慢速开始和结束
                                lienar          匀速
                                steps(步长)
                                
            /* 第四个参数 表示延迟的时间 不能省略单位  (可选)*/
            .box {
            transition: all 1s linear 0s;
                }
-->
```