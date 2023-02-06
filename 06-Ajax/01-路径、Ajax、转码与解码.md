## 一、 路径

在js中书写的路径分为两种：前端路径和服务器端路径

### 1.1 **前端路径**

指的是以html、js、css自身查找的文件路径

绝对路径：相对于服务器上的根目录（所有文件，引入的地址写法一致）

1 补全写法 http://localhost:3000/web/demo.css

2 省略协议 //localhost:3000/web/demo.css

3 省略协议以及端口 /web/demo.css (常用方式)

相对路径：是相对于文件自身开始查找的路径

在路径的前面加上	./	../或者不加都是以自身文件开始查找

注意： 

​			==css文件是相对于自身所在的文件开始查找==			

​			==js文件是html文件自身所在位置开始查找==

### 1.2 **后端路径**

nodejs中使用的路径

绝对路径：相对于分区根目录（mac 和 linux 系统只有一个根目录，windows中一个分区拥有一个根目录）

相对路径：是相对于文件自身来查找的

如果通过require方法引入的文件，没有在node_modules中的时候，引入的时候必须加上./



## 二、Ajax

- 无感刷新

> 在很久以前，浏览器只是为了渲染静态页面。但是随着技术更新换代，人们开始大量使用表单与网页进行交互。但是我们都知道，如果使用表单提交数据的时候，会刷新页面（跳转页面）为了避免跳转页面，Ajax 就出现了

表单提交问题，例如：当我们只是修改表单中某一个字段的时候，点击提交按钮，依然会跳转页面

特点：==使用 AJAX 不仅仅可以与服务器中进行数据交互，还可以在不跳转页面的情况下实现局部页面更新==

全称：Asynchronous javascript And XML 异步的JS和XML

XML：可拓展标记语言，也以前也是进行前后端数据交互的

- 特点：只要具备文档声明， 所有的标签都是自定义的。但是使用XML传递数据的时候，前端解析起来比较麻烦，后端生成文件也是比较麻烦，所以逐渐被json数据所代替

### 2.1 使用 Ajax

> AJAX不是一门新技术，最早由微软应用在IE4的Outlook中，2005年开始因谷歌地图应用而流行。
>
> 在IE8及之前版本的IE浏览器中使用ActiveXObject构造函数实现，在IE8以上及其他浏览器由`XMLHttpRequest`构造函数实现。
>
> IE6~IE8支持XMLHttpRequest对象，但不支持XMLHttpRequest构造函数。

通过构造函数的实例化对象调用某些方法即可发送AJAX请求。

onreadystatechang: 该事件会在`readyState`的值发生改变的时候触发

- readyState: 状态码


>  0：ajax对象被创建，但尚未调用 open() 方法。
>
>  1：已经调用open方法。
>
>  2：`send()` 方法已经被调用，并且头部和状态已经可获得。
>
>  3：下载数据中，`responseText` 属性已经包含部分数据。
>
>  4：下载操作已完成。

- status: 服务器的状态码


>  200 表示成功
>
>  304 表示重定向
>
>  404 表示文件没有找到
>
>  500 服务器内部发生错误

- responseText: 接收到的响应文本


open: 初始化一个新创建的请求。

使用方式：

> xhr.open(method, url, bool)
>
>  method: 本次请求的方式
>
>  url: 本次请求的路径 （可以携带query数据）
>
>  bool: 决定了本次发送请求的时候使用异步或是同步，true表示异步，false表示同步，默认为true。

- send: 用于发送HTTP请求，如果是异步请求（默认为异步请求），则此方法会在请求发送后立即返回；如果是同步请求，则此方法直到响应到达后才会返回。


使用方式：

> xhr.send(query)
>
> query： 本次请求的时候携带的数据 （get请求是不需要传递的）而post请求需要传递

- setRequestHeader： 该方法用于设置 HTTP 请求头部的方法。此方法必须在 `open()` 方法和 `send()` 之间调用。如果多次对同一个请求头赋值，只会生成一个合并了多个值的请求头。


> - *// 1.创建 Ajax 对象*
>
>     ​      `var xhr = new XMLHttpRequest()`
>
> - *// 2.创建请求链接设置请求方式*
>
>     ​        *第一个参数  请求方式*
>
>     ​        *第二个参数  请求地址*
>
>     ​      `xhr.open('post', '/login')`
>
> - *// 3.设置请求头*
>
>     ​      `xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded; charset=utf8");`
>
> - *// 5.接受服务器的返回数据*
>
>     ​      `xhr.onreadystatechange = function () { }`
>
> - *// 4.发生请求*
>
>     ​      `xhr.send( )`

### 2.2 处理 get 请求

get 请求 后端向前端拿数据

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>

    <h2>Ajax Get请求</h2>
    <input type="button" value="换一换">
    <ul class="list"></ul>
    <script>
        var btn = document.querySelector('input');
        var list = document.querySelector('.list');

        btn.onclick = function () {
            // 1， 创建ajax对象
            var xhr = new XMLHttpRequest();

            // 2，创建请求连接，设置请求方式
            // 方式一 
            // ---
            xhr.open('get', '/data.json')
            // ---

            // 方式二 
            // ---
            //                  接口名称
            // xhr.open("get", "/list");
            // ---

            // get方式要提交数据，是将数据拼接在url后面。
            // 在 url 后面 通过 ? 拼接 （查询信息）
            // xhr.open("get", "/list?user=admin&passwd=123321");

            // 4. 接收服务器的返回数据
            xhr.onreadystatechange = function () {
                if (xhr.readyState == 4 && xhr.status == 200) {
                    var data = JSON.parse(xhr.response);

                    // 方式一   
                    // ---
                    data.sort(() => Math.random() - 0.5);
                    data = data.slice(0, 3);
                    // ---

                    list.innerHTML = '';
                    data.forEach(val => {
                        var li = document.createElement('li');
                        li.innerHTML = val;
                        list.appendChild(li);
                    });
                }
            };
            // 3，发送请求
            xhr.send("user=admin&passwd=123321");
        };
    </script>
</body>

</html>
```

json 文件

```js
[
    "先天下之忧而忧，后天下之乐而乐。——范仲淹",
    "学习，永远不晚。——高尔基",
    "度尽劫波兄弟在，相逢一笑泯恩仇。——鲁迅",
    "君子拙于不知己，而信于知己。——司马迁",
    "美在生活中。——刘文西",
    "妥协对任何友谊都不是坚固的基础。——泰戈尔",
    "离别是无声的雨。——莎士比亚",
    "燕子声声里，相思又一年。——周恩来",
    "黄金有价，青春无价。",
    "粮食补身体，书籍丰富智慧。"
]
```

```js
const express = require('express');
const app = express();

app.use(express.urlencoded({ extended: true }));
app.use(express.static("web"));

app.post("/login", (req, res) => {
    console.log(req.body);
    res.setHeader("Content-type", "text/html; charset=utf8");
    res.json({ success: true, msg: "登录成功！" });
});
// 方法二 
/* app.get('/list', (req, res) => {
    var data = [
        "孙颖莎成首位00后年终世界第一",
        "男子殴打足浴店员工还称叔叔是市长",
        "欧洲第二场战争 即将爆发？",
        "感染新冠两周后心肌会有反应热",
        "医生：新冠损伤味觉一月内可恢复",
        "1380元血氧仪成本仅几十块",
        "白肺的一个明显表现是气紧",
        "乌总统称乌克兰已成全球领导者之一",
        "女子阳后胸痛确诊严重肺炎",
        "女子吸朋友递来香烟后毒品阳性"
    ];

    data.sort(() => Math.random() - 0.5);
    data = data.slice(0, 3);

    res.json(data);

}); */
app.listen(80);
```



### 2.3 处理 post 请求

post 请求 前端数据向后端提交

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .hide {
            display: none;
        }
    </style>
</head>

<body>

    <h2>Ajax</h2>

    <div class="login">
        <form>
            <label>
                用户名：
                <input type="text" name="user" required />
            </label>
            <br>
            <label>
                密&emsp;码：
                <input type="password" name="passwd" required />
            </label>
            <p>
                <input type="button" name="btn" value="登录">
            </p>
        </form>
    </div>

    <div class="info hide">
        <p>欢迎回来,admin</p>
    </div>

    <script>
        // 获取标签
        var login = document.querySelector('.login')
        var info = document.querySelector('.info')
        var form = document.querySelector('form')
        var user = form.user
        var pwd = form.passwd
        var btn = form.btn

        btn.onclick = function () {
            // 获取输入的用户名和密码
            var username = user.value;
            var passwd = pwd.value;

            // 判断用户名和密码输入空
            if (!username || !passwd) {
                console.log('请输入用户名和密码');
                return
            }

            // 1.创建 Ajax 对象
            var xhr = new XMLHttpRequest()

            // 2.创建请求链接设置请求方式
            /* 
                第一个参数  请求方式
                第二个参数  请求地址
            */
            xhr.open('post', '/login')

            // 3.设置请求头
            xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded; charset=utf8");

            // 5.接受服务器的返回数据
            xhr.onreadystatechange = function () {
                // 判断状态
                if (xhr.readyState == 4 && xhr.status == 200) {
                    var data = JSON.parse(xhr.response)
                    if (data.success) {
                        login.classList.add('hide')
                        info.classList.remove('hide')
                    }
                }
            }

            // 4.发生请求
            xhr.send('user=' + username + '&passwd=' + passwd)
        }
    </script>
</body>

</html>
```

```js
const express = require('express');
const app = express();

app.use(express.urlencoded({ extended: true }));
app.use(express.static("web"));

app.post("/login", (req, res) => {
    console.log(req.body);
    res.setHeader("Content-type", "text/html; charset=utf8");
    res.json({ success: true, msg: "登录成功！" });
});

app.listen(80);
```



## 三、 jquery中的ajax

### 3.1 发送get请求

-  使用方式：`$.get(url, data, callback, dataType)`

     url: 本次请求的路径 （可以携带query数据）

     data: 本次请求携带的数据 可以是字符串 可以是对象

     callback: 回调函数

     dataType: 数据类型 默认是字符串 当我们传递json的时候，将数据转为json格式

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>

    <h2>Ajax Get请求</h2>
    <input type="button" value="换一换">
    <ul class="list"></ul>

    <script src="./jquery.js"></script>
    <script>
        var btn = document.querySelector('input');
        var list = document.querySelector('.list');

        btn.onclick = function () {
            // jQuery中的 get
            /* 
                $.get(url, data, callback, dataType)
                    1.url: 本次请求的路径 （可以携带query数据）
                    2.data: 本次请求携带的数据 可以是字符串 可以是对象
                    3.callback: 回调函数
                    4.dataType: 数据类型 默认是字符串 当我们传递json的时候，将数据转为json格式
            */
            $.get('/list?user=admin&passwd=123321', function (data) {
                list.innerHTML = '';
                data.forEach(val => {
                    var li = document.createElement('li');
                    li.innerHTML = val;
                    list.appendChild(li);
                })
            })
        }
    </script>
</body>

</html>
```



路由

```js
const express = require('express');
const app = express();

app.use(express.urlencoded({ extended: true }));
app.use(express.static("web"));

app.post("/login", (req, res) => {
    console.log(req.body);
    res.setHeader("Content-type", "text/html; charset=utf8");
    res.json({ success: true, msg: "登录成功！" });
});

app.get('/list', (req, res) => {
    var data = [
        "孙颖莎成首位00后年终世界第一",
        "男子殴打足浴店员工还称叔叔是市长",
        "欧洲第二场战争 即将爆发？",
        "感染新冠两周后心肌会有反应热",
        "医生：新冠损伤味觉一月内可恢复",
        "1380元血氧仪成本仅几十块",
        "白肺的一个明显表现是气紧",
        "乌总统称乌克兰已成全球领导者之一",
        "女子阳后胸痛确诊严重肺炎",
        "女子吸朋友递来香烟后毒品阳性"
    ];
    console.log(req.query);

    data.sort(() => Math.random() - 0.5);
    data = data.slice(0, 3);

    res.json(data);

});
app.listen(80);
```





### 3.2 发送post请求

-  使用方式：`$.post(url, data, callback, dataType)`

     url: 本次请求的路径 （可以携带query数据）

     data: 本次请求携带的数据 可以是字符串 可以是对象

     callback: 回调函数

     dataType: 数据类型 默认是字符串 当我们传递json的时候，将数据转为json格式

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .hide {
            display: none;
        }
    </style>
</head>

<body>

    <h2>Ajax</h2>

    <div class="login">
        <form>
            <label>
                用户名：
                <input type="text" name="user" required />
            </label>
            <br>
            <label>
                密&emsp;码：
                <input type="password" name="passwd" required />
            </label>
            <p>
                <input type="button" name="btn" value="登录">
            </p>
        </form>
    </div>

    <div class="info hide">
        <p>欢迎回来,admin</p>
    </div>

    <script src="./jquery.js"></script>
    <script>
        // 获取标签
        var login = document.querySelector('.login')
        var info = document.querySelector('.info')
        var form = document.querySelector('form')
        var user = form.user
        var pwd = form.passwd
        var btn = form.btn

        btn.onclick = function () {
            // 获取输入的用户名和密码
            var username = user.value;
            var passwd = pwd.value;

            // 判断用户名和密码输入空
            if (!username || !passwd) {
                console.log('请输入用户名和密码');
                return
            }

            // jquery 中的 post
            /* 
                $.post(url, data, callback, dataType)
                    1.url: 本次请求的路径 （可以携带query数据）
                    2.data: 本次请求携带的数据 可以是字符串 可以是对象
                    3.callback: 回调函数
                    4.dataType: 数据类型 默认是字符串 当我们传递json的时候，将数据转为json格式
            */
            $.post("/login", { username, passwd }, (data) => {
                if (data.success) {
                    login.classList.add('hide');
                    info.classList.remove('hide');
                }
            }, 'json');
        }
    </script>
</body>

</html>
```

## 四、相关技术

### 4.1 转为 JSON 对象

JSON内置对象 中提供了两个方法： `parse`、`stringify` （必须严格满足JSON语法）

-  parse: 用于将json字符串解析为json对象

-  stringify: 用于将json对象转为json字符串

eval函数可以将字符串作为代码执行

 使用方式：eval(‘(’ + str +‘)’)

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
<script>
// var str = '{"errno":0,"msg":"post success"}';
// var str = '{"errno":0,\'msg\':"post success"}';
var str = '{"errno":0,"msg":"post success",}';
// 方案一：转成json对象(必须严格转身json语法)
// let data = JSON.parse(str);
// 方案二：通过eval处理
// 'var data = {"errno":0,"msg":"post success",}'
// eval('var data = ' + str)
console.log(data);
</script>
</body>
</html>
```

### 4.2 转码与解码

在URL中本身是不允许出现中文字符以及特殊字符的，但是有些时候还必须要用到中文字符以及特殊字符，此时就需要对URL进行转码

- 转码的使用方式：`encodeURIComponent(code)`

     code: 要转码的中文字符

     返回值就是转码之后的字符

- 解码的使用方式：`decodeURIComponent(code)`

     code: 被转码之后的字符

     返回值解码之后的中文字符

```js
    <script>
        // 转码与解码

        // 获取当前页面地址 location.href
        var url = location.href
        console.log(url);
        // url编码 urlencode
        // http://127.0.0.1:5500/practice/day02/01/01-%E8%BD%AC%E7%A0%81%E4%B8%8E%E8%A7%A3%E7%A0%81/index.html

        // 1.解码   decodeURIComponent()
        console.log(decodeURIComponent(url));
        // http://127.0.0.1:5500/practice/day02/01/01-转码与解码/index.html

        // 2.转码   encodeURIComponent()
        console.log(encodeURIComponent(url));
        // http%3A%2F%2F127.0.0.1%3A5500%2Fpractice%2Fday02%2F01%2F01-%25E8%25BD%25AC%25E7%25A0%2581%25E4%25B8%258E%25E8%25A7%25A3%25E7%25A0%2581%2Findex.html
    </script>
```
