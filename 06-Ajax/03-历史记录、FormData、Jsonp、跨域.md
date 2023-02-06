## 一、 历史记录

在H5中提供了history对象用于操作历史记录的，分别提供了两个方法：

`history.pushState(any, str, url)`

`history.replaceState(any, str, url)`

- 以上两个方法都是改变历史记录列表的

     any: 表示数据

     str: 字符串参数，目前所有浏览器都不会处理该参数

     url： 新的网页的url

- 与之配套的还有一个事件：`window.onpopstate`事件

     该事件会在历史记录发生变化的时候执行

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        dl,
        dt,
        dd,
        p,
        h2,
        h3,
        body {
            margin: 0;
            padding: 0;
        }

        body {
            background: #f2f4f6;
            /* padding: 100px 0; */
        }

        h2 {
            color: #ac3e40;
            line-height: 36px;
            font-size: 21px;
        }

        h3 {
            font-weight: normal;
            font-size: 14px;
            line-height: 22px;
            color: #999;
        }

        span {
            margin-right: 20px;
        }

        p {
            color: #555;
            padding: 1rem 0 0;
            font-size: .9rem;
            height: 2.8em;
            line-height: 1.45em;
            overflow: hidden;
            text-overflow: ellipsis;
            word-break: break-word;
            word-wrap: break-word;
        }

        .wrapper {
            width: 900px;
            margin: 0 auto;
            padding: 100px 0;
        }

        .card {
            background: #fff;
            height: 160px;
            margin-bottom: 20px;
        }

        .card dt {
            width: 300px;
            height: 160px;
        }

        .card dd {
            width: 600px;
            height: 160px;
            padding: 1.1rem;
            box-sizing: border-box;
        }

        .card dt,
        .card dd {
            float: left;
            overflow: hidden;
        }

        .card dt img {
            width: 100%;
        }

        .card:after {
            content: "";
            display: block;
            clear: both;
        }

        .pages {
            padding: 20px 50px 0;
        }

        .pages a {
            display: inline-block;
            width: 50px;
            height: 50px;
            font-size: 30px;
            text-decoration: none;
            margin: 0 10px;
            color: #333;
            cursor: pointer;
        }

        .pages .focus {
            color: red;
            text-decoration: underline;
        }

        .top {
            width: 100%;
            height: 50px;
            background: #26272b;
            position: fixed;
            top: 0;
        }

        .nav {
            width: 900px;
            height: 50px;
            margin: 0 auto;
            background: url(images/top.png) no-repeat left center;
        }

        .search {
            background: #fff;
            width: 240px;
            height: 36px;
            float: right;
            border: none;
            outline: none;
            padding: 0 30px;
            margin-top: 7px;
            margin-right: 10px;
            line-height: 36px;
            border-radius: 30px;
        }
    </style>
</head>

<body>

    <div class="top">
        <div class="nav">
            <input class="search" placeholder="搜索" />
        </div>
    </div>
    <!-- <h2>历史记录</h2> -->

    <div class="wrapper">
        <div class="content">
            <!-- <dl class="card">
      <dt><img src="https://img.alicdn.com/tfs/TB12Qh4vVP7gK0jSZFjXXc5aXXa-900-500.png" alt=""></dt>
      <dd>
        <h2>VS Code 是如何优化启动性能的？</h2>
        <h3><span>作者: 柳千</span><span>发表于: 2021-09-08</span></h3>
        <p>本文主要是对 CovalenceConf 2019: Visual Studio Code – The First Second 这次分享的介绍，CovalenceConf 是一个以 Electron 构建桌面软件为主题的技术会议，这也是 VS Code 团队为数不多的对外分享之一(质量较高)...</p>
      </dd>
    </dl> -->
        </div>
        <div class="pages">
            <a data-page="1">1</a>
            <a data-page="2">2</a>
            <a data-page="3">3</a>
            <a data-page="4">4</a>
            <a data-page="5">5</a>
        </div>
    </div>

    <script>
        var contentTag = document.querySelector('.content')
        var aTags = document.querySelectorAll('.pages a')

        ajaxData('1').then(({ success, data }) => {
            if (success) {
                renderHTML(data)
                /* 
                    1.history.replaceState(any, str, url)
                */
                history.replaceState(data, '', '/#page=1')
            }
        })

        // 页码点击事件
        for (let i = 0; i < aTags.length; i++) {
            aTags[i].onclick = function () {
                // dataset 对象 获取标签中以data- 开头的自定义属性的值
                var page = this.dataset.page

                ajaxData(page).then(({ success, data }) => {
                    if (success) {
                        renderHTML(data)
                        /* 
                            2.history.pushState(any, str, url) 改变历史记录列表
                                any: 表示数据
                                str: 字符串参数，目前所有浏览器都不会处理该参数
                                url： 新的网页的url
                        */
                        history.pushState(data, '', '/#page=' + page)
                    }
                })


            }
        }
        // 3. 相对应的事件 window.onpopstate事件
        // 该事件会在历史记录发生变化的时候执行
        window.onpopstate = function (e) {
            renderHTML(e.state)
        }


        // 渲染页面封装函数
        function renderHTML(data) {

            contentTag.innerHTML = ''

            data.forEach(({ img, title, author, date, content }) => {
                var dl = document.createElement('dl')
                dl.className = 'card'
                dl.innerHTML = `
                <dl class="card">
                <dt><img src="${img}" alt="">
                </dt>
                <dd>
                <h2>${title}</h2>
                <h3>
                    <span>${author}</span >
                    <span>${date}</span>
                </h3 >
                    <p>${content}</p>
                </dd >
                </dl >
                `
                contentTag.appendChild(dl)
            });
        }

        function ajaxData(page) {
            return new Promise(resolve => {
                // 创建Ajax 对象
                var xhr = new XMLHttpRequest()

                xhr.open('get', '/data/' + page)

                // 修改数据类型
                xhr.responseType = 'json'

                xhr.onload = function () {
                    resolve(xhr.response)
                }

                xhr.send()
            })
        }
    </script>

</body>

</html>
```



## 二、Ajax 2.0

### 2.1 FormData

在AJAX2.0中新增了`FormData`构造函数

作用：用户快速进行表单序列化，来代替表单。

使用方式：

let fd = new FormData(form)

form: 原生的form表单元素

参数是可有可无的

如果传递了参数，得到一个fd的实例化对象，我们可以通过其原型中的方法查看内部结构

如果没有传递参数，得到的是一个空的对象，我们可以调用原型中的方法添加数据

- **forEach**

    使用方式： fd.forEach(fn)

    fn: 处理函数

    第一个参数： 输入的内容

    第二个参数： 输入框name值

    第三个参数： FormData对象

    this指向全局作用域

- **append**

    该方法用于添加数据

    使用方式：fd.append(key, value)

    key: name值

    value: 是数据

- **delete**

    delete：该方法用于删除数据中的某一项

    使用方式：fd.delete(key)

    key: 数据名称

- **get**

    该方法用于获取某一项数据

    使用方式：fd.get(key)

    key： 对应的name值，返回值就是获取到数据

- **getAll**

    getAll：该方法用于获取某个name字段的所有数据

    使用方式：fd.getAll(key)

    key: 对应的name属性值，返回值是一个数组

- **has**

    该方法用于判断是否包含某个属性

    使用方式：fd.has(key)

    key: 对应的name值，返回值是布尔值，如果存在返回true，如果不存在返回false。

- **set**

    该方法用于设置内容的，与append方法不同的是，set方法会覆盖掉之前已经添加的数据

    使用方式：fd.set(key, value)

    key: 对应的name值 value: 数据

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
    <form>
        <input type="text" name="a1" value="111"><br>
        <input type="text" name="a2" value="222"><br>
        <input type="text" name="a3" value="333"><br>
        <input type="text" name="a4" value="444"><br>
        <input type="text" name="a5" value="555"><br>
        <input type="button" value="获取">
    </form>
    <script>
        var form = document.querySelector('form');
        var btn = document.querySelector("[type=button]");

        btn.onclick = function () {

            var fd = new FormData(form)

            // 2.添加数据 同名属性可以重复添加
            /* 
                第一个参数  name
                第二个参数  value
            */
            fd.append('b1', 'aaa')
            fd.append('b2', 'bbb')
            // 同名属性可以重复添加
            fd.append('a3', '3333')
            fd.append('a3', '3333')
            fd.append('a3', '3333')

            // 3.修改指定属性的值，如果属性不存在，则添加数据
            fd.set('b2', '666')
            fd.set('z1', 'zzz')

            // 4.删除数据
            // 参数 指定name属性
            fd.delete('a2')

            // 5.获取指定属性的值
            console.log(fd.get('a3'));

            // 6.获取所有指定属性的值
            console.log(fd.getAll('a3'));

            // 7.判断有没有指定的属性
            console.log(fd.has('a1'));

            // 1.遍历 formData 实例对象的值
            /*  
                第一个参数 值
                第二个参数 name 名称
                第三个参数 formData本身
            */
            fd.forEach((value, name, obj) => {
                console.log(value, name, obj);
            })
        }
    </script>
</body>

</html>
```



### 2.2 图片预览

一般图片的src属性指向服务器上资源

所谓图片预览，指的是在图片没有上传到服务器之前，就要预览图片

在H5中提供了一个`FileReader`构造函数用于图片预览的

在H5之前，在URL对象上提供了createObjectURL方法用于图片预览

### 2.3 createObjectURL

window.URL.createObjectURL

该方法也是用于图片预览的，是在H5之前提供的

使用方式：`window.URL.createObjectURL(file)`

file： 上传的文件

返回值是blob数据

blob 是一个大的二进制文件

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .preview {
            border: 1px solid #333;
            padding: 20px;
        }

        .preview:after {
            content: "";
            display: block;
            clear: both;
        }

        .preview div {
            width: 200px;
            height: 200px;
            border: 1px solid #ccc;
            background: url(./images/plus.png) no-repeat center center;
            background-size: contain;
            float: left;
            margin-right: 20px;
            margin-bottom: 20px;
        }

        .preview input {
            display: block;
            width: 100%;
            height: 100%;
            opacity: 0;
            font-size: 0;
        }
    </style>
</head>

<body>

    <h2>图片上传预览</h2>

    <div class="preview">
        <div>
            <input type="file">
        </div>
    </div>
    <br>
    <input type="button" value="上传">

    <script>
        var preview = document.querySelector('.preview');
        var btn = document.querySelector('[type=button]');
        var fd = new FormData();

        preViewImage(preview.querySelector('div'));

        // 点击上传
        btn.onclick = function () {
            var xhr = new XMLHttpRequest();
            xhr.open("post", "/upload");
            xhr.onload = function () {
                console.log(xhr.response);
            }
            xhr.send(fd);
        };

        function preViewImage(div) {
            var input = div.querySelector('input');
            var cloneDiv = div.cloneNode(true);
            // 当表单元素内容发生变化自动触发的事件
            input.onchange = function () {
                var file = this.files[0];


                // 1.ES6 之前获取图片url的方式
                /* var url = URL.createObjectURL(file);
                div.style.backgroundImage = `url(${url})`;
                preview.appendChild(cloneDiv);
                preViewImage(cloneDiv); */

                //  2.ES6之后
                // 读取图片相关信息
                var fr = new FileReader();
                fr.onload = function () {
                    div.style.backgroundImage = `url(${fr.result})`;
                    preview.appendChild(cloneDiv);
                    preViewImage(cloneDiv);
                };
                fr.readAsDataURL(file);

                fd.append("file", file);
            }
        }
    </script>
</body>

</html>
```



## 三、跨域

### 3.1 域

域: 也称源，是指存放资源的服务器。

服务器地址称为域地址。

域地址由三部组成： 访问协议、主机名称、主机端口

主机名称可以是一个域名，也可以是一个ip地址。

协议、主机、端口三者都一致的资源，为同一个域中的资源，也称同源。

跨域：在一个域中访问另一个域中的资源，称为跨域访问，简称跨域。

例如： 有一个服务器A，有一个服务器B，服务器A上面的页面请求服务器B上的资源，这个行为就称为跨域请求资源

### 3.2 跨域鉴定

当前域是[http://localhost:3000，当请求回来的页面中，又发送请求到其它服务器中请求资源：](http://localhost:3000，当请求回来的页面中，又发送请求到其它服务器中请求资源：/)

不跨域 url: 'http://localhost:3000/regist?username=ickt',

跨域 url: 'http://localhost:3001/regist?username=ickt',

当协议、域名、端口号三者有任意一个值不同，则为为跨域。

### 3.3 同源策略

浏览器出于安全考虑，只允许ajax请求同一个源中的资源，称为同源策略。

ajax默认不能跨域，但可以通过与服务器配合设置，实现跨域访问。

```js
<!-- 

同源策略：
    所有资源的 协议 、 主机地址、 端口号 必须是一致的。

    如果请求地址与当前网站地址的协议 、 主机地址、 端口号中任意一项不一致，请求就跨域了。

ajax 不支持跨域请求！

    http://localhost:80/

    http://127.0.0.1/images/1.jpg

    http://localhost/images/1.jpg

-->
```



### 3.4 JSONP

jsonp: Json with padding

> jsonp是一种跨域请求资源的解决方案，JSONP的原理是利用script标签可以跨域请求资源的特性加载其它域的js代码。
>

我们可以利用script标签无视同源策略的特点，**将该标签上的src指向目标服务器上的接口**

向目标服务器发送一个方法名称以及数据，服务器接收数据，并返回数据（要放在方法中）客户端定义好该方法，让方法去执行，即可解决跨域

需要跨域的服务器配合（将数据放在方法中执行）

后端语言生成jsonp数据时，要注意 `json` 字符串必须是标准json字符串。

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

    <h2>服务器 3100</h2>

    <!-- <script>
        var xhr = new XMLHttpRequest()
        // 1.请求自己服务器的资源
        // xhr.open('get', 'http://localhost:3100/data.json')

        // 2.跨域请求 (会被禁止 Access-Control-Allow-Origin)
        xhr.open('get', 'http://localhost:4100/data.json')

        xhr.onload = function () {
            console.log(xhr.response);
        }

        xhr.send()
    </script> -->


    <!-- JSONP -->
    <script>
        function fn1(data) { console.log(data); }

        // JSONP 接口是由后端提供的
        getJSON("http://localhost:4100/data?callback=fn1")
        // 发生JSONP请求的封装函数
        function getJSON(url) {
            var script = document.createElement('script')
            script.src = url
            document.body.appendChild(script)
            // 暴露接口地址 删除script标签
            document.body.removeChild(script)
        }
    </script>
</body>

</html>
```

```js
const express = require('express');
const fs = require('fs');
const app = express();

app.use(express.static("./web"));

app.get("/data", (req, res) => {

    let content, { callback } = req.query;

    try {
        content = fs.readFileSync("./web/data.json").toString();
    } catch (error) {
        content = "暂无数据";
    }

    res.end(`${callback}(${content})`);

});

app.listen(4100, () => console.log("server 4100..."));
```



### 3.5 jQuery 中的 JSONP

- 使用方式：

    ```js
    $.ajax({
         url: 请求的路径
         type： 请求的方式
         dataType: jsonp
         jsonp: jsonp请求的callback参数字段名称
         jsonpCallback: 指定回调函数的名称
         success: 成功时候执行的回调函数
    })
    ```

    ```js
        <script>
            function fn1(data) { console.log(data); }
    
            // 1.jquery中的跨域请求方法
            // $.getScript("http://localhost:4100/data?callback=fn1");
    
            // 2.使用jQuery自带的回调函数接受数据
            /* $.ajax({
                url: 'http://localhost:4100/data',
                dataType: 'jsonp',
                success(data) {
                    console.log(data);
                }
            }) */
    
            // 3.使用自定义函数的接受数据
            /* $.ajax({
                url: 'http://localhost:4100/data',
                dataType: "jsonp",
                jsonpCallback: "fn1"
            }) */
    
            // 4.使用 Promise 风格写法的回调函数
            /* $.ajax({
                url: 'http://localhost:4100/data',
                dataType: "jsonp"
            }).then(data => console.log(data)) */
        </script>
    ```

    

### 3.6 代理模板

在html中，通过iframe定义框架，可以引入其它页面（有跨域问题，不能访问其它域上一层框架）。

在A域中，通过iframe引入B域的页面

在B域中，将请求重定向到A域的页面（代理模板）中

在A域中，通过代理模板解析数据，并执行A域的方法，实现跨域请求数据的获取

注意：需要B域服务器端的配合：重定向配合。



### 3.7 html5 跨域

允许所有的服务器向当前服务器发送跨域请求 `res.setHeader('Access-Control-Allow-Origin', '*');`

```js
const express = require('express');
const fs = require('fs');
const app = express();

app.use(express.static("./web"));

app.get("/data", (req, res) => {

    let content, { callback } = req.query;

    // 允许所有的服务器向当前服务器发送跨域请求   (但是大部分服务器不会开放会降低服务器安全性)
    // 允许所有的请求跨域 Access-Control-Allow-Origin
    res.append('Access-Control-Allow-Origin', '*');

    try {
        content = fs.readFileSync("./web/data.json").toString();
    } catch (error) {
        content = "暂无数据";
    }

    res.json(JSON.parse(content))

});

app.listen(4100, () => console.log("server 4100..."));
```