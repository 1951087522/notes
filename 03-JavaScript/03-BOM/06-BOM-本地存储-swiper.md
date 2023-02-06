## **1.- Window对象**

### **1.1- BOM 浏览器对象模型**

- BOM （Browser Object Model）是浏览器的对象模型

![image-20221218163334024](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218163334024.png)

- window 是浏览器内置中的全局对象，我们所学习的所有 Web APIs 的知识内容都是基于 window 对象实现的
- window 对象下包含了navigator、location、document、history、screen 5个属性，即所谓的BOM(浏览器对象模型)

- document 是实现DOM的基础，它其实是依附于window的属性。
- 注：依附于window对象的所有属性和方法，使用时可以省略window

### **1.2- 定时器 延时函数**

- JavaScript 内置的一个用来让代码延迟执行的函数 setTimeout()
- 仅仅执行一次，平时省略window

```js
    <script>
        // 延迟函数 setTimeout(回调函数，等待的毫秒数)
        let btn = document.querySelector('button')
        let timer = setTimeout(function () {
            console.log(123)
        }, 3000)
        btn.addEventListener('click',function(){
            // 解除定时器
            clearTimeout(timer)
        })
    </script>
```

#### **1.2.1- 递归函数**

- 递归函数 自己调用自己的函数

- - 需要注意 容易造成死递归  一定要加退出条件判断

```javascript
    <script>
        // 递归函数 自己调用自己的函数
        // 需要注意 容易造成死递归  一定要加退出条件判断
        let num = 0;
        function fn() {
            num++
            console.log(123);
            if (num >= 100) {
                return
            }
            // 自己调用自己
            fn()
        }
        fn()
    </script>
```

- 利用递归函数实现setInterval 间隙函数

```js
    <div></div>
    <script>
        let div = document.querySelector('div')
        function fn() {
            // 实例化时间对象.得到本地时间
            div.innerHTML = new Date().toLocaleString()
            // 延迟定时器调取自己
            setTimeout(fn, 1000)
        }
        fn()
    </script>
```

- 间隙函数 与 延迟函数区别

- - 间隙函数 setInterval 重复执行 首次执行延迟
  - 延迟函数 setTimeout 执行一次
  - setTimeout 结合递归函数 能模拟间隙函数重复执行
  - clearInterval 结束 setInterval 间隙函数
  - clearTimeout 结束 setTimeout 延迟函数

### ==**1.3- JS 执行机制**==

经典面试题

```js
console.log(111)
setTimeout(function(){
    console.log(222)
},1000)
console.log(333)
// 输出结果是？    // 132

console.log(111)
setTimeout(function(){
    console.log(222)
},0)
console.log(333)
// 输出结果是？    // 132
```

- **JS 是单线程**

- - JavaScript 语言的一大特点就是==单线程==，也就是说，==同一个时间只能做一件事==。这是因为Javascript 这门脚本语言诞生的使命所致一javaScript是为处理页面中用户的交互，以及操作DOM而诞生的。比如我们对某个DOM元素进行添加和删除操作，不能同时进行。应该先进行添加，之后再删除。
  - 单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。这样所导致的问题是：如果JS 执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉。

- **==同步和异步==**

- - 为了解决这个问题，利用多核CPU的计算能力，HTML5提出Web Worker标准，允许JavaScript脚本创建多个线程。于是，JS中出现了同步和异步。

- 同步

- - ==前一个任务结束后再执行后一个任务==，程序的执行顺序与任务的排列顺序是一致的、同步的。比如做饭的同步
  - 做法：我们要烧水煮饭，等水开了(10分钟之后)，再去切菜，炒菜。

- 异步

- - 你在做一件事情时，因为这件事情会花费很长时间，==在做这件事的同时，你还可以去处理其他事情==。
  - 比如做饭的异步做法，我们在烧水的同时，利用这10分钟，去切菜，炒菜。

- ==同步任务==

- - 同步任务都在主线程上执行，形成一个==执行栈==。

- 异步任务

- - ==JS的异步是通过回调函数实现的。==
  - 一般而言，异步任务有以下三种类型：

- - 1、普通事件，如click、resize等
  - 2、资源加裁，如load、error等
  - 3、定时器，包括setInterval、setTimeout等
  - 异步任务相关添加到==任务队列==中

- ==**JS执行机制**==

- - 1.先执行执行栈中的同步任务。
  - 2.异步任务放入任务队列中。
  - ==3.一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取任务队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行。==

- **先执行同步任务中的执行栈 执行完毕后 再把异步任务 中的 任务队列 拿到 执行栈中执行**

![image-20221218163648911](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218163648911.png)

**事件循环**

![image-20221218163718936](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218163718936.png)

- - ==由于主线程不断的 重复获得任务 执行任务 再获取任务 再执行，所以这种执行机制被称为 事件循环（event loop）==

### **1.4- location 对象**

- location 的数据类型是 对象，它拆分并保存了URL 地址的各个组成部分

- 常用属性和方法

- - href 属性获取完整的 URL 地址，对其赋值可以实现地址的跳转
  - search 属性获取地址中携带的参数，符合 ？ 后面的部分
  - hash 属性获取地址中的哈希值，符合 # 后面的部分
  - reload 方法用来刷新当前页面，传入参数 true 表示强制刷新

#### **1.4.1- href 属性 跳转页面**

- - 获取URL完整地址

```js
// 可以得到当前文件URL地址
console.log(location.href)
// 可以通过js方式 跳转到目标地址
location.href = 'https://www.bilibili.com/?spm_id_from=333.337.0.0'
```

- 倒计时跳转页面案例

```js
<body>
    <a href="https://www.bilibili.com/?spm_id_from=333.337.0.0">将在 <i>5</i> 秒之后跳转</a>
    <script>
        let a = document.querySelector('a')
        let num = 5
        let timer = setInterval(function () {
            num--
            a.innerHTML = `将在 <i>${num}</i> 秒之后跳转`
            if (num === 1) {
                // 跳转页面
                location.href = 'https://www.bilibili.com/?spm_id_from=333.337.0.0'
            }
        }, 1000)
    </script>
</body>
```

#### **1.4.2- search 属性 ？**

- 获取 ? 后面的参数

```js
console.log(location.search)
```

- 可以搭配表单域获取用户填写的内容

- - 表单域页面：

```js
<body>
    <form action="07.1-target.html">
        <input type="text" name="username">
        <button>提交</button>
    </form>
</body>
```

- 收集数据页面

```js
    <script>
        // 完整的地址   L:/AIC/%E7%BB%83%E4%B9%A0/DOM/06-BOM/01/07.1-target.html?username=123
        console.log(location.href);
        // 获取?后面的参数
        console.log(location.search);
    </script>
```

#### **1.4.3- hash 属性 #**

- 获取 地址 # 后面的部分

```js
<body>
    <a href="#one">第一个</a>
    <a href="#two">第二个</a>
    <script>
        console.log(location.hash); //得到#后面的内容
    </script>
</body>
```

#### **1.4.4- reload 方法 刷新**

- 用来刷新当前页面 传入参数 true 表示强制刷新

```js
<body>
    <button>刷新</button>
    <script>
        let btn = document.querySelector('button')
        btn.addEventListener('click', function () {
            // reload 刷新 不传递参数默认false 本地刷新
            // 传递参数 true 表示强制刷新 CTRL + f5  区别 强制刷新不走 本地缓存 直接从网上拉取获取最新内容
            location.reload()
        })
    </script>
</body>
```

### **1.5- navigator 对象 浏览器自身相关信息**

- navigator 的数据类型是对象，该对象下记录了浏览器自身相关信息

- 常用属性和方法

- - 通过 userAgent 检测浏览器的版本及平台

```js
    <script>
        // 检测 userAgent（浏览器信息）
        !(function () {
            const userAgent = navigator.userAgent
            // 验证是否为Android或iPhone
            const android = userAgent.match(/(Android);?[\s\/]+([\d.]+)?/)
            const iphone = userAgent.match(/(iPhone\sOS)\s([\d_]+)/)
            // 如果是Android或iPhone，则跳转至移动站点
            if (android || iphone) {
                location.href = 'http://m.itcast.cn'
            }
        })()
    </script>
```

### **1.6- history 对象**

- history 的数据类型是对象 ，该对象与浏览器地址栏的操作相对应，如前进，后退，历史记录等

- 常用属性 和 方法

- - back()		后退功能
  - forword()		前进功能
  - go(参数)		前进后退功能	参数为1 前进1个页面 2为前进2个页面

参数为-1 后退1个页面

```js
<body>
    <button class="forword">前进</button>
    <button class="back">后退</button>
    <script>
        let forword = document.querySelector('.forword')
        let back = document.querySelector('.back')
        forword.addEventListener('click', function () {
            history.forward()
        })
        back.addEventListener('click', function () {
            history.back()
        })
    </script>
</body>
```

## **2.- swiper 插件**

- 插件: 就是别人写好的一些代码,我们只需要复制对应的代码,就可以直接实现对应的效果

- 学习插件的基本过程

- - 1.熟悉官网,了解这个插件可以完成什么需求 	https://www.swiper.com.cn/ 
  - 2.看在线演示,找到符合自己需求的demo		https://www.swiper.com.cn/demo/index.html
  - 3.查看基本使用流程 					https://www.swiper.com.cn/usage/index.html
  - 4.查看APi文档,去配置自己的插件 			https://www.swiper.com.cn/api/index.html
  - 5.注意: 多个swiper同时使用的时候, 类名需要注意区分

## **3.-** **本地存储**

### **3.1- 本地存储特性**

- 随着互联网的快速发展，基于网页的应用越来越普遍，同时也变的越来越复杂，为了满足各种各样的需求，会经常性在本地存储大量的数据，HTML5规范提出了相关解决方案。

1. 数据存储在用户浏览器中
2. 设置、读取方便、甚至页面刷新不丢失数据
3. 容量较大，sessionStorage和localStorage约5M左右

### **3.2- localStorage** 

1. 生命周期永久生效，除非手动删除，否则关闭页面也会存在
2. 可以多个窗口共享	（同一个浏览器可以共享）
3. 以键值对的形式存储

### **3.3- 使用**

![image-20221218164220218](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218164220218.png)

- 存	localStroage.setItem('键'，值)

- 取	localStroage.getItem('键')

- 删	localStroage.remove('键')

- 简单数据类型 与 复杂数据类型 存储使用

- - 复杂数据类型 转换JSON字符串
  - JSON.stringify()
  - JSON.parse()

```js
    <script>
        // 1.存储数据   localStorage.setItem('键','值')
        // 存储简单数据类型:
        localStorage.setItem('uname', 'like')
        // 2.获取数据   localStorage.getItem('键')
        console.log('uname')
        // 3.删除数据   localStorage.remove('键')
        localStorage.removeItem('uname')

        // 复杂数据类型存储
        let obj = {
            uname: 'Like',
            age: 19,
            address: '北京'
        }
        // 1.转换成JSON字符串存储   JSON.stringify(对象)
        localStorage.setItem('obj', JSON.stringify(obj))
        // 2.取数据 转换成字符串    JSON.parse()
        console.log(JSON.parse(localStorage.getItem('obj')))
    </script>
```

### **3.4-** **sessionStorage（了解）**

1. 生命周期为关闭浏览器窗口
2. 在同一个窗口(页面)下数据可以共享
3. 以键值对的形式存储使用
4. 用法跟localStorage 基本相同

## **4.- 综合案例**

> A.本地存储封装函数
>
> B.打开页面渲染
>
> ​	 读取本地存储的数据
>
> C.新增
>
>  	读取本地存储的数据
>
>  	再存储到本地
>
> D.删除
>
>  	取本地存储的数据

```js
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <link rel="stylesheet" href="css/user.css">
</head>

<body>
  <h1>新增学员</h1>
  <div class="info">
    姓名：<input type="text" class="uname">
    年龄：<input type="text" class="age">
    性别: <select name="gender" id="" class="gender">
      <option value="男">男</option>
      <option value="女">女</option>
    </select>
    薪资：<input type="text" class="salary">
    就业城市：<select name="city" id="" class="city">
      <option value="北京">北京</option>
      <option value="上海">上海</option>
      <option value="广州">广州</option>
      <option value="深圳">深圳</option>
      <option value="曹县">曹县</option>

    </select>
    <button class="add">录入</button>
  </div>

  <h1>就业榜</h1>
  <table>
    <thead>
      <tr>
        <th>学号</th>
        <th>姓名</th>
        <th>年龄</th>
        <th>性别</th>
        <th>薪资</th>
        <th>就业城市</th>
        <th>操作</th>
      </tr>
    </thead>
    <tbody>
      <!-- <tr>
        <td>1001</td>
        <td>欧阳霸天</td>
        <td>19</td>
        <td>男</td>

        <td>15000</td>
        <td>上海</td>
        <td>
          <a href="javascript:">删除</a>
        </td>
      </tr> -->
    </tbody>
  </table>
  <script>
    /* 
      A.本地存储封装函数
      B.打开页面渲染
        读取本地存储的数据
      C.新增
        读取本地存储的数据
        再存储到本地
      D.删除
        读取本地存储的数据
      E.解决所有数据删除bug
    */


    // A.本地存储封装函数
    function getLocalDate() {
      // A.1读取本地存储的数据
      let data = localStorage.getItem('data')
      // A.2判断本地是否有数据
      if (data) {
        // A.3如果本地有数据 则返回出去 JSON.parse()
        return JSON.parse(data)
      } else {
        // A.4如果本地存储没有数据，则默认写入三条数据，注意存储的利用JSON.stringify() 存 储JSON 格式的数据
        let arr = [
          { stuId: 1001, uname: '欧阳霸天', age: 19, gender: '男', salary: '20000', city: '上海' },
          { stuId: 1002, uname: '令狐霸天', age: 29, gender: '男', salary: '30000', city: '北京' },
          { stuId: 1003, uname: '诸葛霸天', age: 39, gender: '男', salary: '2000', city: '北京' },
        ]
        // A.5写入本地存储
        localStorage.setItem('data', JSON.stringify(arr))
      }
    }


    //  1. 准备好数据后端的数据

    // 获取父元素 tbody
    let tbody = document.querySelector('tbody')
    // 添加数据按钮 
    // 获取录入按钮
    let add = document.querySelector('.add')
    // 获取各个表单的元素
    let uname = document.querySelector('.uname')
    let age = document.querySelector('.age')
    let gender = document.querySelector('.gender')
    let salary = document.querySelector('.salary')
    let city = document.querySelector('.city')


    // 渲染函数  把数组里面的数据渲染到页面中
    function render() {
        
      // B.读取本地存储的数据 然后渲染
      let arr = getLocalDate()
      
      // 先干掉以前的数据  让tbody 里面原来的tr 都没有
      tbody.innerHTML = ''
      // 在渲染新的数据
      // 根据数据的条数来渲染增加 tr  
      for (let i = 0; i < arr.length; i++) {
        // 1.创建tr  
        let tr = document.createElement('tr')
        // 2.tr 里面放内容
        tr.innerHTML = `
        <td>${arr[i].stuId}</td>
        <td>${arr[i].uname}</td>
        <td>${arr[i].age}</td>
        <td>${arr[i].gender}</td>
        <td>${arr[i].salary}</td>
        <td>${arr[i].city}</td>
        <td>
          <a href="javascript:" id="${i}">删除</a>
        </td>
        `
        // 3.把tr追加给 tobdy  父元素.appendChild(子元素)
        tbody.appendChild(tr)


      }
    }
    // 页面加载就调用函数
    render()
    
    // 新增模块
    add.addEventListener('click', function () {
        
      // C.先读取最新的本地存储数据
      let arr = getLocalDate()
      
      // alert(11)
      // 获得表单里面的值   之后追加给 数组 arr  用 push方法
      arr.push({
        // 得到数组最后一条数据的学号 1003    + 1
        stuId: arr[arr.length - 1].stuId + 1,
        uname: uname.value,
        age: age.value,
        gender: gender.value,
        salary: salary.value,
        city: city.value
      })
      // C.2 存储到本地 
      localStorage.setItem('data', JSON.stringify(arr))

      // console.log(arr)
      // 重新渲染我们的函数
      render()
      // 复原所有的表单数据
      uname.value = age.value = salary.value = ''
      gender.value = '男'
      city.value = '北京'
    })


    // 删除操作， 删除的也是数组里面的数据 ， 但是我们用事件委托
    tbody.addEventListener('click', function (e) {
      // D.1 读取本地数据
      let arr = getLocalDate()

      // alert(11)
      // 我们只能点击了链接 a ，才会执行删除操作
      // 那我们怎么知道你点击了a呢？
      // 俺们只能点击了链接才能做删除操作
      // console.dir(e.target.tagName)
      if (e.target.tagName === 'A') {
        // alert('你点击了链接')
        // 删除操作  删除 数组里面的数据  arr.splice(从哪里开始删，1)
        // 我要得到a的id 需要
        // console.log(e.target.id)
        
        // E.解决所有数据删除bug
        // 第一条数据不允许删除
        if (e.target.dataset.id === '0') {
          alert('当前数据不允许删除')
          return
        }    
        
        arr.splice(e.target.id, 1)

        // D.2 存储到本地
        localStorage.setItem('data', JSON.stringify(arr))
        
        // 重新渲染我们的函数
        render()
      }
    })
  </script>
</body>

</html>
```

## **5.- 自定义属性**

- 固有属性：

标签天生自带的属性比如class id title等，可以直接使用点语法操作

- 自定义属性：

由程序员自己添加的属性，在DOM对象中找不到，无法使用点语法操作，必须使用专门的API

- html5 推出了专门的**data-自定义属性 命名**

- - getAttribute('属性名')			获取自定义属性	dataset对象获取
  - setAttribute('属性名'，'属性值)	设置自定义属性
  - removeAttribute('属性名')		删除自定义属性

```js
<body>
    <div class="box"></div>
    <script>
        let box = document.querySelector('.box')
            // 设置自定义属性   setAttribute('属性名',属性值)    data-开头
            box.setAttribute('data-id1', 1)
            // 获取自定义属性   getAttribute(属性)
            console.log(box.getAttribute('data-id1'));
            // 获取自定义属性   dataset.属性
            console.log(box.dataset.id1);
            // 移除 remove
            box.removeAttribute('data-id1')
    </script>
</body>
```

