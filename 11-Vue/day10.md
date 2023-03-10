# 					Vue	第十天

## 一、SSR

### 1.1 ssr

全称：server side render（服务器端渲染），让我们可以在服务器端渲染应用程序。



前端渲染问题：

​	 1.白屏时间长，影响用户体验。

​	 2.不利于搜索引擎优化（ SEO ）



所以我们要在服务器端渲染应用程序

服务器端渲染：vue为服务器端渲染提供了模块 -- `vue-server-renderer`

​	 该模块提供了`createRenderer`方法，来创建渲染器

​			 渲染器提供了`renderToString`方法，渲染应用程序组件

​			 渲染器实现了promise 规范，因此：可以通过then方法监听成功，可以通过cache方法监听失败



```js
// 引入express
const express = require('express')
// 获取应用
const app = express()

// 引入Vue
const Vue = require('vue')
// vue为服务器端渲染提供了模块 vue-server-renderer
// 该模块提供了 createRenderer() 渲染应用程序组件
const { createRenderer } = require('vue-server-renderer')

// 创建组件实例
let App = new Vue({
  template: `
    <div>
      <button @click="addNum">点我num++</button>
      <h1>app part {{num}}</h1>
    </div>
  `,
  data() {
    return {
      num: 0
    }
  },
  methods: {
    addNum() {
      this.num++
    }
  },
})

// 获取渲染器
// 渲染器提供了 renderToString() 渲染应用程序组件
let { renderToString } = createRenderer()

// 渲染器实现了Promise规范 可以通过then方法监听成功 catch方法监听失败
app.get('/', (req, res) => {
  renderToString(App)
    .then(html => console.log(html))
    .catch(err => console.log(err))
})

// 监听端口
app.listen(3000, () => console.log(3000))
```



### 1.2 渲染模板

渲染模板：想在模板中渲染应用程序分成两步：

​	 第一步 在`createRenderer`方法中，通过`template`引入模板文件

​	 第二步 在模板文件中,通过`<!--vue-ssr-outlet-->`定义应用程序渲染的位置。



​	 服务器端渲染问题（后端）：

​			 无法绑定交互



```js
// 引入express
const express = require('express');
// 引入应用程序实例
const Demo = require('./demo')
// 引入fs
const fs = require('fs')
// 引入vue
const Vue = require('vue');
// vue为服务器端渲染提供了模块 -- vue-server-renderer
// 该模块提供了 createRenderer 方法，来创建渲染器
const { createRenderer } = require('vue-server-renderer');

// 获取应用
const app = express();

// 获取渲染器
// 渲染器提供了 renderToString 方法，渲染应用程序组件
// 第一步 在createRenderer方法中，通过 template 引入模板文件
let { renderToString } = createRenderer({
  template: fs.readFileSync('./views/index.html', 'utf-8')
});

// 渲染器实现了promise规范，因此：可以通过then方法监听成功，可以通过cache方法监听失败
app.get('/', (req, res) => {
  // 服务器端渲染，无法绑定交互
  renderToString(Demo)
    .then(html => res.end(html))
    .catch(err => console.log(222, err))
})

// 监听端口
app.listen(3000, () => console.log(3000));
```



### 1.3 前后端同步渲染

前后端同步渲染

​	 浏览器端渲染（前端）：

​			 1 白屏时间长，影响用户体验，

​			 2 不利于SEO优化

​	 服务器端渲染（后端）：

​			 无法绑定交互



为了解决浏览器端与服务器端渲染的问题，我们要使用前后端同步渲染技术。



#### **目录部署**

config  存储配置文件

​	webpack.entry.js

​	webpack.serve.js

home  前端开发的项目

​	public

​	src

​		components

​		views

​		store.js

​		App.vue

​		main.js

views  模板目录

static  静态资源

app.js  服务器入口文件



#### **前后端渲染环境**

前端渲染

​	 是为了给用户使用，因此希望资源加载的快（压缩，打包，添加指纹等等）

 	为了更好的效果，我们要加载样式；为了看到页面，我们要处理模板等，......

但是服务器端渲染不需要考虑这些问题，只需要一个应用程序组件，并最终将其渲染成字符串。



==因此前后端渲染有不同的入口文件，有不同的webpack配置==

 	有不同的入口文件

​			 前端入口文件，只需要让应用程序组件上树；

​			 后端入口文件，只需要一个应用程序组件

​	 不同的webpack配置

​			 前端要考虑样式，模板，性能优化（压缩，打包，添加指纹等）等

​			 后端只要将ES Module规范，编译成CommonJS规范即可。



#### **npm 指令**

由于webpack配置存储到特定位置，因此启动webpack指令很长

​	 webpack --config 文件位置



为了简化webpack的运行，我们可以配置一些npm指令，像vue cli一样运行：

​	 npm run client  				 	运行前端配置

​	 npm run server  					运行服务器端配置

​	 npm run start  				  	同时运行前后端配置

​	 npm run start:client  			监听并运行前端配置

​	 npm run start:server 			监听并运行后端配置



我们要在package.json中，配置scripts项，来定义指令。

```js
{
  "scripts": {
    "client": "webpack --config ./config/webpack.entry.js",
    "serve": "webpack --config ./config/webpack.serve.js",
    "start": "npm run client && npm run serve",
    "start:client": "webpack -w --config ./config/webpack.entry.js",
    "start:serve": "webpack -w --config ./config/webpack.serve.js"
  }
}
```



#### **服务器端渲染**

1 不需要模板，不用写html插件

2 不需要样式，不用写style-laoder，不需要拆分样式

3 不需要性能优化、拆分模块，添加指纹，服务器端本地引入文件，因此打包在一起就可以了等

4 通过target: node配置，说明给node使用。

5 在output.`libraryTarget`: commonjs2, 将ES Module规范转成commonjs规范

6 使用vue-server-renderer/server-plugin插件，发布json文件。

​	 在参数对象中，通过filename属性，可以自定义json文件的发布位置。

......



为了使用发布的json文件，vue-server-renderer提供了`createBundleRenderer`方法。

​	 第一个参数表示json文件

​	 第二个参数表示配置对象

​			 template定义模板文件的位置



#### **插值语法**

插值语法：vue服务器端渲染允许我们在渲染页面的时候，向页面中，插入一些数据。

​	 在`renderToString`方法中传递数据。

​	 在模板中，通过插值语法插入数据

​			 {{}}  渲染文本插值

​			 {{{}}} 渲染标签插值

```js
// 渲染器实现了promise规范，因此：可以通过then方法监听成功，可以通过cache方法监听失败
app.get('/', (req, res) => {
  // 定义配置对象 传递数据!!!
  renderToString({
    title: '前后端同步渲染',
    meta: `
      <meta name="keywords" content="北京 IT Web 前端">
      <meta name="description" content="北京市昌平区"
    `
  })
    .then(html => res.end(html))
    .catch(err => console.log(222, err))
})
```



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <!-- 渲染标签 -->
    {{{meta}}}
    <!-- 渲染文本 -->
    <title>{{title}}</title>
  </head>
  <body>
    <!-- <div id="app"></div> -->
    <!--vue-ssr-outlet-->
  </body>
</html>
```



#### **同步数据**

​	同步数据就是与页面一起返回的数据。

​	 由于是与页面一起返回的，因此我们可以在页面中直接使用这些数据，不必发送请求了。

​	 一些重要的，需要首屏渲染的内容，我们可以放在同步数据中。



```html
  <body>
    <!-- <div id="app"></div> -->
    <!--vue-ssr-outlet-->

    <!-- 1.模拟同步数据 textarear -->
    <!-- <textarea id="newsdata" style="display: none">{{JSON.stringify(data)}}<textarea> -->

    <script>
      // 2.模拟同步数据 在全局注入
      window.globalData = {
        clors: ["red", "gold"],
      };
    </script>
  </body>
```



#### **路由同步**

 路由同步就是说 -- 前后端渲染的页面是一致的。

 如果使用hash路由，改变hash，不会向服务器端发送请求，因此服务器端不会更新页面，并且服务器端无法获取hash数据，无法解析hash数据。

 所以在前后端同步渲染的时候，只能使用`history`路由测试（多页面应用程序了）。

 当使用history策略的时候，不同页面应该请求同一个路径下的静态文件，所以静态文件的路径应该是绝对路径。注意：使用history策略，服务器端要做重定向。



#### **前后端解析**

前端解析：前端有路由模块，是专门用来解析前端路由的。

后端解析：后端要在渲染页面的时候解析，

​		 所以要通过请求对象获取url地址

​		 在将url地址提交给vue-router模块创建的路由实例化对象去解析。

为了将url信息传递给后端入口文件，我们分成两步：

​		 第一步 在renderToString方法中，传递数据

​		 第二步 将后端入口文件改成一个返回promise对象的方法。

​				 方法的参数就是renderToString方法传递的数据。

​				 在方法中，我们解析路由，并最终使用promise对象的resovle或者reject方法，返回处理结果。



### 1.8 解析路由

我们可以使用路由模块解析路由，路由实例化对象提供了一些方法

​	 push  将一个新的地址加入到路由序列中。

​	 onReady 监听路由解析完毕

​	 getMatchedComponents  当前路由规则下，匹配的组件。



```js
// 服务器端入口文件
import app from './main';
import router from './router';

// export default app;
// 暴露方法，可以接收renderToString传递过来的数据
export default data => new Promise((resolve, reject) => {
    // console.log(data, 111);
    // 利用路由对象，处理地址
    router.onReady(() => {
        // console.log(router.getMatchedComponents(), 22222222);
        if (router.getMatchedComponents().length) {
            resolve(app)
        } else {
            reject({
                status: 404,
                msg: 'not found'
            })
        }
    })
    router.push(data.url)
})
```



## 二、Vue3

### 2.1 vue3

2020年9月18日，Vue.js发布3.0版本，代号：One Piece（海贼王）

​		 耗时2年多、2600+次提交 、30+个RFC、600+次PR 、99位贡献者

​		 github上的tags地址：[https://github.com/vuejs/vue-next/releases/tag](https://github.com/vuejs/vue-next/releases/tag/v3.0.0)[/v3.0.0](https://github.com/vuejs/vue-next/releases/tag/v3.0.0)

vue3带来了：

 	1.性能的提升

​	 2.源码的升级

​	 3.拥抱TypeScript- Vue3可以更好的支持TypeScript

​	 4.新的特性



### 2.2 vue-cli

使用 vue-cli 创建:

​		 查看@vue/cli版本，确保@vue/cli版本在4.5.0以上

​		 vue –version

 安装或者升级你的@vue/cli

​		 npm install -g @vue/cli

 创建

​		 vue create vue_test

 启动

​		 cd vue_test  

​		 npm run serve



### 2.3 vite

官网：[https://](https://vitejs.cn/)[vitejs.](https://vitejs.cn/) 

​		 一个轻量级的vue cli，运行更快，体验更好（ vite需要 Node.js 版本 >= 12.0 ）

创建vite项目

​		 `npm init vite` > 输入项目名称 > cd 进入项目 > npm install > npm run dev

​		 npm init vite-app <project-name>

进入工程目录： 

​		cd <project-name>

安装依赖： 

​		npm install

运行： 

​		npm run dev

指令

​		dev 			运行开发环境

​		build 		 发布项目

​		preview 	预览发布的项目 



### 2.4 创建应用

import App from './App.vue’

​	Vue 2.0 创建项目

​			 import Vue from 'vue'

​			 new Vue({ render: h => h(App) }).$mount('#app')

​	Vue 3.0 创建项目（函数式编程）

​			 import { createApp } from 'vue’

​			 createApp(App).mount('#app')



```js
// vue3
// createApp: 是一个工厂函数
import { createApp } from 'vue'
import './style.css'
import App from './App.vue'

// 通常所说的vm实例
let app = createApp(App)

// 上树
app.mount('#app')
```



