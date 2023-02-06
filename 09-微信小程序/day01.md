# day 01

## 一、微信小程序

### 1.1 小程序简介

微信小程序，简称小程序，英文名 Mini Program ，是一种不需要下载安装即可使的应用，它实现了应用 “触手可及”的梦想，只需通过扫一扫或搜一下即可打开应用



#### 1.1.1 为什么使用小程序

1.微信有海量用户，而且粘性很高，在微信⾥开发产品更容易触达用户 ；

2.推广app 或公众号的成本太高。

3.开发适配成本低。 

4.容易小规模试错，然后快速迭代。

5.跨平台



#### 1.1.2 微信小程序历史

​	2016年1月11日，微信之父张小龙时隔多年的公开亮相，解读了微信的四大价值观。张小龙指出， 越来越多产品通过公众号来做，因为这里开发、获取用户和传播成本更低。拆分出来的服务号并没有提供更好的服务，所以微信内部正在研究新的形态，叫「微信小程序」 需要注意的是，之前是叫做应用号 

​	2016年9月21日，微信⼩程序正式开启内测。在微信生态下，触手可及、用完即走的微信小程序引起⼴泛关注。腾讯云正式上线微信小程序解决方案，提供⼩程序在云端服务器的技术方案。 

​	2017年1月9日，微信推出的“小程序”正式上线。“小程序”是一种无需安装，即可使用的手机“应通用”。不需要像往常一样下载App，用户在微信中“用完即走”。



#### 1.1.3 疯狂的微信小程序

 1. 微信月活已经达到10.82亿。其中55岁以上的用户也达到6300万 

 2. 信息传达数达到450亿，较去年增长18%;视频通话4.1亿次,增长100% 

 3. 小程序覆盖超过200+行业，交易额增长超过6倍，服务1000亿+人次,创造出了5000亿+的商业价值

    

#### 1.1.4 其它小程序

还有其他的⼩程序 不容忽视:
​       1. 支付宝小程序 
​       2. 百度小程序 
​       3. QQ小程序 
​       4. 今日头条 + 抖音小程序

其他优秀的第三方小程序 
    拼多多、滴滴出行 、欢乐斗地主、智能火车票、唯品会。。。

环境准备 :

​    开发微信小程序之前，必须要准备好相应的环境



#### 1.1.5小程序注册

注册账号：进入https://mp.weixin.qq.com/，点击立即注册按钮，选择小程序.输入相关信息， 在信息登记中，选择个人账户，输入身份信息。注册完成，进入设置页面，选择开发设置，获取`appid`

开发文档：

​		https://developers.weixin.qq.com/miniprogram/dev/reference/

开发工具下载地址：

​		 https://developers.weixin.qq.com/miniprogram/dev/devtools/devtools.html



### 1.2 目录部署

![image-20230201150725698](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230201150725698.png)

pages   用于配置页面的

​		 index  是首页 每一个文件里面都有四个文件

​				 index.js: 脚本文件、index.json: 配置文件、index.wxml: 模板文件、index.wxss: 样式文件

​		 logs    日志页面

utils    工具插件

app.js   应用程序脚本文件

app.json 应用程序配置文件

app.wxss 全局样式配置文件

project.config.json 全局项目配置文件

#### 取消 `sitemap` 提示

![image-20230202173005644](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230202173005644.png)

```js
    "checkSiteMap":false
```



### 1.3 应用配置

app.json 是全局的应用配置文件，在该文件下要严格遵守json语法(逗号，单引号，双引号都需要注意) “pages” 用于配置页面，是一个数组，数组中的每一项都是一个页面，数组中的第一项表示首页

 "window"：  用于配置窗口的

  		"`backgroundTextStyle`":  字体样式

​		  "`navigationBarBackgroundColor`":  背景颜色

 		 "`navigationBarTitleText`":  提示文字

​		  "`navigationBarTextStyle`":  导航栏文字样式

 		"`enablePullDownRefresh`" : true 页面向下拖动

```json
{
  "usingComponents": {},
  "navigationBarBackgroundColor": "#000",
  "navigationBarTextStyle": "white",
  "navigationBarTitleText": "指南针"
}
```



 "`tabBar`":  用于配置页面下方的icon

​		 list： 用于配置icon 值是一个数组 数组中的每一项都是一个对象 通常 2-5

​				 "pagePath":  对应的是一个页面

​				 "text":  文字

​				 "iconPath":  图标的路径

​				 "selectedIconPath":  选中时候图标的路径



 "networkTimeout":   配置网络请求时间

​		  "request":  请求时间

​		  "connectSocket":  socket时间,

 		 "uploadFile":   上传时间

​		  "downloadFile":  下载时间

```json
{
  "pages":[
     "pages/index/index",
    "pages/demo/demo",
    "pages/logs/logs"
  ],
  "window":{
    "backgroundTextStyle":"light",
    "navigationBarBackgroundColor": "#000",
    "navigationBarTitleText": "微信",
    "navigationBarTextStyle":"white"
  },
  "tabBar": {
    "list": [{
      "pagePath": "pages/index/index",
      "text": "首页",
      "iconPath": "images/logo.png",
      "selectedIconPath": "images/logo.png"
    }, {
      "pagePath": "pages/demo/demo",
      "text": "demo",
      "iconPath": "images/logo.png",
      "selectedIconPath": "images/logo.png"
    }, {
      "pagePath": "pages/logs/logs",
      "text": "日志",
      "iconPath": "images/logo.png",
      "selectedIconPath": "images/logo.png"
    }]
  },
  "networkTimeout": {
    "request": 20000,
    "connectSocket": 20000,
    "uploadFile": 20000,
    "downloadFile": 20000
  },
  "style": "v2",
  "sitemapLocation": "sitemap.json"
}
```



### 1.4 项目配置

project.config.json 全局项目配置文件

 在内部提供了大量信息：

 例如： `appid`就在其中

 类似于npm 中的package.json



### 1.5 应用程序

app.wxss  是全局样式配置文件（与css语法类似）



app.js    是应用程序脚本文件

 在应用程序中提供了一套周期方法

​	 `onLaunch`：初始化的时候执行的方法

​	 `onShow`:  进入前台时候执行的方法 

​	 `onHide`:  进入后台时候执行的方法

​	 `onError`： 出错时候执行的方法



```js
App({

  /* 当小程序初始化完成时，会触发 onLaunch（全局只触发一次） */
  onLaunch: function () {
    console.log('onLaunch');

    // 抛出错误
    // throw new Error('出错...')
  },

  /* 当小程序启动，或从后台进入前台显示，会触发 onShow */
  onShow: function (options) {
    console.log('onShow');
  },

  /* 当小程序从前台进入后台，会触发 onHide */
  onHide: function () {
    console.log('onHide');
  },

  /* 当小程序发生脚本错误，或者 api 调用失败时，会触发 onError 并带上错误信息*/
  onError: function (msg) {
    console.log('onError');
  }
})
```



### 1.6 内置方法

wx.login: 登录成功之后执行的方法

​	 参数是一个对象， 对象中有一个success方法

wx.getSetting： 获取授权的方法

​	 参数是一个对象， 对象中有一个success方法

wx.getUserInfo: 获取用户信息

​	 参数是一个对象， 对象中有一个success方法



```js
  // 获取用户信息的方法
    wx.getUserInfo({
      success() {
        console.log(arguments);
      }
    })
```



### 1.7 全局方法

APP:  创建应用程序的方法

getAPP: 获取应用程序的方法

Page:  创建页面的方法

getCurrentPages: 获取当前页面的方法



### 1.8 页面周期方法

onLoad： 页面加载方法

onReady：  页面渲染方法

onShow： 监听页面显示的方法

onHide: 页面隐藏方法

onUnload： 页面卸载方法

onPullDownRefresh：  监听用户下拉动作

onReachBottom：  监听页面触底方法

onShareAppMessage： 监听分享方法

```js

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    console.log('onLoad');
  },

  /**
   * 生命周期函数--监听页面初次渲染完成
   */
  onReady: function () {
    console.log('onReady');
  },

  /**
   * 生命周期函数--监听页面显示
   */
  onShow: function () {
    console.log('onShow');
  },

  /**
   * 生命周期函数--监听页面隐藏
   */
  onHide: function () {
    console.log('onHide');
  },

  /**
   * 生命周期函数--监听页面卸载
   */
  onUnload: function () {
    console.log('onUnload');
  },

  /**
   * 页面相关事件处理函数--监听用户下拉动作
   */
  onPullDownRefresh: function () {
    console.log('onPullDownRefresh');
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
    console.log('onShareAppMessage');
  }
})
```





### 1.9 页面渲染

**数据渲染原理**

​	 在小程序中，视图与脚本是在不同的线程中执行的

​			 视图： 通过WebView渲染

​			 脚本： 通过另一个线程执行的



**数据驱动**

​	 小程序是基于MVVM模式实现的，因此实现了数据驱动，定义什么数据就渲染什么数据

​	 使用方式：

​		 1 在脚本文件中通过data属性定义数据

​		 2 在页面中通过插值语法渲染数据



**插值语法**

 	语法： `{{数据}}`
 	
 			小程序提供的插值语法是一个伪js环境，内部只能使用一些简单的操作

​		 	例如：**加减乘除、对象的点语法、数组中括号语法都支持，但是对于一些方法就不支持**



**数据丢失**

 数据发生改变，视图就会同步更新。如果数据改变了，但是视图没有更新，此时我们就说数据丢失了。我们不要直接修改data中的数据，

为了避免数据丢失，要通过`setData`方法去修改



**更新数据**

在脚本文件中提供了一个`setData`方法用于更新数据的，参数是一个对象

​		 key： 是要更新的数据名称，可以是直接属性，也可以`间接属性`

 				例如：let obj = { a: { b: { c: 123 } } } 

​						a是obj的直接属性，而b、c都是obj的间接属性

​				 更新数据的时候：更新a key就是a，更新b key就是a.b，更新c key就是 a.b.c 

​		value: 表示新的数据值

​		 工作中：尽量只更新需要更新的数据



```js
<view>hello demo</view>

<!-- 
    插值语法：{{}}
        微信插值语法并没有提供一个真正的Js环境 （伪Js环境）
        对象获取属性的点语法 数组获取成员的中括号语法
        只能运行简单的运算(加减乘除 三元)
        不支持语句(内置的方法)
        组件的属性和内容都可以使用插值语法
 -->

<view>{{msg}}</view>

<!-- 对象的点语法 数组获取成员的中括号语法 -->
<view>{{size.width}}</view>
<view>{{size.height}}</view>

<!-- 数组读取下标 -->
<view>{{colors[1]}}</view>

<!-- 简单运算 -->
<view>{{size.width + size.height}}</view>
<view>{{size.width - size.height}}</view>
<view>{{size.width * size.height}}</view>
<view>{{size.width / size.height}}</view>

<!-- 不支持语句 -->
<view>{{msg.toUpperCase()}}</view>

<!-- 小程序中组件的属性可以使用插值 -->
<view class='{{cls}}'>hello box</view>

<!-- 修改数据 -->
<view>{{msg}}</view>
<view>{{size.width}}</view>
<view>{{size.height}}</view>
```



```js
onLoad: function (options) {
    // console.log('onLoad');

    // 直接访问data数据
    console.log(this.data.msg);
    console.log(this.data.colors);

    // 直接修改数据 此时数据修改了 但是视图没有更改 数据丢失
    // this.data.msg = 'hello 修改数据'
    // console.log(this.data.msg);

    // 为了避免数据丢失 使用setData()
    this.setData({
      msg: 'hello 修改数据',
      // 直接进行修改 会覆盖原来的数据 
      // size:{
      //   width:1000
      // }
      // 使用 间接子属性
      'size.width' :1000
    })
  },
```



### 1.10 WXML

wxml是模仿html创建的一种文件格式

 html是可以被浏览器识别的

 但是wxml是小程序封装之后的，称为组件

所以，wxml中的组件是不适用于浏览器中使用的，

 例如web端的一些框架：jQuery框架就不适用去操作wxml中的组件



### 1.11 属性

html中可以为每一个元素添加属性。wxml中也是可以为每一个组件添加属性，一共分为两种：

 第一种 可以为所有组件添加的属性 ，称为共有属性

​		 id:   添加id

​		 class: 添加类名

​		 style: 添加样式

​		 hidden: 隐藏组件

​		 data-name: 添加自定义数据

​		 bind|catch： 添加事件

```html
<!-- 
共有属性
		 id:   添加id
		 class: 添加类名
		 style: 添加样式
		 hidden: 隐藏组件
		 data-name: 添加自定义数据
		 bind|catch： 添加事件
 -->
<view id='box'>hello 共有属性</view>
<view class="box1">hello 共有属性</view>
<view style="color:gold ;font-size:24px">hello 共有属性</view>

<!-- hidden 隐藏组件 属性值是布尔值 true表示隐藏 -->
<view hidden='{{true}}'>hello 共有属性</view>

<!-- 自定义数据 data-name -->
<view data-id='box2'>自定义数据</view>
```



 第二种 可以为指定的组件添加的属性，称为特有属性



### 1.12 事件

在wxml中可以为每一个组件添加事件（`添加的事件要在脚本文件中定义出来`），一共分为两种：

​		 第一种 为每一个组件添加的事件，称为共有事件

​				 共有事件：touchstart、touchmove、touchend、touchcancel、tap、longTap(350ms)

 		第二种 为了特定的组件添加的事件，称为特有事件

按照冒泡和非冒泡，事件又分为两种：冒泡事件与非冒泡事件

​		 冒泡：事件执行的时候，从子组件传递到父组件的过程

​		 我们可以通过bind或者是catch为组件绑定事件

​				 `bind`绑定的事件 会冒泡

​				 `catch`绑定的事件 不会冒泡

**冒泡事件**

​	touchstart： 触摸开始 

​	touchmove： 触摸中

​	touchend：   触摸结束 

​	touchcancel： 手指触摸动作被打断，如来电提醒，弹窗

​	tap： 轻拍

​	longpress： 替代了longTap事件

​	transitionend： 过度结束事件

​	animationstart:    动画开始

​	animationend:  动画结束

​	animationiteration：  迭代一次之后触发

​	touchforcechange： 强制touch



```js
  rootHandle(e) {
    // 获取到的手指信息位置 距离顶部不包括导航栏位置
    console.log('rootHandle',e.touches[0].clientX,e.touches[0].clientY);

    // 通过dataset.xxx 获取自定义数据
    console.log(e.target.dataset.name);
    // console.log(e.currentTarget.dataset.name);
  },
  childHandle() {
    console.log('childHandle');
  },
  innerHandle() {
    console.log('innerHandle');
  },
```



```html
<!-- 
  通过 bind 或 catch 为组件绑定事件 添加的事件要在脚本文件中定义出来
     bind 冒泡
     catch 不会冒泡
 -->

 <!-- 通过bind绑定 -->
<!-- <view class="root " bindtap="rootHandle">
  root
  <view class="child " bindtap="childHandle">
    child
    <view class="inner " bindtap="innerHandle">
      inner
    </view>
  </view>
</view> -->

<!-- 通过catch绑定 -->
<view class="root " catchtap="rootHandle">
  root
  <view class="child " catchtap="childHandle">
    child
    <view class="inner " catchtap="innerHandle">
      inner
    </view>
  </view>
</view>
```



**事件对象**

​	changedTouches: 改变的触屏手指对象

​	touches：   触发事件的手指对象

​				以上两个属性的属性值都是数组，成员是对象

​					clientX|Y 		距离可视区域左上角坐标的距离（不包含导航）

​					pageX|Y 		距离页面左上角坐标的距离（不包含导航)

​	detail: 手指的信息

​	 dataset属性用于获取自定义数据



​	currentTarget: 绑定事件的组件对象

​	target: 触发事件的组件对象

​		 dataset属性用于获取自定义数据



```js
  rootHandle(e) {
    // 获取到的手指信息位置 距离顶部不包括导航栏位置
    console.log('rootHandle',e.touches[0].clientX,e.touches[0].clientY);

    // 通过 e.target.dataset.xxx 获取自定义数据
    console.log(e.target.dataset.name);
    // console.log(e.currentTarget.dataset.name);
  },
```



### 1.13 view 组件

view组件是视图组件

​	 类似于html中的div元素，是容器组件，会独占一行

​	 在view组件中有几组特定的属性：

​			 hover-class:  点击时候添加的类

​			 hover-start-time:   延迟类的添加时间

​			 hover-stay-time: 类停留的时间

​			 hover-stop-propagation:  阻止冒泡



```html
<!-- 类似于html中div元素  独占一行-->
<!-- 
	 在view组件中有几组特定的属性：
			 hover-class:             点击时候添加的类
			 hover-start-time:        延迟类的添加时间
			 hover-stay-time:         类停留的时间
			 hover-stop-propagation:  阻止冒泡
 -->
<view class="root " >
  root
  <view class="child " hover-class="box" hover-stay-time="2000">
    child
    <view class="inner " hover-stop-propagation>
      inner
    </view>
  </view>
</view>
```



### 1.14 text 组件

text是文本组件，不会独占一行

​	  通过space设置空格模块 如果没有设置该属性 则默认是空白折叠

​			 ensp:   中文字符空格一半的大小

​			 emsp:  中文字符空格大小

​			 nbsp:   根据字体设置空白大小

​	 selectable： 选中文本

​	 decode:   转码

```html
<view>
  <text>hello text</text>
</view>
<!-- text组件文本组件 类似于html文件中的span元素 -->
<!-- 
  text是文本组件，不会独占一行
	  通过space设置空格模块 如果没有设置该属性 则默认是空白折叠
			 ensp:   中文字符空格一半的大小
			 emsp:  中文字符空格大小
			 nbsp:   根据字体设置空白大小
	 selectable： 选中文本
	 decode:   转码
 -->
<view>
  <text space="ensp">hello text</text>
</view>
<view>
  <text space="emsp">hello text</text>
</view>
<view>
  <text space="nbsp">hello text</text>
</view>

<view>
    <text decode>&gt; &lt;</text>
</view>
<view>
  <text>&gt; &lt;</text>
</view>
```



### 1.15 icon 组件

用于定义图标的组件

​	图片的资源可能很大，在小程序中提供了icon组件，用于代替图片的引入

​	type 定义图标的类型

​		success, success_no_circle, info, warn, waiting, cancel, download, search, clear

​	size： 	定义图标的大小	

​	color: 		定义图标的颜色



```html
<!-- 
  type 定义图标的类型
    success, success_no_circle, info, warn, waiting, cancel, download, search, clear
    
    size： 	定义图标的大小	

    color: 		定义图标的颜色
 -->
<view>
  <icon type="success" size="42"></icon>
</view>
<view>
  <icon type="success_no_circle" size="36px"></icon>
</view>
<view>
  <icon type="info" color="gold" size="68"></icon>
</view>
<icon type="warn"></icon>
<icon type="waiting"></icon>
<icon type="cancel"></icon>
<icon type="download"></icon>
<icon type="search"></icon>
<icon type="clear"></icon>
```



### 1.16 image 组件

`image`  类似于html中的img标签，通过src定位图片的资源

 支持本地图片，也支持线上的图片（必须支持HTTPS）

 通过`mode`属性，定义裁剪、缩放模式：



 left: 裁剪模式 显示图片的左边部分 

right: 裁剪模式 显示图片的右边部分

 top:   裁剪模式 显示图片的上边部分 

bottom: 裁剪模式 显示图片的底部部分



 scaleToFill： 缩放模式 拉伸图片

 aspectFit：  缩放模式  完整的显示图片

 aspectFill： 缩放模式 在一个方向显示完整的图片 另一个方向会截取图片

 widthFix：  缩放模式 保持原图宽高比不变



```html
<!-- 
  类似于html中的img标签，通过src定位图片的资源
  支持本地图片，也支持线上的图片（必须支持HTTPS）

  通过mode属性，定义裁剪、缩放模式：

    left 裁剪模式 显示图片的左边部分 
    right 裁剪模式 显示图片的右边部分
    top  裁剪模式 显示图片的上边部分 
    bottom 裁剪模式 显示图片的底部部分

    scaleToFill 缩放模式 拉伸图片
    aspectFit  缩放模式  完整的显示图片
    aspectFill 缩放模式 在一个方向显示完整的图片 另一个方向会截取图片
    widthFix  缩放模式 保持原图宽高比不变
 -->
<view>
  <image src="https://t7.baidu.com/it/u=2203816221,2375101800&fm=193&f=GIF"></image>
</view>

<!-- 裁剪模式 -->
<view>
  <image src="https://t7.baidu.com/it/u=2203816221,2375101800&fm=193&f=GIF" mode="top"></image>
</view>
<view>
  <image src="https://t7.baidu.com/it/u=2203816221,2375101800&fm=193&f=GIF" mode="right"></image>
</view>
<view>
  <image src="https://t7.baidu.com/it/u=2203816221,2375101800&fm=193&f=GIF" mode="bottom"></image>
</view>
<view>
  <image src="https://t7.baidu.com/it/u=2203816221,2375101800&fm=193&f=GIF" mode="left"></image>
</view>

<!-- 缩放模式 -->
<view>
  <image src="https://t7.baidu.com/it/u=2203816221,2375101800&fm=193&f=GIF" mode="scaleToFill"></image>
</view>
<view>
  <image src="https://t7.baidu.com/it/u=2203816221,2375101800&fm=193&f=GIF" mode="aspectFit"></image>
</view>
<view>
  <image src="https://t7.baidu.com/it/u=2203816221,2375101800&fm=193&f=GIF" mode="aspectFill"></image>
</view>
<view>
  <image src="https://t7.baidu.com/it/u=2203816221,2375101800&fm=193&f=GIF" mode="widthFix"></image>
</view>
```



### 1.17 地图组件

map组件是用于定义地图的

​	longitude:	设置经度

​	latitude:		设置纬度



- 在 app.json 配置

  ```js
    "permission": {
      "scope.userLocation": {
        "desc": "你的位置信息将用于小程序位置接口的效果展示" 
      }
    },
  ```

- 获取经纬度

  ```js
    /**
     * 生命周期函数--监听页面加载
     */
    onLoad: function (options) {
      // 获取经纬度
      wx.getLocation({
        success(res) {
          console.log(res);
        }
      })
    },
  ```

- ```html
  <map latitude='40.22077' longitude='116.23128'></map>
  ```



### 1.18 媒体组件

video	定义视频组件通过

​		src	定义视频资源路径

camera:	开启摄像头

​		device-postioin:	开启摄像头朝向

```html
<video src="http://wxsnsdy.tc.qq.com/105/20210/snsdyvideodownload?filekey=30280201010421301f0201690402534804102ca905ce620b1241b726bc41dcff44e00204012882540400&bizid=1023&hy=SH&fileparam=302c020101042530230204136ffd93020457e3c4ff02024ef202031e8d7f02030f42400204045a320a0201000400"></video>
```



### 1.19 表单组件

form 定义表单组件

​	 可以为form添加submit事件，具体的数据在事件对象中的detail.value中获取



input 定义输入框的

​	 placeholder: 定义显示的内容

​	 password: 以密码的形式展示



label 定义标题的

​	 for  关联控件的



checkbox 定义多选框，每一组多选框必须放入checkbox-group中



radio  定义单选框，每一组单选框必须放入radio-group中



textarea 定义文本域



button   定义按钮的

​	 type  定义按钮的类型

​		 primary 绿色

​		 wran 红色

​	 form-type  定义按钮提交的方式

​		 submit 提交

​		 reset 重置+



```js
  // 定义获取表单数据的事件函数
  receiveData(e) {
    console.log(e);
  },
```



```html
<!-- 定义表单 -->
<form bindsubmit="receiveData">
  <!-- 定义输入框 -->
  <view>
    <label for="">请输入用户名：</label>
    <!-- input 标签闭合符号 -->
    <input type="text" placeholder="请输入用户名" name="username" />
  </view>

  <view>
    <label for="">请输入密码：</label>
    <input type="text" password placeholder="请输入密码" name="password" />
  </view>

  <!-- 定义多选框 -->
  <checkbox-group name="hobby">
    <label for="">请选择兴趣爱好：</label>
    <checkbox value="basketball">篮球</checkbox>
    <checkbox value="football">足球</checkbox>
    <checkbox value="badminton">羽毛球</checkbox>
  </checkbox-group>

  <!-- 定义单选框 -->
  <radio-group name="sex">
    <label for="">请选择性别：</label>
    <radio value="man">男</radio>
    <radio value="women">女</radio>
  </radio-group>

  <!-- 开关组件 -->
  <switch>是否同意该协议</switch>

  <!-- 滑动选择器 -->
  <slider value="10" show-value></slider>

  <!-- 文本域 -->
  <textarea name="txt" id="" cols="30" rows="10"></textarea>


  <!-- 按钮 -->
  <!-- 
    获取提交数据  detail value
        给表单元素添加name字段
        多选框 单选框 子选项添加value字段
   -->
  <button type="primary" form-type="submit">提交</button>
  <button type="warn" form-type="reset">重置</button>
</form>
```



### 1.20 swiper 组件

swiper	是用于制作轮播图的

​	indicator-dots:				开启小圆点

​	indicator-color:				小圆点的颜色

​	indicator--avtive-color:	选中时候小圆点的颜色

​	autoplay:						  自动播放

​	interval:							定义间隔时间

​	circular:							是否衔接

swiper--item:				子组件，用于定义每一帧图片或页面



```js
<view>--------------制作轮播图-----------------</view>
<swiper indicator-dots indicator-color="#fff" indicator-active-color="#000" autoplay circular interval="2000">
  <swiper-item>
    <image src="https://t7.baidu.com/it/u=1956604245,3662848045&fm=193&f=GIF"></image>
  </swiper-item>
  <swiper-item>
    <image src="https://t7.baidu.com/it/u=1956604245,3662848045&fm=193&f=GIF"></image>
  </swiper-item>
  <swiper-item>
    <image src="https://t7.baidu.com/it/u=1956604245,3662848045&fm=193&f=GIF"></image>
  </swiper-item>
  <swiper-item>
    <image src="https://t7.baidu.com/it/u=1956604245,3662848045&fm=193&f=GIF"></image>
  </swiper-item>
</swiper>
```



### 1.20  条件指令

指令：一种特殊的属性，为了实现某种功能

 在小程序中使用的语法： wx：来使用

条件指令

 指令名称：

 	wx:if=“condition”

 如果在条件中出现了变量，我们要使用插值语法

 是真正的创建与删除，不是控制元素的显隐



```js
  /**
   * 页面的初始数据
   */
  data: {
    isShow: false
  },

  // 点击切换
  toggle() {
    // 通过setData  取反处理
    this.setData({
      isShow:!this.data.isShow
    })
  },
```

```html
<!-- 定义按钮控制组件显影 -->
<button type="primary" bindtap="toggle">切换</button>
<!-- 使用hidden 属性实现 -->
<view hidden="{{isShow}}">hello view111</view>

<!-- 使用wx:if 指令实现 -->
<!-- 是真正的创建与删除 不是控制元素的显影 -->
<view wx:if="{{isShow}}">hello view222</view>
```



### 1.21 循环指令

指令名称：

 	`wx:for="data"`

​	 data表示要遍历的数据：可以是数字可以是数组

​	 但是在遍历的时候，一定要设置`wx:key` 保证数据的唯一性，通常我们可以设置id或者是this

>  如果列表中项目的位置会动态改变或者有新的项目添加到列表中，并且希望列表中的项目保持自己的特征和状态（如 input 中的输入内容，switch 的选中状态），需要使用 wx:key 来指定列表中项目的唯一的标识符。
>
> 
>
>  wx:key 的值以两种形式提供
>
> ​	字符串，代表在 for 循环的 array 中 item 的某个 property，该 property 的值需要是列表中唯一的字符串或数字，且不能动态改变。
>
>  ​	保留关键字 *this 代表在 for 循环中的 item 本身，这种表示需要 item 本身是一个唯一的字符串或者数字。
>
>  当数据改变触发渲染层重新渲染的时候，会校正带有 key 的组件，框架会确保他们被重新排序，而不是重新创建，以确保使组件保持自身的状态，并且提高列表渲染时的效率。
>
> 
>
> 如不提供 wx:key，会报一个 warning， 如果明确知道该列表是静态，或者不必关注其顺序，可以选择忽略。

 在循环指令中:

 	默认的成员变量名称  item
 			默认的索引值变量名称   index 



 通过指令改变默认的成员以及索引值变量名称：

​	 wx:for-item：要改变的成员名称 

​	 wx:for-index：要改变的索引值名称

```html
<!-- 
 在循环指令中:
 	默认的成员变量名称  item
	默认的索引值变量名称   index 
 -->
<view wx:for="{{num}}">{{item}}--{{index}}</view>

<!-- 
 通过指令改变默认的成员以及索引值变量名称：
	 wx:for-item：要改变的成员名称 
	 wx:for-index：要改变的索引值名称
 -->

<!-- 
  wx:key 的值以两种形式提供

  字符串，代表在 for 循环的 array 中 item 的某个 property，该 property 的值需要是列表中唯一的字符串或数字，且不能动态改变。
  保留关键字 *this 代表在 for 循环中的 item 本身，这种表示需要 item 本身是一个唯一的字符串或者数字。
当数据改变触发渲染层重新渲染的时候，会校正带有 key 的组件，框架会确保他们被重新排序，而不是重新创建，以确保使组件保持自身的状态，并且提高列表渲染时的效率。
  -->
<view wx:for="{{num}}" wx:key="*this" wx:for-item='a' wx:for-index='b'>{{a}}--{{b}}</view>

<!-- 遍历数组 -->
<view wx:for="{{colors}}" wx:key="*this">{{item}}--{{index}}</view>

<!-- 实现九九乘法表 -->
<view wx:for="{{9}}" wx:key="*this" wx:for-item="i">
  <text wx:for="{{9}}" wx:key="*this" wx:for-item="j" wx:if="{{i>=j}}">{{(i+1)}} * {{(j+1)}} = {{(i+1) * (j+1)}}</text>
</view>
```



### 1.22 block

block用于定义模板组件

在该组件中可以添加样式、添加指令，但是就是不会渲染出来（可以作为模板组件使用)



类似 `block wx:if`，也可以将 `wx:for` 用在`<block/>`标签上，以渲染一个包含多节点的结构块。例如：

```html
<!-- 为了不引入多余的组件 可以使用block组件 -->
<block wx:for="{{9}}" wx:key="*this" wx:for-item="i">
    <text wx:for="{{9}}" wx:key="*this" wx:for-item="j" wx:if="{{ i >= j }}">{{ i + 1 }} * {{ j + 1 }} = {{(i + 1) * (j + 1) }} ||</text>
</block>
```

