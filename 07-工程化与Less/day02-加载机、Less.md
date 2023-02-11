# 工程化第2天

## 一、webpack

### 1.1 加载机

> webpack中一切资源都要模块化加载，css文件也是资源，所以也要模块化加载
>
> webpack仅仅内置了对js资源的模块化加载，并没有实现对css资源的模块化加载，所以我们要安装css资源加载机（器）
>

​	 在webpack与模块相关的配置，定义在module配置中，加载机是加载模块的，所以定义在module配置中，通过`rules`属性配置加载机规则：

- 属性值是数组，每一个成员代表一个加载机对象

  test 表示资源特征（是正则） 

  loader 引入加载机的

  css资源需要两个加载机：`style-loader`, `css-loader`

当引入多个加载机的时候，我们用`!`级联。使用加载机要本地安装，想传入配置可以使用use属性引入



### 1.2 **CSS** 加载机

```js
// 定义配置对象
const config = {
  // 配置环境
  mode: 'development',
  // 入口文件
  entry: {
    main: './modules/main.js'
  },
  // 出口配置
  output: {
    filename: './[name].js'
  },
  // 通过module 属性配置加载机
  module: {
    // 定义规则 属性值是数组
    rules: [
      // 每一个对象都表示一条加载机
      {
        // test 用于匹配正则
        test: /\.css$/,
        // loader 使用加载机 当引入多个加载机的时候用 ! 级联
        // loader: 'style-loader!css-loader'

        // 或者当需要其他配置的使用 可以使用 use
        use: ['style-loader', 'css-loader']
      }
    ]
  }
}

// 暴露
module.exports = config
```



### 1.3 图片加载机

在webpack看来图片文件也是资源，也可以模块化加载，我们要安装图片加载机，实现图片模块化加载

`url-loader` (依赖file-loader)。图片加载有两种方式

- 同步加载：将图片写入js文件中，通过html5中，图片的base64编码技术实现的


- 异步加载：存储图片的地址，在使用图片时候，在异步加载图片


- 图片到底采用哪种方式，我们可以通过传递`limit`参数定义，值表示图片大小,单位是字节(b)

  
  - 注意是？链接 不是 ！
  
  > 加载机通过`query`形式传递参数。例如 url-loader?limit=4096
  >
  > 当图片大小小于等于4kb的时候，同步加载。当图片大小大于4kb的时候，异步加载

```js
// 定义配置对象
const config = {
  // 配置环境
  mode: 'development',
  // 入口文件
  entry: {
    main: './modules/main.js'
  },
  // 出口配置
  output: {
    filename: './[name].js'
  },
  // 通过属性配置加载机
  module: {
    // 定义规则:
    rules: [
      // 配置图片的加载机
      {
        // test 匹配正则
        test: /\.(jpg|gif|png)$/,
        // 图片有同步/异步加载方式
        // 通过传递 limit 参数定义加载方式 值表示图片大小 单位字节
        // 当图片小于等于4kb 同步加载
        // 大于4kb 异步加载
        loader: 'url-loader?limit=4096'	// 注意是？链接 不是 ！
      }
    ]
  }
}

// 暴露接口
module.exports = config

// 异步加载的图片 后面跟default才是真正路径
// img2.src = url2.default
// 异步加载图片 服务器模式打开 webpack-dev-server
```



### 1.4 ES Module

ES Module是在ES6中提出的，但是被纳入了ES2016（ ES7 ）中。

```js
// 暴露接口(导出)
export let msg = 'hello EsModule'

export let num = 999

// 暴露默认接口
export default (num1, num2) => {
  return num1 + num2
}
```

- 引入模块：通过import引入

  ​		 import * as M from ‘’ 引入所有的接口，并存储在M命名空间下

  ```js
  // 导入接口
  // 1.导入所有接口
  import * as M from './common.js'
  console.log(M.msg, M.num, M.default(1, 2));
  ```

  ​		 import {} from ‘’ 引入某些接口，可以直接使用接口

  ```js
  // 2.解构某些接口
  import { msg, num } from './common.js'
  console.log(msg, num);
  ```

  ​		 import M from ‘’  引入默认接口

  ```js
  // 3.解构默认接口
  import M from './common.js'
  console.log(M(2, 3));
  ```

  ```js
  // 4.同时可以引入默认接口 和 某个接口
  import M, { msg } from './common'
  console.log(M(1, 2), msg);
  ```

  css资源可以直接通过`import`		引入 import ‘css地址’

  ```js
  // 5.对于样式文件可以直接引入 不要忘记加载机
  import './style.css';
  ```

  

- 暴露接口：通过export或者export default暴露

  ​		 export 暴露接口，可以被前两种引入接口的方式引入。

  ​		 export default 暴露默认接口，可以被第三种引入接口的方式引入。

  ```js
  // 暴露接口(导出)
  export let msg = 'hello EsModule'
  
  export let num = 999
  
  // 暴露默认接口
  export default (num1, num2) => {
    return num1 + num2
  }
  ```

  

编译ES6 | ES7也需要加载机：babel-loader 编译器 @babel/presets-es2015， @babel/presets-env

## 二、Less

### 2.1 less

> less一个CSS预编译语言
>
> ​	在我们开发中，使用css的时候往往会遇到一些问题，如无法复用样式，权重问题，无法计算，无法使用语句，没有方法，单位转换等等问题，所以才有了css预编译语言，是解决css开发中遇到的这些问题的

Github：http://github.com/less/less

官网：http://lesscss.org/

中文官网：http://lesscss.cn/

### 2.2 编译方式

#### 2.2.1 浏览器端编译

- 浏览器端编译less
    style 属性更改 type=text/less
    引入less.js文件
    不建议使用：
      	浏览器不能识别该语法
     	 需要将less语法编译称css语法
     	 引入文件造成耗时

```js
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style type="text/less">
    .box {
      .box1 {
        a {
          color: red;
        }
      }
    }
  </style>
</head>

<body>
  <!-- 
    浏览器端编译less
      style 属性更改 type=text/less
      引入less.js文件
      不建议使用：
        浏览器不能识别该语法
        需要将less语法编译称css语法
        引入文件造成耗时
   -->
  <script src="./less.js"></script>
  <div class="box">
    <div class="box1"><a href="">浏览器端编译less</a></div>
  </div>
</body>

</html>
```

#### 2.2.2 指令编译

- 安装 less

  - `npm install less -g`

  此时得到一个全局的lessc指令，可以通过lessc -v查看版本号

  浏览器不认识less语言，所以我们要将less编译成css

  执行指令 `lessc style.less ***.css `

```js
$ lessc style.less style.css
```

#### 2.2.3 工程化编译

webpack编译

​		 在webpack看来，less文件也是资源，因此也可以模块化加载。

​		 要安装less加载机 `less-loader`，less文件默认拓展名是.less

​				 匹配less文件：test: /\.less$/

​		 ==由于less最终编译成css，因此也需要css加载机==：`rules: ‘style-loader!css-loader!less-loader’`

```js
// 暴露接口
module.exports = {
  // 配置环境
  mode: 'development',
  // 入口文件
  entry: './modules/main.js',
  // 出口配置
  output: {
    filename: './[name].js'
  },
  // 加载机
  module: {
    rules: [
      // css加载机
      {
        test: /\.css$/,
        loader: 'style.loader!css-loader'
      },
      // less加载机
      {
        test: /\.less$/,
        loader: 'style-loader!css-loader!less-loader',
      }
    ]
  }
}
```

#### 2.2.4 插件编译

安装 Easy LESS 插件即可

![image-20230103174039972](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230103174039972.png)



### 2.3 语法

#### 2.4 嵌套与&

> 嵌套式语法
>
> ​	使用预编译语言最大的特点就是可以使用嵌套式语法，也就是说直接**在一个选择器中使用另一个选择器并书写样式**，编译的时候会在将内外选择器同时保留

- **外部选择器类似命名空间，确保选择器名称不会相互覆盖。**

- **嵌套的时候，我们可以通过&符号获取当前选择器**

```js
.box {
    .box2 {
        font-size: 48px;
        color: rebeccapurple;
        &:hover {
            background-color: red;
        }
    }
}

/* 
    嵌套式语法 在一个选择器嵌套其他的选择器
        外层的选择器类似于命名空间 保证标签不会想回覆盖
    & 符号表示当前选择器
*/
```



#### 2.5 变量

less支持变量，定义的语法跟js语法相似

​	 @key: value;

​	 @相当于var，是变量的标志

​			 key表示变量名称（符合js变量命名规范）

​			 :相当于赋值符号

​			 value表示值

​			 ;表示语句结束

```js
/* 
    通过@key:value 定义变量 (作用：复用)
*/

// 定义全局变量
@color: red;
@size: 24px;
@font-family: "楷体";

h2 {
    color: @color;
    font-size: @size;
    font-family: @font-family;
}

// 定义局部变量
.box {
    @bgc: hotpink;
    @with: 100px;
    @height: 100px;

    width: @with;
    height: @height;
    background-color: @bgc;
    font-family: @font-family;
    font-size: @size;
}
```



#### 2.6 运算

less支持数学运算，并且会做单位转换

​		 加减法：保留第一个加数或者被减数的单位

​		 乘法：保留第一个乘数的单位

​		 除法：保留第一个单位

```less
// less 支持数学运算

.box {
    background-color: red;
    // 1.加减法 保留第一个加数或减数的单位
    // width: 100px + 200px;
    // height: 100pt + 200px;

    // 2.乘法 保留第一个乘数的单位
    // width: 10px * 20px;
    // height: 10pt * 20px;

    // 3.除法 保留第一个单位 除法相对于css来说是行高
    // 必须加上括号转为表达式执行
    width: (10in / 1px);
    height: (10in / 1px);
}
```



#### 2.7 混合

混合的作用就是为了复用选择器的样式

在less中，支持两类混合：类混合和id混合

​		 定义混合：定义这些选择器

​		 使用混合：直接写这些选择器

在选择器内部使用混合的时候，想覆盖继承下来的样式一定要注意选择器的权重

less中的混合在编译的时候保留（变量会删除），混合继承的样式不会合并选择器

```less
// 混合分为 id混合 和 类混合
.module1 {
    .header {
        color: red;
    }
    .body {
        color: green;
    }
    .footer {
        color: blue;
    }
}

// id混合
#demo {
    background-color: hotpink;
}

// 使用混合
.module2 {
    .module1;
    // 想要覆盖继承下来的样式 直接覆盖即可
    // 覆盖不成功 可能是权重不够 后面加!important
    .header {
        #demo;
    }
}
```



#### 2.8 方法

> 方法跟混合一样，都是为了复用选择器的样式
>
> ​		 混合在编译的时候会保留
>
> ​		 方法在编译的时候会删除
>

如果想将复用的样式，在编译的时候删除，我们可以使用方法。

如果复用的样式是可变的，我们可以使用方法

定义方法的语法跟js中定义方法的语法类似

```js
.方法名称(@arg1, @arg2) {
    // 定义样式
}
```

​		 .相当于function用来定义方法

​		 方法名称跟js中方法命名规范一致（但less可以使用-）

​		 ()表示参数集合，可以定义参数

​		 @arg1, @arg2表示参数

​		 {}表示方法体，我们可以定义样式



- 使用方法：.方法名称(arg1, arg2);

  ​	默认参数：.方法名称(@arg: value, @arg2: value2) {}

  ​	定义默认参数的语法跟定义变量的语法类似

  ​		 如果定义了默认参数，使用方法的时候，可以不传递参数

  ​	 	如果没有定义默认参数，使用方法的时候，必须传递参数

- 获取所有参数：我们可以通过@arguments获取所有的参数

- !important：如果对方法使用!important关键字，此时方法中的每一个样式都会提高权重

```less
// 定义方法

// // 1.定义盒子大小的方法
// .size (@w,@h,@bg) {
//     width: @w;
//     height: @h;
//     background-color: @bg;
// }

// // 使用
// .box1 {
//     .size(100px ,200px,hotpink);
// }

// 2.默认参数
.size(@w:100px,@h:200px,@bg:hotpink) {
    width: @w;
    height: @h;
    background-color: @bg;
}

// 定义盒子居中的方法
.wcenter(@w:500px,@mt:0,@mb:@mt) {
    width: @w;
    margin: @mt auto @mb;
}

// 定义封装制作盒子阴影的方法
.box-shadow(@x:1px,@y:2px,@r:3px,@c:gold) {
    // 3.获取剩余参数
    box-shadow: @arguments;
    // 兼容各个浏览器前缀
    -webkit-box-shadow: @arguments;
    -moz-box-shadow: @arguments;
    -o-box-shadow: @arguments;
    -ms-box-shadow: @arguments;
}

.box3 {
    // 使用默认参数 当一个参数都不传递 可以省略参数集合
    // .size

    .size(100px,200px,red) !important;
    // 盒子居中
    .wcenter(10px ,20px);
}

// 设置盒子阴影
.box4 {
    .size;
    .box-shadow(10px);
}
```

#### 2.9 内置方法

字符串方法：e 避免less编译的

数学方法：js中数学对象支持的，less也支持

​	 ceil（向上取整），floor（向下取整），round（四舍五入），max，min，`percentage（求百分数）`

色彩方法

​	定义色彩方法：rgb， rgba， hsl(hue, saturation, lightness)

​		 hue 色相 0-360 saturation 饱和度 0-100% lightness 亮度 0-100%

​	色彩通道方法 red，green, blue, alpha, hue, saturate, lightness

​	色彩操作方法 fadeIn, fadeOut, fadeTo, saturate, desaturate, lighten, darken

 		最小值是0，最大值是100%；

​	色彩混合方法 softLight，hardLight

```less
.box1 {
    background-color: hotpink;
    // 1.数学对象中的方法less都支持
    width: ceil(4.5px); // 向上取整
    height: floor(4.6px); // 向下取整
    margin-top: round(4.7px); // 四舍五入
    // 百分比
    margin-bottom: percentage(0.2);

    // 2.色彩方法
    // 局部变量
    @color: hsla(300, 50%, 50%, 50%);

    // 色彩通道方法 red，green, blue, alpha, hue, lightness
    // color: red(@color);
    // color: green(@color);
    // color: blue(@color);
    // color: alpha(@color);
    // color: hue(@color);

    // 色彩操作方法 fadeIn, fadeOut, fadeTo, saturate, desaturate, lighten, darken
    // color: fadeIn(@color, 30%);

    // color: saturate(@color, 30%);

    // 色彩混合方法 softLight，hardLight
    @color2: #f30;
    @color3: #30f;
    color: softLight(@color2, @color3);
    color: hardLight(@color2, @color3);
}
```

#### 2.10 条件语句

less不支持if条件语句，但是我们可以通过方法来模拟：`.方法名称() when () {}`

 when关键字就是用来定义该方法的适用条件的

- 两个变形

  ​	 且条件 .方法名称() when () and () {}

  ​	 非条件 .方法名称() when not () {}

支持比较运算符（跟js一样，但是**‘等于’有变化**）：>, >=, <, <=, =

 	注意：less中的等于比较运算符用’=’，比较的时候，不需要带单位

在js中，满足了一个条件就不会执行else后面的了

在less中，所有的方法，只要条件满足，都会执行，并且后执行的会覆盖先执行的

```less
// 1.先定义方法
.size(@w: 100px, @h: @w) {
    width: @w;
    height: @h;
}

// 2.定义条件语句 且语句
.size (@w:100px,@h:@w) when (@w < 200px ) and (@h<200px) {
    background-color: red;
}

// 非语句
.size(@w:100px,@h:w) when not (@w<200px) {
    background-color: blue;
}

.box1 {
    .size(199px,199px);
}

.box2 {
    .size (201px ,201px);
}
```



#### 2.11 插值与引入文件

- 插值语法：插值语法 @{key}

  ​		 js中插值：是为了复用字符串，less中插值；为了复用样式

  ​		在less中，有三种情况下可以使用插值语法

  ​		 	1 在字符串中可以使用插值

  ​		 	2 在选择器上可以使用插值

  ​		 	3 在样式属性名称上（key）可以使用插值

```js
// less 中 有三种情况可以使用 插值语法

// 定义变量
@day: "星期二";
@num: 2;
@dir: bottom;

// 1.在字符串中可以使用插值
.box1::after {
    content: "是@{day}";
    width: 20px;
    height: 20px;
}

// 2.在选择器可以使用插值
.box@{num} {
    width: 200px;
    height: 200px;
    background-color: red;
}

// 3.在样式属性名称上 (key) 可以使用插值
.box3 {
    border-@{dir}: 10px solid #000;
}
```



- less引入文件，跟css引入文件相似，有两种方式

  ​		 1 css方式引入：@import url()  

  ​		 2 直接引入：@import 地址

```js
// 引入文件的方式1 css引入方式
// @import url("./common.less");

// 方式2 less引入方式
@import "./common.less";

.box1::after {
    content: "是@{day}";
    width: 20px;
    height: 20px;
}

.box@{num} {
    width: 200px;
    height: 200px;
    background-color: red;
}

.box3 {
    border-@{dir}: 10px solid #000;
}
```

