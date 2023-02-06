## **1.- Web API 基本认知**

### **1.1- 作用和分类**

- 作用：使用JS操作HTML和浏览器
- 分类：DOM(文档对象模型）、BOM(浏览器对象模型)

### **1.2- 什么是DOM**

- DOM(Document Object Model——文档对象模型)是用来呈现以及任意HTML或XML文档交互的API

- 白话文：DOM是浏览器提供的一套专门用来**操作网页内容**的功能

- DOM的作用：

- - 开发网页的内容和实现用户的交互

### **1.3- DOM 树**

- 将HTML文档已树状结构直观的表现出来，我们称之为文档或DOM树
- 描述网页内容关系的名称
- 作用：**文档树直观的体现了标签与标签之间的关系**

 ![image-20221218155413072](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218155413072.png)

### **1.4- DOM 对象**

- DOM对象：浏览器根据HTML标签生成的JS对象

- - 所有标签的属性都可以在这个对象上面找到
  - 修改这个对象的属性会自动映射到标签身上

- DOM的核心思想

- - 把网页内容当作对象来处理

- document对象

- - 是DOM提供的一个**对象**

  - 所以他提供的属性和方法都是**用来访问和操作网页内容的**

  - - 例：document.write()

  - 网页所有内容都在doucument里面

![image-20221218155438380](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218155438380.png)

## **2.- 获取DOM元素**

### **2.1- 根据css选择器来获取DOM元素（重点）**

#### **2.1.1- 选择匹配的第一个元素 querySelector**

- 语法：

```javascript
document.querySelector('css选择器');

//示例：
    <div>1</div>
    <div class="abc">1</div>
    <ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
    </ul>
    <script>
        let div = document.querySelector('div');
        console.log(div);
        let abc = document.querySelector('.abc');
        console.log(abc);
        let li = document.querySelector('ul li:last-child');
        console.log(li);
    </script>
```

- 参数：包含一个或多个有效的css选择器**字符串**

- 返回值：css选择器匹配的**第一个元素**，一个HTMLElement对象

- - 如果没有匹配到，返回null

- `querySelector()` 方法可以直接操作修改

#### **2.1.2- 选择匹配的多个元素 querySelectorAll**

- 语法：

```javascript
document.queryElementAll('css选择器')
```

- 参数：包含一个或多个有效的css选择器**字符串**
- 返回值：css选择器匹配的**NodeList 对象集合**
- **例如：**

```javascript
// 2.获取所有的元素
let lis = document.querySelectorAll('ul li');
console.log(lis);
```

- `querySelectorAll()` 不可以直接修改，只能通过遍历的方法依次给里面的元素做修改

- 得到的是一个伪数组：

- - 有长度有索引号的数组
  - 但是没有pop() push() 等数组方法
  - 想要得到里面的每一个对象，则需要遍历（for）的方式获得

哪怕只有一个元素，通过querySelectorAll() 获取过来的也是一个伪数组，里面只有一个元素而已。

### **2.2- 其他获取DOM元素的方法（了解）**

```javascript
// 根据 id 获取一个元素
document.getElementById('nav');
// 根据 标签 获取某一类元素 获取页面 所有的div
document.getElementByTagName('div');
// 根据 类名 获取元素 获取页面 所有类名为 w 的
document.getElementByClassName('w');
```

### **3.- 设置/修改 DOM元素内容**

#### **3.1- `document.write()` 方法**

- 只能将文本内容追加到前面的位置
- 文本种包含的标签会被解析

```javascript
// 永远都只是追加操作 且只能位置</body>前
document.write('hello world');
document.write('<h3>你好，世界</h3>');
```

#### **3.2- 对象`inner.Text` 属性**

- 将文本内容添加/更新到任意标签位置
- 文本种包含的标签不会被解析

```javascript
    <div>坚持就是胜利</div>
    <script>
        let box = document.querySelector('div');
        // 修改元素内容
        // 对象.属性
        box.innerText = '明天会更好';
        // 不会识别标签
        box.innerText = '<h2> 明天会更好 </h2>';
    </script>
```

#### **3.3- 对象`inner.HTML` 属性**

- 将文本内容添加/更新到任意标签位置
- 文本种包含的**标签会被解析**

```javascript
    box.innerHTML =  '<h2> 明天会更好 </h2>';
```

### **4.- 设置/修改 DOM元素属性**

#### **4.1- 设置/修改 元素的****常用****属性**

- 可以通过 js 设置/修改 标签元素属性，比如通过src更换图片
- 最常见的属性：href title src
- 语法：

```javascript
对象.属性 = 值
// 示例
    <img src="../../../images/8.jpg">
    <script>
        // 获取元素
        let pic = document.querySelector('img');
        // 修改元素属性 对象.属性 = 值
        pic.src = '../../../images/10.jpg';
        // 新增
        pic.title = '你好吗'
    </script>
```

#### **4.2- 设置/修改 元素的****样式****属性**

##### **4.2.1- 通过style 属性操作css**

**注意：**

1. **修改样式通过style属性引出**
2. **如果属性有-连接符，需要转换为小驼峰命名法**
3. **赋值的时候，不要忘记加css单位**

- 语法：

```javascript
对象.style.样式属性 = 值
//示例
    <script>
        // 通过 style 控制样式属性
        // 1.获取元素
        let box = document.querySelector('div');
        // 2.改变样式属性
        box.style.background = 'hotpink';
        // 注意：属性中有连接符 - 需要转换为小驼峰命名
        box.style.backgroundColor = 'gold';
        box.style.marginTop = '100px';
    </script>
```

##### **4.2.2- 通过类名（`className`）操作css**

如果修改的样式比较多，直接通过style属性修改比较繁琐，我们可以通过借助于css类名的形式。

- 语法：

```javascript
// active 是一个css类名
元素.className = 'active';
// 示例
    <div class="one"></div>
    <script>
        // 1.获取元素
        let box = document.querySelector('div');
        // 2.设置样式
        // 注意事项 直接使用会覆盖以前的类名
        box.className = 'active';
        box.className = 'one active'
    </script>
```

- 注意：

- - 由于class是关键字，所以使用`className`代替
  - className 是使用新值换旧值，如果需要添加类，需要保留之前的类

- 好处

- - 可以同时修改多个样式

##### **4.2.3- 通过classList 操作控制css**

- 语法:

- - 元素.classList.add('类名');		追加
  - 元素.classList.remove('类名');		移除
  - 元素.classList.toggle('类名');		切换
  - 元素.classList.contains('类名');	判断有没有某个类

```javascript
// add    追加类
元素.classList.add('类名');
//remove    删除类
元素.classList.remove('类名');
//toggle    切换类    有则增加 没有则删除
元素.classList.toggle('类名');

//示例
    <div class="one"></div>
    <script>
        // 使用classList 修改样式

        // 获取元素
        let box = document.querySelector('div');
        // add 追加
        box.classList.add('active');
        // remove 删除
        box.classList.remove('active');
        // toggle 切换类 有则删除 没有则增加
        box.classList.toggle('active');
    </script>
```

- 使用`className` 和 `classList` 的区别

- - 修改大量样式更方便
  - classList是追加和删除不影响以前的类名

#### **4.3- 设置/修改** **表单元素****属性**

- 表单很多情况，也需要修改属性，比如点击眼睛，可以看到密码，本质就是把表单类型转换为文本框

  ![image-20221218155807958](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218155807958.png)

- - 正常的有属性有取值的 跟其他标签属性没有任何区别

```javascript
//获取：DOM.属性名
表单.value = '用户名';
//设置：DOM对象.属性名 = 新值
表单.type = 'password'
```

- **表单属性中添加就有效果，移除就没有效果，一律使用布尔值表示，如果为true代表添加了该属性，如果为false，代表移除了该属性**

- - **比如： disabled checked selected**

```javascript
    <input type="text" value="请输入">
    <button disabled>按钮</button>
    <input type="checkbox" name="" id="" class="agree">
    <script>
        // 1.获取元素
        let input = document.querySelector('input');
        // 2.取值或者设置值
        input.value = '密码';
        input.type = 'password';

        // 2.启用按钮
        let btn = document.querySelector('button');
        // disabled 不可用 = false 这样可以让按钮启用
        btn.disabled = false;
        // 3.勾选复选框
        let checkbox = document.querySelector('.agree');
        checkbox.checked = true;
    </script>
```

### **5.- 定时器 间歇函数**

#### **5.1- 定时器函数介绍**

- 网页中经常会需要一种功能：每隔一段时间需要自动执行一段代码，不需要手动执行
- 例如：网页中的倒计时

![image-20221218155839694](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218155839694.png)

#### **5.2- 定时器函数基本使用**

##### **5.2.1- 开启定时器**

```javascript
setInterval(函数，间隔时间)

// 示例
function repeat() {
    console.log('定时器')
}
// 每隔一秒调用repeat函数
setInterval(repeat,1000)
```

- 作用：每隔一段时间调用这个函数

- 间隔时间单位是毫秒

- 注意：

- - 函数名字不需要加小括号
  - 定时器返回的是一个id数字

##### **5.2.2- 关闭定时器**

```javascript
let 变量名 = setInterval(函数，间隔时间);
clearInterval(变量名);

//示例
    <script>
        // 间隙函数
        function show() {
            console.log('间隙函数');
        }
        let num = setInterval(show, 1000);
        // 清除定时器
        clearInterval(num);
    </script>
```

#### **6.- 综合案例**

##### **6.1- 用户注册倒计时**

```javascript
<body>
    <textarea name="" id="" cols="30" rows="10">
        用户注册协议
        欢迎注册成为京东用户！在您注册过程中，您需要完成我们的注册流程并通过点击同意的形式在线签署以下协议，请您务必仔细阅读、充分理解协议中的条款内容后再点击同意（尤其是以粗体或下划线标识的条款，因为这些条款可能会明确您应履行的义务或对您的权利有所限制）。
        【请您注意】如果您不同意以下协议全部或任何条款约定，请您停止注册。您停止注册后将仅可以浏览我们的商品信息但无法享受我们的产品或服务。如您按照注册流程提示填写信息，阅读并点击同意上述协议且完成全部注册流程后，即表示您已充分阅读、理解并接受协议的全部内容，并表明您同意我们可以依据协议内容来处理您的个人信息，并同意我们将您的订单信息共享给为完成此订单所必须的第三方合作方（详情查看
    </textarea>
    <br>
    <button class="btn" disabled>我已经阅读用户协议(6)</button>
    <script>
        // 用户注册倒计时   
        // 1.按钮添加禁用属性  disabled
        // 2.获取元素
        let btn = document.querySelector('.btn');
        // 3.变量计数 倒计时
        let i = 6;
        // 4.开启定时器
        let num = setInterval(function () {
            // 5.每一秒钟减减
            i--;
            // 6.更换文字
            btn.innerHTML = `我已经阅读用户协议(${i})`;
            // 7.判断 当秒数等于0时 清除定时器
            if (i === 0) {
                clearInterval(num);
                // 开启按钮
                btn.disabled = false;
                // 更换文字
                btn.innerHTML = '我同意';
            }
        }, 1000)
    </script>
</body>
```

##### **6.2- 焦点图**

```javascript
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>QQ音乐10屏轮播图</title>
    <style>
        .img-box {
            width: 700px;
            height: 320px;
            margin: 50px auto 0;
            background: #000;
            position: relative;
        }

        .img-box .tip {
            width: 700px;
            height: 53px;
            line-height: 53px;
            position: absolute;
            bottom: 0px;
            background-color: rgba(0, 0, 0, 0.8);
            z-index: 10;
        }

        .img-box .tip h3 {
            width: 82%;
            margin: 0;
            margin-right: 20px;
            padding-left: 20px;
            color: #98E404;
            font-size: 28px;
            float: left;
            font-weight: 500;
            font-family: "Microsoft Yahei", Tahoma, Geneva;
        }

        .img-box .tip a {
            width: 30px;
            height: 29px;
            display: block;
            float: left;
            margin-top: 12px;
            margin-right: 3px;
        }

        .img-box ul {
            position: absolute;
            bottom: 0;
            right: 30px;
            list-style: none;
            z-index: 99;
        }
    </style>
</head>

<body>
    <div class="img-box">
        <img class="pic" src="./imgs/b01.jpg" alt="第1张图的描述信息">
        <div class="tip">
            <h3 class="text">挑战云歌单，欢迎你来</h3>
        </div>
    </div>

    <script>
        // 数据
        let data = [
            {
                imgSrc: './imgs/b01.jpg',
                title: '挑战云歌单，欢迎你来'
            },
            {
                imgSrc: './imgs/b02.jpg',
                title: '田园日记，上演上京记'
            },
            {
                imgSrc: './imgs/b03.jpg',
                title: '甜蜜攻势再次回归'
            },
            {
                imgSrc: './imgs/b04.jpg',
                title: '我为歌狂，生为歌王'
            },
            {
                imgSrc: './imgs/b05.jpg',
                title: '年度校园主题活动'
            },
            {
                imgSrc: './imgs/b06.jpg',
                title: 'pink老师新歌发布，5月10号正式推出'
            },
            {
                imgSrc: './imgs/b07.jpg',
                title: '动力火车来到西安'
            },
            {
                imgSrc: './imgs/b08.jpg',
                title: '钢铁侠3，英雄镇东风'
            },
            {
                imgSrc: './imgs/b09.jpg',
                title: '我用整颗心来等你'
            },
        ]
        // 1.获取元素 图片和标题
        let pic = document.querySelector('.pic');
        let title = document.querySelector('.text');
        // 2.计数 图片的张数
        let i = 0;
        // 3.开启定时器
        setInterval(function () {
            // 4.每隔一秒 ++
            i++;
            // 5.修改图片的src属性
            pic.src = data[i].imgSrc;
            // 6.修改文字内容
            title.innerHTMl = data[i].title;
            // 无缝衔接 判断当图片的张数 等于 数组长度-1(图片的个数)
            if (i === data.length - 1) {
                // 因为 i会++ 所以 i = -1
                i = -1;
            }
        }, 1000)
    </script>
</body>
```

