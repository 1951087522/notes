## Vue 第七天

### 跨域代理请求

让我们在本地开发中可以使用线上的数据。webpack-dev-server支持跨域请求代理技术：

我们通过`devServer`配置项，定义webpack-dev-server的配置



```js
// 定义配置对象
const config = {
    mode: 'development',
    // 解决问题
    resolve: {
        // 入口文件
        alias: {
            'vue$': 'vue/dist/vue.esm.js'
        },
        // 配置拓展名
        extensions: ['.js']
    },
    // 入口文件
    entry: './main.js',
    // 出口配置
    output: {
        filename: './[name].js'
    },
    // 加载机
    module: {
        rules: [
            // css
            {
                test: /\.css$/,
                loader: 'style-loader!css-loader'
            },
            // scss
            {
                test: /\.scss$/,
                loader: 'style-loader!css-loader!sass-loader'
            }
        ]
    },
    // 通过devServer配置
    devServer: {
        // port定义端口，
        port: 8085,
        // open是否自动打开浏览器
        open: true,
        // proxy定义跨域请求代理
        proxy: {
            //     key 		表示代理的请求
            //     value 	表示代理的配置对象
            '/demo/test': {
                //     target 			表示目标地址
                target: 'https://api.aichuangleyu.com/weixin_waimai/data/list',
                //     pathRewrite		表示是否重写路径
                pathRewrite: {
                    '/demo/test': ''
                },
                // secure 是否对https协议校验。
                secure: false
            }
        }
    }
}

// 暴露接口
module.exports = config;
```

```js
import Vue from 'vue'
Vue.config.productionTip = false
// 引入axios
import axios from 'axios'

// 定义组件
let Demo = Vue.extend({
  template: `<h1>hello demo</h1>`,
  // 组件创建完毕
  created() {
    axios.get('/demo/test', { color: 'red' })
      .then(({ data }) => console.log(data))
  },
})

new Vue({
  components: { Demo },
  el: "#app",
})
```



## 一、单文件组件

### 1.1 使用单文件组件

在vue中，组件包含模板，样式和脚本，但是我们定义组件的时候

​	 我们将模板写在了html文件中

​	 我们将样式写在了css | less | scss文件中

​	 我们将脚本写在js | es文件中

vue为了简化维护组件的成本，建议我们将这三个部分放在同一个文件中。

​	 通过`template`元素定义模板，最外层根元素只能有且只有一个

​	 通过`style`元素定义样式

​	 通过`script`元素定义脚本，我们要使用ES Module规范。只定义组件对象即可（Vue.extend参数对象）

​	 将文件的拓展名定义成`.vue`，这就是单文件组件。规范：将组件文件名称的首字母大写。



### 1.2 编译

浏览器不识别.vue文件，因此我们要将vue文件编译成js文件。

​	 我们通过/\.vue$/匹配vue文件

​	 	通过vue-loader加载机编译vue文件

​	注意：新版本中，要在plugins中配置VueLoaderPlugin功能



```js
// 引入vue-loader模块
let { VueLoaderPlugin } = require('vue-loader')
// 定义配置对象
const config = {
    mode: 'development',
    // 解决问题
    resolve: {
        // 入口文件
        alias: {
            'vue$': 'vue/dist/vue.esm.js'
        },
        // 配置拓展名
        extensions: ['.js']
    },
    // 入口文件
    entry: './main.js',
    // 出口配置
    output: {
        filename: './[name].js'
    },
    // 加载机
    module: {
        rules: [
            // vue
            {
                test: /\.vue$/,
                loader: 'vue-loader'
            },
            // css
            {
                test: /\.css$/,
                loader: 'style-loader!css-loader'
            },
            // scss
            {
                test: /\.scss$/,
                loader: 'style-loader!css-loader!sass-loader'
            }
        ]
    },
    // 使用插件
    plugins: [
        new VueLoaderPlugin()
    ]
}

// 暴露接口
module.exports = config;
```



### 1.3 ShadowDOM

在组件中，我们定义的样式会污染其它的组件，我们可以通过shadowDOM技术来解决。

为style标签设置`scoped`属性，

​	 此时当前组件内部的元素会添加属性选择器，

​	 添加的样式也会设置属性选择器

shadowDOM样式：只对当前组件生效，对其它的组件无效。

注意：子组件只在容器元素上设置属性选择器，内部的元素没有被添加

```vue
<!-- 通过style元素定义样式 -->
<style scoped>
h1 {
  color: gold;
}
</style>
```



### 1.4 CSS 预编译

vue单文件组件内置了css预编译语言的解析器，我们可以直接使用这些语言。

​	 通过`lang`属性来设置

​			 lang=”less” 使用less

​			 lang=”scss” 使用scss

​	 注意：在4.0中配置中，要定义less | sass加载机。



```vue
<!-- 通过template元素定义模板，最外层根元素只能有且只有一个 -->
<template>
  <div>
    <h1>hello part --{{ msg }}</h1>
  </div>
</template>

<!-- 通过script元素定义脚本，我们要使用ES Module规范。只定义组件对象即可（Vue.extend参数对象） -->
<script>
export default {
  data() {
    return {
      msg: "demo msg",
    };
  },
};
</script>

<!-- 通过style元素定义样式 -->
<!-- 通过scoped 定义 shadowDOM技术来解决。 -->

<!-- <style scoped lang="scss">
$color: gold;
h1 {
  color: $color;
}
</style> -->

<style scoped lang="less">
@color: red;
h1 {
  color: @color;
}
</style>
```

```js
import Vue from 'vue'
Vue.config.productionTip = false

// 引入单文件组件
import Home from './Home.vue'

new Vue({
  components: { Home },
  el: "#app",
})
```



### 1.5 函数组件

我们在子组件中，为了接收父组件的数据，要使用props属性接收，

vue为了简化这一过程，提供了函数组件：

​	 在模板中，设置`functional`属性即使一个函数组件

​	 此时我们就可以直接通过“`props.属性`”语法获取数据。

​	在当前版本中，函数组件被看成是一个模板片段，因此受父组件的`scroped`样式的影响



```vue
<!-- 函数组件 functional -->
<template functional>
  <div>
    <h2>Demo page</h2>
    <!-- 此时通过 props.xx 获取 -->
    <h2>{{ props.msg }} {{ props.num }}</h2>
  </div>
</template>
```



### 1.6 异步组件

我们将所有的资源都打包在一起会导致文件很大，浏览器加载很慢，影响用户体验。

​	 vue为了减小文件体积，可以使用异步组件的技术。我们需要什么组件，加载什么组件，

首次不需要加载的组件就不需要打包在一起了。有两种异步组件的形式



​	 第一种：在函数的返回值中，返回`Promise`对象 （异步创建）

​			 在Promise方法中，执行异步操作，操作结束之后返回组件



​	 第二种：在函数的返回值中，返回通过`import`方法异步引入组件（异步加载）

​			 为了使用import方法。我们要使用:babel-plugin-syntax-dynamic-import，babel的插件在plugins中配置

```js
    // 出口配置
    output: {
        filename: './[name].js',
        // 修改静态资源引入的相对位置
        publicPath: './dist/'
    },
```



注意：

​		4.0默认向dist目录下发布，因此要设置publicPath：表示引入静态资源相对位置。

​		4.0改动：**默认支持异步引入**（import方法）



```js
// 引入Vue
import Vue from 'vue';
// 引入Home
// import Home from './Home.vue';

// 关闭生产提示
Vue.config.productionTip = false;

// 异步加载组件的方式1: promise
// let Home = () => new Promise(resolve => {
//     // 执行异步操作
//     // setTimeout(() => {
//     //     resolve(import('./Home.vue'));
//     // }, 3000);


//     // 点击的时候加载
//     window.addEventListener('click', () => {
//         resolve(import('./Home.vue'));
//     })
// })


// 异步加载组件的方式2: 懒加载
let Home = () => import('./Home.vue');
// 实例化
new Vue({
    // 注册组件
    components: { Home },
    // 绑定视图
    el: '#app',
    // 绑定数据
    data: {
       
    }
})
```



### 1.7 拆分应用程序组件

我们目前定义的vue实例化对象既包含模板，样式，脚本，也包含注册store, router等功能。因此vue实例化对象做了很多的事情。为了让vue实例化对象职责更加的单一，我们要将实例化对象中的模板，样式，脚本单独拆分出来，作为应用程序组件。这样实例化对象中，就只剩下注册store，router等功能了



​	 1 我们将应用程序组件定义成App.vue，在内部，通过template定义模板，通过style定义样式，通过script定义脚本。

​	 2 为了在实例化对象中渲染应用程序组件，vue提供了一个`render`方法，参数是一个渲染方法，可以用来渲染应用程序。

 	3 为了让应用程序上树（渲染到页面中），vue提供了`$mount`方法，用来上树。参数表示css选择器。会将该选择器对应的元素作为容器元素。



 	以后只需要在vue实例化对象中，注册路由，注册store等，实现一些功能即可。

注意：拆分的应用程序组件App.vue要注意：模板的根元素要设置id属性，与模板的id同值



```js
import Vue from 'vue'
Vue.config.productionTip = false
// 引入应用程序
import App from './App.vue'

new Vue({
  // 为了在实例化对象中渲染应用程序组件，vue提供了一个render方法，参数是一个渲染方法，可以用来渲染应用程序。
  // render(h) {
  //   return h(App)
  // }

  // 简写
  render: (h) => h(App)

  // 为了让应用程序上树（渲染到页面中），vue提供了$mount方法，用来上树。参数表示css选择器。会将该选择器对应的元素作为容器元素。
}).$mount('#app')
```



### 1.8 混合

混合就是对组件的模板，样式，数据，方法等相关功能的复用。

vue支持两种混合：全局混合，局部混合

**全局混合**

​		全局混合会对所有的组件生效。通过`Vue.mixin（）`方法定义，参数就是继承的数据和方法等

```js
// 全局混合 (让所有组件拥有数据方法) 参数就是继承的数据和方法等
Vue.mixin({
  // 定义数据
  data() {
    return {
      num: 500
    }
  },
  // 拓展周期方法
  created() {
    console.log('mixin created');
  },
  // 与组件不相干的数据方法不会被继承
  getNum() {
    console.log('getNum');
  }
})
```



**局部混合**

​		局部混合只能对当前组件生效。

​		在组件中通过`mixins`属性，实现对组件，数据，方法的继承

​				是一个数组，每一个成员表示一个继承的对象，可以组件，可以是对象等。

注意：

​	如果多个数据之间出现同名的属性数据

​			如果是模型中的数据和方法，后面的覆盖前面的。

​		    如果是生命周期方法，会暴露多个。并按照先后顺序依次执行。

​	我们继承之后，还可以重写被继承的数据，使用的时候，会优先使用重写的数据。

==注意==	与组件不相关的数据和方法不会被组件继承。



- Home

```js
<script>
// 引入Demo组件
import Demo from "./Demo.vue";
export default {
  // 注册子组件
  // components: { Demo },

  // 局部混合
  mixins: [
    Demo,
    // 重写数据
    {
      data() {
        return {
          msg: "home msg",
        };
      },
    },
  ],
};
</script>
```

- Demo

```js
<template>
  <div>
    <h1>Demo page {{ msg }}</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: "Demo msg",
    };
  },
};
</script>

<style lang="scss" scoped>
h1 {
  color: green;
}
</style>

```





### 1.9 插件

vue允许我们自定义插件，实现对vue拓展的功能的复用。

我们封装好插件，就可以在不同的项目中，复用这些功能了。

**定义插件**

​	 插件就是一个函数或者包含`install`方法的对象

​			 第一个参数表示Vue类

​			 从第二个参数开始表示使用插件的时候，传递给插件的数据。

**使用插件**

 	我们通过`Vue.use`方法来安装插件。

 		第一个参数表示插件

 		从第二个参数开始表示传递给插件的数据



- tools.js

```js
// 定义插件 函数
/* 
  第一个参数  vue类
  第二个参数 传递的数据
*/
// export default (...args) => {
//   console.log(args);
// }

// 引入axios
import axios from 'axios'

// 包含install 对象的方法
export default {
  install(Vue) {
    // 拓展版本号
    Vue.prototype.verson = '2.01'

    // 拓展指令
    Vue.directive('demoHtml', (dom, obj) => {
      console.log(dom, obj);
      dom.innerHTML = obj.value
    })

    // 拓展过滤器
    Vue.filter('upper', value => {
      return value.toUpperCase()
    })

    // 拓展全局组件
    let demo2 = Vue.extend({ template: '<h2>demo comp</h2>' })
    // 全局注册
    Vue.component('demo2', demo2)

    // 拓展axios
    Vue.prototype.$http = axios
  }
}
```

- main.js

```js
import Vue from 'vue'
Vue.config.productionTip = false
// 引入APP应用程序组件
import App from './App.vue'

// 使用插件
// 引入插件
import tools from './tools'
/* 
从第二个参数开始传递给插件的数据
*/
// Vue.use(tools, 100, true, 'abc')
Vue.use(tools)

new Vue({
  render: (h) => h(App)
}).$mount('#app')
```

- Home

```js
<template>
  <div>
    <h2>Home Page</h2>
    <!-- 使用插件 -->
    <h3 v-demo-html="msg"></h3>
    <h4>{{ msg | upper }}</h4>
    <demo2></demo2>
    
    <Demo></Demo>
  </div>
</template>

<script>
import Demo from "./Demo.vue";
export default {
  components: { Demo },
  data() {
    return {
      msg: "home msg",
    };
  },
};
</script>

<style></style>
   
```



## 二、UI 库

vue是一个框架，却是一个半成品，提供了大量的工具方法，我们还要自己去实现，因此开发成本还是很高。

我们使用组件开发的意义：让我们可以复用已经写好的组件。

​	 UI库就是一个Vue的插件，提供了大量的UI组件，可以直接使用，提高开发效率。

我们首先讲解一个富文本编辑器UI库，并且根据pc端和移动端介绍两个UI库，

​	  vue-quill-editor 富文本编辑器

​	 mint-ui  给移动端使用的

​	 element-ui 给pc端使用的



### 2.1 富文本编辑器

就是一个超级textarea，包含大量的功能，类似word工具。

​	 我们介绍：vue-quill-editor，该插件依赖于quill插件。

​		 npm i vue-quill-editor quill

​	 注意：我们要引入quill模块的样式：

​	 		import 'quill/dist/quill.core.css'
​					import 'quill/dist/quill.snow.css'
​					import 'quill/dist/quill.bubble.css'

该插件在全局注册了`quill-editor`组件，我们可以直接使用

​	 并且可以通过v-model绑定数据。渲染数据的时候，要使用v-html指令渲染标签。



- main.js

```js
import Vue from 'vue'
Vue.config.productionTip = false
// 引入APP应用程序组件
import App from './App.vue'

// 引入富文本编辑器
import VueQuillEditor from 'vue-quill-editor'
// require styles
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import 'quill/dist/quill.bubble.css'
// 安装
Vue.use(VueQuillEditor)

new Vue({
  render: (h) => h(App)
}).$mount('#app')
```



```vue
<template>
  <div>
    <h1>app part</h1>
    <hr />

    <!-- 使用编辑器 -->
    <!-- 通过v-model绑定数据。渲染数据的时候，要使用v-html指令渲染标签。 -->
    <quill-editor v-model="msg"></quill-editor>
    <h1 v-html="msg"></h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg: "hello msg",
    };
  },
};
</script>

<style></style>

```



### 2.2 Vant

官网：[https://youzan.github.io/vant/#/zh-CN/](https://youzan.github.io/vant/)

 	安装：npm install vant

​			 vue2版本 npm install vant@latest-v2

​			 Vant也是vue家族的插件，因此用use方法安装：Vue.use(Vant)

​	组件的特点：都是以vant为前缀的。

​	我们要导入样式：

​			 import 'vant/lib/index.css’;

**基础组件**

​	van-button  按钮

​	van-cell   单元格

​	van-popup 弹出框

​	van-icon  字体图标，name属性定义图标名称

​	van-row  栅格布局

​	 van-col  单元格所占列数（共24列）

**表单组件**

​	van-form 表单组件

​	van-field 输入框，rules 属性定义校验规则

​	van-calendar 日历组件

​	van-search 搜索框

​	van-slider 滑块

​	van-rate  评分组件 

​		 表单组件通过v-model绑定数据

功能组件

​	van-swipe-cell 滑动单元格 

​			#left左侧模板， 

​			#right右侧模板

​	van-nav-bar 导航条

​	van-tabs 分页栏

​	 van-tab 分页栏页面

​	van-tabbar 标签栏

​	 van-tabbar-item 标签项

​	van-count-down  倒计时

​	van-index-bar 滚动视图

​	 van-index-anchor 滚动标签



```vue
<template>
  <div class="js">
        <!-- 导航组件 -->
        <van-tabs v-model:active="active">
            <van-tab title="标签 1">内容 1</van-tab>
            <van-tab title="标签 2">内容 2</van-tab>
            <van-tab title="标签 3">内容 3</van-tab>
            <van-tab title="标签 4">内容 4</van-tab>
        </van-tabs>
        <hr>


        <!-- 定义轮播图 -->
        <van-swipe class="my-swipe" :autoplay="3000" indicator-color="white">
            <van-swipe-item :style="{ height: '300px', backgroundColor: 'green' }">1</van-swipe-item>
            <van-swipe-item :style="{ height: '300px', backgroundColor: 'blue' }">2</van-swipe-item>
            <van-swipe-item :style="{ height: '300px', backgroundColor: 'red' }">3</van-swipe-item>
            <van-swipe-item :style="{ height: '300px', backgroundColor: 'pink' }">4</van-swipe-item>
        </van-swipe>


        <van-tabbar v-model="active">
            <van-tabbar-item icon="home-o">标签</van-tabbar-item>
            <van-tabbar-item icon="search">标签</van-tabbar-item>
            <van-tabbar-item icon="friends-o">标签</van-tabbar-item>
            <van-tabbar-item icon="setting-o">标签</van-tabbar-item>
        </van-tabbar>

        <hr>
  </div>
</template>

<script>
export default {
    data() {
        return {
            active: 0
        }
    }
}
</script>

<style>

</style>
```

- main.js

```js
// 引入核心库
import Vue from 'vue';
// 引入App应用程序
import App from './App.vue';
// 引入组件
import vant from 'vant';
//引入组件样式
import 'vant/lib/index.css';
// 注册你需要的组件
Vue.use(vant)


// 关闭生成提示
Vue.config.productionTip = false;

// 实例化
new Vue({
  // 使用箭头函数简写
  render: h => h(App)
  // 与el属性等价
}).$mount('#app')
```



### 2.3 ElementUI

element-ui是基于vue实现的一套在pc端的UI框架，内置了大量的组件，我们可以直接使用。

​	官网：https://element.eleme.cn/#/zh-CN/component/button

使用element-ui

​	 element-ui中的组件默认都是以el-为前缀的。

​	 默认组件没有样式，我们要引入：element-ui/lib/theme-chalk/index.css

​	 内置了大量的字体图标，我们可以直接使用：

​			 https://element.eleme.cn/#/zh-CN/component/icon

​			 字体图标：跟字体一样，通过font-size属性来设置其大小。



​			 ==注意==我们使用字体图标，要定义加载机： url-loader.

```js
    // 加载机
    module: {
        rules: [
            // woff | ttf
            {
                test: /\.(woff|ttf)$/,
                loader: 'url-loader'
            }
        ]
    },
```



- main.js

```js
// 引入核心库
import Vue from 'vue';
// 引入App应用程序
import App from './App.vue';

// 引入ui库
import ElementUI from 'element-ui';
// 引入样式文件
import 'element-ui/lib/theme-chalk/index.css';
// 安装
Vue.use(ElementUI);


// 关闭生成提示
Vue.config.productionTip = false;

// 实例化
new Vue({
  // 使用箭头函数简写
  render: h => h(App)
  // 与el属性等价
}).$mount('#app')
```



### 2.4 表单校验

就是在表单输入的时候，对表单的内容做校验。

 	定义表单: el-form

​	 每一行：el-form-item

​	 表单控件：el-input

设置样式：

​	 1 el-input设置placeholder

​	 2 el-form-item设置label

​	 3 el-form设置label-width



输入校验

​	 1 el-input组件，设置v-model指令，实现数据双向绑定，为了数据访问方便，通常放在同一个命名空间下

​	 2 el-form设置model属性，表示校验哪一组数据

​	 3 el-form-item设置prop属性，表示校验哪一个数据

​	 4 el-form设置rulus属性，定义校验规则

​		 校验规则是一个对象

​			 key表示校验的字段名称，

​			 value是一个数组，每一个成员代表一条规则，是一个对象

​					 required：是否是必填的

​					 message：提示信息

​					 validator：校验的方法

​					 trigger：什么时候触发校验，默认是一边输入一边校验

提交校验

​	 1 为按钮绑定事件

​	 2 在事件回调函数中，获取表单组件（通过ref属性获取）

​	 3 执行表单组件的validate方法，校验内容。



```vue
<template>
  <div>
    <h1>app part</h1>
    <hr />

    <el-button>默认按钮</el-button>
    <el-button type="primary">主要按钮</el-button>
    <el-button type="success">成功按钮</el-button>
    <el-button type="info">信息按钮</el-button>
    <el-button type="warning">警告按钮</el-button>
    <el-button type="danger">危险按钮</el-button>

    <hr />

    <!-- 
      设置样式：
        1 el-input设置placeholder
        2 el-form-item设置label
        3 el-form设置label-width

      输入校验
        1 el-input组件，设置v-model指令，实现数据双向绑定，为了数据访问方便，通常放在同一个命名空间下
        2 el-form设置model属性，表示校验哪一组数据
        3 el-form-item设置prop属性，表示校验哪一个数据
        4 el-form设置rulus属性，定义校验规则

      提交校验
        1 为按钮绑定事件
        2 在事件回调函数中，获取表单组件（通过ref属性获取）
        3 执行表单组件的validate方法，校验内容。
    -->

    <el-form label-width="200px" :model="formData" :rules="rules" ref="form">
      <el-form-item label="用户名" prop="username">
        <el-input
          type="password"
          placeholder="请输入用户名"
          v-model="formData.username"
        ></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input
          type="password"
          placeholder="请输入密码"
          v-model="formData.password"
        ></el-input>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="submMit">提交</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
export default {
  data() {
    // 定义校验规则
    let checkUsername = (obj, value, cb) => {
      if (!/^\w{4,8}$/.test(value)) {
        return new Error(cb("用户名是4-8位字符!!!"));
      } else {
        cb();
      }
    };

    return {
      formData: {
        username: "",
        password: "",
      },
      rules: {
        // 自定义校验规则
        username: [
          // { required: true, message: "请输入用户名", trigger: "blur" },
          { required: true, trigger: "blur", validator: checkUsername },
        ],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }],
      },
    };
  },
  methods: {
    submMit() {
      // 获取组件 并调用组件中的校验方法
      this.$refs.form.validate((valid) => {
        console.log(valid);
      });
    },
  },
};
</script>

<style>
.el-form {
  width: 600px;
  margin: 50px auto;
}
</style>

```

