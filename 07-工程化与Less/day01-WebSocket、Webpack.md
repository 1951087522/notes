# 工程化第1天

## 一、服务器

### 1.1 搭建https服务器

HTTP是一个无状态的协议，在请求的过程中，不会进行安全校验，会注入广告或者被拦截，因此HTTPS协议就出现了，HTTPS协议在请求过程中会进行安全校验，所以更安全可靠;

如果要搭建HTTPS服务器需要证书（工作中需要购买，学习中通过openssl创建即可）：

http协议默认的端口号  80

https协议默认的端口号  443

```js
// 引入express
const express = require('express');
// 引入http
const http = require('http');
// 引入https
const https = require('https');
// 引入fs
const fs = require('fs');

// 获取应用程序
const app = express();

// 开放权限
app.use(express.static('./web/'));

// 定义端口号
// let httpPort = 3000;
// let httpsPort = 3001;

// http协议默认的端口号 80
// https协议默认的端口号 443

let httpPort = 80;
let httpsPort = 443;


// 创建http应用
http.createServer(app)
    // 监听端口号
    .listen(httpPort, () => console.log('监听在'+ httpPort +'端口号'));



// 定义key
let key = fs.readFileSync('./ssl//private.pem');
let cert = fs.readFileSync('./ssl/file.crt');

// 创建https应用
https.createServer({ key, cert }, app)
    .listen(httpsPort, () => console.log('监听在'+ httpsPort +'端口'))
```

### 1.2 匿名聊天室

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            list-style: none;
        }

        .container {
            width: 800px;
            margin: 50px auto;
        }

        .list {
            border: 1px solid #ccc;
            height: 400px;
            overflow-y: auto;
        }

        .list>li {
            line-height: 30px;
            border-bottom: 1px dashed #ccc;
        }

        .btns {
            display: flex;
            height: 50px;
        }

        input {
            flex: 1;
            padding: 0 15px;
            outline: none;
        }

        button {
            width: 100px;
            font-size: 20px;
        }
    </style>
</head>

<body>
    <!-- 布局 -->
    <div class="container">
        <ul class="list" id="list">
            <li>大家说:</li>
        </ul>
        <div class="btns">
            <input type="text" id="inp">
            <button id="btn">发言</button>
        </div>
    </div>
    <!-- 引入jquery -->
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.6.3/jquery.js"></script>
    <script>
        // 获取元素
        let btn = document.getElementById('btn');
        let inp = document.getElementById('inp');


        // 点击btn发言 将用户说的话放入到服务器中存储
        btn.onclick = function () {
            // 获取输入框内容
            let val = inp.value;
            // 发送请求
            $.post('/sendMsg', { word: val }, res => {
                // 判断
                if (res.errno === 0) {
                    // 清空输入框内容
                    inp.value = '';
                }
            })
        }


        // 定义信号量
        let idx = 0;
        // 开启定时器 每间隔一段时间 就向服务器发送请求 获取数据 （该行为 称为长轮询技术）
        setInterval(() => {
            // 发送请求
            $.get('/getMsg', { idx }, res => {
                // 判断状态码
                if (res.errno === 0) {
                    // 遍历数组
                    res.arr.forEach(({ word }) => {
                        // 创建li
                        let $li = $('<li></li>');
                        // 设置内部文本
                        $li.html(word);
                        // 追加元素
                        $('.list').append($li);
                        // 改变idx
                        idx++;
                    })
                }

                // 该变list滚动条位置
                $('.list').scrollTop(100000);
            })

        }, 1000)

    </script>
</body>

</html>
```

```js
const express = require('express')
const http = require('http')
const https = require('https')
const fs = require('fs')
const app = express()

// 开放目录权限
app.use(express.static('./web'))
// 配置解析body请求数据
app.use(express.urlencoded({ extended: false }));


// 定义数组 存储用户说的话
let wordArr = []
// 处理接口
app.post('/sendMsg', (req, res) => {
    // 存储
    wordArr.push(req.body)
    // 返回结果
    res.json({ errno: 0 })
})

// 处理getMsg
app.get('/getMsg', (req, res) => {
    // 返回结果
    res.json({ errno: 0, arr: wordArr.slice(+req.query.idx) })
})

// 定义端口
// http的默认端口是 80
let httpPort = 3000
// https的默认端口是 443
let httpsPort = 3001

// 创建http服务
http.createServer(app)
    .listen(httpPort, () => console.log(httpPort))

// 读取文件
const key = fs.readFileSync('./ssl/private.pem')
const cert = fs.readFileSync('./ssl/file.crt')

// 创建https服务
https.createServer({ key, cert }, app)
    .listen(httpsPort, () => console.log(httpsPort))
```





## 二、WebSocket

### 2.1 websocket

webSocket是H5中新增的，与HTTP协议是同级别的，只不过它是有状态的（有持久连接）

HTTP协议：

 前端发送请求，后端得到响应并返回数据，断开连接，之后想要再次发送新的请求，就要再次建立连接通道才能发送请求

webSocket:

 ==前端发送请求，后端得到响应并返回数据，就保持连接，之后想要再次发送新的请求，就可以使用已经建立起来的通道再次发送请求==

### 2.2 socket.io

socket.io是nodejs第三方模块文件，用于统一浏览器发送socket请求的方式

> 下载：npm install socket.io

使用socket.io

 `let socket_io = require('socket.io');`

 socket_io(server);

在地址栏中输入：http://localhost:3000/socket.io/socket.io.js

```js
const express = require('express')
const http = require('http')
const socket_io = require('socket.io')

// 获取应用
const app = express()
// 转为实例
const server = http.Server(app)

// 使用socket
socket_io(server)

// 监听端口
server.listen(3000, () => console.log(3000))

// 在地址栏中输入：http://localhost:3000/socket.io/socket.io.js
```



### 2.3 socket 服务

- 后台搭建

    第一步执行socket.io

    第二步监听`connection`事件

    该事件会在前端发送socket请求的时候触发

- 前端搭建

    第一步：通过script标签引入socket.io.js文件

    第二步：当引入socket.io.js文件之后，向全局暴露一个io变量 。

    要执行io方法， 并且要监听`connect`方法

当我们执行io方法的时候，就会自动发送一个socket请求。这样后台就可以接收到信息。

```js
// 引入socket
let socket = require('socket.io');
// 引入http
let http = require('http');

// 创建应用程序
// let app = http.createServer(function() {
//     console.log('success');
// })

let app = http.Server(function() {
    console.log('succee123');
})

// 执行socket
socket(app);

// 监听端口号
app.listen(3000, () => console.log('listen server 3000'));
```



### 2.4 前后端通信

- 前端socket: 

    on方法监听消息

    第一个参数表示消息名称 

    第二个参数是执行的函数

    emit方法触发消息

    第一个参数是消息名称 

    从第二个参数开始，是传递的数据

- 后端socket:

    on方法监听消息

    第一个参数表示消息名称

    第二个参数是执行的函数

    emit方法触发消息 

    第一个参数是消息名称 

    从第二个参数开始，是传递的数据

```js
// 引入express
let express = require('express');
// 引入socket
let socket = require('socket.io');
// 引入http
let http = require('http');

// 创建应用程序
let app = express();

// 静态化
app.use(express.static('./web'))

// 使用app
let server = http.Server(app);

// 使用socket服务
let io = socket(server);

// 后端建立连接
io.on('connection', client => {
    // console.log(222, client);

    // 通过client建立连接
    client.on('sendMsg', function(num, bool, str) {
        // console.log(111, num, bool, str);
    })

    // 提交消息 emit
    client.emit('message', 200, false, 'hello');
})

// 监听端口号
server.listen(3000, () => console.log('listen server 3000'));
```

### 2.5 聊天室

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="./css/bootstrap.css">
    <style>
        ul,
        ol,
        li {
            list-style: none;
        }

        .col-lg-4,
        .col-lg-8 {
            padding: 0;
        }

        #word_list,
        #user_list {
            height: 300px;
        }

        .btns {
            display: flex;
            height: 50px;
        }

        input {
            flex: 1;
            padding: 0 10px;
        }

        button {
            width: 100px;
            font-size: 20px;
        }
    </style>
</head>

<body>
    <div class="container">
        <!-- 信息部分 -->
        <div class="content_contain clearfix">
            <div class="userList col-lg-4">
                <div class="panel panel-default">
                    <div class="panel-heading">用户列表</div>
                    <div class="panel-body">
                        <ul id="user_list">

                        </ul>
                    </div>
                </div>
            </div>
            <div class="wordList col-lg-8">
                <div class="panel panel-default">
                    <div class="panel-heading">
                        发言列表</div>
                    <div class="panel-body">
                        <ul id="word_list">

                        </ul>
                    </div>
                </div>
            </div>
        </div>

        <!-- 表情包 -->
        <div class="face">

        </div>

        <!-- 发言部分 -->
        <div class="btns">
            <input type="text" id="inp">
            <button id="send">发言</button>
        </div>
    </div>

    <!-- 引入jQuery -->
    <script src="./js/jquery.js"></script>
    <!-- 引入socket -->
    <script src="http://localhost:3000/socket.io/socket.io.js"></script>
    <script>
        let $btn = $('#send')
        let $user_list = $('#user_list')
        let $word_list = $('#word_list')
        let $inp = $('#inp')
        let $face = $('.face')

        // 当引入socket.io.js 文件后 会向全局暴露一个io变量
        let socket = io()

        // 获取用户的名称
        let username = prompt('请输入用户名', 'admin') || 'admin'

        // 捕获 face 目录错误消息
        socket.on('readFaceError', err => console.log(err))

        // 发送消息
        socket.emit('login', username)

        // 监听用户登录信息
        socket.on('userLogin', arr => {
            // 清空之前的内容
            $user_list.empty()
            // 追加子元素
            $user_list.append(arr.map(item => `<li>${item}</li>`))
        })

        // 点击发言
        $btn.click(function () {
            // 获取输入内容
            let val = $inp.val()
            // 发送消息
            socket.emit('sendMsg', { username, val })
            // 清空内容
            $inp.val('')
        })

        // 监听消息
        socket.on('backMsg', arr => {
            // 清空之前的内容
            $word_list.empty()
            $word_list.append(arr.map(obj => `
            <li>
                <span>${obj.username}</span>
                <span>说：</span>
                <span>${obj.val}</span>
            </li>
            `))
        })

        // 监听 face 消息
        socket.on('face', arr => {
            $face.append(arr.map(item => `<img src="face/${item}" data-daihao="${item.split('.')[0]}" alt="" />`))
        })

        // 委托代理
        $face.delegate('img', 'click', function () {
            // 获取代号
            let name = $(this).data('daihao')
            // 追加内容
            $inp.val($inp.val() + '[' + name + ']')
        })
    </script>
</body>

</html>
```

```js
// 引入模块
const express = require('express')
const http = require('http')
const socket_io = require('socket.io')
const fs = require('fs')

// 获取应用
const app = express()
// 静态化目录
app.use(express.static('./web'))
// 转为实例
const server = http.Server(app)

// 定义空数组 存储用户名
let userArr = []
// 存储发言内容
let wordArr = []

// 使用 socket
let io = socket_io(server)
// 后端监听connection方法建立链接
io.on('connection', socket => {
    // 读取标签包文件
    fs.readdir('./web/face/', (err, arr) => {
        // 捕获异常
        if (err) {
            return socket.emit('readFaceError', err)
        }
        // 返回结果
        socket.emit('face', arr)
    })

    // 通过on监听消息
    socket.on('login', username => {
        // 存储
        userArr.push(username)

        // 如果想要让所有用户看到登录状态 使用 io变量 广播
        io.emit('userLogin', userArr)
    })

    // 监听消息
    socket.on('sendMsg', obj => {
        // 处理obj.val
        obj.val = obj.val.replace(/\[(\w+)\]/g, (match, $1) => {
            return `<img src="face/${$1}.gif" alt="" />`
        })
        // 存储数据
        wordArr.push(obj)

        // 广播所有人
        io.emit('backMsg', wordArr)
    })
})


// 监听端口
server.listen(3000, () => console.log(3000));
```



## 三、webpack

### 3.1 webpack

webpack是由facebook公司推广并维护的一套工程化工具，早先为react使用，后来应在其它框架中，

 核心理念是：一切文件都是资源，是资源都可以模块化打包加载

 js文件是资源，css文件是资源，模板文件是资源，图片文件是资源等等，所以这些资源我们都可以模块化打包加载，并且webpack推荐使用commonjs规范，所以我们可以像开发node一样来发webpack项目

 特点：模块化开发、打包加载

github：http://github.com/webpack/webpack

官网：http://webpackjs.org/

 我们基于webpack4版本讲解。

### 3.2 安装

- 需要进行两次安装：

    -  第一次全局安装（全局安装是为了提供指令）：

    ​		 npm install webpack -g

    ​		 npm install webpack-cli –g

    ​		 npm install webpack-dev-server -g

    -  第二次本地安装（本地安装是为了在项目开发中使用）：

    ​		 npm install webpack

- 安装完成，输入webpack -v查看版本号


- webpack配置文件是webpack.config.js，要像定义接口对象一样定义配置。


### 3.3 入口文件

重要概念：

- ​		入口：所有文件开始打包的地方（引入） 


- ​		出口：所有文件打包之后的地方（发布）


- ​		 加载机：由于webpack只能识别js文件，除了这个类型之外的文件都不能识别，必须要借助加载机


 		插件：webpack本身不具备的功能，我们可以为webpack拓展

我们通过entry配置项定义入口文件（webpack最先引入最先处理的文件）

​		 属性值

​				 字符串，表示一个文件地址

​				 ==对象，配置多个入口文件:==

​						 key表示文件名称（发布的文件的名称） 

​						 value 表示文件真实地址

- 引入多个入口文件

```js
// 定义配置对象
const config = {
    // 配置环境 (开发环境下 代码是不会压缩的)
    // 注意: 通常在开发过程中 建议使用开发环境  在项目开发完毕之后 在上线之前 一键压缩（发布环境）
    mode: 'development',
    // 入口文件
    entry: {
        // 对象，配置多个入口文件
        // Key 表示发布之后的文件名称(自定义)
        // value: 文件的路径
        // a: './modules/header.js',
        // b: './modules/footer.js'

        header: './modules/header.js',
        footer: './modules/footer.js'
    },
    // 出口配置
    output: {
        // 如果入口文件中有多个文件 此时可以使用[name]的方式 替换文件的名称
        filename: './[name].js'
    }
}

// 暴露出去
module.exports = config;
```

- ==配置目录问题==
    - `webpack --config `

```js
// 定义配置对象
const config = {
  // 配置环境
  mode: 'development',
  // 入口文件
  entry: {
    main: './modules/main.js' // 此时路径相对于整个根目录
  },
  // 出口配置
  output: {
    filename: './[name].js'
  }
}

// 暴露
module.exports = config
/* 
  总结  
    如果将 webpack配置文件 存储到特殊的位置
    此时使用 webpack --config ./webpack配置文件
*/
```



### 3.4 发布文件

webpack自身没有实现资源定位，所以我们要配置发布的文件（html中引入的文件），通过output配置

​		 属性值是对象

​				 filename定义发布后的文件名称

​						 如果有一个入口文件，filename直接写发布的文件

​						 如果有多个入口文件，用[name]表示文件的名称

​				 path定义文件发布的地址

​						 未定义path，默认向dist目录向下发布

​						 定义了path，将向path目录下发布

​		运行 `webpack` 即可发布，4.0中默认发布到dist目录下。

```js
// 引入path
const path = require('path');

// 基于commonjs规范暴露接口
const config = {
    // 通过mode属性 指定开发环境 
        // production 发布环境  (默认是一个发布环境)
        // development 开发环境 （代码不会压缩 不会花费时间）
    mode: 'development',

    // 定义入口文件通过entry
    entry: './modules/main.js',
    // 出口
    output: {
        filename: './abc.js',
    }
}

// 暴露接口
module.exports = config;
```

### 3.4 webpack 重要属性介绍

- 通过设置 `mode` 模式压缩代码
    - `production`	作为产品发布 代码压缩(默认值)
    - `development`  作为开发项目 代码不压缩
- 入口文件 `entry`
    - 属性值是一个字符串表示 单个入口文件;还可以是多个入口文件
- 出口配置 `output`
    - 默认是当前目录下的dist目录 还可以通过path自定义发布位置
    - 默认名称叫做 main.js  还可以通过`filename` 自定义名称

```js
// 引入path
const path = require('path')
// 定义接口对象
module.exports = {
    // 1.通过设置 mode 模式压缩代码
    /*
        production:作为产品发布 代码压缩(默认值)
        development:作为开发项目 代码不压缩
    */
    // mode: 'development',

    /* 
        2.入口文件
            属性值是一个字符串表示 单个入口文件
            还可以是多个入口文件
    */
    entry: './modules/main.js',

    // 3.出口配置
    output: {
        // 2.1默认是当前目录下的dist目录
        // 还可以通过path自定义发布位置
        // path: path.join(__dirname, './modules'),

        // 2.2默认名称叫做 main.js 
        // 还可以通过filename 自定义名称
        filename: './bundle.js',
    },
}
```



### 3.5 服务器

- `webpack -w` 监听与发布

- webpack没有内置服务器，我们需要安装`webapck-dev-server`模块

     跟webpack一样，提供了webapck-dev-server指令，所以要安装两次：全局安装，本地安装

     安装完成，通过webapck-dev-server --version查看版本号

-  进入项目，输入webpack-dev-server即可启动服务器

     启动服务器时，使用文件的时候写绝对路径

     默认启动8080端口号，如果8080端口号被占用，webapck-dev-server端口号会自动加1

     可以通过webpack-dev-server --port xxx 指定端口号

```js
    <script>
        // 发送请求
        let xhr = new XMLHttpRequest()

        // 内置服务器只支持 get 请求
        xhr.open('get', '/data/home.json', true)

        xhr.send()
    </script>
```