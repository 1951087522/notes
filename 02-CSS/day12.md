# day12 h5与css3新增标签选择器

## 1.HTML5 语法

```css
    <!-- 1.标签的type属性可以不用书写 -->
    <style type="text/css">
    </style>

    <!-- 2.单标签可以不用写关闭符号 -->
    <h2>1</h2>
    <hr />
    <h2>1</h2>

    <!-- 3.对于不认识的标签/属性采取静默不报错处理 -->
    <demo>标签</demo>

    <!-- 4.标签的属性值 可以不用双引号包裹      坚决不让用 -->
    <div class='box'></div>

    <!-- 5.标签可以大写     坚决不让用 -->
    <H5>五级标题</H5>
```

## 2.h5新增布局标签

```css
<!-- 
（有利于SEO 代码量减少,低版本IE浏览器不支持）
有意义的div
        头部信息 header
        导航区域 nav
        内容区域 
            主题内容
                main
            部分内容
                section
            侧边栏 aside
            文章区 article
            标题组 hgroup
            媒体组 figure  
                媒体组标题 figcaption
        底部信息 footer
        
有意义的span
        mark 默认值选中 有一个黄色的底色
        time 时间
-->
```

## 3.其他标签

```css
details 详情小三角 可以内嵌dt dd
        
<details>
        // summary 更改名称
        <summary>二季度</summary>
        <p>1</p>
        <p>1</p>
        <p>1</p>
</details>
--------------------------------------------------------------------
meter 状态条
        max最大 min最小 value当前值 high最高 low最低
<meter max="10" min="0" value="7" high="8" low="3"></meter>
--------------------------------------------------------------------
progress 安装进度条
<progress max="100" min="0" value="70"></progress>
--------------------------------------------------------------------
注释标签 Ruby rt需要注释的内容 rp 针对低版本浏览器显示
<ruby>阳
    <rt>yang</rt> 需要注释的内容
</ruby>

<ruby>夼
    <rp>(</rp>   针对低版本浏览器显示
    <rt>kuang</rt>
    <rp>)</rp>
</ruby>
```

## 4.视频音频标签

```css
<!-- 
视频标签 video
        支持的格式：mp4 ogg webm
        width 更改宽度
        控制台 controls
        循环播放 loop
        自动播放 autoplay  ie的高版本支持 谷歌火狐需要在静音的情况下支持
        静音 muted
        加载等待的画面 poster
<!-- 插入视频 -->
    两张插入方式：
    方式一：
    <video src="./video/1.mp4" width="300" controls loop autoplay muted poster="./pic1.png">
        此处应该是视频，由于您的浏览器版本比较低请<a href="video/1.mp4">点击下载</a>播放
    </video>
    方式二：（注意需要写type属性后跟视频格式）
    <video width="300" controls poster="./pic1.png">
        <source src="./video/1.mp4" type="video/mp4">
    </video>
----------------------------------------------------------------------------------------------
音频标签 audio        
        支持的格式：mp3 mp4 ogg
        控制台 controls （注意 音频需要写此属性否则不显示）
        自动播放 autoplay  ie的高版本支持 火狐需要在静音的情况下支持
        循环播放 loop
        静音 muted
<!-- 音频插入 -->
    <audio src="./video/菊次郎的夏天.mp3" controls autoplay muted loop>

    </audio>
    <audio controls>
        <source src="./video/菊次郎的夏天.mp3" type="audio/mp3">
    </audio>
-->
```

## 5.新增input的type类型

```css
<form>
        搜索：<input type="search"><br>
        颜色-调色板：<input type="color"><br>
        滑块：<input type="range"><br>
        时间：<input type="time"><br>
        日期：<input type="date"><br>
        周期：<input type="week"><br>
        月份：<input type="month"><br>
        当前本地时间：<input type="datetime-local"><br>
        数字：<input type="number"><br>
        网址：<input type="url"><br>
        邮箱：<input type="email"><br>
        <input type="submit" value="提交">
</form>
```

## 6.新增的表单属性

```css
<!-- 
    h5支持可以不用把所有的input都写在form里面
        实现方式：给form添加一个id  指向这个form的所有表单空间添加form="id名称"
        缺点：少一个错所有
-->
    <form id="login"></form>
    <input type="text" form="login">
```

```css

        //表单必填项 required  只有填写了此字段 才可以提交
            <input type="text" required>
            
        //正则验证 pattern
            <input type="text" pattern="\d{3}">  \d表示阿拉伯数字 3表示书写三位
            
        //自动聚焦 autofocus
            <input type="text" autofocus> 不用点击直接书写
            
        //提示信息 placeholder
            <input type="text" placeholder="123" value="默认值">
            value 会进行覆盖 如同时存在value和placeholder 会显示value
            
        //表单不进行验证 novalidate
            <form action="" novalidate>
                <input type="submit" value="提交">
            </form>
            
        //历史记录 autocomplete 默认是on开启
            <form autocomplete="off">
                <input type="text" name="use">
                <input type="submit" value="提交">
            </form>
            
         // input标签绑定  智能关联
            <form>
                <!-- input list属性值与 datalist id属性值对应 -->
                <input list="list" name="nex">
                <datalist id="list">
                    <option value="鲁班"></option>
                    <option value="小乔"></option>
                    <option value="关羽"></option>
                </datalist>
                <input type="submit">
            </form>
```

## 7.浏览器前缀

- 为了兼容各个浏览器

```css
    // 谷歌
    -webkit-box-shadow:1px 2px 3px red;
    //火狐
    -moz-box-shadow:1px 2px 3px red;
    //opera
    -o-box-shadow:1px 2px 3px red;
    //ie        -ms-(MicrSoft缩写)
    -ms-box-shadow:1px 2px 3px
```

## 8.新增颜色表示法

```css
    //单词法
        .box {
            color:red;        
        }
    //rgb格式
        .box {
            color:rgba(255,255,255,0.4)        
        }
    //十六进制
        .box {
            color:#f7f7f7;        
        }
    //工业用色hsl    h色相(0-360)    s饱和度(0-100%)    l明度(0-100%)
        .box {
            color:hsl(255,50%,50%)        
        }
        
    // rgba 设置不会影响子元素
    //opacity 会影响字元素
```

## 9.css3 新增选择器

- **outline: none; 去除input本身的边框线**

```css
input {   
            outline: none; 去除input本身的边框线
            
            outline-color:red; 获取焦点时 input边框颜色发生改变
                }
```

### 1.基本选择器

```css
1:基本选择器

      标签选择器 
          div {}
      id选择器 
          #id {}
      class选择器 
          .box {}
      并集选择器 
          .box,box1 {中间用逗号隔开，选中多组}
      交集选择器 
          .box.box1 {连续书写，选中共有}
      通配符选择器
          * {选中所有选择器}
```

### 2.关系选择器

```css
2:关系选择器
      2.1子代选择器         >    只能选中亲儿子    (注意继承)
          .box>.box1 {}
      2.2相邻兄弟选择器     +      向后查找与自己相邻的第一个兄弟  (注意 书写时起名不同)
        div+span {
            color: red;
        }
      2.3通用兄弟选择器     ~     向后查找所有与自己同级的这个兄弟
        div~span {
            background: blue;
        }
```

### 3.伪类选择器

```css
        1 :focus 获取焦点伪类
        input {   
            outline: none; 去除input本身的边框线
            border：none
                }
        input:focus {
            border: 1px solid #f00;
        }
       2:enabled  启用状态
        input:enabled {
            background: blue;
        }
       3:disabled 禁用状态
        input:disabled {
            background: pink;
        }
       4:checked  默认选中
        <input type="radio" id="sex" checked name="sex"><label for="sex">男</label>
        input:checked+label {
            background: #f00;
        }
       5:否定伪类选择器
          :not(你要排除的)
          p:not(.c) {
            color: red;
            }
            <p>1</p>
            <p class="c">2</p>
            <p>3</p>
        6:目标伪类选择器
           :target  定位超链接锚点id所在的元素
            :target {
                background: pink;
                }   
            <a href="#top1">第一个</a>
            <a href="#top2">第2个</a>
            <a href="#top3">第3个</a>
            <div id="top1">111</div>
            <div id="top2">222</div>
            <div id="top3">333</div>
        7:empty空节点伪类
            内容为空时  需要指定宽高才进行显示
            div:empty {
                    background: orange;
                } 

```

### 4.伪元素选择器

````css
        8:伪元素选择器
       :before 内容之前
       /* 内容之前 */
        p:before {
            content: "123";
        }
       :after 内容之后
        /* 内容之后 */
        p:after {
            content: "456"
        }
       注意：after 与 before 要与content连用
   
       8.1:first-line 第一行
       /* 第一行 */
        p:first-line {
            background: #f00;
        }
       8.2:first-letter 第一个字符
       /* 第一个字符 */
        p::first-letter {
            font-size: 50px;
        }
        p {
            display: inline-block;
        }
       注意：first-line与first-letter 必须是块级元素或者行内块
       8.3::selection 鼠标选中的状态
       注意：必须是双冒号
       /* 选中的状态 */
        p::selection {
            color: #f00;
            background: orange;
        }
````

### 5.属性选择器

```css
<!-- 
        //属性选择器
        xxx[属性] 存在此属性就可以 (注意标签后紧跟[]不要有空格)
        input[value] {
            background: red;
        }
        
        //属性值选择器
        xxx[属性="属性值"]  或者xxx[属性~="属性值"]  存在此属性并且属性值是给定值仅此
        xxx[属性|="属性值"] 存在此属性并且属性值必须包含给定值，并且给定值必须是开头独立存在
        xxx[属性^="属性值"] 存在此属性并且属性值必须有给定值，必须以给定值开头不需要独立存在
        xxx[属性$="属性值"] 存在此属性并且属性值必须有给定值，必须以给定值结尾
        xxx[属性*="属性值"] 存在此属性并且属性值必须有给定值，给定值在任意位置都可以
-->
```

### 6.结构伪类选择器

```css
<!-- 
结构伪类选择器
    1序选择器         注意：会受其他标签影响  看父级进行数第几个
     第一个   :first-child
                     div:first-child {
                            background: yellow;
                                    }
--------------------------------------------------------------------------------
     最后一个 :last-child
--------------------------------------------------------------------------------
     第几个   :nth-child(n)     
              n代表所有   可以写具体的阿拉伯数字    从1开始
                      div:nth-last-child(2) {
                        background: green;
                                        }
--------------------------------------------------------------------------------
    关键字     odd       奇数    
              even    偶数
--------------------------------------------------------------------------------
      n的倍数     n是从0开始的  2n+1代表    每两个中的第一个
--------------------------------------------------------------------------------
     倒数第几个 :nth-last-child(n)  
     整个结构中只有一个标签  :only-child 


    2.类型选择器        不会受其他标签影响
     第一个 :first-of-type
             div:first-of-type {
            background: yellow;
        }
--------------------------------------------------------------------------------
     最后一个      :last-of-type
--------------------------------------------------------------------------------
     第几个        :nth-of-type(n)
                   n的倍数     n是从0开始的  2n或者 even 代表偶数
                                      2n+1 或者 2n-1 或者odd 奇数
                    div:nth-of-type(2n) {
                                background: orange;
                            }
--------------------------------------------------------------------------------
     倒数第几个    :nth-last-of-type(n)
--------------------------------------------------------------------------------
     整个结构中只有一个标签      :only-kof-type
--------------------------------------------------------------------------------
  
:root 根  
    html的上一级
    root>html>body>...
    :root {
            background: #000;
        }
-->
```