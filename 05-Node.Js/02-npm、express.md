## 一、静态服务器

### 1.1 静态服务器

通过从浏览器发送请求到服务器，请求的资源类型有两种：

- 静态资源

     例如： html、css、js、图片等... 这些称为静态资源

-  [动态资源]()

     例如: 实现登录、注册等发送的请求，这些称为动态资源

### 1.2 MIME TYPES

Mime Types： 是互联网中每一种资源类型的定义

 例如：

>  ‘html’: ‘text/html’,
>
>  ‘css’: ‘text/css’,
>
>  ‘js’: ‘application/x-javascript’
>
>  ‘jpg’: ‘image/jpg’,
>
>  ‘jpeg’: ‘image/jpeg’

```js
// 1.引入模块
const http = require('http')
const fs = require('fs')
const path = require('path')

// 2.文件类型  也成为mime类型 浏览器通过服务器返回的 mime 值信息确定收到的是什么资源
// 用来表示不同类型文件 content-type 值的对象
const MIME_TYPES = {
    'html': 'text/html; charset=utf-8',
    'xml': 'text/xml',
    'txt': 'text/plain; charset=utf-8',
    'css': 'text/css',
    'js': 'text/javascript',
    'json': 'application/json',
    'pdf': 'application/pdf',
    'swf': 'application/x-shockwave-flash',
    'svg': 'image/svg+xml',
    'tiff': 'image/tiff',
    'png': 'image/png',
    'gif': 'image/gif',
    'ico': 'image/x-icon',
    'jpg': 'image/jpeg',
    'jpeg': 'image/jpeg',
    'wav': 'audio/x-wav',
    'wma': 'audio/x-ms-wma',
    'wmv': 'video/x-ms-wmv',
    'woff2': 'application/font-woff2'
}

// 3.创建一个 http 服务
const server = http.createServer((req, res) => {
    // 4. write() 向前端浏览器页面写入数据，write方法会一直保持写入状态，浏览器短的表现效果为一直加载状态
    // res.write("<h1>hello</h1>")
    // 4.1 如果需要多次写入 重复调用 write() 方法
    // res.write("<h1>hello</h1>")

    // 4.2 end() 向前端浏览器页面写入内容并结束写入状态
    // 只允许调用一次
    // res.end()

    // 5.获取前端的请求地址
    var url = req.url
    // 将地址解析为对象
    var urlObj = path.parse(url)
    // 从地址对象中获取文件名
    // 如果没有文件名就使用默认的index
    var name = urlObj.name || "index"
    // 从地址对象中获取文件的扩展名(扩展名会附带?和#部分的信息)
    // 如果没有扩展名就使用默认的 .html
    var ext = urlObj.ext || ".html"
    // 获取扩展名后面的?的位置
    var index = ext.search(/\?/)
    // 去除扩展名问号后面的内容只保留扩展名
    ext = index === -1 ? ext : ext.slice(0, index)
    // 将文件和扩展名拼接得到完整的文件名
    var filename = name + ext

    // 读取
    let content
    try {
        content = fs.readFileSync(filename)
    } catch (error) {
        content = "<h1>读取失败</h1>"
    }

    // 利用扩展名从 MIME_TYPES 对象获取到对象的mime类型
    res.setHeader("Content-Type", MIME_TYPES[ext.slice(1)]);

    // 向前端返回数据
    res.end(content)
})
server.listen(4000, () => console.log("服务器启动成功"))
```

## 二、nodejs 中变量

`__dirname`: 指向是当前文件所在的目录

`__filename`: 指向当前文件所在的完整的绝对路径

`process` : 执行当前文件时，终端所在的路径

注意： esModule 模式中没有 `__dirname` 和` __filename` 变量

```js
// 当前文件所在的路径
console.log(__dirname);
// 当前文件所在的绝对路径
console.log(__filename);
// 执行当前文件时，终端所在的路径
console.log(process.cwd());
```

## 三、NPM

### 3.1 npm 介绍

> NPM： Node Package Manager （node的第三方包管理器）
>
> 官网：https://www.npmjs.com/
>

在nodejs中的文件分为三类：

​	1 核心模块 这类文件可以直接引入

​	2 第三方模块文件 这类模块要安装之后才能使用

​	3 文件模块 我们写的js文件就是一个模块

​		node_modules文件夹：

​		该文件夹中用于存储所有的第三方文件，当我们需要引入内部文件的时候，就可以像引入内置模块那样直接引入了

### 3.2 npm 命令

npm install ModuleName

​	该指令可以实现将ModuleName 下载到本地

​	注意： install 可以简写为i

​	npm install ModuleName1 ModuleName2

​	该指令可以实现将ModuleName1以及ModuleName2共同下载到本地

- 注1：在下载模块的时候，如果在同级目录中存在node_modules文件，则直接下载到该文件中，如果当前层级没有node_modules文件夹，则往上级或者是上上级目录开始查找，如果找到了，直接下载到文件中，如果直到根目录下还没有找到node_modules文件夹，则返回到指令执行的目录，创建一个node_modules文件夹并下载


- 注2：如果在当前目录下，存在一个package.json文件的话，则直接创建node_modules文件并下载


- 注3：

- 在下载模块文件的时候，如果使用指令 npm install ModuleName --save

    此时会下载package.json文件的dependencies配置依赖的模块，表示“运行的时候的模块”

- 在下载模块文件的时候，如果使用指令 npm install ModuleName --save-dev

    此时会下载package.json文件的Devdependencies配置依赖的模块，表示“开发的时候的模块”

    

    当我们需要去下载别人的项目的时候，可以npm install 即可将dependencies以及Devdependencies相关的文件全部下载到本地

    如果模块存在指令文件，需要向全局安装：npm install ModuleName -g

```markdown
## npm命令
### 1.安装指定安装包
​```
# 全局安装
npm install -g typescript

# 项目环境安装(当前文件夹)
npm install typescript

# 简写
`install` 可以简写为 `i`

# 安装多个 空格隔开连续书写即可 
npm install -g typescript nodemon ...
​```
### 2.卸载指定安装包
​```
# 卸载全局安装包
npm uninstall -g typescript

# 卸载当前项目的安装包
npm uninstall typescript

# 简写
`uninstall` 可以简写为 `un`

# 卸载多个 空格隔开连续书写即可 
npm uninstall -g typescript nodemon ...
​```
### 3.查看已安装的安装包
​```
# 查看全局
npm list -g

# 查看当项目环境
npm list
​```
```



### 3.3 package.json

每一个项目的根目录中，都有一个package.json文件，用于定义了这个项目所需要的各种模块,以及项目的配置信息生成 package.json

- 指定 npm 的安装目录

    - 执行`npm`命令安装插件时，默认会查找 `node_modules` 文件夹和 `package.json`配置文件，如果当前目录中既没有`node_modules`，也没有 `package.json`，会自动往上一层目录中查找，直到找到 `node_modules`文件夹或`package.json`文件，如果当前文件夹中有 `node_modules`或`package.json`，则不会往上层查找，直接在当前目录中安装插件。

- 记录安装包信息

    - 使用 `npm`命令安装项目插件时（即不使用`-g`参数安装），默认会将安装包信息记录到 `package.json` 配置文件中，当项目迁移或分享给其它人时，不需要分享`node_modules`文件夹（因为该文件夹通常价积较大，打包或复制的速度很慢），只需要在新电脑中执行 `npm i`命令，会自动读取 `package.json`文件中记录的安装包信息来安装插件。

- 配置 `node` 模块以哪种规范加载

    - `node` 模块默认使用 `commonjs` 规范语法加载，如果希望使用 es module 规范加载，可以在 `package.json`配置文件中增加配置：

    - \```json

        "type": "module"

        \```

通过 npm init 生成 package.json 文件（生成过程中会有提问输入，全部保持默认，直接回车即可）

### 3.4   包管理配置文件

​	在项目根目录中，创建一个叫做package.json的配置文件，即可用来记录项目中安装了哪些包。从而方便别除node modules目录之后，在团队成员之间共享项目的源代码.
​			注意：今后在项目开发中，一定要把node modules文件夹，添加到.gitignore忽略文件中。

- 快速创建package.json

  npm包管理工具提供了一个快捷命令，可以在执行命令时所处的目录中，快速创建package.json这个包管理
  		配置文件：
  		1.作用：
  			在执行命令所处的目录中，快速新键package.json文件
  		2 npm init -y
  			注意：
  			①上述命令只能在英文的目录下成功运行！所以，项目文件夹的名称一定要使用英文命名，不要使用中文，不能出现空格。

- dependencies节点

  package.json文件中，有一个dependencies节点，专门用来记录您使用npm install 命令安装了哪些包.

- 一次性安装所有的包

  可以运行npm install命令（或npm i)一次性安装所有的依赖包：

  ![image-20230123143857675](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123143857675.png)

- devDependencies节点

  如果某些包只在项目开发阶段会用到，在项目上线之后不会用到，则建议把这些包记录到devDependencies节点中，
  与之对应的，如果某些包在开发和项目上线之后都需要用到，则建议把这些包记录到dependencies节点中。
  您可以使用如下的命令，将包记录到devDependencies节点中：

  ![image-20230123144928225](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123144928225.png)



### 3.5 解决下包速度慢的问题

#### 1.为什么下包速度慢

在使用npm下包的时候，默认从国外的https://registry.npmis..orgl服务器进行下载，此时，网络数据的传输需要经
过漫长的海底光缆，因此下包速度会很慢。

#### 2.淘宝NPM镜像服务器

淘宝在国内搭建了一个服务器，专门把国外官方服务器上的包同步到国内的服务器，然后在国内提供下包的服务.从而极大的提高了下包的速度
		扩展：
			镜像(Mirroring)是一种文件存储形式，一个磁盘上的
			数据在另一个磁盘上存在一个完全相同的副本即为镜像

![image-20230123145857605](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123145857605.png)

#### 3.切换npm的下包镜像源

下包的镜像源，指的就是下包的服务器地址，

![image-20230123145935812](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123145935812.png)

#### 4.nrm

为了更方便的切换下包的镜像源，我们可以安装m这个小工具，利用nm提供的终端命令，可以快速查看和切换下
包的镜像源

![image-20230123150312048](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123150312048.png)

### 3.6 包的分类

#### 1.项目包

那些被安装到项目的node modules目录中的包，都是项目包.

- 项目包又分为两类，分别是：
  - 开发依赖包（被记录到devDependencies节点中的包，只在开发明间会用到
  - 核心依赖包（被记录到dependencies节点中的包，在开发期间和项目上线之后都会用到）
  - ![image-20230123150542043](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123150542043.png)

#### 2. 全局包

在执行 npm install 命令时，如果提供了 -g 参数，则会把包安装为全局包。
		全局包会被安装到 C:\Users\用户目录\AppData\Roaming\npm\node_modules 目录下。

![image-20230123151021663](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123151021663.png)

注意：
			只有工具性质的包，才有全局安装的必要性。因为它们提供了好用的终端命令。
			判断某个包是否需要全局安装后才能使用，可以参考官方提供的使用说明即可。



#### 3. i5ting_toc

i5ting_toc 是一个可以把 md 文档转为 html 页面的小工具，使用步骤如下：

![image-20230123151109987](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123151109987.png)



### 3.7  规范的包结构

在清楚了包的概念、以及如何下载和使用包之后，接下来，我们深入了解一下包的内部结构。

​		一个规范的包，它的组成结构，必须符合以下 3 点要求：
​			包必须以单独的目录而存在
​			包的顶级目录下要必须包含 package.json 这个包管理配置文件
​			package.json 中必须包含 name，version，main 这三个属性，分别代表包的名字、版本号、包的入口。

注意：以上 3 点要求是一个规范的包结构必须遵守的格式，关于更多的约束，可以参考如下网址：
https://yarnpkg.com/zh-Hans/docs/package-json



### 3.8  开发属于自己的包

#### 	1.需要实现的功能

  格式化日期
		 转义 HTML 中的特殊字符
		 还原 HTML 中的特殊字符

![image-20230123151256098](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123151256098.png)

![image-20230123151314453](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123151314453.png)

![image-20230123151322241](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123151322241.png)

#### 2. 初始化包的基本结构

 新建 itheima-tools 文件夹，作为包的根目录
		在 itheima-tools 文件夹中，新建如下三个文件：
			package.json （包管理配置文件）
			index.js          （包的入口文件）
			README.md  （包的说明文档）



#### 3. 初始化 package.json

![image-20230123151427363](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123151427363.png)

关于更多 license 许可协议相关的内容，可参考 https://www.jianshu.com/p/86251523e898



#### 4. 在 index.js 中定义格式化时间的方法

![image-20230123151447242](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123151447242.png)



#### 5. 在 index.js 中定义转义 HTML 的方法

![image-20230123151500253](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123151500253.png)



#### 6.  在 index.js 中定义还原 HTML 的方法

![image-20230123151518661](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123151518661.png)



#### 7. 将不同的功能进行模块化拆分

 将格式化时间的功能，拆分到 src -> dateFormat.js 中
		将处理 HTML 字符串的功能，拆分到 src -> htmlEscape.js 中
		在 index.js 中，导入两个模块，得到需要向外共享的方法
		在 index.js 中，使用 module.exports 把对应的方法共享出去



#### 8. 编写包的说明文档

 包根目录中的 README.md 文件，是包的使用说明文档。通过它，我们可以事先把包的使用说明，以 markdown 的格式写出来，方便用户参考。
		README 文件中具体写什么内容，没有强制性的要求；只要能够清晰地把包的作用、用法、注意事项等描述清楚即可。
		我们所创建的这个包的 README.md 文档中，会包含以下 6 项内容：
		安装方式、导入方式、格式化时间、转义 HTML 中的特殊字符、还原 HTML 中的特殊字符、开源协议



### 3.9  发布包

#### 1.注册 npm 账号

 访问 https://www.npmjs.com/ 网站，点击 sign up 按钮，进入注册用户界面
		填写账号相关的信息：Full Name、Public Email、Username、Password
		点击 Create an Account 按钮，注册账号
		登录邮箱，点击验证链接，进行账号的验证

#### 2. 登录 npm 账号

npm 账号注册完成后，可以在终端中执行 npm login 命令，依次输入用户名、密码、邮箱后，即可登录成功。

![image-20230123151735649](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123151735649.png)

注意：在运行 npm login 命令之前，必须先把下包的服务器地址切换为 npm 的官方服务器。否则会导致发布包失败！

#### 3. 把包发布到 npm 上

将终端切换到包的根目录之后，运行 npm publish 命令，即可将包发布到 npm 上（注意：包名不能雷同）。

![image-20230123151753669](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123151753669.png)

#### 4. 删除已发布的包

运行 npm unpublish 包名 --force 命令，即可从 npm 删除已发布的包。

![image-20230123151812059](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230123151812059.png)

注意：
		npm unpublish 命令只能删除 72 小时以内发布的包
		npm unpublish 删除的包，在 24 小时内不允许重复发布
		发布包的时候要慎重，尽量不要往 npm 上发布没有意义的包！

### 3.10 模块的加载机制

#### 4.1 优先从缓存中加载

模块在第一次加载后会被缓存。 这也意味着多次调用 require() 不会导致模块的代码被执行多次。
			注意：不论是内置模块、用户自定义模块、还是第三方模块，它们都会优先从缓存中加载，从而提高模块的加载效率。



#### 4.2 内置模块的加载机制

内置模块是由 Node.js 官方提供的模块，内置模块的加载优先级最高。
				例如，require('fs') 始终返回内置的 fs 模块，即使在 node_modules 目录下有名字相同的包也叫做 fs。



#### 4.3 自定义模块的加载机制

使用 require() 加载自定义模块时，必须指定以 ./ 或 ../ 开头的路径标识符。在加载自定义模块时，如果没有指定 ./ 或 ../ 这样的路径标识符，则 node 会把它当作内置模块或第三方模块进行加载。

同时，在使用 require() 导入自定义模块时，如果省略了文件的扩展名，则 Node.js 会按顺序分别尝试加载以下的文件：
	按照确切的文件名进行加载
	补全 .js 扩展名进行加载
	补全 .json 扩展名进行加载
	补全 .node 扩展名进行加载
	加载失败，终端报错



#### 4.4 第三方模块的加载机制

如果传递给 require() 的模块标识符不是一个内置模块，也没有以 ‘./’ 或 ‘../’ 开头，则 Node.js 会从当前模块的父目录开始，尝试从 /node_modules 文件夹中加载第三方模块。
如果没有找到对应的第三方模块，则移动到再上一层父目录中，进行加载，直到文件系统的根目录。
例如，假设在 'C:\Users\itheima\project\foo.js' 文件里调用了 require('tools')，则 Node.js 会按以下顺序查找：
	 C:\Users\itheima\project\node_modules\tools
	 C:\Users\itheima\node_modules\tools
	 C:\Users\node_modules\tools
	 C:\node_modules\tools



#### 4.5 目录作为模块

当把目录作为模块标识符，传递给 require() 进行加载的时候，有三种加载方式：
			在被加载的目录下查找一个叫做 package.json 的文件，并寻找 main 属性，作为 require() 加载的入口
			如果目录里没有 package.json 文件，或者 main 入口不存在或无法解析，则 Node.js 将会试图加载目录下的 index.js 文件。
			如果以上两步都失败了，则 Node.js 会在终端打印错误消息，报告模块的缺失：Error: Cannot find module 'xxx'



## 四、Express

### 4.1 Express

Express是基于Nodejs平台的一个快速、开放、极简的web应用框架

可以帮助我们快速的搭建后台服务器，快速的处理get请求、post请求......

官网：http://www.expressjs.com.cn/

下载：npm install express

### 4.2 搭建服务器

引入express

```markup
     let express = require('express');复制代码复制失败，请稍候重试...代码已复制
```

- 创建应用程序

 let app = express();

- 监听端口号

 app.listen(3000);

- 当我们需要访问某一个目录的时候，此时就要对该目录进行静态化

    我们可以使用Express一个中间件 -- `static` 方法实现目录的静态化

    中间件：处理请求的方法， 使用中间件用use方法。

```js
// 引入模块
const express = require('express')

//  创建应用程序
const app = express()

// 资源静态化
// use给应用程序添加中间件（功能）  ()路径地址
app.use(express.static('./web'))

// 监听端口号
app.listen(3000, () => console.log('express启动中....'))
```



### 4.3 处理 GET 请求

在Express中处理请求的方式有两种： 

​	1 通过app.get 

​	2 通过Router路由对象

- 第一种：使用方式：app.get(path, callback)

    path: 请求的路径接口

    callback: 回调函数，有三个参数：

    ​	第一个参数： req 请求对象

    第二个参数： res 响应对象

    第三个参数： next放行函数 在Express中允许对一个接口，添加多个处理函数

```markup
在接口函数中的第三个参数就是next放行函数

该函数不执行，后面的回调函数就不会执行。
```

获取`query`数据：可以通过`req.query`直接获取上串的数据

**req.query是Express对请求对象封装后的**

```js
// 全局插件 监控文件变化 nodemon
// 引入模块
const express = require('express')

//  创建应用程序
const app = express()

// 资源静态化
// use给应用程序添加中间件（功能）  ()路径地址
app.use(express.static('./web'))

// 登录页面路由地址
// get() 只能处理 get 请求
// get() 请求 相关信息会暴露在地址栏不安全
app.get("/login", (req, res) => {
    // req.query 保存所有通过 get 请求提交的数据
    console.log(req.query);

    // 请求头信息
    res.setHeader("Content-type", "text/html; charset=utf-8");
    
    // get 方式提交的数据并不是保存在 body 属性中
    let data = req.query, msg = ''

    // 判断用户名密码
    if (data.user === 'admin' && data.passwd === '123456') {
        msg = "登录成功";
    } else {
        msg = "用户名或密码不正确！";
    }

    res.end(msg)
})


// 监听端口号
app.listen(5000, () => console.log('express启动中....'))

```



### 4.4 处理 POST 请求

在Express中处理post请求,同get请求使用方式一致

​	`app.post(path, callback)`

- 获取post请求的数据

    ​	如果想要获取post请求传递的数据，则需要借助中间件body-parse之后还要进行配置

    ​	app.use(bodyParser.urlencoded({ extended: false }));

    ​	然后我们就可以通过req.body来获取浏览器端提交的数据

- post 请求必须通过该中间件处理，才能获取到提交的数据

    ​	`app.use(express.urlencoded({extended:true}));`

```js
// 全局插件 监控文件变化 nodemon
// 引入模块
const express = require('express')

//  创建应用程序
const app = express()

// 资源静态化
// use给应用程序添加中间件（功能）  ()路径地址
app.use(express.static('./web'))

// 启用 post 请求扩展
// post 请求必须通过该中间接处理 才能获取到提交的数据
app.use(express.urlencoded({ extended: true }))

// 登录页面路由地址
app.post("/login", (req, res) => {
    // req.body ：保存所有通过post 请求的数据
    console.log(req.body);

    res.setHeader("Content-type", "text/html;charset=utf-8")

    let data = req.body, msg = ''

    if (data.user === 'admin' && data.passwd === '123456') {
        msg = '登录成功'
    } else {
        msg = '用户名或密码不正确'
    }

    res.end(msg)
})


// 监听端口号
app.listen(4000, () => console.log('express启动中....'))

```

### 4.5 路由对象

在Express中也可以通过路由对象处理各种请求

使用方式：

1通过Express获取路由模块，

2 创建路由实例化对象，并配置路由

3 安装路由对象

创建路由对象 let router = express.Router();

```js
// 引入express
let express = require('express');

// 创建应用程序
let app = express();

// 静态化
// use给应用程序添加中间件（功能）
app.use(express.static('./web/'))
// 拓展功能	post 请求必须通过该中间接处理 才能获取到提交的数据
app.use(express.urlencoded({ extended: true }))

// 1 创建路由对象
let router =  express.Router();
// 2 通过路由对象，定义接口，代替app对象
router.post('/login', (req, res) => {

     res.json({ errno: 0, msg: 'post success' })
})

router.get('/test', (req, res) => {

     res.json({ errno: 0, msg: 'get success' })
})
    
// 安装路由中间件
app.use(router);
// 引入文件
let router = require('./router')

// console.log(router);
router(app);

// 监听端口号
app.listen(3000, () => console.log('http server listen at 3000'));

```



### 4.6 Cookie

安装cookie插件

```js
	$ npm i cookie-parser
```

cookie是HTTP协议请求头中的一个字段

用于验证用户是否登录

由服务器设置，由浏览器保存

登录原理

> 当用户通过AJAX或者是表单填写数据完毕之后，浏览器会发送一个HTTP请求到服务器，服务器接收响应并按照HTTP协议里面的规定，给响应头中的set-cookie字段设置，之后返回给前端，前端就按照HTTP协议里面的规定开始解析，检测到set-cookie字段之后，生成cookie文件夹，之后向同一个服务器发送任何请求的时候，都会携带cookie字段，服务器就可以验证用户是否登录

- **设置cookie**

使用方式：`res.cookie(key, value, options)`

 key: 数据名称

 value: 设置的数据

 options: 配置项

- **获取cookie**

想要获取cookie中的内容 必须借助`cookie-parser`

通过req.cookies对象获取，之后要进行设置

浏览器端获去cookie数据，通过document.cookie属性获取

- 数据安全性低
- ==有效期过期时间是世界时间，需要加八小时为中国时间==

```js
// 引入express
let express = require('express');
let cookie = require('cookie-parser')

// 创建应用程序
let app = express();

// 静态化
// use给应用程序添加中间件（功能）
app.use(express.static('./web/'))
// 接收请求体数据
app.use(express.urlencoded({ extended: true }))
// 使用cookie
app.use(cookie())

// 接口
app.post('/login', (req, res) => {
    console.log(req.body);
    // 校验数据是否合法，合法就存储
    res.cookie('username', req.body.username, {
        // 有效期过期时间
        maxAge: 1000 * 60 * 60 * 24
    })
    res.cookie('password', req.body.password, {
        maxAge: 1000 * 60 * 60 * 24
    })
    res.json({ errno: 0, msg: 'success' })
})

// 服务器端校验cookie接口
app.get('/check', (req, res) => {
    console.log(req.cookies);
    res.json({ msg: 'success' })
})

// 监听端口号
app.listen(3000, () => console.log('http server listen at 3000'));
```



### 4.7 session

- 安装session插件

```js
$ npm i express-session
```

- 服务器重启数据丢失

在Express中可以通过req.session用于设置以及获取session

当想要获取session中的内容的时候，需要借助中间件 express-session

我们也要进行配置：

```markup
     app.use(expressSession({ 
     	secret: 配置密钥
           resave: 每一次访问session的时候，是否重置
	   saveUninitialized: 在初始化的时候是否设置session
 }))
```

```js
// 引入express
let express = require('express');
let cookie = require('cookie-parser')
let session = require('express-session');

// 创建应用程序
let app = express();

// 静态化
// use给应用程序添加中间件（功能）
app.use(express.static('./web/'))
// 接收请求体数据
app.use(express.urlencoded({ extended: true }))
// 使用cookie
app.use(cookie())
app.use(session({
    secret: 'aichuangleyu',
    resave: true,
    saveUninitialized: true
}))

/**
 * cookie
 *  是放到请求头上的，因此大小收到限制（4kb）
 *  每次发送请求都携带，前后端都可以获取，都可以修改
 *  cookie是以明文存储的，因此不够安全，不能存储重要的信息
 *  可以持久存储，并且可以设置时间限制
 * session
 *  是放到服务器端的内存中存储，因此不要存储大量的数据，否则会消耗内存空间
 *  session通常用来存储一些敏感的，重要的数据
 *  在服务器端存储，因此前端无法直接获取，只能在服务器端操作
 *  session也是借助cookie实现的，是一个会话级别的，因此关闭浏览器就不存在的，刷新的时候存在
 * 
 * cookie的优势是在浏览器端存储，持久存储，session的优势是安全
 * **/ 


// 接口
app.post('/login', (req, res) => {
    console.log(req.body);
    // 校验数据是否合法，合法就存储
    res.cookie('username', req.body.username, {
        // 有效期
        maxAge: 1000 * 60 * 60 * 24
    })
    // 将密码等重要。敏感的信息放到session中
    req.session.password = req.body.password
    res.json({ errno: 0, msg: 'success' })
})

// 服务器端校验cookie接口
app.get('/check', (req, res) => {
    console.log('cookie', req.cookies);
    console.log('session', req.session);
    res.json({ msg: 'success' })
})

// 监听端口号
app.listen(3000, () => console.log('http server listen at 3000'));
```

### 4.8 token

> 含义：凭证、令牌 生成：由后端生成 存储：存储在前端的cookie中或者本地存储中
>
> 格式：头部，数据，签名 作用：验证用户身份
>
> 流程机制：客户端使用用户名跟密码发送登录请求。服务端收到请求，去验证用户名与密码
>
> 验证成功后，服务端会生成一个 Token字符串，再把这个 Token字符串发送给客户端，服务器端不会保留该Token字符串。
>
> 客户端收到 Token字符串，可以把它存储起来，比如放在 Cookie 里或者 Local Storage 里。客户端每次向服务端请求资源的时候需要带着服务端签发的 Token字符串
>
> 服务端收到请求，然后去验证客户端请求里面带着的 Token，如果验证成功，响应本次请求，如果验证失败则服务器可以拒绝

- **token 特点：**

服务器无状态：因为服务器只负责解密而不负责存储

把所有状态信息都附加在 Token 上，服务器就可以不保存。但是服务端仍然需要认证 Token 有效。

只要服务端能确认是自己签发的 Token，而且其信息未被改动过，那就可以认为 Token 有效。“签名”就是做这个的。

Token 是在服务端产生的，如果前端使用用户名/密码向服务端请求认证，服务端认证成功，那么在服务端会返回 Token 给前端。

前端可以在每次请求的时候带上 Token 证明自己的合法地位。如果这个 Token 在服务端持久化（比如存入数据库），那它就是一个永久的身份令牌。

- **JWT标准：**

因为这个过程的验证并不是HTTP协议中规定的方式，而是自定义的。所以很可能一个公司就有一个公司的使用方式。（程序员们从一个公司到了另一个公司，还要再学习另一套使用方式）。所以，就出现了标准。

标准的 Token 有三个部分：

 header（头部）

 payload（数据）：iss：Issuer，发行者，sub：Subject，主题，aud：Audience，观众，exp：Expiration time，过期时间，nbf：Not before，iat：Issued at，发行时间，jti：JWT ID

 signature（签名）：header，payload，secret

- **使用步骤**：

1 引入jwt(jsonwebtoken)模块

2 定义指定加密字符串

3 当用户登录成功之后 通过jwt提供了sign方法。可以将用户的信息以及加密字符串捆绑到一起生成token字符串

4 将用户的信息返回给前端。前端可以将token字符串保存在本地存储中

5 当前端再次发送请求的时候，将token字符串携带到服务器中

6 经过jwt提供的verify方法进行解密。之后返回给前端

```js
// 引入express
let express = require('express');
let jwt = require('jsonwebtoken')
let cookie = require('cookie-parser')

// 创建应用程序
let app = express();

// 用于加密的字符串
const key = 'aichuangleyu';

// 静态化
// use给应用程序添加中间件（功能）
app.use(express.static('./web/'))
// 接收请求体数据
app.use(express.urlencoded({ extended: true }))
// 使用cookie
app.use(cookie());

// 接口
app.post('/login', (req, res) => {
    // console.log(req.body);
    // 生成token
    jwt.sign(req.body, key, (err, data) => {
        // console.log(err, data);
        // 方案二，通过cookie存储
        res.cookie('token', data, {
            maxAge: 1000 * 60 * 60 * 24
        })
        // 方案一。直接返回数据，前端接收，存储到本地存储
        // res.json({ errno: 0, msg: 'success', data })
        res.json({ errno: 0, msg: 'success' })
    })
})

// 服务器端校验cookie接口
app.get('/check', (req, res) => {
    // console.log(req.cookies.token, 111);
    // 通过token解密
    jwt.verify(req.cookies.token, key, (err, data) => {
        console.log(err, 222, data);
    })
    res.json({ msg: 'success' })
})

// 监听端口号
app.listen(3000, () => console.log('http server listen at 3000'));


/**
 * cookie
 *  是放到请求头上的，因此大小收到限制（4kb）
 *  每次发送请求都携带，前后端都可以获取，都可以修改
 *  cookie是以明文存储的，因此不够安全，不能存储重要的信息
 *  可以持久存储，并且可以设置时间限制
 * session
 *  是放到服务器端的内存中存储，因此不要存储大量的数据，否则会消耗内存空间
 *  session通常用来存储一些敏感的，重要的数据
 *  在服务器端存储，因此前端无法直接获取，只能在服务器端操作
 *  session也是借助cookie实现的，是一个会话级别的，因此关闭浏览器就不存在的，刷新的时候存在
 * 
 * cookie的优势是在浏览器端存储，持久存储，session的优势是安全
 * 
 * token
 *  可以放在cookie中存储(还可以设置有效期)，也可以放在localStorage|sessionStorage中存储
 *  不占用服务器端内存空间，
 *  加密形式存在，更安全
 *  只能在服务器端解密，浏览器端无法解密
 *  每次发送请求都要携带token
 * **/ 
```