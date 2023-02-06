# 小程序第3天



## 一、微信小程序

### 1.1 组件

在小程序中的内置组件是有限的，有些时候为了实现更多的功能，可以自定义组件



 1 创建自定义组件文件：

​		创建文件夹 components

​		创建组件文件夹 		例:demo

​		新建 Component 	 例:demo

​	此时.json文件中，会出现一个component: true的配置，用来区分组件和页面

![image-20230203165058147](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230203165058147.png)



 2 在该文件中有四个文件js、json、wxml、wxss。在这些文件中我们可以定义模板、定义样式等等

​		 在自定义组件中的构造器是`Component`  

- `properties` 用于来接收传递的数据 值是一个对象

  -  `key`      表示数据名称
  - `value` 值是一个对象
    - `type` ：	数据类型 
    - `value`：    默认的属性值 
    - `observer`:  监听数据的变化

- `data` 定义组件数据的

  ​	定义的数据，会直接赋值在`data`中， 因此可以使用`setData`更新数据

- `methods` 定义组件方法

```js
// component/demo/demo.js
Component({
  /**
   * 组件的属性列表
   */
  properties: {
	
  },

  /**
   * 组件的初始数据
   */
  data: {

  },

  /**
   * 组件的方法列表
   */
  methods: {

  }
})
```



 3 在页面中的json文件中配置自定义组件

​		 通过`usingComponents`属性配置

​				 `key`   自定义组件名称

​				 `value` 对应的自定义组件的地址 使用绝对路径



```js
{
  "usingComponents": {
    "demo":"/component/demo/demo"
  }
}
```



 4 在页面中使用组件



```html
<!-- 使用组件 -->
<demo></demo>
```



### 1.2 组件生命周期

官方文档： https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/lifetimes.html



组件的生命周期，指的是组件自身的一些函数，这些函数在特殊的时间点或遇到一些特殊的框架事件时被自动触发。

其中，最重要的生命周期是 `created` `attached` `detached` ，包含一个组件实例生命流程的最主要时间点。

- 组件实例刚刚被创建好时， `created` 生命周期被触发。此时，组件数据 `this.data` 就是在 `Component` 构造器中定义的数据 `data` 。 **此时还不能调用 `setData` 。** 通常情况下，这个生命周期只应该用于给组件 `this` 添加一些自定义属性字段。
- 在组件完全初始化完毕、进入页面节点树后， `attached` 生命周期被触发。此时， `this.data` 已被初始化为组件的当前值。这个生命周期很有用，绝大多数初始化工作可以在这个时机进行。
- 在组件离开页面节点树后， `detached` 生命周期被触发。退出一个页面时，如果组件还在页面节点树中，则 `detached` 会被触发。



- 定义生命周期的方法

生命周期方法可以直接定义在 `Component` 构造器的第一级参数中。

自小程序基础库版本 [2.2.3](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) 起，组件的的生命周期也可以在 `lifetimes` 字段内进行声明（这是推荐的方式，其优先级最高）。





- 代码示例:

```js
Component({
  lifetimes: {
    attached: function() {
      // 在组件实例进入页面节点树时执行
    },
    detached: function() {
      // 在组件实例被从页面节点树移除时执行
    },
  },
  // 以下是旧式的定义方式，可以保持对 <2.2.3 版本基础库的兼容
  attached: function() {
    // 在组件实例进入页面节点树时执行
  },
  detached: function() {
    // 在组件实例被从页面节点树移除时执行
  },
  // ...
})
```



### 1.3 组件通信

组件间的通信有三个方向：

####  1.父组件(页面)向子组件通信

​		 通过传递属性数据的方式，子组件在`properties`中接收



- 父组件页面

```html
<!-- 父组件向子组件传递数据 直接传递属性数据 -->
<demo msg="msg"></demo>
<!-- 不传递数据 将使用默认数据 -->
<demo></demo>
```



- 子组件页面

```html
<!-- 接收展示到的数据 -->
<view>hello-{{msg}}</view>
```

- 子组件.js

```js
// component/demo/demo.js
Component({
  lifetimes: {
    attached: function () {
      // 在组件实例进入页面节点树时执行

      // 更新数据 小程序可以更改传递到的数据
      this.setData({
        msg: "Hi"
      })
    },
    detached: function () {
      // 在组件实例被从页面节点树移除时执行
    },
  },
  /**
   * 组件的属性列表
   */
  properties: {
    // key 表示接受到的数据名称
    // value 是一个对象
    msg: {
      // type 数据类型
      type: String,
      // value 默认的属性值
      value: "world",
      // observer 监听数据的变化
      observer() {
        console.log(arguments);
      }
    }
  },

  /**
   * 组件的初始数据
   */
  data: {
    num: 100
  },

  /**
   * 组件的方法列表
   */
  methods: {

  }
})
```



####  2.子组件向父组件(页面)通信

​		 通过自定义事件：（观察者模式）

​		a. `让父组件订阅自定义事件`，通过 `bind`

​		b. 在子组件中通过`triggerEvent`方法，来发布自定义事件，并且传递数据

​		c. 在父组件中，定义事件回调函数，通过事件对象中的`e.detail`来接收数据

​				 如果想接收多条数据，我们可以将数据放在一个数组中传递。 



- 父组件页面

```html
<!-- 子组件向父组件通信 -->
<!-- 1.为子组件准备一个自定义事件 -->
<child bindabc="receiveChildData"></child>

<!-- 4.父组件使用子组件传递的数据 -->
<view>子组件传递数据--{{msg}}</view>
```

- 子组件.js

```js
// component/child/child.js
Component({
  lifetimes: {
    // 组件构建完毕
    attached() {
      // 2.通过 triggerEvent 发布事件 并传递数据
      /* 
        第一个参数 自定义的名称
        第二个参数开始 传递的数据
          默认只能传递一个数据 传递多个数据 放入数组中
      */
      // this.triggerEvent('abc', [100, 200, 300])
      this.triggerEvent('abc', 'child msg')

    }
  },
  /**
   * 组件的属性列表
   */
  properties: {

  },

  /**
   * 组件的初始数据
   */
  data: {

  },

  /**
   * 组件的方法列表
   */
  methods: {

  }
})
```

- 父组件.js

```js
// pages/component/component.js
Page({

  /**
   * 页面的初始数据
   */
  data: {
    msg: ''
  },

  // 为子组件准备的方法
  receiveChildData(e) {
    // 3.通过 e.detail 获取子组件传递的数据
    // console.log(e.detail);
    // 4.接收并更新数据
    this.setData({
      msg: e.detail
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





####  3.兄弟间的通信

​		 综合使用以上两个技术：

​				 1.子组件向父组件传递属性数据

​			 	2.再由父组件将数据传递给另一个子组件



- 父组件页面

```html
<!-- 兄弟组件通信 -->
<!-- 1.1 子向父 准备方法 -->
<first bindMsg="receiveChildData"></first>

<!-- 2.1 父向子 通过传递属性数据的方法 -->
<second msg="{{msg}}"></second>
```

- 父组件.js

```js
// pages/component/component.js
Page({

  /**
   * 页面的初始数据
   */
  data: {
    msg: ''
  },

  // 1.2 为子组件准备的方法
  receiveChildData(e) {
    // .通过 e.detail 获取子组件传递的数据
    // console.log(e.detail);
    // 1.5 接收并更新数据
    this.setData({
      msg: e.detail
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

- 第一个兄弟页面

```html
<!-- 定义输入框 在first组件输入内容 在second组件展示内容 -->
<view>
  <label for="">first 请输入内容:</label>
  <!-- 1.3 定义方法 -->
  <input type="text" placeholder="请输入内容" bindinput="sendMsg"/>
</view>
```

- 第一个兄弟.js

```js
// component/first/first.js
Component({
  /**
   * 组件的属性列表
   */
  properties: {

  },

  /**
   * 组件的初始数据
   */
  data: {

  },

  /**
   * 组件的方法列表
   */
  methods: {
    sendMsg(e) {
      // 1.4 子向父 子组件通过 triggerEvent 发布自定义事件 并传递数据
      this.triggerEvent('Msg', e.detail.value)
    }
  }
})
```

- 第二个兄弟页面

```html
<!-- 展示 在first组件输入内容 在second组件展示内容 -->
<view>second 展示内容：{{msg}}</view>
```

- 第二个兄弟.js

```js
// component/second/second.js
Component({
  /**
   * 组件的属性列表
   */
  properties: {
    // 2.2 接收数据
    msg: {
      type: String
    }
  },

  /**
   * 组件的初始数据
   */
  data: {

  },

  /**
   * 组件的方法列表
   */
  methods: {

  }
})
```



### 1.4 slot 组件

用于在组件中定义显示的内容

使用方式：

 1 在自定义组件中定义要显示的内容

​		 通过`slot`属性 定义内容组件的名称

- 父组件页面

```html
<outer>
  <view slot="header">header</view>
  <!-- 没有指定slot名称 作为默认插槽存在 -->
  <view>body</view>
  <view slot="footer">footer</view>
</outer>
```



 2 在组件的脚本文件中传递options配置属性，传递`multipleSlots: true`属性

```js
  // 2 在组件的脚本文件中传递options配置属性，传递multipleSlots: true属性
  options: {
    multipleSlots: true // 在组件定义时的选项中启用多 slot 支持
  },
```



 3 在组件的wxml文件中，通过slot组件使用内容。

​		 通过`name`属性 指定内容组件

​		 如果没有`name`属性，将引入默认的。

文档 https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/wxml-wxss.html#%E7%BB%84%E4%BB%B6%20wxml%20%E7%9A%84%20slot



```html
<!-- 使用插槽 通过slot插槽使用 -->
<slot name="header"></slot>
<!-- 没有传递name 将引入默认插槽 -->
<slot></slot>
<slot name="footer"></slot>
```



### 1.5 其它 API

- 用于请求数据：	`wx.request` 


​		 url 				 定义请求的地址 (必须支持https协议)

​		 method 		定义请求的方式

​		 data		 	 携带的数据

​		 success 	    成功时候执行的回调函数

注意：

​		关闭证书校验

​		发送请求的地址，配置在个人账号页面中设置，服务器域名（一个月只能修改5次）。请求地址必须实名认证，必须有备案，必须是https协议



- 线上地址：

```js
  /**
   * 生命周期函数--监听页面加载
   */
  // https://api.aichuangleyu.com/weixin_waimai/data/list
  onLoad: function (options) {
    // 发送请求
    wx.request({
      url: 'https://api.aichuangleyu.com/weixin_waimai/data/list',
      method: "GET",
      success: res => {
        console.log(res);
      }
    })
  },
```

- 请求自己搭建的对应接口

app.js

```js
// 引入express
const express = require('express');
// 引入http
const http = require('http');
// 引入https
const https = require('https');
// 引入fs
const fs = require('fs');

// 获取应用程序
const app = express();


// 配置引擎
app.engine('html', require('ejs').__express);

// get请求
app.get('/getMsg', (req, res) => {
    res.json({ errno: 0, msg: 'get success' })
})

// post请求
app.post('/postMsg', (req, res) => res.json({ errno: 0, msg: 'post success' }));

// 创建http服务
http.createServer(app).listen(3000, () => console.log(3000));

// 创建https服务
// 读取密钥
const key = fs.readFileSync('./ssl/private.pem');
const cert = fs.readFileSync('./ssl/file.crt');
https.createServer({ key, cert }, app).listen(3001, () => console.log(3001));
```



```js
  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    // 发送请求
    wx.request({
      // 请求自己搭建的服务器 get 接口
      // url:'https://localhost:3001/getMSg',
      // 请求自己搭建的服务器 post 接口
      url:'https://localhost:3001/postMSg',
      // method: "GET",
      method: "POST",
      success: ({data}) => {
        console.log(data);
      }
    })
  },
```



- 下载文件： `donwloadFile`

```js
    // 下载文件
    wx.downloadFile({
      url: 'https://t7.baidu.com/it/u=3281686603,2838365105&fm=193&f=GIF',
      success:(res) => {
        // 存储数据
        this.setData({ img:res.tempFilePath })
      }
    })
```

```html
<image src="{{img}}"></image>
```



- 选择图片： `chooseImage`

- 支付API: `wx.requestPayment`({


​		 timeStamp: ‘’,

​		 nonceStr: ‘’,

​		 package: ‘’,

​		 signType: ‘’,

​		 paySign: ‘’,

 })

- 获取步数： `getWeRunData`


```js
    // 获取步数
    wx.getWeRunData({
      success: (result) => {
        console.log(result);
      },
    })
```



### 1.6 指南针

混合开发介于源生与web之间的一种开发模块，既可以快速的开发迭代，上线部署，也可以访问设备的功能。例如：指南针

​	通过`onCompassChange`方法监听手机移动方向的改变

​		 参数是一个对象 

​		 `direction` 指示手机角度的变化

- wxml

```html
<view class="angle">角度°{{angle}}</view>
<view class="dir">方向：{{dir}}</view>
<image src="/images/compass.png" style="transform:rotate(-{{angle}}deg)"></image>
```

- wxss

```css
/* pages/compass/compass.wxss */
page {
  background-color: #000;
  color: #fff;
  text-align: center;
}

image {
  width: 325px;
  height: 325px;
  margin: 20px 30px;
}
```

- js

```js
// pages/compass/compass.js
Page({

  /**
   * 页面的初始数据
   */
  data: {
    // 角度
    angle: 0,
    // 方向
    dir: ''
  },

  // 改变方向
  dealDir(direction) {
    let arr = ['正北', '东北', '正东', '东南', '正南', '西南', '正西', '西北'];
    // 获取每个方向的角度
    let pos = 360 / arr.length
    // 对于一个方向来说 存在一个范围区间  -22.5~22.5  监听手机方向改变的时候 是从0开始计数的 少了22.5
    // 获取方向对应的索引
    let index = parseInt((direction + 22.5) / pos)
    // 判断 index 有效范围区间
    if (index > 8) {
      index = 0
    }
    // 返回结果
    return arr[index]
  },

  /**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    // 监听手机移动的方法
    wx.onCompassChange(({
      direction
    }) => {
      // 更新数据
      this.setData({
        angle: direction.toFixed(0),
        dir: this.dealDir(direction)
      })
    })
  },
})
```



### 1.7 打卡小程序

在小程序中为了获取位置信息提供了三个方法：

 `getLocation` 				获取位置

 `chooseLocation` 	 	 选择位置

 `openLocation` 			 打开位置

- 演示方法

```js
/**
   * 生命周期函数--监听页面加载
   */
  onLoad: function (options) {
    // getLocation 				获取位置
    wx.getLocation({
      success(res) {
        console.log(res); // latitude: 40.22077 // longitude: 116.23128
      }
    })

    // chooseLocation 	 	 选择位置
    wx.chooseLocation({
      success(res) {
        console.log(res); // latitude: 40.102493 // longitude: 116.383449
      }
    })

    // openLocation 			 打开位置
    wx.openLocation({
      latitude: 40.102493,
      longitude: 116.383449,
    })
  },
```

- util  计算两个经纬度的距离(千米)

```js
/**
* 计算两个经纬度的距离(千米)
*/

module.exports.getDistance = function(lat1, lng1, lat2, lng2){
    var radLat1 = lat1*Math.PI / 180.0;
    var radLat2 = lat2*Math.PI / 180.0;
    var a = radLat1 - radLat2;
    var b = lng1*Math.PI / 180.0 - lng2*Math.PI / 180.0;
    var s = 2 * Math.asin(Math.sqrt(Math.pow(Math.sin(a/2),2) +
    Math.cos(radLat1)*Math.cos(radLat2)*Math.pow(Math.sin(b/2),2)));
    s = s *6378.137 ;// EARTH_RADIUS;
    s = Math.round(s * 10000) / 10000;
    return s;
}
```

- wxml

```html
<button type="primary" bindtap="showResult">打卡</button>
<view>{{result}}</view>
```

- js

```js
// pages/location/location.js

// 引入计算距离的方法
let {
  getDistance
} = require('../../utils/tools')

Page({

  /**
   * 页面的初始数据
   */
  data: {
    result: '',
    // 记录公司位置
    latitude: 40.22077,
    longitude: 116.23128
  },

  // 点击打卡
  showResult() {
    // 当点击按钮的时候 计算公司和手机两点之间的经纬度之差
    wx.getLocation({
      success: res => {
        // 调用方法 （返回的是千米）
        let result = getDistance(this.data.latitude, this.data.longitude, res.latitude, res.longitude)
        // 判断距离
        if (result < 1) {
          this.setData({result:"打卡成功"})
        } else {
          this.setData({result:"打卡失败"})
        }
      }
    })
  },
})
```




