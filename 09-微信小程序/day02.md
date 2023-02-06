# day 02

## 一、微信小程序

### 1.1   模板组件

 	`template` 用于定义模板组件

​	 	`name` 定义模板组件的名称

 		`is`  使用模板

 	与 `block` 组件相比，template是可以复用的

 	也就是说：block是为指令设计的，template是为视图设计的



```html
<!-- 定义模板 通过name定义模板的名称 -->
<template name="header">header</template>
<template name="body">body</template>
<template name="footer">footer</template>

<!-- 使用模板 通过is 指定显示的模板 -->
<template is="header"></template>
<view>
  <template is="body"></template>
</view>

<!-- 模板可以复用 -->
<template is="body"></template>
<view>
  <template is="body"></template>
</view>
<template is="body"></template>
```



### 1.2 引入文件

- 在小程序的脚本文件中引入文件的方式是通过 `require` 方法（遵守commonjs规范和Es Module规范）。



```js
// 定义数据 暴露接口
let msg = "hello msg"

let num = 100

let addNum = (num1, num2) => {
  return num1 + num2
}

// commjs规范
module.exports = {
  msg,
  num,
  addNum
}

// ES Module规范
export let msg = "hello msg"

export let num = 100

export default (num1, num2) => {
  return num1 + num2
}
```

```js
// 支持commjs规范 引入
let demo = require('./demo.js')
console.log(demo);

// 支持 ES Module 规范引入
import demo, {
  msg,
  num
} from './demo.js'
console.log(demo, msg, num);
```



- 在小程序中的 wxml 文件中引入文件的方式通过两种组件：

​	 	`import` 组件   引入文件

​	 	`include` 组件   引入文件

 通过src指定引入文件的路径，使用相对路径

 区别：

 import 只能导入模板，不能导入其它组件

 include 只能导入其它组件，不能导入模板

```html
<!-- 定义模板 -->
<template name="common">import 导入模板</template>

<!-- 定义视图 -->
<view class="demo">include 导入组件</view>
```



```html
<!-- 
  在小程序中的 wxml 文件中引入文件的方式通过两种组件：
	 	import 组件  引入文件
	 	include 组件   引入文件
 通过src指定引入文件的路径，使用相对路径

 区别：
  import 只能导入模板，不能导入其它组件
  include 只能导入其它组件，不能导入模板
 -->

<!-- 导入模板  即可通过 template使用 -->
<import src="./common.wxml"></import>

<template is="common"></template>
<view>
  <template is="common"></template>
</view>
<template is="common"></template>

<!-- 导入组件 -->
<include src="./common.wxml"></include>
```



### 1.3  wxss与rpx

wxss中的样式是支持CSS3.0的规范，所以在wxss中同样适用CSS3中的特性

wxss还拓展了一些新的功能，如：rpx单位

 rpx不是一个像素单位，是px单位的升级，可以实现响应式（类似rem布局）

 在小程序中将屏幕分辨率分成750份，因此1rpx 代表1份

 例如：

 在iphone6中的分辨率是375px, 但是真实分辨率是750px

 在iphone6中 1rpx = 1px

 但是，在iphone6中的Dpr(设备像素比)是2,因此1px = 2rpx;



```html
<view class="px">box1</view>
<view class="rpx">box2</view>
```



```css
/* pages/rpx/rpx.wxss */
.px {
  width: 100px;
  height: 100px;
  background-color: gold;
}
/* 在iphone6中的Dpr(设备像素比)是2,因此1px = 2rpx; */
.rpx {
  width: 200rpx;
  height: 200rpx;
  background-color: pink;
}
```



### 1.4  flex

flex布局也可以实现响应式。但是，需要给容器添加display: flex;



flex-direction: 	布局方向

​	row: 				 	 	从主轴的起点到终点渲染 

​	row-reverse: 		 	从主轴的终点到起点渲染

​	column: 					从侧轴的起点到终点渲染 

​	column-reverse:   	 从侧轴的终点到起点渲染



justify-content: 	   	决定成员在主轴上对齐方式

​	flex-start: 				从主轴的起点到终点渲染

​	flex-end: 				 从主轴的终点到起点渲染

​	center: 					居中 

​	space-around: 		成员之间均分空白，两侧贴边

​	space-evenly: 		 成员之间均分空白

​	space-between: 	 成员之间均分空白，两侧空白减半



align-items: 	决定成员在侧轴上对齐方式

​	flex-start: 	从侧轴的起点到终点渲染 

​	flex-end: 	从侧轴的终点到起点渲染

​	center: 		居中



align-content: 	设置多行

​	flex-start: 				从侧轴的起点到终点渲染

​	flex-end: 	 			从侧轴的终点到起点渲染

​	center: 					居中

​	space-around: 		成员之间均分空白，两侧成员空白减半

​	space-between:  	成员之间均分空白, 两侧成员贴边

​	space-evenly: 		 成员之间均分空白



### 1.5 open-data

`open-data` 是用于定义开放组件 开放用户的信息

在当前我们可以通过`wx.getUserInfo`获取用户的信息 ==不建议==

```js
  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    // 获取用户信息 不建议
    wx.getUserInfo({
      success(res) {
        console.log(res);
      }
    })
  },
```



小程序中建议我们使用open-data组件

​	 type: 获取用户的信息

​	 语法：user + userInfo

| groupName     |                拉取群名称                |
| ------------- | :--------------------------------------: |
| userNickName  |    用户昵称。不再返回，展示“微信用户”    |
| userAvatarUrl |    用户头像。不再返回，展示 [灰色头像    |
| userGender    |   用户性别。不再返回，将展示为空（“”）   |
| userCity      | 用户所在城市。不再返回，将展示为空（“”） |
| userProvince  | 用户所在省份。不再返回，将展示为空（“”） |
| userCountry   | 用户所在国家。不再返回，将展示为空（“”） |
| userLanguage  |  用户的语言。不再返回，将展示为空（“”）  |



详情查看：https://developers.weixin.qq.com/miniprogram/dev/component/open-data.html

```html
<!-- 获取用户数据 -->
<open-data type="userAvatarUrl"></open-data>
<!-- 用户的昵称 -->
<open-data type="userNickName"></open-data>
```



### 1.6 web-view

小程序是在js、css、html基础之上衍生出的一个框架是通过 `webView` 来渲染页面

所以，在小程序中提供了web-view组件用来引入一个html页面

使用方式：

​	 1 通过src定义引入的页面路径（必须支持https协议）

​	 2 要在后台进行配置 （可选）

-  https://mp.weixin.qq.com/

-  开发管理	开发设置	服务器域名	开始配置

![image-20230202161010203](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230202161010203.png)

​	 3 要关闭证书检验

​	![image-20230202160744382](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230202160744382.png)



搭建https服务器：http没有对请求的数据进行加密，可能在传递的过程中会被拦截或者注入广告，于是HTTPS就出现了，相比于http来更加安全 

​	 http协议 默认端口号 80 

​	https协议 默认端口号443



- 自己搭建的服务器页面

```js
// 引入express
const express = require('express');
// 引入http
const http = require('http');
// 引入https
const https = require('https');
// 引入fs
const fs = require('fs');

// 获取应用程
const app = express()

// 配置引擎
app.engine('html', require('ejs').__express)

// 渲染模板
app.get('/', (req, res) => {
  res.render('index.html')
})

// 创建http服务
http.createServer(app).listen(3000, () => { console.log(3000); })

// 创建https服务
// 读取秘钥
const key = fs.readFileSync('./ssl/private.pem')
const cert = fs.readFileSync('./ssl/file.crt')

https.createServer({ key, cert }, app).listen(3001, () => { console.log(3001); })
```





```html
<!-- 引入其他页面 -->
<!-- <web-view src="https://www.bilibili.com/?spm_id_from=333.337.0.0"></web-view> -->

<!-- 引入自己搭建的服务器页面 -->
<web-view src="https://localhost:3001/"></web-view>
```



### 1.7 wxs

wxs组件用于定义一个执行代码空间

使用方式：

​	 1 要创建一个`.wxs`后缀名称的文件

​		 在该文件中，不支持`es6`语法

```js
// 创建一个.wxs后缀名称的文件
// 在该文件中，不支持es6语法

var msg = 'hello msg'
var num = 100

// 求数组最大值
function getMax(arr) {
  // 获取第一个成员
  var min = arr[0]
  
  // 遍历数组
  for (var i = 0; i < arr.lengt; i++) {
    min = min > arr[i] ? min : arr[i]
  }

  return min
}

// 暴露接口
module.exports.msg = msg
module.exports.num = num
module.exports.getMax = getMax
```



​	 2 在wxml页面中通过`wxs`组件引入wxs文件

​		 通过`src`属性引入文件

​	 3 在wxs组件上通过`module`属性定义代码空间--模块名称

​	 4 使用插值语法调用wxs模块中暴露的功能（通过module属性值来调用）



```html
<!-- 
    通过 wxs 组件引入wxs文件
      通过src属性引入文件
	  在wxs组件上通过module属性定义代码空间--模块名称
-->

<wxs src="./demo.wxs" module="demo"></wxs>

<!-- 通过命名空间使用数据 -->
<view>{{demo.msg}}</view>
<view>{{demo.num}}</view>
<view>{{demo.getMax([451,124,12,42,2])}}</view>
```



## 二、小程序API

### 2.1 路由

在小程序中切换页面的方式有两种：

 第一种通过`navigator`组件实现页面的切换

​		 通过`url`属性定义要切换页面的地址

​				 页面地址建议使用绝对路径，==除了tab页面==，我们是可以添加`query`参数

​				 传递的数据，可以在==目标页面==的`load`方法中，通过参数获取

​		 通过`open-type` 定义打开页面的方式

​				 navigate是默认值

 第二种通过全局方法实现页面的切换

​		 参数url属性表示切换的页面地址



### 2.2 页面类型

在小程序中的页面分为两种：

​	 一类是在`pages`中定义的普通页面

​	 一类是在`tabbar`中定义的tab页面，

​	 它们之间切换的方式是不同的



`open-type` 打开页面的方式是不同的，有四种方式：

 	1 open-type=”navigate”   			用于打开普通页面	`默认值`

​	 2 open-type=”switchTab”    		 用于打开tab页面

​	 3 open-type=“navigateBack”   	 用于返回上一个页面

​	 4 open-type=”redirect” 				用于重定向到一个页面



```html
<!-- 定义路由组件 -->
<!-- URL 定义跳转的页面 建议使用绝对路径 -->
<navigator url="/pages/card/card"  open-type="navigate">跳转到card页面</navigator>

<!-- navigate 默认值 -->
<!-- 可以添加query参数
      传递的数据，可以在目标页面的load方法中，通过参数获取
 -->
<navigator url="/pages/rpx/rpx?a=1&b=2">跳转到 rpx 页面</navigator>

<!-- 跳转到tab页面 -->
<navigator  url="/pages/logs/logs" open-type="switchTab">跳转到tab页面</navigator>

<!-- 重定向 -->
<navigator url="/pages/index/index" open-type="redirect">重定向</navigator>
```

```html
<!-- 返回上一个页面 -->
<navigator open-type="navigateBack">返回上一个页面</navigator>
```



 传递的数据，可以在==目标页面==的`load`方法中，通过参数获取

```js
  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    console.log(options);
  },
```





### 2.3 路由方法

在当前我们可以通过 navigator 组件实现页面的跳转 不方便传递数据



在小程序中也提供了一套跳转页面的方法

- `navigateTo`方法 用于跳转一个普通页面

  ​				 在跳转页面的时候，可以传递query数据，在==跳转到的目标页面==中的`load`事件中获取数据

-  `switchTab`方法 用于跳转一个tab页面


- `navigateBack`方法 用于返回上一个页面

- `redirect`方法 重定向方法

​		 它们的使用方式都是一样的，通过url定义跳转的页面



```js
    // 通过路由方法实现页面跳转 同时传递query数据
    wx.navigateTo({
      url: `/pages/card/card?` + this.dealQuery(e.detail.value)
    })
```



### 2.5 同步与异步

在小程序中为很多异步操作提供了两种使用方式：

异步操作的同步写法：

​		 标志：会在所有的方法后面添加一个`Sync`名称

​		 特点：可以顺序书写，将结果保存在变量中，供后文使用，是一个阻塞的方法，通常放在`try catch`中

异步操作的异步写法：

​		 特点：将所有的逻辑放入回调函数中，是一个非阻塞的方法，通常有三个回调函数

​		 `success` 成功时执行的方法 

​			`fail` 失败时执行的方法

​		`complete` 完成时执行的方法

例如：获取系统信息的方法

​		 同步方法：getSystemInfoSync，为了保证防止错误出现让代码正常执行，要将语句放入到try catch中

​		 异步方法：getSystemInfo，异步获取用户信息，在回调函数中获取结果



### 2.6 本地存储

小程序的本地存储用的就是h5的本地存储

自身特点

​	 1 一个小程序中，对于一个用户来说，最多存储10m数据

​	 2 对于小程序来说，不同的用户之间的数据是不会共享的

​	 3 如果内存空间不足，微信会删除最近未使用的数据

对于数据的增删改查有同步和异步两种方法

设置数据：

​		 同步写法：`setStorageSync(key, value)` 

​			 `key`: 数据名称 

​		 	`value`: 数据值

​		 异步写法：`setStorage({ key, data, success, fail, complate })`

​			 key:  			   数据名称 

​			data:	 		   设置的数据 

​			complete:		完成时候执行的回调函数

​			 success: 		成功时候执行的回调函数 

​			 fail: 				失败时候执行的会得到函数

获取数据：

​		 同步写法：`getStorageSync(key, value)` 

​		 异步写法：`getStorage({ key, data, success, fail, complate })`

删除数据：

​		 同步写法：`removeStorageSync(key, value)` 

​		 异步写法：`removeStorage({ key, data, success, fail, complate })`

清空数据：

​		 同步写法：`clearStorageSync(key, value)` 

​		 异步写法：`clearStorage({ key, data, success, fail, complate })`

获取存储数据信息：

​		 同步写法：`getStorageInfoSync(key, value)` 

​		 异步写法：`getStorageInfo({ key, data, success, fail, complate })`



```js
/**
 * 生命周期函数--监听页面加载
 */
onLoad: function (options) {
    // 同步
    // 设置数据
    try {
        wx.setStorageSync('msg', 'hello');
        wx.setStorageSync('num', 100);
    } catch (err) {
        console.log(err)
    }

    // 异步
    // 获取数据
    wx.getStorage({
        // key 表示获取的数据名称
	// data: 设置的数据 
      key:'num',
      data:100,
        // success成功时候的回调
        success: res => {
            console.log(111, res);
        },
        // fail 失败时候的回调
        fail: res => {
            console.log(222, res);
        },
        // complete完成时候的回调 (不论成功还是失败都要执行)
        complete: res => {
            console.log(333, res);
        }
    })
},
```

### 2.7 案例

编辑提交数据

<img src="C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230202220959741.png" alt="image-20230202220959741" style="zoom: 80%;" />



#### app.json

```js
{
  "pages": [
    "pages/card/card",
    "pages/index/index",
  ],
  "window": {
    "backgroundTextStyle": "light",
    "navigationBarBackgroundColor": "#fff",
    "navigationBarTitleText": "Weixin",
    "navigationBarTextStyle": "black"
  },
  "style": "v2",
  "sitemapLocation": "sitemap.json",
  "lazyCodeLoading": "requiredComponents"
}
```



#### card

card.js

```js
// pages/card/card.js
Page({

  /**
   * 页面的初始数据
   */
  data: {
    // username:'张三',
    // tel:'12345678901',
    // email:'19421212@qq.com',
    // company:'北京市幼儿园',
    // job:'老师',
    // address:'北京市昌平区',
    // weibo:'zhangsan'
  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    // console.log(options);

    // 添加logo字段 更新用户姓名头像
    // options.logo = options.username && options.username[0] || ''

    // 更新数据
    // this.setData(options)

    // 读取本地数据
    wx.getStorage({
      key: 'card',
      success:({data}) => {
        // 添加logo字段 更新用户姓名头像
        data.logo = data.username && data.username[0] || ''

        // 存储数据
        this.setData(data)
      }
    })
  },

  /**
   * 生命周期函数--监听页面初次渲染完成
   */
  onReady: function () {

  },

  /**
   * 生命周期函数--监听页面显示
   */
  onShow: function () {

  },

  /**
   * 生命周期函数--监听页面隐藏
   */
  onHide: function () {

  },

  /**
   * 生命周期函数--监听页面卸载
   */
  onUnload: function () {

  },

  /**
   * 页面相关事件处理函数--监听用户下拉动作
   */
  onPullDownRefresh: function () {

  },

  /**
   * 页面上拉触底事件的处理函数
   */
  onReachBottom: function () {

  },

  /**
   * 用户点击右上角分享
   */
  onShareAppMessage: function () {

  }
})
```



card.wxml

```html
<view class="card">
  <view class="username">
    <text class="user">{{username}}</text>
    <text class="job">{{job}}</text>
  </view>
  <view class="tel">{{tel}}</view>
  <view class="company">{{company}}</view>
  <view class="address">{{address}}</view>
  <view class="email">{{email}}</view>
  <view class="weibo">{{weibo}}</view>
  <!-- 没有数据不创建 -->
  <view class="logo" wx:if="{{logo}}">{{logo}}</view>
</view>


<!-- 点击按钮实现跳转 -->
<navigator url="/pages/index/index">
  <button type="primary">编辑卡片</button>
</navigator>
```



card.wxss

```css
/* pages/card/card.wxss */
.card {
  padding: 10px;
}

.username {
  font-size: 18px;
}

.user {
  margin-right: 15px;
}

.job {
  font-size: 14px;
  color: #666;
}

.tel {
  font-size: 22px;
  color: skyblue;
  padding: 5px 0;
}

.address,
.company,
.email,
.weibo {
  font-size: 12px;
  color: #555;
  line-height: 22px;
}

.logo {
  position: fixed;
  right: 30px;
  top: 20px;
  width: 50px;
  height: 50px;
  background-color: #f30;
  line-height: 50px;
  text-align: center;
  color: #fff;
  font-weight: bold;
  border-radius: 50%;
  font-size: 20px;
}
```



#### index

index.js

```js
Page({

  /**
   * 页面的初始数据
   */
  data: {
    username: '',
    tel: '',
    email: '',
    company: '',
    job: '',
    address: '',
    weibo: ''

    // username: '张三',
    // tel: '12345678901',
    // email: '19421212@qq.com',
    // company: '北京市幼儿园',
    // job: '老师',
    // address: '北京市昌平区',
    // weibo: 'zhangsan'
  },

  // 定义处理query数据的方法
  dealQuery(obj) {
    // console.log(obj);

    // 实现将对象转为query参数的形式
    // 定义结果变量
    let str = ''
    // 遍历对象
    for (let key in obj) {
      // 拼接
      str += `&${key}=${obj[key]}`
    }

    // 截取并返回结果
    return str.slice(1)
  },

  // 定义接收数据的方法
  receiveData(e) {
    // console.log(e);

    // 通过路由方法实现页面跳转 同时传递query数据
    // wx.navigateTo({
      // url: `/pages/card/card?` + this.dealQuery(e.detail.value)
    // })

    // 设置本地存储数据
    wx.setStorage({
      key:'card',
      data:e.detail.value,
      success(res){
        // 跳转页面
        wx.navigateTo({
          url: '/pages/card/card',
        })
      }
    })
  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {

  },

  /**
   * 生命周期函数--监听页面初次渲染完成
   */
  onReady: function () {

  },

  /**
   * 生命周期函数--监听页面显示
   */
  onShow: function () {

  },

  /**
   * 生命周期函数--监听页面隐藏
   */
  onHide: function () {

  },

  /**
   * 生命周期函数--监听页面卸载
   */
  onUnload: function () {

  },

  /**
   * 页面相关事件处理函数--监听用户下拉动作
   */
  onPullDownRefresh: function () {

  },

  /**
   * 页面上拉触底事件的处理函数
   */
  onReachBottom: function () {

  },

  /**
   * 用户点击右上角分享
   */
  onShareAppMessage: function () {

  }
})
```



index.wxml

```html
<!-- 定义表单 -->
<form bindsubmit="receiveData">
  <view class="list">
    <view class="item"><label for="">姓名:</label><input type="text" name="username" value="{{username}}"
        placeholder="请输入姓名" /></view>
    <view class="item"><label for="">手机:</label><input type="text" name="tel" value="{{tel}}" placeholder="请输入手机" />
    </view>
    <view class="item"><label for="">邮箱:</label><input type="text" name="email" value="{{email}}" placeholder="请输入邮箱" />
    </view>
    <view class="item"><label for="">单位:</label><input type="text" name="company" value="{{company}}"
        placeholder="请输入单位" /></view>
    <view class="item"><label for="">职位:</label><input type="text" name="job" value="{{job}}" placeholder="请输入职位" />
    </view>
  </view>

  <view class="list">
    <view class="item"><label for="">地址:</label><input type="text" name="address" value="{{address}}"
        placeholder="请输入地址" /></view>
    <view class="item"><label for="">微博:</label><input type="text" name="weibo" value="{{weibo}}" placeholder="请输入微博" />
    </view>
  </view>
  
  <button type="primary" form-type="submit">提交</button>

  <!-- 点击按钮实现跳转 -->
  <!-- <navigator url="/pages/card/card">
    <button type="primary" form-type="submit">提交</button>
  </navigator> -->
</form>
```



index.wxss

```css
page {
  background-color: #efefef;
}

.list {
  background-color: #fff;
  margin-bottom: 20px;
  padding: 0 10px;
}

.item {
  display: flex;
  border-bottom: 1px solid #ccc;
  line-height: 40px;
  align-items: center
}

.item label {
  margin-right: 10px;
}
```

