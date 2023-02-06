## 一、服务器

### 1.1 服务器模型

根据架构可以分成B/S 和 C/S

B/S 就是 Browser-Server 也就是浏览器 对应 服务器

C/S 就是 Client-Server 也就是客户端对应服务器

其实Browser也是一个客户端。但是与C/S的客户端的不同在于：qq的客户端只能够去连接qq的服务器，微信的客户端只能够去连接微信的服务器。游戏的客户端只能连接对应的游戏（LPL只能连接LPL的服务器、 梦幻西游只能连接梦幻西游的服务器）而Browser并没有限制连接的是哪个服务器 你既可以访问百度的服务器，又可以访问网易的服务器，还可以访问其它任何拥有域名或IP地址的服务器。

服务器（Server）简单来说是可以提供服务（响应）的机器。是在网络环境下运行的应用程序， 为网上的用户提供共享信息和各种服务的一种高性能计算机。浏览器作用: 是为了给用户渲染页面的

## 二、NodeJS

### 2.1 介绍

我们知道js这门语言是需要依赖于宿主环境，而最受欢迎的宿主环境是浏览器

但是，它并不是唯一的选择，完全可以脱离浏览器在后端运行，nodejs就是运行在服务器端的一门语言。

官网

https://nodejs.org/en/

- NodeJS有三大特点：

     单线程，

     非堵塞IO，

     事件驱动

### 2.2 NodeJS 特点

- **单线程**

整个程序只有一条线程执行（同一时间只能做一件事）

- **非阻塞I/O**

I/O： input/output 输入/输出

input: 从磁盘中输入内容到内存中 output: 从内存中读取内容到磁盘中

非阻塞I/O： 当线程执行的时候，如果遇到了I/O操作，只是开启一个任务，线程不会等待，去执行下一条任务

阻塞I/O： 当线程执行的时候，如果遇到了I/O操作，开启一个任务并等待任务执行完毕之后，才去执行下一个任务

- **事件驱动**

由于nodejs是非阻塞的，线程不会等待，但是后续的任务如何执行呢？此时会触发一个事件，由该事件将后续的任务重新放入执行队列中

（nodejs循环队列）

适用场景

使用nodejs搭建的服务器，特别适用异步多，高并发，不适合计算，因为计算是同步的，会造成阻塞

举例：比如火车站的售票窗口，所有人都要去售票窗口买票，如果每一个人都争抢着去买票，这样的话，什么也做不了，所以要引入排队机制再比如我们都要坐公交车，有些时候司机时候只会打开前门，此时每一个人都抢着上车的话，可想而知，什么也做不了，所以也要引入排队机制，现实生活中如此，程序也是如此，如果不引入排队机制将会一直阻塞。

回到nodejs中，既然它是单线程，就不能造成阻塞，就好比一个人在售票窗口买票的时候，发现自己没有带钱，于是打电话叫朋友过来送钱，此时这个人依然站在售票窗口前，坚决不肯让出自己的位置，这样的话就势必会造成阻塞

现实中，最好的解决方式是：这个人应该始终与后面这个人交换位置。

所谓事件驱动就是，一旦你造成阻塞了，你就靠边站，当等到什么时候不再阻塞，你再通知我，我再把你加入排队当中，再次等待被处理

### 2.3 运行 NodeJs

- cd 目录 用于打开目录

- 执行命令： node 文件名称

- 盘符命令：
    - 想要从C盘切换到D盘：在控制台输入D冒号按下回车即可。
    - 想要从D盘切换到E盘：在控制台输入E冒号按下回车即可

- 退回上一级目录： cd .. 或者是 cd ../ 即可退回上一级目录

- 补全文件名称： 点击tab键， 用于补全文件名称 还可以切换文件

- 命令历史记录： 在使用命令的时候，我们可能要使用之前的命令，此时在键盘上按下上下箭头，即可找回之间的使用的命令

- 清屏命令： 输入 cls 指令即可清屏
    - clear

### 2.4 模块化

nodejs也是模块化的。模块化的代表框架：

RequireJS：遵循的是AMD规范（异步规范），一个文件就是一个模块

- 引入文件：通过require方法引入。
- 暴露接口：exports，module.exports，也可以使用return这种方式

Seajs：遵循的是CMD规范（异步规范），一个文件就是一个模块。

- 引入文件：通过require方法。
- 暴露接口：exports，module.exports

Nodejs是遵循CommonJS规范（同步规范）：一个文件就是一个模块

- 引入文件：通过require方法
- 暴露接口：exports，module.exports
- ==时刻谨记，require0模块时，得到的永远是module.exports指向的对象：==
  - ![image-20230123134714417](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123134714417.png)
  - 注意：为了防止混乱，建议不要在同一个模块中同时使用exports和module.exports

### 2.5 模块分类

在nodejs中模块分为三类：

- 1，内置模块（核心模块）

例如：HTTP、HTTPS、Path、FS、QueryString……

内置的模块可以直接引入并使用，通过require方法直接引入模块名称

- 2，第三方模块（自定义的模块）

例如：babel-core、typescript

第三方模块需要通过npm安装后才能使用，通过require方法直接引入模块的名称

- 3，文件模块 （一个文件就是一个模块）

引入文件模块要使用相对路径（相对于当前文件所在的位置，如：./ ../）

### 2.6 node_modules

该文件夹用于存储所有的第三方模块，当把文件存储在该文件夹中，我们就可以不用写路径，像引入内置模块那样来引入模块文件了

*注意：该文件夹的名称必须是node_modules 只有这样nodejs才认识它*

特点：

该文件所处的位置可以是在引入文件的同级也可以是上级目录或者是上上级目录（祖先目录，直到硬盘的根路径），当我们引入模块的时候，就可以根据就近原则，找到node_modules文件夹中对应的模块

```js
// 使用内置模块
// 注意：引入内置的模块，不要加相对路径，直接写模块名字
// let http = require('http');
// console.log(http);
// let fs = require('fs');
// console.log(fs);


// 第三方模块
// 第三方模块也是直接写名称，不要加相对路径
// let color = require('color');
// console.log(color);
// 一个目录下有很多模块，我们将这个文件夹称之为“包”
// 别人开发的第三方模块不仅仅是一个文件（模块），里面有很多的模块，所以我们将其称之为包
// npm安装的一个包（包含很多模块），而不是一个模块
// let vue = require('vue');
// 包里面有很多的默认。引入包就是引入其默认模块，我们还可以引入其他的模块
// let vue = require('vue/dist/vue.global.js');
// console.log(vue);

// 引入自己写的文件模块
// 区分自己写的模块：是否添加了相对路径：./, ../
// var demo = require('./demo');
// console.log(demo);

// 引入目录下的模块
// var abc = require('./test/abc.js')
// console.log(abc);

// .js拓展名可以省略。，文件夹下面的index.js也可以省略
// var index = require('./test/index.js')
// 当text/index.js与test.js文件同时存在的时候，优先引入test.js文件
// var index = require('./test')

// 将自己写的文件变成第三方模块
// let demo = require('./demo');
// let demo = require('demo');
// console.log(demo);

// 第三方模块会从当前目录一直向上查找node_modules目录，直到找到未知
// require('ickt')
// console.log(require('react'));
// 找不到了
console.log(require('react10'));
```

```js
var obj = require('./a')
console.log(obj);

// 加载 node_modules 文件夹里的模块，只需要写文件名就可以了，会自动找到这个模块
var demo = require('demo')
console.log(demo);

-----
 
// 文件夹存放在 node_modules 里 文件夹存放路径没有要求 优先在当前目录下查找 找不到会一直查找到根目录
// 以模块名为文件夹名
// 文件名必须是 index.js

module.exports = {
    test: '自动找到这个模块不需要写路径'
}
```



## 三、内置模块

### 3.1 HTTP

HTTP模块用于搭建服务器的

- **创建服务器**

    `createServer(handle)` 该方法用于搭建HTTP服务器

    handle: 处理函数，函数中有”两个”参数：

    第一个参数：req对象： 全称request 请求对象，常用的属性：

    - url（本次请求的路径）
    - method（本次请求的方式)
    - headers（请求头对象相关信息）

    第二个参数：res对象： 全称response 响应对象，常用的属性：

    - write（返回数据 不会断开连接 必须与end方法一起使用）
    - end（返回数据 会断开连接，该方法只接受字符串类型的参数以及Buffer数据类型）
    - setHeader（用于设置响应头）返回值是服务器对象 
        - writeHead() 修改多个响应头

- **监听方法**

    `listen(port, ip, handle)`

    - port: 监听的端口号 不要使用1000以内的（可能被占用）
    - ip: 指定的ip地址，可以省略
    - handle: 监听成功之后执行的方法

```js
// 1.引入模块
const http = require('http')

/* 
    第一个参数：req对象： 全称request 请求对象，常用的属性：
        url（本次请求的路径）
        method（本次请求的方式)
        headers（请求头对象相关信息）

    第二个参数：res对象： 全称response 响应对象，常用的属性：
        write（返回数据 不会断开连接 必须与end方法一起使用）
        end（返回数据 会断开连接，该方法只接受字符串类型的参数以及Buffer数据类型）
        setHeader（用于设置响应头）返回值是服务器对象
*/

// 2.createServer(handle) 该方法用于搭建HTTP服务器
const server = http.createServer(function (req, res) {
    // 5.2设置请求头信息(修改多个响应头)
    /* res.writeHead(210, {
        'Content-Type': 'text/html; charset=utf-8',
        color: 'red',
        hi: 'demo'
    }) */

    // 5.3设置请求头信息(单个)  二选一
    // plain 纯文本
    res.setHeader('Content-Type', 'text/plain; charset=utf-8');

    // 5.1中文无法识别需要设置编码格式
    res.end('<h1>行百里者半九十</h1>')


    // 3.获取表单信息
    /*
        /?user=123&pwd=123
        /favicon.ico
    */
    console.log(req.url);
})

/* 
    4.指定默认端口
        默认局域网
        第一个参数：配置站点的访问端口
        第二个参数：限制访问ip
            特殊ip 0.0.0.0 表示不限制ip
        第三个参数：服务器启动的回调函数
    本地站点默认域名 localhost:端口
    改变服务器内容需要终止终端
    ctrl + c 
*/
//指定默认端口
server.listen(
    3000,   // 第一个参数：配置站点的访问端口
    '0.0.0.0',  // 第二个参数：限制访问ip
    () => console.log('服务器启动成功') // 第三个参数：服务器启动的回调函数
)
```

### 3.2 FS

FS模块全称: file System 文件系统。作用是用于操作文件夹以及文件的，使用的时候要引入fs模块。操作文件是异步的，因此fs模块为每一个操作提供了两个方法：同步方法（sync），异步方法（回调函数监听）

##### **创建文件**

fs.appendFile(fileName, content, callback)

- fileName: 创建的文件名称（合法的路径）
- content: 追加的内容
- callback： 回调函数
    - 参数表示错误异常，如果创建成功 则返回null，如果创建失败 则返回一个错误对象
    - 文件不存在会创建新文件 不会覆盖 往后追加

 writeFile()

- fs.writeFile('test.txt', 'writeFile()创建并写入内容,会覆盖原有内容', (err) => console.log(err))	
    - 如果文件已经存在会覆盖原内容

```js
// 1.引入模块
const fs = require('fs')

// 2.创建文件
/* 
    第一个参数  创建的文件名称
    第二个参数  文件内容
    第三个参数  回调函数
        创建成功 返回 null
        创建失败 返回对象
*/

// 2.1 writeFile() 创建并写入内容到文件
// 如果文件已经存在会覆盖原内容
fs.writeFile('test.txt', 'writeFile()创建并写入内容,会覆盖原有内容', (err) => console.log(err))

// 2.2 appendFile() 往文件的最后追加新内容
// 文件不存在会创建新文件
fs.appendFile('test.txt', '\n行百里者半九十', (err) => console.log(err))
fs.appendFile('test1.txt', '行百里者半九十', (err) => console.log(err))
```



##### **创建文件夹**

fs.mkdir(pathName, callback) 该方法用于创建文件夹

- pathName: 文件夹名称
- callback: 回调函数

参数表示错误异常，如果创建成功则返回null，如果创建失败 则返回一个错误对象

```js
// 3.创建文件夹 mkdir()
// 如果文件夹已经存在或者没有创建文件夹的权限会创建失败
// 权限查看文件夹属性是否是只读
/* 
    第一个参数  文件夹名称
    第二个参数  回调函数
*/
fs.mkdir('demo', (err) => {
    // console.log(err);

    // 判断是否创建成功 依据返回值null假值
    if (null) {
        console.log('文件夹创建失败');
    } else {
        console.log('文件夹创建成功');
    }
})
```



##### **删除文件**：

fs.unlink(fileName, callback) 该方法用于删除文件

- fileName: 要删除的文件
- callback: 回调函数
    - 参数表示错误异常，如果删除成功 则返回null，如果删除失败 则返回一个错误对象

==删除操作第一个参数 不要轻易尝试删除其他路径的文件==

##### **删除文件夹**

fs.rmdir(dirName, callback) 该方法用于删除文件夹

注意： 该方法只能删除空文件夹

- dirName： 要删除的文件夹名称
- callback: 回调函数
    - 参数表示错误异常，如果删除成功 则返回null，如果删除失败 则返回一个错误对象

```js
// 4.删除文件
/* 
    第一个参数  要删除的文件/文件夹名称
    第二个惨    回调函数
*/
fs.unlink('test1.txt', (err) => {
    if (err) {
        console.log('删除失败');
    } else {
        console.log('删除成功');
    }
})

// 5.删除文件夹
fs.rmdir('demo', (err) => {
    if (err) {
        console.log('删除失败');
    } else {
        console.log('删除成功');
    }
})
```



##### **修改文件名称**

fs.rename(oldName, newName, callback) 该方法用于修改文件名称

- oldName： 被修改的文件名称
- newName: 被替换的文件名称
- callback: 回调函数

参数表示错误异常，如果修改成功 则返回null，如果修改失败 则返回一个错误对象

```js
// 6.重命名文件 rename()
fs.rename('test.txt', 'test1.txt', (err) => console.log('err'))
```



##### **读取文件内容**

fs.readFile(fileName, callback) 该方法用于读取文件

fileName: 要读取的文件名称

- callback: 回调函数，有两个参数：
    - 第一个参数：错误异常，如果创建成功 则返回null，如果创建失败 则返回一个错误对象
    - 第二个参数：读取成功时候的数据，默认是Buffer数据 我们可以调用`toString`方法转为字符串之后查看

```js
// 7.读取文件状态 readFile()
fs.readFile('test1.txt', (err, content) => {
    if (err) {
        console.log('文件内容读取失败');
    } else {
        console.log(content.toString());    // tostring() 方法会将二进制内容转换成正常的字符
    }
})
```



##### **判断文件的状态(*获取文件或文件夹的属性信息*)**

fs.stat(targetName, callback) 该方法用于判断文件的状态

- targetName： 要判断的文件名称
- callback: 回调函数，有两个参数：
    - 第一个参数是错误异常
    - 第二个 是状态对象

==可以通过状态对象调用`isDirectory`，如果为真 则表示是文件夹，如果为假 则表示文件==

```js
// 8.判断文件的状态(获取文件或文件夹的属性信息) stat()
/* 
    第一个参数  判断的文件名称
    第二个参数  回调函数
        第一个参数  错误异常
        第二个参数  状态对象
    可以通过状态对象调用 isDirectory() 为真则是文件夹 为假则是文件
*/
fs.stat('test1.txt', (err, info) => {
    console.log(err, info.isDirectory(), info);
})
```



##### **读取文件夹**

fs.readdir(dirName, callback) 该方法用于读取文件夹

- dirName: 读取的文件夹的名称
- callback: 回调函数，有两个参数
    - 第一个参数表示错误异常
    - 第一个参数是一个数组，数组中的每一项都是读取到的每一个文件

```js
// 9.读取文件夹(会将文件夹和文件作为一个数组返回)   readdir()
/* 
    第一个参数  读取文件夹的名称
    第二个参数  回调函数
        第一个参数  错误异常
        第二个参数  是一个数组，数组中的每一项是读取到的每一个文件
*/
fs.readdir('demo', (err, list) => console.log(err, list))
```



##### FS 的同步方法

注意：在fs模块中，方法后面加上Sync就是同步的方法

```js
// 10.FS的同步方法
// 异步方法
/* fs.appendFile('a.txt', '测试', (err, content) => console.log(err, content)) */
// 同步方法
let content;
// 所有的同步方法都需要使用try...catch 处理异常情况
try {
    // content = fs.appendFile('b.txt')
    content = fs.appendFile('a.txt', '测试', (err) => console.log(err))
} catch (err) {
    content = '失败'
}
```

### 3.6 url模块

> 该模块的作用可以实现将url字符串与url对象互相转换。使用的时候需要引入url模块
>

- `parse`：该方法可以将url字符串解析为url对象

    使用方式：url.parse(url_str, bool)

    - url_str: url字符串
    - bool: 是一个布尔值，默认是false, 当传递true的时候，会将url对象中的query部分变为对象

- `format`：该方法用于实现将url对象再次解析为url字符串

```js
// 引入模块
const url = require('url')

const str = "https://www.abc.com:8080/a/b/123.html?user=admin&pwd=123321#p1";

// 1.parse()    将url字符解析为url对象
/* 
    第一个参数  url字符串
    第二个参数  布尔值
        默认false
        true    会将url对象中的query部分转换为对象
*/
/* let obj = url.parse(str, true)
console.log(obj); 
    Url {
  protocol: 'https:',
  slashes: true,
  auth: null,
  host: 'www.abc.com:8080',
  port: '8080',
  hostname: 'www.abc.com',
  hash: '#p1',
  search: '?user=admin&pwd=123321',
  query: [Object: null prototype] { user: 'admin', pwd: '123321' },
  pathname: '/a/b/123.html',
  path: '/a/b/123.html?user=admin&pwd=123321',
  href: 'https://www.abc.com:8080/a/b/123.html?user=admin&pwd=123321#p1'
}
*/

var urlObj = {
    protocol: "http:",              
    hostname: "www.test.com",       
    port: 3000,                     
    // search: "page=1",
    search: "?page=1",
    hash: "top",
    pathname: "/abc/index.html"
};

// 2.format()   将url对象转换为url字符串
var urlStr = url.format(urlObj)
console.log(urlStr);    // http://www.test.com:3000/abc/index.html?page=1#top

// https://www.icketang.com:443/static/img/student2.jpg?color=red&num=100#demo
// 完整的URL组成部分：
// 协议     https://
// 域名     www.icketang.com
// 端口号   :443        http:80, https:443
// 路径     /static/img/
// 文件名   student2.jpg
// 搜索词   ?color=red&num=100
// 哈希     #demo
```

### 3.7 path

该模块的作用用于处理请求的路径的。使用的时候，需要引入path模块

##### 将路径字符串转为对象

使用方式：`path.parse(path_str)`

```js
// 1.引入模块
const path = require('path')

const url = "http://www.test.com:3000/abc/index.html?page=1#top";

// windows风格的路径地址，单\需要改写成双\
const filepath1 = 'D:\\课程记录\\54期\\1227\\code\\上午\\1，fs模块\\abc\\1.html';

// linux / unix 风格的路径不需要改写
const filepath2 = 'D:/课程记录/54期/1227/code/上午/1，fs模块/abc/1.html';

// 2.parse()  将路径字符串转换为对象
// console.log(path.parse(url));
/* 
{
    root: '',
    dir: 'http://www.test.com:3000/abc',
    base: 'index.html?page=1#top',
    ext: '.html?page=1#top',
    name: 'index'
}
*/

// windows风格的路径地址
// console.log(path.parse(filepath1));
/* 
{
root: 'D:\\',
dir: 'D:\\课程记录\\54期\\1227\\code\\上午\\1，fs模块\\abc',
base: '1.html',
ext: '.html',
name: '1'
}
*/

// linux / unix 风格的路径
// console.log(path.parse(filepath2));
/* 
{
root: 'D:/',
dir: 'D:/课程记录/54期/1227/code/上午/1，fs模块/abc',
base: '1.html',
ext: '.html',
name: '1'
}
*/
```



##### 该方法用于拼接路径

使用方式：`path.join(path1, path2 ...)`

- path1 第一个路径
- path2 第二个路径

可以传递多个路径，返回值就是拼接后的路径。

```js
// 3.join() 将多个路径拼接成一个完整路径
/* var str = path.join('d:/', 'a', 'b', 'index.html')
console.log(str);   // d:\a\b\index.html */
```



##### 获取路径中的最后一部分(可以获取到路径中的文件名)

`basename` 

```js
// 4.basename() 获取路径中的最后一部分(可以获取到路径中的文件名)
const filepath3 = '/a/b/c/index.html'

// 4.1 只传入一个参数   不去除文件扩展名
// console.log(path.basename(filepath3));  // index.html
// 4.2 传入第二个参数 去除文件扩展名
// console.log(path.basename(filepath3, '.html')); // index
```



##### 获取路径中的扩展名部分

`extname`	

```js
// 5.extname() 获取路径中的扩展名部分
const filepath4 = '/a/b/c/index.html'
console.log(path.extname(filepath4));   // .html
```