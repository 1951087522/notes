## 一、模板引擎

### 1.1 EJS

EJS是后台服务器模板，天生可以与Express搭配使用，无需引入，但是需要下载：`npm install ejs`

可以通过res.render方法渲染一个模板，在该页面中提供了<%=%>插值语法

在<%=%>是真正的js环境，因此可以表达式。使用步骤：

1 下载ejs

```js
npm i ejs
```



2 创建一个views文件夹

3 在views文件中创建以.ejs后缀名称的文件

```js
    // 启用 ejs 的 .html 后缀模板支持 （默认只支持 .ejs 后缀）
    app.engine(".html", ejs.__express);
```



4 可以通过`res.render(path, data)`渲染一个模板

5 在<%=%>书写要被替换的内容

​	注意单引号双引号嵌套

​	= 不会解析标签

​    - 解析标签



- 更改插值符号 ejs.delimiter = '$';



```js
// 处理cookie的中间件
const cookie = require('cookie-parser');
const jwt = require('jsonwebtoken');

// 引入ejs模块
const ejs = require('ejs');
const key = "hellojsonwebtoken";


module.exports = function (app, express) {

    const router = express.Router();

    app.use(express.urlencoded({ extended: true }));

    // 通过 use 使用 cookie 中间件
    app.use(cookie());

    // 启用 ejs 的 .html 后缀模板支持 （默认只支持 .ejs 后缀）
    app.engine(".html", ejs.__express);

    router.get("/", (req, res) => {

        const { token } = req.cookies;

        // token : 从cookie中获取的经过jwt加密的字符串
        jwt.verify(token, key, (err, data) => {
            // res.end( tpl("首页 - 前端开发", "hello world", data && data.user) );

            // console.log(data && data.user, 'aaaa');

            /* res.render("../views/index.ejs", {
                title: "首页 - Hello, eJs!",
                content: "使用ejs模板",
                username: data ? data.user : ''
            }); */

            res.render("../views/index.html", {
                title: "首页 - Hello, eJs!",
                content: "使用ejs模板",
                username: data ? data.user : ''
            });

        });

    });

    router.post("/login", (req, res) => {

        var data = req.body;

        res.setHeader("Content-type", "text/html; charset=utf-8");

        if (data && data.user === 'admin' && data.passwd === '123321abc') {

            jwt.sign(req.body, key, (err, token) => {

                res.cookie('token', token, {
                    // 过期时间： 以毫秒为单位的正整数
                    maxAge: 1000 * 3600 * 24 * 31
                })

                //  res.end( tpl("登录成功！", '<a href="/">返回首页</a>', data.user) );

                /* res.render("../views/index.ejs", {
                    title: "登录成功！",
                    content: "<a href='/'>返回首页</a>",
                    username: data.user
                }); */

                res.render("../views/index.html", {
                    title: "登录成功！",
                    content: "<a href='/'>返回首页</a>",
                    username: data.user
                });

            });

        } else {
            // res.end(tpl("登录失败！", '<a href="/form.html">返回重新登录</a>'));

            res.render("../views/index.html", {
                title: "登录失败！",
                content: "<a href='/form.html'>返回重新登录</a>",
                username: ''
            });
        }

    });

    // 退出登录
    router.get("/logout", (req, res) => {

        res.clearCookie("token");
        // res.end(tpl("退出成功！", "<a href='/'>返回首页</a>"));

        res.render("../views/index.html", {
            title: "退出成功！",
            content: "<a href='/'>返回首页</a>",
            username: ''
        });

    });

    return router;

};
```



## 二、文件上传

### 2.1 单文件上传

如果想要上传文件，我们必须借助formidable模块

该模块引入之后，除了可以处理普通的post请求之外的数据，还可以处理上传的图片文件等数据……

注意：此时，必须要给form表单设置enctype属性，才能够让formidable解析req对象

使用步骤：

1 浏览器端，修改enctype类型为multipart/form-data

2 服务器端，引入 formidable 模块并实例化

3 服务器端，修改文件存储位置

4 服务器端，解析请求对象，获取上传的文件信息

5 服务器端，通过 fs 模块，存储上传的文件。

### 2.2 多文件上传

如果想要选中多个文件，必须给input元素添加`multiple`属性

此时，就可以按下ctrl同时选中多个文件

但是如果还是使用单文件上传的方式接收文件就不行了

此时要再服务器端监听一个事件: file事件

该事件会在每一次上传的时候都会执行一次

```js
// 注意 formidable@3 以上的版本只支持 import 模式加载 ，不支持 require 方式加载
// 1.引入模块
const formidable = require('formidable')

module.exports = function (app, express) {
    const router = express.Router()

    router.post('/upload', (req, res) => {
        res.setHeader("Content-type", "text/html;charset=utf-8")

        // 实例化上传对象
        const form = formidable({
            // 2.1指定上传文件的存放目录 以app.js 问相对路径
            uploadDir: "./uploads",
            // 2.2 保留文件后缀
            keepExtensions: true,
            // 2.3 返回上传所有文件的结果
            multiples: true,
            // 2.4 自定义文件名
            filename(name, ext) {
                return name + '_' + +new Date() + ext
            }
        })

        // 处理上传结果
        form.parse(req, (err, fileds, { file: files }) => {
            var data;

            // 判断是多选结果， 还是最后一个文件
            if (files.length) {
                data = files.map(file => file.originalFilename);
            } else {
                data = files.originalFilename;
            }

            res.json({ msg: 'ok', data });

        });
    })
    return router;
}
```

## 三、MongoDB

### 3.1 mongodb

数据库：简单理解为存储数据的仓库。世界上有各种各样的数据库应用程序。大致可分为两类：

SQL数据库（关系型数据库）： mySql、oracle、sqlserver等

NoSql数据库（非关系型数据库）：Mongodb数据库等

SQL数据库：

结构化查询语言：structure query language

它的结构组成：应用程序 => 数据库 => 表 => 数据。使用SQL数据库要学习SQL语言。

NoSql数据库

它的结构组成：应用程序 => 数据库 => 集合 => 文档。

SQL与NOSQL的最大区别在于NoSQL的集合中的文档，它的结构可以不固定。

### 3.2 安装与配置 mongodb

官网：https://www.mongodb.com/

- 安装 mongdbshell

https://www.mongodb.com/products/shell

将 D:\package\mongosh-1.6.0-win32-x64\mongosh-1.6.0-win32-x64 文件替换 

![image-20221229122209731](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221229122209731.png)

C 盘下的 C:\Program Files\MongoDB\Server\6.0

![image-20221229122036613](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221229122036613.png)

- 配置环境变量

作用： 是为了扩大调用命令的范围

步骤：我的电脑点右键-属性-高级系统设置-环境变量-系统变量-path

将 `C:\Program Files\MongoDB\Server\[版本号]\bin` 目录粘贴到path路径的最下面。

最后点击确定按钮 我们就可以在任何盘符下使用命令

![image-20221229122552524](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221229122552524.png)

- 可以将 mongosh 更名为 mongo![image-20221229172028915](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221229172028915.png)



### 3.3 终端常用指令

- 连接数据库: mongo
- 查看数据库：show dbs



- 添加数据：
    - use 切换数据库	use mydb
    - 添加一条数据                db.表名称.insertOne({"name":"小红","age":18})      
    - 添加多条数据  [] 分割    db.表名称.insertMany([{"name":"小王","age":18},{"name":"小刚","age":12}])    
- 查看数据：                                 db.表名称.find({"name":"小明})
- 删除数据：
    - 删除单条：	                db.表名称.deleteOne({"name":"小明"})
    - 删除多条：                    db.表名称.deleteMany({"name":"小王"},{"age":22})
- 更新数据：（第一个值是查询条件，第二个值是更新的值  $set: ）
    - 更新单条：                     db.表名称.updateOne({"name":"小明"},{$set:{"sex":"男 "}})
    - 更新多条：                     db.表名称.updateMany({"name":"小明"},{$set:{"sex":"男 "}})
- 删除数据库：                               db.表名称.drop()

### 3.5 创建多个数据库 

- 开启和关闭服务

    - 关闭 net stop MongoDB
    - 开启 net start MongoDB

- 也可以通过 控制面板 开启关闭

    - 控制面板 -- 管理工具 -- 服务 -- MongooDB -- 服务状态 ： 启动 / 停止

    

- 默认的数据库目录是：`C:\Program Files\MongoDB\Server\[版本号]\db`

- 默认端口号： `27017`

创建指定的数据库:

1.在盘符下创建 mongoDB 文件夹

2.创建 数据库data 和 日志log 目录，建议将数据库和日志目录创建在同一个目录下

3.指定新的端口号	启动过程中窗口不要关闭

​	`mongod --dpath data --logpath log/mongo.log --port 端口号`

4.进入指定数据库

​	mongo --host 127.0.0.1 --port 端口号

​	一般情况使用默认数据库

### 3.6 NodeJS 中操作数据库

nodejs连接mongodb模块： mongodb

中文手册：https://docs.mongoing.com/mongodb-crud-operations/query-documents

```js
// 1.安装插件   mongodb
// 2.引入
// 解构
const { MongoClient } = require('mongodb')
// 引用通过id查找的插件
const { ObjectID } = require('bson');
// 3.数据库地址
const url = 'mongodb://127.0.0.1:27017'
// 4.数据库名字
const db = 'mydb'
// 5.数据库表名称
const col = 'one'

// 1.添加数据
async function insertOne(data) {
    // 链接数据库
    // MongoClient(url) 创建数据库链接  参数数据库地址
    // .connect() 链接

    const client = await new MongoClient(url).connect()
    // 插入一条数据到数据库
    // client.db(db) 选中数据库
    // collection(col) 选中表

    return client.db(db).collection(col).insertOne(data)
}
var result = insertOne({
    name: "李雷",
    age: 27
});
result.then(d => console.log(d));


// 2.添加多条数据
async function insertMany(datas) {
    // 链接数据库
    const client = await new MongoClient(url).connect()
    // 插入多条数据到数据库中
    return client.db(db).collection(col).insertMany(datas)
}
var result = insertMany([
    {
        name: '刘德华',
        age: 20
    },
    {
        name: '周润发',
        age: 21
    }
])
result.then(d => console.log(d))

// 3.删除一条数据
async function delOne(data) {
    const client = await new MongoClient(url).connect()
    return await client.db(db).collection(col).deleteOne(data)
}
var result = delOne(
    {
        'name': '刘德华',
        'age': 20
    }
)
result.then(d => console.log(d))

// 4.删除多条数据
async function delMany(datas) {
    const client = await new MongoClient(url).connect()
    return await client.db(db).collection(col).deleteMany(datas)
}
var result = delMany({
    'name': '刘德华',
    'age': 20
})
result.then(d => console.log(d))

// 5.更新一条数据
async function updateOne(query, update) {
    const client = await new MongoClient(url).connect()
    return await client.db(db).collection(col).updateOne(query, update)
}
var result = updateOne(
    { name: '周润发' },
    { $set: { sex: '男' } }
)
result.then(d => console.log(d))

// 6.更新多条数据
async function updateMany(query, updates) {
    const client = await new MongoClient(url).connect()
    return await client.db(db).collection(col).updateMany(query, updates)
}
var result = updateMany(
    { name: '周润发' },
    { $set: { sex: '男' } }
)
result.then(d => console.log(d))

// 7.查找单条数据
async function findOne(query) {
    const client = await new MongoClient(url).connect()
    return await client.db(db).collection(col).findOne(query)
}
var result = findOne({ name: '小明' })
result.then(d => console.log(d))

// 8.查找多条数据
async function findMany(query) {
    const client = await new MongoClient(url).connect();
    // toArray()
    return await client.db(db).collection(col).find(query).toArray();
}
var result = findMany({ name: '周润发' })
result.then(d => console.log(d))

// 9.通过 _id 查找
// 需要安装插件 npm i bson
// mongoDB 的 _id 属性的值是一个 ObjectId 类型的对象，是 mongodb专有的对象类型。

async function findMany(query) {
    const client = await new MongoClient(url).connect();
    return await client.db(db).collection(col).find(query).toArray();
}
var id = ObjectID("63ae6634e415df111217a999");
var result = findMany({ _id: id });
result.then(d => console.log(d)); 

// 10.清空集合
async function clearCollection(data) {
    const client = await new MongoClient(url).connect();
    return await client.db(db).collection(col).remove({})
}
var result = clearCollection()
result.then(d => console.log(d))
```



### 3.7 数据库导入导出

#### 3.7.1 导出数据

- Compass工具导出 
    - ![image-20221229203859651](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221229203859651.png)
    - ![image-20221229203921324](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221229203921324.png)
    - ![image-20221229204003847](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221229204003847.png![image-20221229204058131](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221229204058131.png)
- 终端导出
    - 配置环境变量
    - ![image-20221229204631797](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221229204631797.png)

```js
#完整命令
mongoexport“host 服务器地址 --port 端口 ---db 数据库 --collection 集合 --out文件名
#简写形试
mongoexport -h 服务器地址：端口 -d 数据库名称 -c 表名称 -o 导出文件路径名称
#如果是从默认数据库导出，可以省略服务器和端口号
mongoexport -d 数据库名称 -c 集合名称 -o 导出文件名称
```

#### 3.7.2 导入数据

- 工具导入
    - ![image-20221229205613412](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221229205613412.png)

```js
#完整命令
mongoimport --host服务器地址 --port 端口 --db数据库 --collection集合 --file文件名
#简写形式
mongoimport -h 服务器地址：端口 -d 数据库 -c 表名称 --file 文件路径名称
#如果是从默认数据库导出，可以省略服务器和端口号
mongoimpiort -d 数据库名称 -c 表名称 --file 文件路径名称
```





