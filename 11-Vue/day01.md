# vue第1天

# 一、Vue

### 1.1 MVVM

MVVM模式包含三个部分：

​		 M  模型model

​		 V  视图 view

​		 VM  视图模型 view-model

特点：实现了数据双向绑定

​		 数据由模型进入视图，通过数据绑定实现的

​		 数据由视图进入模型，通过事件监听实现的

vue就是基于MVVM模式实现的。



### 1.2 MVVM模式的由来

 早期js被设计的很简单，主要解决一些简单的交互问题。主要的问题就是浏览器兼容性，所以jQuery就出现了，解决了兼容性问题，简化了开发。

 随着技术的发展，富客户端应用，单页面应用程序的出现，前端逻辑更加的复杂，因此如何维护好前端的代码，成了主要的问题。所以MVC一类的框架就出现

了，让我们将逻辑分成模型，视图和控制器，变得更容易开发和维护。

 但是在MVC模式中，开发项目非常的慢，因此MVVM模式就出现了，提供了数据双向绑定技术，极大的提高了开发效率。

Vue就是基于MVVM模式实现的。



### 1.3 获取 vue

官网： https://cn.vuejs.org/

GitHub：https://github.com/vuejs/vue

获取Vue

​	 在ES5开发中，我们要获取vue.js文件，可以通过bower工具

​			 bower install vue 

​			 我们可以通过npm来安装bower指令

​						 npm install -g bower

​	 在ES6开发中，我们要获取vue模块

​			 npm install vue



### 1.4 体验 vue

vue是基于MVVM模式实现的，因此也包含三个部分

​		 模型：js中的数据对象

​		 视图：页面中的模板视图

​		 视图模型：就是vue实例化对象

实例化vue类的时候，要传递参数对象

​		 通过`el`属性与页面中的模板绑定上

​		 通过`data`属性与模型数据绑定上。



### 1.5 vue 实例化对象

$el 代表视图中的容器元素。

模型中的数据会添加给实例化对象自身，并设置了特性。

并且在_data以及$data属性中，对模型数据做了备份。

当模型中的数据改变，视图会同步更新，这个过程就是vue实例化对象实现的（数据的绑定技术）。



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <!-- 2 模板视图 -->
    <div id="app">
        <!-- 实现数据双向绑定 -->
        <input type="text" v-model="msg">
        <h1>{{msg}}</h1>
    </div>
    <!-- 引入Vue -->
    <script src="./vue.js"></script>
    <script>
    // 关闭生产提示
    Vue.config.productionTip = false;


    // 1 定义模型数据
    let data = {
        msg: 'hello msg',
        num: 100
    }

    // 3 视图模型 
    let vm = new Vue({
        // el属性 绑定视图的
        // data属性 绑定模型数据的
        el: '#app',
        data
    })

    </script>
</body>
</html>
```



### 1.6 数据绑定的实现

 vue中，模型中的数据被设置了特性，因此ES5中属性的特性是vue数据绑定技术实现的关键。

 vue中数据的绑定是通过ES5中属性的特性实现的。

 因此不支持ES5中特性技术的浏览器，是不能使用vue的。

 vue是一个运行在高级浏览器下的框架。



```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <!-- 视图 -->
    <div id="app">
        <h1>{{msg}}</h1>
    </div>
    <script>
    // 模型
    let data = {
        msg: 'hello',
        // 备份对象
        _data: {}
    }

    // 通过设置特性的方式实现的
    Reflect.defineProperty(data, 'msg', {
        // 取值器方法
        get() {
            return data._data.msg;
        },
        // 赋值器方法
        set(val) {
            data._data.msg = val;
            // 更新视图
            updateView(this._data);
        }
    })

    // 获取模板
    let html = document.getElementById('app').innerHTML;
    // console.log(html);
    // 定义更新视图的方法
    function updateView(data) {
        // 如果将该条语句放入到内部 无法匹配插值语法 
        // let html = document.getElementById('app').innerHTML;
        // console.log(html);
        // 替换模板内容
        let tpl = html.replace(/{{(\w+)}}/g, (match, $1 = '') => {
            return data[$1];
        })

        // 更新视图 
        document.getElementById('app').innerHTML = tpl;
    }

    // 更新数据
    data.msg = 'hello';

    </script>
</body>
</html>
```



### 1.7 webpack编译

我们基于ES6语法开发，因此需要通过webpack将ES6编译成ES5或者ES3.1的版本。让更多的浏览器支持。

​		 在nodejs中，我们使用commonjs规范

​		 在ES6中，我们使用ES Module规范

ES6文件拓展名可以是.es,也可以是.js等。但是在工作中到底使用哪种方式，要看使用的编辑器。

![image-20230207152945233](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230207152945233.png)

解决  runtime-only  问题

resolve配置

​		 我们在`resolve`配置中，通过`alias`配置模块入口文件



​		 alias配置有两点作用：

​				1 可以更改入口文件。 

​				2 简化路径的书写

​					 在resolve通过`extensions`配置文件的默认拓展名

​				     是一个数组，每一个成员代表一个默认拓展名。



```js
// 使用commonjs规范，定义配置
module.exports = {
    // 开发环境
    mode: 'development',
    
    // 解决问题
    resolve: {
        // vue入口文件
        alias: {
            // 以vue结尾
            'vue$': 'vue/dist/vue.esm.js'
        },
        // 默认拓展名
        // 工作中，样式文件一般不会设置默认拓展名，是为了区分脚本文件和样式文件
        extensions: ['.js', '.css']
    },
    
    // 入口文件
    entry: {
        '00': './modules/00.js',
        '01': './modules/01.js',
        '02': './modules/02.js',
        '03': './modules/03.js',
        '04': './modules/04.js',
        '05': './modules/05.js',
        '06': './modules/06.js',
        '07': './modules/07.js',
        '08': './modules/08.js',
        '09': './modules/09.js',
        '10': './modules/10.js'
    },
    // 发布
    output: {
        filename: '[name].js'
    },
    // 模块
    module: {
        // 加载机
        rules: [
            // css
            {
                test: /\.css$/,
                loader: 'style-loader!css-loader'
            },
            // sass
            {
                test: /\.scss$/,
                loader: 'style-loader!css-loader!sass-loader'
            },
        ]
    }
}
```



### 1.8 数据丢失

vue实现了MVVM模式，因此实现了对数据的绑定（当我们修改模型中的数据的时候，视图会同步更新）

​		 当我们修改数据的时候，如果视图没有更新，那么我们就说数据丢失了。

注意：数据丢失不是什么好的特性，是框架的bug。

数据绑定的实现原理：基于ES5中属性的特性实现的， 因此设置了特性的数据是不会丢失的。

所以没有设置特性的数据就丢失了。工作中常见的数据丢失有四类



​		 第一类：数组中的值类型：用新数组替换原来的数组

​		 第二类：数组中的新成员：用新数组替换原来的数组

​		 第三类：对象中的新属性：用新对象替换原来的对象

​		 第四类：未初始化的：将数据初始化。



```js
// 导入Vue
import Vue from 'vue'

// 实例化
let vm = new Vue({
  // 绑定视图
  el: '#app',
  // 绑定数据
  data: {
    msg: 'hello msg',
    size: {
      width: 100,
      height: 200
    },
    colors: ['red', 'green', 'gold'],
    // 将数据初始化
    abc: 123
  }
})

// 修改数据
// vm.msg = 'hello Vue'

// 工作中常见的数据丢失有四类
// 1.数组中的值类型   用新数组替换原来的数组
// vm.colors[0] = 'pink'

vm.colors = ['pink', 'red', 'blue']

// 2.数组中的新成员   用新数组替换原来的数组
// vm.colors[3] = 'orange'

vm.colors = ['pink', 'red', 'blue', 'orange']

/* 
  Vue 将被侦听的数组的变更方法进行了包裹，所以它们也将会触发视图更新。这些被包裹过的方法包括：

  push()
  pop()
  shift()
  unshift()
  splice()
  sort()
  reverse()

*/
vm.colors.push('purple')


// 3.对象中的新属性   用新对象替换原来的对象
// vm.size.total = 300

vm.size = {
  width: 100,
  height: 200,
  total: 300
}

// 4.第四类 未初始化的数据  将数据初始化
vm.abc = 666
```



**`$set`**

为了解决数据丢失的问题，提供了一个更加保险的方法：$set.

​		 第一个参数表示数据对象

​				 可以是vue实例化对象，也可以是其它对象

​		 第二个参数表示属性名称

​		 第三个参数表示属性值



```js
/* 
为了解决数据丢失的问题，提供了一个更加保险的方法：$set.
    第一个参数表示数据对象
      可以是vue实例化对象，也可以是其它对象
    第二个参数表示属性名称
    第三个参数表示属性值
*/
vm.$set(vm.colors, 0, 'ko')
```



### 1.9 计算属性数据

我们定义在data中定义的模型数据是固定不变的，我们想在获取数据的时候，动态改变数据，可以使用计算属性数据技术。

​		 静态的数据定义在data属性中：定义的是什么数据，获取的就是什么数据

​		 计算属性数据定义在`comptued`属性中：==定义的是方法，获取的时候，会将执行的结果返回（是计算的）==

computed与data的用法是一样的，添加给vue实例化对象自身，并设置了特性，定义的时候都是一个对象

​		 key表示数据名称 

​		 value是一个函数

​		 	 参数和this都指向vue实例化对象，

​					因此通过参数或者this可以获取vue中的其它数据。

​			  必须有返回值，就是获取的数据。

==注意==：当多次使用计算属性数据的时候，该方法只会执行一次。只有当内部使用的数据发生改变的时候，计算数据数据的方法才会执行一次。

不论是data中定义的数据还是computed中定义的数据都会添加给vue实例化对象自身并设置特性。



```html
  <body>
    <div id="app">
      <input type="text" v-model="msg" />
      <h1>{{msg}}</h1>
      <!-- 使用计算属性数据 -->
      <h1>{{dealMsg}}</h1>
      <!-- 当多次使用计算属性数据 该方法只会执行一次只有当内部的值发送改变的时候 该方法才会执行一次  -->
      <h1>{{dealMsg}}</h1>
      <h1>{{dealMsg}}</h1>
    </div>
  </body>
```

```js
// 导入vue
import vue from 'vue'

// 实例化
let vm = new vue({
  // 绑定视图
  el: '#app',
  // 绑定数据
  data: {
    msg: 'hello msg',
  },
  // 定义计算属性数据
  computed: {
    // key 表示数据名称 value 是一个函数
    dealMsg(v) {
      // 返回值
      console.log(this, v);
      return this.msg.toUpperCase()
    }
  },
})
```



### 1.10 插值

为了在模板中，使用模型中的数据，我们可以使用插值语法：`{{}}`

插值语法提供了一个真正的js环境，我们可以直接书写js表达式。

==注意==：插值语法中的js表达式无法复用，想复用可以放在计算属性数据中（多定义监听器，性能）。

​		 如果逻辑复杂：建议计算属性数据

​		 如果逻辑简单：建议js表达式。



```html
  <body>      
<!-- 插值 -->
      <!-- 插值符号中是真正的js环境 因此可以书写表达式 -->
      <h2>{{msg.toUpperCase()}}</h2>
      <!-- 无法复用 想要实现相同效果只能重新书写 -->
      <h2>{{msg.toUpperCase()}}</h2>
      <!-- 
          逻辑复杂  建议使用计算属性数据
          逻辑简单  建议使用插值
      -->
    </div>
  </body>
```

```js
// 导入vue
import vue from 'vue'

// 实例化
let vm = new vue({
  // 绑定视图
  el: '#app',
  // 绑定数据
  data: {
    msg: 'hello msg',
  },
})
```



### 1.11 属性绑定

我们不能在元素的属性中使用插值语法。想动态设置属性，要使用属性绑定的技术。

​		 vue提供了`v-bind`指令，

​		 指令：指令就是对DOM元素的拓展，使其具有一定的行为特征（功能）。

​				 指令的属性值都是js环境，我们可以直接使用js表达式，

​		 属性绑定的语法：`v-bind:key=”value”`

​		 语法糖：语法糖就是对某个操作的简化，来提高我们的开发效率。

​				 v-bind指令的语法糖是冒号语法糖

​				 v-bind:key=”value” 可以简写成	 :key=”value”



```html
    <div id="app">
      <!-- 通过 v-bind  指令动态设置  指令的属性值都是js环境 -->
      <h1 v-bind:title="msg.toUpperCase()">{{msg}}</h1>

      <!-- v-bind指令的语法糖是冒号语法糖 因此
        v-bind:key=”value” 可以简写成	 :key=”value”-->
      <h2 :title="msg.toUpperCase()">{{msg}}</h2>
    </div>
```



### 1.12 插值指令

`v-text`：该指令用来设置元素的内容，

​		与插值语法相比：

​				1 避免插值符号闪烁。

​				2 只能渲染元素的全部内容，我们还可以用字符串拼接形式来解决这个问题。

​		注：v-text指令与插值语法一样，都不能渲染标签。



```html
      <!-- v-text 渲染元素的全部内容 不能渲染标签 -->
      <!-- 想要保留原内容 通过字符串拼接解决 -->
      <h1 v-text="'v-text:'+ a">v-text:</h1>
```



`v-html`：该指令用来渲染带有html标签的文本。

​		与插值符号相比：

​				1 避免插值符号闪烁。

​				2 只能渲染元素的全部内容，我们还可以用字符串拼接形式来解决这个问题。

​				3 可以渲染标签。

​		注：渲染的内容一定要确保是安全的。否则会导致xss注入的问题。



```html
      <!-- v-html 渲染带有html标签的文本 -->
      <h1 v-html="'v-html:'+ a">v-text:</h1>
```



`v-once`：该指令用来让内容单次渲染。

​		该指令不需要设置属性值。

​		该指令会让其所在的元素及其所有子元素上的所有插值与指令只执行一次。



```html
      <!-- v-once 该指令用来让内容单次渲染 不需要设置属性值-->
      <h1 v-once>{{a}}</h1>
```

```js
// 2s 之后改变数据
setTimeout(() => {
  vm.a = 'hello v-once'
}, 2000);
```



### 1.13 插值过滤器

我们想对插值语法中的数据进行修改，可以使用js表达式，但是如果逻辑很复杂，会导致模板很臃肿。

为了解决臃肿的问题，vue1提供了插值过滤器技术。

插值过滤器技术有三个优势：

​		 1 避免模板臃肿的问题。

​		 2 可以复用。

​		 3 可以跨组件使用。

在2.0中，作者弱化了过滤器，移除了内置的过滤器。

==注意==：在2.0中，作者建议用插值表达式以及计算属性数据的形式代替插值过滤器。



**自定义过滤器**

在vue中自定义过滤器有两种方式

​		第一种：全局定义：

​				 全局定义过滤器的时候，要注意：1 filter方法不能解构。2 要在vue实例化对象之前定义。

​			 	全局定义的过滤器可以在任何实例化对象（组件）中使用。

​				 通过`Vue.filter(name, fn)`

​		第二种：局部定义：

​				 局部定义的过滤器只能在当前实例化对象（组件）中使用。

​				 在vue实例化对象（组件）中，通过filters属性定义。

​				 `filters: { name(value) {} }`



​						 name表示过滤器名称：建议驼峰式命名

​						 fn(data, ...args)表示过滤函数：

​								data表示处理的数据。

​								args表示使用过滤器时候传递的参数。

​							    必须有返回值，就是处理的结果，

​		

​	**使用过滤器**

​	我们可以在插值语法以及指令中使用插值过滤器。

 	语法 `{{data | 过滤器(参数1, 参数2) | 过滤器2}}`

​			 前一个过滤器的输出将作为后一个过滤器的输入。



```js
import vue from 'vue'

// 全局过滤器 Vue.filter(name, fn) 放在实例化之前
// name表示过滤器名称
/* 
    第一个参数     永远是数据值
    从第二个参数开始 表示传递的数据值
*/
vue.filter('dealMsg', (value, ...args) => {
  return value.toUpperCase()
})

// 实例化
new vue({
  // 绑定视图
  el: '#app',
  // 绑定数据
  data: {
    msg: 'hello msg',
    title: 'hello title'
  },
  // 局部过滤器
  filters: {
    /* 
      key 表示名称
      value 函数
    */
    dealTitle(val, ...args) {
      // 返回值作为下一个过滤器参数使用
      return val.slice(2, 5)
    },
    uppercase(val) {
      return val.toUpperCase()
    }
  }
})
```



```html
    <!-- 定义视图 -->
    <div id="app">
      <!-- 使用过滤器	语法 {{data | 过滤器(参数1, 参数2) | 过滤器2}} -->
      <h1>{{msg | dealMsg(100,true,'abc')}}</h1>
      <h2>{{title | dealTitle(2,5) | uppercase}}</h2>
    </div>
```



### 1.14 数据双向绑定

vue实现了数据双向绑定，vue为了简化双向绑定，提供了`v-model`指令。

​		 属性值就是绑定的数据

​		注意： 

​				1 只能绑定数据，不能使用表达式 

​			    2 绑定的数据必须在模型中定义。如；data等。

​				3 v-model  只能给具有交互性的元素使用



​		数据双向绑定有两个方向：

 				数据由模型进入视图：通过数据绑定实现的。为value绑定模型数据

​				 数据由视图进入模型：通过事件监听实现的。监听input事件，更新数据

​	v-model指令简化了这两个过程，因此也可以看成是语法糖。



```html
  <body>
    <!-- 定义视图 -->
    <div id="app">
      <input type="text" v-model="msg" />
      <h1>{{msg}}</h1>
      <hr />

      <!-- 1. v-model 的属性不能使用表达式 -->
      <!-- <input type="text" v-model="msg.toUpperCase()" /> -->

      <!-- 2.v-model 是两个指令的语法糖 -->
      <input type="text" v-bind:value="msg" v-on:input="e => msg = e.target.value" />

      <h1>{{msg}}</h1>

      <!-- 3.v-model  只能给具有交互性的元素使用 -->
      <!-- <h2 v-model="msg">msg</h2> -->
    </div>

    <!-- 引入Vue.js -->
    <script src="./dist/09.js"></script>
  </body>
```



### 1.15 v-cloak

用来解决插值符号闪烁的问题。

只需要两步：

​		 第一步 为容器元素设置`v-cloak`指令（属性）

​		 第二步 在style标签内（style标签要定义在容器元素的前面），通过v-cloak属性选择器，设置display: none样式，将元素隐藏。

注意：

​		 1 style标签要定义在容器元素的前面

​		 2 v-cloak==只能给容器元素==内部的元素使用（包括容器元素）。



```html
    <style>
      /* 在style标签内（style标签要定义在容器元素的前面），通过v-cloak属性选择器，设置display: none样式，将元素隐藏。 */
      [v-cloak] {
        display: none;
      }
    </style>
  </head>
  <!-- v-cloak 不能管理除app容器元素之外的容器 -->
  <!-- <body v-cloak> -->
  <body>
    <!-- 定义视图 -->
    <div id="app" v-cloak>
      <h1>{{msg}}</h1>
      <!-- 1.定义 v-cloak 指令 -->
      <h1 v-cloak>{{msg}}</h1>
    </div>

    <!-- 引入Vue.js -->
    <script src="./dist/10.js"></script>
  </body>
```






