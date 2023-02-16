# Vue 	第八天

## 一、Vue cli

### 1.1 vue cli

​	 我们使用vue开发项目，我们要书写webpack配置，创建项目，创建文件等需要时间，为了做性能优化，需要大量的配置等等，在开发项目前，要准备大量的工作内容。

​	 vue为了简化这一开发方式，提供了vue-cli脚手架。

安装

​	 通过npm安装vue-cli： npm install -g @vue/cli

​	 此时提供了vue指令，输入vue -v可以查看

创建项目

​	 执行‘vue create 项目名称’指令就可以创建项目

​	 创建过程中，可以输入一些选项。



#### 创建vue项目

1、使用脚手架创建vue项目:vue create test (test为项目名)

2、选择第三项自定义添加：

![](E:\AIC\notes\11-Vue\01.png)

​				Default([Vue 3] babel, eslint):vue3的项目，只包含js编译器babel，代码检测工具eslint。

​				Default([Vue 2] babel, eslint):vue2的项目，只包含js编译器babel，代码检测工具eslint。

​				Manually select features:自定义添加选择功能。

3、选择配置 (空格选中）：

![](E:\AIC\notes\11-Vue\2.png)

​	

- Babel：js编译器
- Typescript：js的超集
- Progressive Web App Support:渐进式的网页应用程序
- Router:vue的路由
- Vuex:vue的状态管理
- CSS Pre-processors:css的预处理器
- Linter/Formatter:代码风格检测与格式化(如果自己代码不是很规范的话可以用这个约束下自己,也可不选择，按照自己的风格)
- Unit Testing:单元测试
- E2E Testing:端对端测试

4、选择vue3(按照自己需求来)

![](E:\AIC\notes\11-Vue\3.png)

5、路由采用history模式

![](E:\AIC\notes\11-Vue\4.png)

6、选择CSS预处理器：（with node sass)

![](E:\AIC\notes\11-Vue\5.png)

7、选择第三个标准配置：（第一个）

![](E:\AIC\notes\11-Vue\6.png)

8、选择第一个，编写代码时提示错误：

![](E:\AIC\notes\11-Vue\7.png)

9、Babel、ESLint 等的配置存放选择都存放在package.json中，选择第二项：（单独创建 选择第一项）

![](E:\AIC\notes\11-Vue\8.png)

10、保存设置为demo,下次再次创建项目时便会多出一个demo选项，不需再次配置即可直接使用：（ 选择  n)

![](E:\AIC\notes\11-Vue\9.png)

11、敲击回车等待后创建完成：

![](E:\AIC\notes\11-Vue\10.png)



### 1.2 目录部署

node_modules 依赖的模块



public 	静态资源

​	 index.html 			入口文件

​	 manifest.json 		离线缓存配置

​	 favicon.ico 			网页logo

​	 robots.txt 			  爬虫配置

​	 img 						图片资源目录



src  		开发目录

​	 assets 					静态资源

​	 components 	  	页面间共享的组件

​	 views  					页面组件

​	 App.vue 				应用程序组件

​	 router.js 				路由文件

​	 store.js  				vuex文件

​	 main.js 				入口文件

​	 registerServiceWorker.js 		web workers文件



test 单元测试目录



 .browserslistrc 				  	浏览器配置

.eslintrc 							 	es语法校验

 .gitnore 								提交配置

babel.config.js 					  babel配置

 jest.config.js 						单元测试配置

package.json 						npm包配置

 postcss.config.js 				  css配置

readme.rd 							项目介绍文件

 yarn.lock							 yarn锁文件



### 1.3 指令

ts开发

​	 我们创建项目的时候，可以选择ts语法或者es6语法，

​	 选择ES6会使用.js文件

​	 选择ts会使用.ts文件

指令

​	 serve 启动项目，默认端口号是8080

​	 build  发布项目，默认向dist目录下发布

​	 test:unit 启动单元测试

我们既可以使用yarn指令运行，也可以使用npm run指令运行。



### 1.4 配置

插件webpack配置，用`inspect`指令：vue inspect > 文件路径



我们可在vue.config.js中。自定义配置

​	 在配置文件中，有两个环境，

​			 一个是开发环境，执行yarn serve时候的环境

​			 一个是发布环境，执行yarn build时候的环境

​	 我们通过`process.env.NODE_ENV`来识别环境：

​			 development表示开发环境 

​			 production表示发布环境



在vue.config.js文件中。我们要分别为开发环境和发布环境定义配置

 我们可以定义两类配置

​	 一类是webpack语法配置

 			在configureWebpack中配置

​	 一类是vue cli自身的语法配置

​			 outputDir 静态资源发布位置

​			 indexPath 模板发布位置

​			 publicPath  模板中引入的静态资源相对位置 



```js
// 识别环境
if (process.env.NODE_ENV === 'production') {
  // 发布环境
  // 配置
  module.exports = {
    // outputDir 静态资源发布位置
    outputDir: "../static/home",
    //  indexPath 模板发布位置  (相对于home)
    indexPath: "../views/home.html",
    //  publicPath  模板中引入的静态资源相对位置
    publicPath: "../home/"
  }
} else {
  // 开发环境
  module.exports = {
    // 在 configureWebpack 中配置
    configureWebpack: {
      // 可以加载多个入口文件
      entry: {
        app: [
          './src/test.js'
        ]
      }
    }
  }
}
```



### 1.5 PWA

是一个渐进式的web应用，介于web应用与源生应用之间的一类应用、

​	 可以像web应用一样开发。

​	 可以具有源生应用的一些功能，

其中以下文件就是为了实现这些功能的。

 	`manifest.json` 离线缓存配置

​	 registerServiceWorker.js web workers文件



## 二、单元测试

测试就是描述一段话，判断是否正确（断言）。基于文件或者模块（组件）的测试，我们称之为单元测试。



### 2.1 单元测试

在vue cli中，我们使用的是jest框架。测试结果有两种

​	 一种是测试成功，所有的单元测试都成功。

​	 一种是测试失败，有一个测试是失败的。



 启动测试：

​	yarn test:unit  

​	`npm run test:unit`



测试文件：在单元测试中，有三类文件可以被测试

​	 1 放在__test__目录中的文件

​	 2 文件的名添加.test.后缀的文件

​	 3 文件的名添加.spec.后缀的文件

命名规范：通常与被测试的文件同名。



### 2.2 测试方法

`describe` 测试整体描述

​	 第一个参数表示描述

​	 第二个参数是函数，表示测试内容



`it`  一次测试的描述

​	 第一个参数表示描述

​	 第二个参数是函数，表示本次测试的内容



`expect` 断言方法

​	 参数就是描述

​	 我们要对返回值做判断（断言）



### 2.3 断言方法

常见的断言方法有：

​	 toBe  表示===

​	 toEqual   字面量形式是否相等

​	 toMatch   是否正则匹配

​	 toContain 是否包含

​	 toBeTruthy 是否为真

​	 toBefalsy  是否为假

​	 ......



### 2.4 周期方法

beforeEach  	每一个it语句执行前

afterEach 		每一个it语句执行后

beforeAll 		所有的it语句执行前

afterAll 			所有的it语句执行后

==注意== 只对当前文件的it语句生效，其它文件无效。



```js
// 暴露接口
export let msg = 'hello msg'
export let num = 100
// 暴露方法
export default (num1, num2) => {
  return num1 + num2
}
```



```js
// 引入测试文件
import addNum, { msg, num } from '@/demo.js'

// npm run test:unit

// 整体描述
/* 
  describe 测试整体描述
    第一个参数表示描述
    第二个参数是函数，表示测试内容
*/
describe('测试demo文件', () => {
  /* 
    it  一次测试的描述
      第一个参数表示描述
      第二个参数是函数，表示本次测试的内容
  */
  it('测试msg', () => {
    /* 
      expect 断言方法
        参数就是描述
        我们要对返回值做判断（断言）
    */
    expect(msg)
      // 是否包含 toContain
      .toContain('hello')
  })

  it('测试num', () => {
    expect(num)
      .toBe(100)
  })

  it('测试addNum()', () => {
    expect(addNum(1, 2))
      .toBe(3)
  })

  /* 
      beforeEach  	每一个it语句执行前
      afterEach 		每一个it语句执行后
      beforeAll 		所有的it语句执行前
      afterAll 			所有的it语句执行后

      注意 只对当前文件的it语句生效，其它文件无效。
  */
  // beforeEach(() => {
  //   console.log(111, 'beforeEach');
  // })

  // afterEach(() => {
  //   console.log(222, 'afterEach');
  // })

  // beforeAll(() => {
  //   console.log(333, 'beforeAll');
  // })

  // afterAll(() => {
  //   console.log(444, 'afterAll');
  // })
})
```



### 2.5 测试 store

 由于store中有包含修改数据的逻辑（mutations以及action中），因此要对它们进行测试。

 此时store的这些组成部分要单独定义出来，这样才能测试。



- store.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

// 为了测试 store 方便 可以将mutation 提取出来
export let mutations = {
  addNum(state, num) {
    return state.num = num
  }
}

export default new Vuex.Store({
  state: {
    num: 0
  },
  mutations,
  actions: {

  }
}) 
```



```js
// 引入文件
import { mutations } from "@/store";

// 整体的测试
describe('测试store', () => {
  // 解构数据
  let { addNum } = mutations

  // 单次测试
  it('测试addNum方法', () => {
    // 定义state对象
    let state = { num: 0 }
    expect(addNum(state, 10))
      .toBe(10)
  })
});
```



### 2.6 测试组件

我们在vue文件中，定义的组件只是Vue.extend方法的参数对象。因此我们不能直接使用，要转成实例化对象再使用，有两种方式

​	 第一种：

​			new Vue(obj)，返回的是vue实例化对象

​	 第二种:  

​			通过组件类的方式，

​			第一步 let Comp = Vue.extend(obj); 

​			第二步 实例化 new Comp()



不论是哪一种方式，都可以得到组件，但是组件没有上树，无法获取$el容器元素。

为了获取 `$el` 容器元素，我们要使用`$mount`方法。



```js
import Vue from 'vue'
// 引入组件
import Hello from '@/Hello.vue'

// 整体测试
describe('测试组件', () => {
  // 定义结果变量
  let instance = null

  // it语句执行之前
  beforeEach(() => {
    let Comp = Vue.extend(Hello)
    instance = new Comp()
    instance.$mount()
  })

  // it语句执行之后
  afterEach(() => {
    // 销毁组件
    instance.$destroy()
  })

  // 单次测试
  it('测试msg', () => {

    /* 
    不论是哪一种方式，都可以得到组件，但是组件没有上树，无法获取$el容器元素。
      为了获取 $el 容器元素，我们要使用$mount方法。

    */
    // 第一种： new Vue(obj)，返回的是vue实例化对象
    // let instance = new Vue(Hello)
    // instance.$mount()
    // console.log(instance.$el.textContent);

    /*
      第二种:  
        通过组件类的方式，
        第一步 let Comp = Vue.extend(obj); 
        第二步 实例化 new Comp()
    */
    // let Comp = Vue.extend(Hello)
    // let instance = new Comp()
    // instance.$mount()
    // 断言
    expect(instance.$el.textContent)
      // .toMatch 是否正则匹配
      .toMatch('hello msg')
  })
});
```



**$nextTick**

​	会检测视图的更新，更新完毕，执行回调函数。

​	 该方法实现了Promise规范。

​	 所以我们可以通过then方法监听结果。



```js
import Vue from 'vue'
// 引入组件
import Hello from '@/Hello.vue'

// 整体测试
describe('测试组件', () => {
  // 定义结果变量
  let instance = null

  // it语句执行之前
  beforeEach(() => {
    let Comp = Vue.extend(Hello)
    instance = new Comp()
    instance.$mount()
  })

  // it语句执行之后
  afterEach(() => {
    // 销毁组件
    instance.$destroy()
  })

  // 测试修改数据
  it('测试修改msg数据', () => {
    // 修改数据
    instance.msg = 'hello world'

    /*
      $nextTick
        会检测视图的更新，更新完毕，执行回调函数。
        该方法实现了Promise规范。
        所以我们可以通过then方法监听结果。
    */
    instance.$nextTick()
      .then(() => {
        expect(instance.$el.textContent)
          // .toMatch 是否正则匹配
          .toMatch('hello world')
      })
  })
});

```



**shallowMount**

​	vue cli为了方便我们测试组件，提供了一个 `shallowMount` 方法。

​	 该方法可以将组件实例化，并且执行$mount方法上树。

​	 我们通过该方法，我们就可以得到一个组件实例化对象。

​			 第一个参数是组件对象 

​			第二个参数是配置对象（propsData： 传递给组件的属性数据）

​	 提供了`text`方法，可以获取其视图中的内容。



注意：使用单元测试去测试视图等很麻烦，效率低，因此工作中慎用。

​		我们用单元测试，去测试一些业务逻辑收益还是很大的。

  

```js
// 引入组件
import Hello from '@/Hello.vue'
// 引入测试方法
import { shallowMount } from '@vue/test-utils'

// 整体测试
describe('测试组件', () => {

  // shallowMount 进行测试
  it('测试title数据', () => {
    const wrapper = shallowMount(Hello, {
      // 向组件中传递数据
      propsData: { title: 'Gold' }
    })

    expect(wrapper.text())
      .toMatch('hello')
  })
});
```



## 三、实现 Vue Cli

我们基于webpack来实现一个vue cli的功能

实现vue cli的目录部署

实现一些性能优化的功能。

​	 静态资源压缩，打包，添加指纹等等。



**资源发布**

 将静态资源发布到外面的/static目录中

 将模板资源发布到外面的/views/index.html文件中



```js
  // 出口
  output: {
    // 将静态资源发布到外面的static目录中
    path: path.join(ROOT, '../server/static'),
    // 添加hash
    filename: './[name].[hash:8].js',
    // 静态资源引入的相对位置
    publicPath: '/static/'
  },
```



**发布模板**

​	 发布html文件用html-webpack-plugin插件

​			 template  定义模板文件位置

​			 filename 模板文件发布位置

​			 hash  是否添加指纹（添加在query上）

​			 inject 是否注入静态资源（默认是注入的）



 **压缩资源**

​	压缩js，压缩html，压缩css



**拆分文件**

​	将模块文件打包在一起：main.js

​	将样式文件打包在一起: style.css

​		 我们使用mini-css-extract-plugin插件

​			 vue组件中样式拆分： extractCSS: true

​			 css和scss|less样式拆分，使用该插件加载机

​			 我们通过插件的filename属性定义文件发布位置。



对资源添加指纹：css资源，js资源等

​		 压缩css使用optimize-css-assets-webpack-plugin插件

​		 压缩js： mode: ‘production’



```js
  // 使用插件
  plugins: [
    new VueLoaderPlugin(),
    // 拆分HTML
    new HtmlWebpackPlugin({
      // template  定义模板文件位置
      template: './public/index.html',
      //  filename 模板文件发布位置 (相对于path路径)
      filename: '../views/home.html',
      //  hash  是否添加指纹（添加在query上）
      hash: true,
      // inject 是否注入静态资源（默认是注入的）
    }),
    // 拆分css
    new MiniCssExtractPlugin({
      //  filename 模板文件发布位置 (相对于path路径)
      filename: './style.[contenthash:8].css'
    }),
    // 压缩css
    new optimizeCssAssetsWebpackPlugin()
  ],
```



**拆分库文件**

将库文件打包在一起：lib.js

```js
  // 优化 将库文件打包在一起：lib.js
  optimization: {
    // 拆分文件
    splitChunks: {
      // 缓存分组
      cacheGroups: {
        // 公用缓存分组
        lib: {
          // 库文件名称
          name: 'lib',
          chunks: 'initial',
          // 库文件特征
          test: /node_modules/
        }
      }
    }
  }
```



```js
// 引入path
const path = require('path')
// 定义根目录
const ROOT = process.cwd()
// 引入vue-loader
const { VueLoaderPlugin } = require('vue-loader')
// 发布html文件用 html-webpack-plugin 插件
const HtmlWebpackPlugin = require('html-webpack-plugin')
// 使用 mini-css-extract-plugin 插件 拆分css
const MiniCssExtractPlugin = require('mini-css-extract-plugin')
// 压缩css使用optimize-css-assets-webpack-plugin插件
const optimizeCssAssetsWebpackPlugin = require('optimize-css-assets-webpack-plugin')

// 定义配置
module.exports = {
  // 定义环境
  // mode: "development",
  // 压缩js
  mode: 'production',
  // 解决问题
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': path.join(ROOT, './src')
    },
    // 配置拓展名
    extensions: ['.js', '.vue']
  },
  // 入口文件
  entry: './src/main.js',
  // 出口
  output: {
    // 将静态资源发布到外面的static目录中
    path: path.join(ROOT, '../server/static'),
    // 添加hash
    filename: './[name].[hash:8].js',
    // 静态资源引入的相对位置
    publicPath: '/static/'
  },
  // 定义加载机
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
        use: [
          'style-loader',
          MiniCssExtractPlugin.loader,
          'css-loader'
        ]
      },
      // scss
      {
        test: /\.scss$/,
        use: [
          'style-loader',
          MiniCssExtractPlugin.loader,
          'css-loader',
          'sass-loader'
        ]
      },
      // less
      {
        test: /\.less$/,
        use: [
          'style-loader',
          MiniCssExtractPlugin.loader,
          'css-loader',
          'less-loader'
        ]
      },
    ]
  },

  // 使用插件
  plugins: [
    new VueLoaderPlugin(),
    // 拆分HTML
    new HtmlWebpackPlugin({
      // template  定义模板文件位置
      template: './public/index.html',
      //  filename 模板文件发布位置 (相对于path路径)
      filename: '../views/home.html',
      //  hash  是否添加指纹（添加在query上）
      hash: true,
      // inject 是否注入静态资源（默认是注入的）
    }),
    // 拆分css
    new MiniCssExtractPlugin({
      //  filename 模板文件发布位置 (相对于path路径)
      filename: './style.[contenthash:8].css'
    }),
    // 压缩css
    new optimizeCssAssetsWebpackPlugin()
  ],

  // 优化 将库文件打包在一起：lib.js
  optimization: {
    // 拆分文件
    splitChunks: {
      // 缓存分组
      cacheGroups: {
        // 公用缓存分组
        lib: {
          // 库文件名称
          name: 'lib',
          chunks: 'initial',
          // 库文件特征
          test: /node_modules/
        }
      }
    }
  }
}
```



- app.js

```js
// 引入express
const express = require('express');
// 引入http
const http = require('http');
// 引入https
const https = require('https');
// 引入fs
const fs = require('fs');

// 创建路由应用
const app = express();

// 配置引擎
app.engine('html', require('ejs').__express);

// 静态化
app.use('/static', express.static('./static/'));


// 渲染模板
app.get('/', (req, res) => {
  res.render('home.html');
})


// 创建应用
http.createServer(app).listen(3000, () => console.log(3000));

// 读取文件
const key = fs.readFileSync('./ssl/private.pem');
const cert = fs.readFileSync('./ssl/file.crt');
https.createServer({ key, cert }, app).listen(3001, () => console.log(3001));
```

