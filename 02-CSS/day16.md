# day16 媒体查询.视口

## 1. **最值问题**

```css
<!-- 
        width 固定值     
        如果浏览器的窗口宽度比给定值小则会出现左右滚动条，内容比较多高度会溢出
        
        min-width 最小值 
        如果浏览器的窗口宽度比给定值小，则按照给定值的宽度去显示，会出现左右滚动条，
        如果浏览器的宽度比给定值大，则按照浏览器的宽度解析
        
        max-width 最大值 
        如果浏览器的窗口宽度比给定值小，则按照浏览器的窗口去显示宽度
        如果浏览器的窗口宽度比给定值大，则按照给定值去显示宽度
        
        height 固定值 如果内容比较多 会溢出 内容比较少没有影响
        min-height 最小值 如果内容比较多 则按照内容的高度去显示 如果内容比较少 则按照给定值的高度去显示
        max-height 最大值 如果内容比较多 则按照给定值的高度去显示 多的内容会溢出  如果内容比较少 则按照内容的高度去显示
-->
```

## 2. **媒体查询**

- 尽可能用标准文档流书写(margin padding挤)
- 尽可能不用flex布局
- 不要设置固定宽高

```css
<!-- 
        媒体查询：可以智能的根据用户或浏览器屏幕个改变而改变网页的排版或布局
        注意：一定要存在一个默认的css
        
        媒体查询
            screen          屏幕限制
            min-width       最小宽度
            max-width       最大宽度    
                不论是最大值还是最小值，都包括
            问题：当处在临界点上，哪个样式再后面引入，临界点就显示哪个样式
                解决：工作中，临界点上，都会错开1px

        media属性定义媒体查询  
            media并不是限制文件是否加载，而是限制文件中的样式是否生效

        多个条件，用and链接

        IE678不支持css3，因此想兼容IE，就要定义一组默认样式，并在最前面引入
        
        
方式一：内部 
        大于或等于这个尺寸
        @media 设备 and (尺寸) {css}
        
        
        在尺寸1和尺寸2之间
        @media 设备 and (尺寸1) and (尺寸2) {css}
                
            body {
            background: red;
        }

        @media (min-width:300px) {
            body {
                background: #999;
            }
        }

        @media (min-width:500px) and (max-width:700px) {
            body {
                background: pink;
            }
        }
        
方式二：外部样式表
    <link rel="stylesheet" href="./2.2粉色.css">
    <link rel="stylesheet" href="./2.3蓝色.css" media="(min-width:300px)">
    <link rel="stylesheet" href="./2.4黄色.css" media="(min-width:500px) and (max-width:700px)">
            
-->
```

## 3. **视口**

```css
    <!-- 
        总结:
            1.Retina(视网膜)屏幕显示技术 （同样的尺寸，像素多了一倍）
            2.所谓的1px，实际上手机用了2点多个像素来渲染。
            3.开发移动端的时候 必须设置约束视口 - meta标签
                <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0 minimum-scale=1.0 user-scalable=no">  
                    width=device-width 约束视口（视口宽度与设备宽度一致）
                    initial-scale=1.0 初始视口倍数是1倍
                    minimum-scale=1.0 最小允许视口宽度是1倍
                    maximum-scale=1.0 最大允许视口宽度是1倍
                    user-scalable=no 不允许用户缩放视口
            4.在移动端开发的时候 使用的图片 要缩放宽高的一半来使用 不会导致图片失真 
    -->
```

