# Vue	第六天

##  一、Vue Router

### 1.1 URL 组成部分

路由就是根据一个地址找到对应的页面。

url组成部分：一个完整的URL由几部分组成？

https://www.icketang.com:443/static/img/banner_news.jpg?color=red&num=100#title

​	 协议 https://

​	 域名 www.icketang.com

​	 端口号 :443

​	 路径 /static/img/

​	 文件名 banner_news.jpg

​	 搜索词 ?color=red&num=100

​	 哈希 #title 

 一个url由七部分组成，前三个部分的改变会导致跨域文件。前六个部分的改变会导致浏览器端向服务器端发送新的请求。只有hash的改变不会向服务器端发送新的请求，因此前端路由就是基于hash实现的。



### 1.2 前端路由的实现

由于hash的改变不会向服务器端发送新的请求，因此我们可以监听hash的变化（通过`hashchange`事件），根据不同的hash渲染不同的页面（通过location.hash获取当前的hash值）。

​	 这种不向服务器端发送请求，而实现切换页面的功能，就是单页面应用程序（SPA：single page application）。

​			 单页面应用程序就是基于前端的hash路由实现的。特点就是：快。



```html
    <div id="app">
      <button @click="page = 'Home' ">Home</button>
      <button @click="page = 'List' ">List</button>
      <button @click="page = 'Detail' ">Detail</button>

      <!-- 动态路由 -->
      <components :is="page"></components>
    </div>
```



```js
import Vue from 'vue'

Vue.config.productionTip = false

let Home = Vue.extend({
  template: `
    <h1>Home</h1>
  `
})
let List = Vue.extend({
  template: `
    <h1>List</h1>
  `
})
let Detail = Vue.extend({
  template: `
    <h1>Detail</h1>
  `
})

let vm = new Vue({
  components: { Home, List, Detail },
  el: '#app',
  data: {
    page: 'Home'
  }
})

let router = () => {
  // 获取hash
  let page = window.location.hash.slice(1)

  // 修改
  vm.page = page
}

// 前端路由的实现是基于 hashchange 事件
window.addEventListener('hashchange', router)

// 自执行
router()
```



### 1.3 Vue 路由

vue为了让我们更方便的使用路由，提供了路由模块：`vue-router`

使用路由分成六步

​	 第一步 

​				引入路由模块 `import VueRouter from 'vue-router';`

​				安装路由：`Vue.use`方法安装

​	 第二步 创建组件对象：定义Vue.extend的参数对象（简化了对组件的定义）

​	 第三步 定义路由规则`routes`：是一个数组，每一个成员代表一条规则

​			 `name`：代表名称，

​			 `component`：代表渲染的组件，

​			 `path`：匹配规则（与express类似）

​					 静态路由：一个规则对应一个页面地址，

​							 如：/home/search，

 									匹配：/home/search，

​							 		不匹配：/home, /home/search/1, /list/search  

​					 动态路由：一个规则对应多个页面地址

​							 如：/list/:page

 							 		匹配：/list/1, /list/100, /list/demo， 

​							 		不匹配：/list, /list/1/2, /home/1

​	 第四步 实例化路由，`new Router({ routes })`。通过`routes`属性传递路由规则

​	 第五步 在vue实例化对象中，注册路由，通过`router`属性注册。

​	 第六步 在模板中，通过`router-view`组件，定义路由的渲染位置。



```html
    <div id="app">
      <h1>app part</h1>
      <hr />

      <!-- 6.在模板中 通过 router-view 组件定义渲染位置 -->
      <router-view></router-view>

    </div>
```



```js
import Vue from 'vue'

// 关闭生产提示
Vue.config.productionTip = false

// 1.引入路由模块
import Router from 'vue-router'
// 安装
Vue.use(Router)

// 2.创建页面组件 创建路由组件只需要定义Vue.extend 参数对象即可
let Home = {
  template: `
    <div>
      <h1>hello Home</h1>
    </div>
  `
}
let List = {
  template: `
    <div>
      <h1>hello List</h1>
    </div>
  `
}
let Detail = {
  template: `
    <div>
      <h1>hello Detail</h1>
    </div>
  `
}

// 3.创建路由规则
/* 
  是一个数组 每一个成员代表一条规则
  name 名称
  component 组件
  path  匹配规则
*/
let routes = [
  { path: '/', name: 'home', component: Home },
  { path: '/list/:page', name: 'list', component: List },
  { path: '/detail/:id', name: 'detail', component: Detail },
]

// 4.创建路由实例
let router = new Router({ routes })

new Vue({
  // 5.注册路由
  router,
  el: "#app",
})
```



### 1.4 路由数据

当我们在vue中注册了路由之后，每一个组件都会具有两个属性：`$route` `$router`

​	 $router 表示路由实例，包含一些切换路由的方法

​			 push 进入一个新页面

​			 replace  替换当前的页面

​			 back 返回上一个页面

​			 forward  进入下一个页面

​			 go  返回第一个页面



​	 $route 存储了路由相关数据

​			 路径 `path`，名称，`query`，动态路由数据（`params.xxx`）等等

​					 注意：在hash策略下，hash属性代表的是第二个#后面的内容，

 由于这些数据都设置了特性，因此既可以在模板中使用，也可以在js中使用。



```js
let List = {
  template: `
    <div>
      <h1>hello List </h1>
      <h2>展示路由数据 {{$route.path}} {{$route.query}} {{$route.params}} </h2>
    </div>
  `
}
```



### 1.5 props

我们在定义路由规则的时候，可以传递`props`属性，属性值有两种情况

​	 第一种：属性值是true

​			 会将==动态路由数据==传递给组件



​	 第二种：属性值是函数

​			 参数是`route`数据对象

​			 返回值表示给组件传递的数据



在组件中，通过`props`属性去接收这些数据（类似父组件向子组件通信）



```js
// 在组件中，通过props属性去接收这些数据（类似父组件向子组件通信）
let List = {
  // 通过props接收数据
  props: ['page', 'path', 'query'],
  template: `
    <div>
      <h1>hello List </h1>
      <h2>展示路由数据 {{$route.path}} {{$route.query}} {{$route.params}} </h2>
      <h2>简化路由数据:page:{{page}} {{path}} {{query}}</h2>
    </div>
  `
}

// 3.定义路由规则
let routes = [
  { path: '/', name: 'home', component: Home },
  {
    path: '/list/:page',
    name: 'list',
    component: List,
    // 第一种形式 属性值是 true 传递动态路由
    // props: true

    // 第二种 属性中是函数 参数是 route 数据对象 返回值表示给组件传递的数据
    props(route) {
      // console.log(arguments);
      return {
        path: route.path,
        query: route.query,
        page: route.params.page
      }
    }
  },
  { path: '/detail/:id', name: 'detail', component: Detail },
]
```



### 1.6 默认路由

我们让path匹配`*`。既可以定义默认路由

注意：由于*匹配的很广，因此通常定义在最后面。

默认路由：当前地址没有匹配的规则，就会渲染默认路由定义的组件。



```js
// 3.定义规则
let routes = [
  { path: '/list/:page', name: 'list', component: List },
  { path: '/detail/:id', name: 'detail', component: Detail },
  // 默认路由：当前地址没有匹配的规则 就会渲染默认路由定义的组件， 由于匹配范围广 放在最后
  { path: '*', name: 'home', component: Home }
]
```



### 1.7 路由重定向

我们通过path定义匹配的规则

我们通过`redirect`属性定义重定向的路径

当有与path匹配的路径就会重定向到新的路径。

bug：当重定向的时候，携带query，hash等数据，会导致路由对象解析错误。

==注意==：工作中，做路由重定向的时候，不要携带query等其它数据。



```js
// 3.定义规则
let routes = [
  // { path: '/list/:page', name: 'list', component: List, props: true },
  { path: '/list/:page', name: 'list', component: List },
  { path: '/detail/:id', name: 'detail', component: Detail },

  // 路由重定向 redirect  当有与path匹配的路径就会重定向到新的路径。
  { path: '/abc', redirect: '/list/1' },

  // 默认路由：当前地址没有匹配的规则 就会渲染默认路由定义的组件， 由于匹配范围广 放在最后
  { path: '*', name: 'home', component: Home }
]
```



### 1.8 子路由

子路由允许我们在页面的局部切换视图。

使用子路由分成两步

​		 第一步 在父路由模板中，通过`router-view`组件定义子路由渲染位置。

​		 第二步 在父路由规则中，通过`children`属性定义子路由规则。

​				 是一个数组，每一个成员代表一条规则：path, name, component, redirect, children ...

定义规则的要注意：

​		 如果是绝对路径：子路由的路径就是子路由的绝对路径（子）

​		 如果是相对路径：子路由的路径就是父路由路径+子路由的相对路径（父 + 子）



```js
import Vue from 'vue'
Vue.config.productionTip = false
// 1.引入路由模块
import Router from 'vue-router'
// 安装
Vue.use(Router)

// 2.创建路由模块组件
let Home = {
  template: `
    <div>
      <h1>hello Home</h1>
    </div>
  `
}
let List = {
  /* 
    路由数据  $route
      query
      path
      params
    
    props  在规则定义
      属性值为true 传递动态路由
      属性值为函数 参数为route 返回值为给组件传递的数据
    在组件props接收
  */
  props: ['page', 'path', 'query'],

  template: `
    <div>
      <h1>hello List</h1>
      <h2>路由数据 {{$route.query}} {{$route.path}}  {{$route.params}}</h2>
      <h3>简写 {{page}} {{query}} {{path}}</h3>
    </div>
  `
}
let Detail = {
  /* 
    子路由
      1.在父路由模板中 通过 router-view 组件定义子路由渲染的位置
      2.在父路由规则中 通过 children 属性定义子路由规则
  */
  template: `
    <div>
      <h1>hello Detail</h1>
      <router-view></router-view>
    </div>
  `
}
let Search = {
  template: `<h2>child Search</h2>`
}
// 3.定义规则
let routes = [
  // { path: '/list/:page', name: 'list', component: List, props: true },
  {
    path: '/list/:page', name: 'list', component: List,
    props(route) {
      return {
        path: route.path,
        query: route.query,
        page: route.params.page
      }
    }
  },
  {
    path: '/detail/:id', name: 'detail', component: Detail,
    children: [
      /* 
        定义规则时注意 
        */
      //  绝对路径  子路由的路径就是子路由的绝对路径
      { path: '/search', component: Search },
      //  相对路径  父路径 + 子路径 也可以写成：
      { path: 'demo', component: { template: '<h2>demo part</h2>' } }
    ]
  },

  // 路由重定向 redirect
  { path: '/abc', redirect: '/list/1' },

  // 默认路由：当前地址没有匹配的规则 就会渲染默认路由定义的组件， 由于匹配范围广 放在最后
  { path: '*', name: 'home', component: Home }
]

// 4.定义路由实例对象
let router = new Router({ routes })

new Vue({
  // 5.注册路由
  router,
  el: '#app',
})
```



### 1.9 路由策略

vue中默认使用的是hash策略（根据hash的变化，切换页面，实现SPA）

我们想使用path策略，可以通过==设置路由实例化==对象的`mode`属性实现

​		 mode: ‘history‘ 此时就是path(history模式)策略。**需要服务器端的配合**（重定向）。

​		 由于切换路径会向服务器端发送请求，因此这是一个多页面应用程序

​		 使用多页面应用，要配置服务器。



```html
    <div id="app">
      <router-view></router-view>
    </div>
    <!-- <script src="./dist/05.js"></script> -->
    <!-- 此时要改为绝对路径 -->
    <script src="/dist/05.js"></script>
```



```js
// 4.定义路由实例对象
let router = new Router({
  routes,
  // 通过 mode改变模式
  mode: 'history'
})
```



### 1.10 路由导航

为了方便我们切换路由，vue路由为我们提供了路由导航组件：`router-link`组件

​	 `tag`  定义渲染的标签（默认是a标签）

​			 渲染成a标签，通过a标签的href属性实现切换

​			 渲染成其它标签，通过js实现切换。

​	 `to`  定义目标地址，必须要定义的，

​			 即使是hash路由，也不要以#开始。

router-link组件与a标签相比，router-link组件会适配不同的策略



```html
  <body>
    <div id="app">
      <router-view></router-view>

      <!-- vue提供路由导航  会适应不同策略
        to 定义目标地址
        即使是hash策略 也不要#开头
        tag 定义渲染的标签  默认a标签
      -->
      <!-- tag 定义渲染的标签  默认a标签 -->
      <!-- <router-link tag="h1" to="/">home</router-link> -->

      <router-link to="/">home</router-link>
      <router-link to="/list/1">list</router-link>
      <router-link to="/detail/2">detail</router-link>
    </div>
    <script src="./dist/06.js"></script>
  </body>
```

```js
// 引入express
const express = require('express');
// 创建路由应用
const app = express()
// 配置引擎
app.engine('html', require('ejs').__express);
// 静态化
app.use('/dist', express.static('./dist'))
// 服务器端重定向
app.get('*', (req, res) => {
  console.log(req.url);
  res.render('../06-路由导航-路由过渡-监听滚动.html')
})
// 监听端口
app.listen(3000, () => { console.log(3000) })
```

```js
// 4.定义路由实例对象
let router = new Router({
  routes,
  // 通过 mode改变模式
  mode: 'history'
})
```



### 1.11 路由过渡

我们切换页面的时候，可以添加过度动画。

​	 在router-view组件的外面，定义`transition`组件，添加过渡动画。

​	    	 `name` 定义名称

​			 `mode`属性定义切换方式

​					out-in

​					in-out

​			 `appear`属性定义是否在加载的时候引入动画。



```css
.home,
.list,
.detail {
    height: 500px;
    width: 100%;
}

.home {
    // 渐变
    background-image: linear-gradient(#f30, #aaf);
}

.list {
    // 渐变
    background-image: linear-gradient(purple, yellowgreen);
}

.detail {
    // 渐变
    background-image: linear-gradient(green, pink);
}

// 显示态
.demo-enter-to,
.demo-leave {
    opacity: 1;
}

// 隐藏态
.demo-enter,
.demo-leave-to {
    opacity: 0;
}

// 显示过程
// 隐藏过程
.demo-enter-active,
.demo-leave-active {
    transition: all 0.5s;
}
```



```html
      <!-- 切换页面添加动画 transition
              name 定义名称
              mode  切换方式
              appear  加载时引入动画
      -->
      <transition name="demo" mode="out-in" appear>
        <router-view></router-view>
      </transition>
```



### 1.12 监听滚动

vue的路由允许我们在切换页面的时候，改变滚动条的位置。

我们在==路由实例化对象==中，通过`scrollBehavior`方法监听页面切换



​	 第一个参数表示当前路由对象

​	 第二个参数表示上一个路由对象

​	 第三个参数表示当前滚动条位置

​			 x表示横向滚动条位置

​			 y表示纵向滚动条位置



​	 返回值表示新的滚动条位置。



```js
// 4.定义路由实例对象
let router = new Router({
  routes,
  // 通过 mode改变模式
  // mode: 'history'

  // 监听页面滚动
  /*
    scrollBehavior()
      第一个参数 当前路由对象
      第二个参数  上一个路由对象
      第三个参数  当前滚动条位置
  */
  scrollBehavior(nowRouter, lodRouter) {
    // 判断路由名称
    if (nowRouter.name === 'list') {
      // 返回值就是改变滚动条位置结果
      return {
        y: 500
      }
    }
  }

})
```



### 1.13 路由守卫

路由守卫就是监听路由的切换（改变）。

在vue中有三种方式可以监听路由的改变：



​	 第一种 全局路由守卫

​			 可以监听所有的页面的切换

​			 通过对==路由实例化对象==定义`beforeEach`，`afterEch`等方法，来监听

​					 第一个参数表示当前路由对象

​					 第二个参数表示上一个路由对象

​					 如果是beforeEach，有第三个参数，表示`next`方法，必须要执行，否则看不到新的页面。



```js
// 通过路由实例化对象定义全局路由守卫
/* 
  第一个参数  当前路由对象
  第二个参数  上一个路由对象
  beforeEach第三个参数  next()  必须执行
*/

router.beforeEach((nowRouter, nextRouter, next) => {
  // 必须执行next方法 否则看不到页面
  next()
})

router.afterEach((nowRouter, nextRouter) => {
  console.log(nowRouter);
})
```



​	第二种 局部路由守卫

​			 可以监听当前页面的路由的切换。

​			 在==组件实例化对象==中，定义`beforeRouteEnter`，`beforeRouteLeave`，`beforeRouteUpdate`等方法监听。

​					 第一个参数表示当前路由对象，

​					 第二个参数表示上一个路由对象，

​					 第三个参数表示next方法，必须执行，



```js
let List = {
  template: `
    <div class="list">
      <h1>hello List</h1>
    </div>
  `,

  // 局部路由守卫
  /* 
    在组件实例化对象中 定义beforeRouteEnter beforeRouteLeave beforeRouteUpdate
      第一个参数 当前路由对象
      第二个参数 上一个路由对象
      第三个参数 next() 必须执行 否则看不到页面
  */
  beforeRouteEnter(nowRouter, oldRouter, next) {
    // console.log(arguments);
    next()
  },
}
```



​	 第三种 在`watch`监听器中，监听`$route`数据的改变。

​			 第一个参数表示当前的路由对象，

​			 第二个参数表示上一个路由对象

注意： 

​	 为了页面载入的时候可以被监听，可以配合局部路由守卫一起使用。

 	为了让页面消失的时候也可以监听路由，可以配置keep-alive组件使用。



```html
<transition name="demo" mode="out-in" appear>
    <keep-alive>
        <router-view></router-view>
    </keep-alive>
</transition>
```



```js
let List = {
  /* 
    路由数据  $route
      query
      path
      params
    
    props  在规则定义
      属性值为true 传递动态路由
      属性值为函数 参数为route 返回值为给组件传递的数据
    在组件props接收
  */
  props: ['page', 'path', 'query'],

  template: `
    <div class="list">
      <h1>hello List</h1>
      <h2>路由数据 {{$route.query}} {{$route.path}}  {{$route.params}}</h2>
      <h3>简写 {{page}} {{query}} {{path}}</h3>
    </div>
  `,

  // 局部路由守卫
  /* 
    在组件实例化对象中 定义beforeRouteEnter beforeRouteLeave beforeRouteUpdate
      第一个参数 当前路由对象
      第二个参数 上一个路由对象
      第三个参数 next() 必须执行 否则看不到页面
  */
  beforeRouteEnter(nowRouter, oldRouter, next) {
    // console.log(arguments);
    next()
  },
  beforeRouteLeave(nowRouter, oldRouter, next) {
    console.log('beforeRouteLeave');
    next()
  },

  // watch监听器中 监听 $route数据的改变
  /* 
    第一个参数  当前路由对象
    第二个参数  上一个路由对象
    为了页面消失的时候可以监听路由 配置keep-alive组件使用
  */
  watch: {
    $route() {
      console.log('route', arguments);
    }
  }

}
```



## 二、axios

### 2.1 使用 axios

vue为了开发方便，为我们提供了vue全家桶：vue, vuex, vue-router, vue-resource.

在ES5开发中，我们使用vue-resource发送异步请求，但是在ES6中，作者不再维护vue-resource，而是建议我们使用axios框架发送异步请求。

注意：axios不是vue家族的插件，因此不能用vue.use方法来安装。



axios的使用方式与jquery类似，提供了一些简便方法：

​	 `get(url, config`) 发送get请求

​			 url表示请求地址，

​			 config表示配置项（我们可以定义parmas，headers等）

​	 `post(url, data, config)`  发送post请求

​			 url表示请求地址，

​			 data：表示携带的数据，

​			 config：表示配置项（我们可以定义parmas，headers等）



==注意==：不论是get请求还是post请求，都可以携带query数据，query数据可以在两个位置添加

​	 1 在url上添加query数据。

​	 2 在config配置中的params属性中传递。



axios实现了promise规范，因此，通过`then`方法监听结果

​	 第一个参数表示成功时候的回调函数:参数表示请求对象，其中data属性表示返回的数据

​		 当多次使用then方法的时候，前一个then方法的返回值，将作为后一个then方法的参数。

​	 第二个参数表示失败时候的回调函数，也可以通过catch方法监听失败



```js
let Demo = {
  template: `<h1>hello Demo</h1>`,
  created() {
    // 发送请求
    /*
      get 请求两个位置可以携带query数据
        在URL上
        在params上
    */
    // axios.get('/getMsg?a=9', { params: { num: 199 } })
    //   // .then(res => res.data)
    //   // 上一个 then的返回值 作为下一个then的参数
    //   // .then(res => console.log(res))

    //   .then(({ data }) => console.log(data))

    axios.post('/postMsg?a=9', { color: 'red' }, {
      params: { num: 199 },
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
      }
    })
      .then(({ data }) => console.log(data))
  },
}
```



axios提交的数据，默认使用的是json格式。

​	 我们可以通过修改headers中的`Content-Type`字段，来模拟表单提交。

​	 模拟表单： application/x-www-form-urlencoded

```js
    axios.post('/postMsg?a=9', { color: 'red' }, {
      params: { num: 199 },
      headers: {
        'Content-Type': 'application/x-www-form-urlencoded'
      }
    })
```



- async

```js
  // 也可以使用async
  async created() {
    // 解构并重命名
    let { data: res } = await axios.get('/getMsg?a=9', { params: { num: 199 } })
    console.log(res);
  }
```



- app.js

```js
// 引入express
const express = require('express');
// 创建路由应用
const app = express()
// 配置引擎
app.engine('html', require('ejs').__express);
// 静态化
app.use('/dist', express.static('./dist'))

// 处理get请求
app.get('/getMsg', (req, res) => res.json({ errno: 0, data: 'get success' }))
// post
app.post('/postMsg', (req, res) => res.json({ errno: 0, data: 'post success' }))

// 服务器端重定向
app.get('*', (req, res) => {
  console.log(req.url);
  // res.render('../05-路由策略.html')
  // res.render('../06-路由导航-路由过渡-监听滚动.html')
  res.render('../08-axios.html')
})
// 监听端口
app.listen(3000, () => { console.log(3000) })
```





### 2.2 安装 axios

axios不是vue家族的插件，因此不能通过Vue.use方法来安装

所谓的安装就是让每一个组件可以获取，使用更方便。

​	 所有的组件都继承了Vue类，如果Vue的原型具有axios对象，那么**每一个组件（包括vue实例）都可以获取axios**了。

​	 所以对于非vue家族的插件我们可以==通过拓展其原型的方式来安装==。

​	 工作中，为了语义化，我们常常将其命名为`$http.`



```js
import Vue from 'vue'
Vue.config.productionTip = false
// 引入axios
import axios from 'axios'
// 为了让每一个组件都使用axios发送请求 可以放在原型中
// 为了语义化 命名为$http
Vue.prototype.$http = axios

let Demo = {
  template: `<h1>hello Demo</h1>`,

  // 也可以使用async
  async created() {
    // 使用安装之后的axios
    let { data: res } = await this.$http.get('/getMsg?a=9', { params: { color: 'red' } })
    console.log(res);
  }
}

new Vue({
  components: { Demo },
  el: '#app'
})
```



### 2.3 请求拦截器与响应拦截器

==注意==：通常利用axios.create方法复制一个Axios,设置拦截器,这样就不会==影响全局==的axios了



#### 请求拦截器

​		 `axios.interceptors.request.use(callback)`

​		 回调函数中，对请求做统一处理



```js
import Vue from 'vue'
Vue.config.productionTip = false
// 引入axios
import axios from 'axios'

// 注意：通常利用axios.create方法复制一个axios 避免污染全局axios
let newAxios = axios.create()

// 为了让每一个组件都使用axios发送请求 可以放在原型中
// 为了语义化 命名为$http
Vue.prototype.$http = newAxios

// 请求拦截
newAxios.interceptors.request.use(config => {
  // 为请求头设token字段
  config.headers.token = 'hello'
  // 必须有返回值
  return config
})

let Demo = {
  template: `<h1>hello Demo</h1>`,

  async created() {
    let { data } = await this.$http.get('/getMsg')
  }
}

new Vue({
  components: { Demo },
  el: '#app'
})
```



#### 响应拦截器

​		 `axios.interceptors.response.use (callback)`

​		 回调函数中，对响应做统一处理

​		 **请求的时候配置表单数据**



```js
import Vue from 'vue'
Vue.config.productionTip = false
// 引入axios
import axios from 'axios'

// 注意：通常利用axios.create方法复制一个axios 避免污染全局axios
let newAxios = axios.create()

// 为了让每一个组件都使用axios发送请求 可以放在原型中
// 为了语义化 命名为$http
Vue.prototype.$http = newAxios

// 请求拦截
newAxios.interceptors.request.use(config => {

  // 请求的时候配置表单数据
  if (config.url === '/postMsg') {
    // 模拟表单数据
    config.headers['content-type'] = 'application/x-www-form-urlencoded';
  }

  // 为请求头设token字段
  config.headers.token = 'hello'
  // 必须有返回值
  return config
})

// 响应拦截
newAxios.interceptors.response.use(data => {
  // 返回需要的数据
  return {
    data: data.data,
    status: data.status
  }
})

let Demo = {
  template: `<h1>hello Demo</h1>`,

  async created() {
    let data1 = await this.$http.post('/postMsg', { color: 'red' })
    console.log(data1);
  }
}

new Vue({
  components: { Demo },
  el: '#app'
})
```



-  app.js

```js
// 引入express
const express = require('express');

// 创建路由应用
const app = express();

// 配置引擎
app.engine('html', require('ejs').__express);

// 静态化
app.use('/dist', express.static('./dist/'));


// 处理get请求
app.get('/getMsg', (req, res) => res.json({ errno: 0, data: 'get success' }))
// 处理post请求
app.post('/postMsg', (req, res) => res.json({ errno: 0, data: 'post success' }))

// 服务器端做重定向
app.get('*', (req, res) => {
    // res.render('../04 路由策略.html');

    // res.render('../05 路由导航 监听滚动 过度.html');

    // res.render('../07 axios.html');

    res.render('../08 请求拦截和响应.html');
})

// 监听端口
app.listen(3000, () => console.log(3000));
```



```js
/* 
  vue3中的改变
    当前项目需要的模块  yarn add || npm i 
      vuex vue-router sass axios

    store
      引入vuex
      解构 {createStore}
      执行方式创建vuex

    vue-router
      引入vue-router
      解构数据 {createRouter,createWebHashHistory}
      createRouter({routes},history:createWebHashHistory())

    router-link
      <router-link custom v-slot="{navigate}">自定义标签</router-link>

    注册功能
      app.use(xxx)

*/
```

