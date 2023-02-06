## 1. 事件

### **1.1- 事件监听**

- **什么是事件？**

- - **事件是在编程时系统发生的动作或者发生的事情**
  - **比如用户在网页上单击一个按钮**

- **什么是事件监听**

- - **就是让程序检测是否有事件发生，一旦有事件触发，就立即调用一个函数做出反应，也称为注册事件**

- 语法：

- -  `事件源.addEventListener('事件',要执行的函数)`

```javascript
// 事件源.addEventListener('事件',要执行的函数)

//示例
<body>
    <button>点击我</button>
    <script>
        // 1.获取元素
        let btn = document.querySelector('button');
        // 2.事件监听 绑定事件 注册事件 事件侦听
        // 事件源.addEventListener('事件',事件处理函数)
        btn.addEventListener('click',function(){
            alert('Nihao')
        })
    </script>
</body>
```

- **事件监听三元素**

- - **事件源：哪个DOM元素被事件触发了，要获取DOM元素**
  - **事件：用什么方式触发，比如单击鼠标 click 、 鼠标经过 mouseover**
  - **事件调用的函数：要做什么事**

**随机点名完整案例：**

1. 获取元素
2. 注意获取方法 类名不要忘记加"."
3. 开始按钮绑定事件
4. 随机抽取数据中的数组 不断刷新 需要定时器
5. 获取到的值添加到盒子中
6. 结束按钮取消事件
7. 关闭定时器
8. 每抽取一个数据 删除值
9. 在开始按钮中 判断数据中的数组 等于1 时 禁用按钮
10. 注意 局部变量提升

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        h2 {
            text-align: center;
        }

        .box {
            width: 600px;
            margin: 50px auto;
            display: flex;
            font-size: 25px;
            line-height: 40px;
        }

        .qs {

            width: 450px;
            height: 40px;
            color: red;

        }

        .btns {
            text-align: center;
        }

        .btns button {
            width: 120px;
            height: 35px;
            margin: 0 50px;
        }
    </style>
</head>

<body>
    <h2>随机点名</h2>
    <div class="box">
        <span>名字是：</span>
        <div class="qs">这里显示姓名</div>
    </div>
    <div class="btns">
        <button class="start">开始</button>
        <button class="end">结束</button>
    </div>

    <script>
        // 数据数组
        let arr = ['薛润丽', '王伟洋', '曹东东', '顾欣宜', '袁宜豪', '张春齐', '朱军', '唐子辰']

        function getRandom(min, max) {
            return Math.floor(Math.random() * (max - min + 1)) + min
        }
        // 1.获取元素   两个按钮 和 盒子
        // 注意 不要忘记加“.” css类名选择器
        let start = document.querySelector('.start');
        let end = document.querySelector('.end');
        let qs = document.querySelector('.qs');
        // 2.给按钮注册事件
        // 由于 定时器在局部变量 要提升全局变量
        let timer;
        let random;
        start.addEventListener('click', function () {
            // 2.1 随机抽取数据 不断闪烁    定时器
            timer = setInterval(function () {
                random = getRandom(0, arr.length - 1)
                // 2.2 盒子添加数据
                qs.innerHTML = arr[random]
            }, 15);
            // 4.判断 当数据数组 值到了最后一个 禁用两个按钮
            if (arr.length === 1) {
                start.disabled = end.disabled = true;
            }
        })
        // 3.给按钮注销事件 停止定时器
        end.addEventListener('click', function () {
            // 关闭定时器
            // 由于 定时器在局部变量 要提升全局变量
            clearInterval(timer);
            // 3.1 删除数组元素
            arr.splice(random, 1)
        })
    </script>
</body>
</html>
```

### **1.2- 拓展阅读-事件监听版本**

- DOM L0

- - 事件源.on事件 = function() {}

```javascript
<body>
    <button>点击</button>
    <script>
        let btn = document.querySelector('button');
        btn.onclick = function () {
            alert('111')
        }
    </script>
</body>
```

- DOM L2

- - 事件源.addEventListener(事件，事件处理函数)

- 发展史

- - DOM  L0:是DOM的发展的第一个版本；L:level
  - D0M  L1:D0M级别1于1998年10月1日成为W3C推荐标准
  - DOM  L2:使用addEventListener注册事件
  - DOM  L3:DOM3级事件模块在DOM2级事件的基础上重新定义了这些事件，也添加了一些新事件类型

### **1.3- 事件类型**

- 鼠标移动事件	mousemove
- 鼠标经过 mouseover	会冒泡
- 鼠标离开 mouseout		会冒泡
- change 事件 失去焦点表单里的值 发生改变 触发
- submit 提交事件	提交触发

![image-20221218160432757](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218160432757.png)

#### **案例：**

##### **1.小米搜索框案例**

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        ul {

            list-style: none;
        }

        .mi {
            position: relative;
            width: 223px;
            margin: 100px auto;
        }

        .mi input {
            width: 223px;
            height: 48px;
            padding: 0 10px;
            font-size: 14px;
            line-height: 48px;
            border: 1px solid #e0e0e0;
            outline: none;
            /* transition: .5s; */
        }

        .mi .search {
            border: 1px solid #ff6700;
        }

        .result-list {
            display: none;
            position: absolute;
            left: 0;
            top: 48px;
            width: 223px;
            border: 1px solid #ff6700;
            border-top: 0;
            background: #fff;
        }

        .ts {
            transition: .5s;
        }

        .result-list a {
            display: block;
            padding: 6px 15px;
            font-size: 12px;
            color: #424242;
            text-decoration: none;
        }

        .result-list a:hover {
            background-color: #eee;
        }
    </style>

</head>

<body>
    <div class="mi">
        <input type="search" placeholder="小米笔记本">
        <ul class="result-list">
            <li><a href="#">全部商品</a></li>
            <li><a href="#">小米11</a></li>
            <li><a href="#">小米10S</a></li>
            <li><a href="#">小米笔记本</a></li>
            <li><a href="#">小米手机</a></li>
            <li><a href="#">黑鲨4</a></li>
            <li><a href="#">空调</a></li>
        </ul>
    </div>
    <script>
        // 1.获取元素   搜索框 和 下拉菜单
        let search = document.querySelector('input[type=search]');
        let list = document.querySelector('.result-list');
        // 2. 获取焦点 触发事件 focus
        // 也可以 鼠标经过 mouseenter
        search.addEventListener('mouseenter', function () {
            // 触发显示下拉菜单和搜索框变色
            list.style.display = 'block';
            // 追加类
            search.classList.add('search');
            search.classList.add('ts');
        })
        // 3.失去焦点 移除事件
        // 也可以是 鼠标离开 mouseleave
        search.addEventListener('mouseleave', function () {
            list.style.display = 'none';
            search.classList.remove('search');
        })
    </script>
</body>

</html>
```

##### **2.全选反选案例**

```javascript
<!DOCTYPE html>

<html>

<head lang="en">
  <meta charset="UTF-8">
  <title></title>
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    table {
      border-collapse: collapse;
      border-spacing: 0;
      border: 1px solid #c0c0c0;
      width: 500px;
      margin: 100px auto;
      text-align: center;
    }

    th {
      background-color: #09c;
      font: bold 16px "微软雅黑";
      color: #fff;
      height: 24px;
    }

    td {
      border: 1px solid #d0d0d0;
      color: #404060;
      padding: 10px;
    }

    .allCheck {
      width: 80px;
    }
  </style>
</head>

<body>
  <table>
    <tr>
      <th class="allCheck">
        <input type="checkbox" name="" id="checkAll"> <span class="all">全选</span>
      </th>
      <th>商品</th>
      <th>商家</th>
      <th>价格</th>
    </tr>
    <tr>
      <td>
        <input type="checkbox" name="check" class="ck">
      </td>
      <td>小米手机</td>
      <td>小米</td>
      <td>￥1999</td>
    </tr>
    <tr>
      <td>
        <input type="checkbox" name="check" class="ck">
      </td>
      <td>小米净水器</td>
      <td>小米</td>
      <td>￥4999</td>
    </tr>
    <tr>
      <td>
        <input type="checkbox" name="check" class="ck">
      </td>
      <td>小米电视</td>
      <td>小米</td>
      <td>￥5999</td>
    </tr>
  </table>
  <script>
    // 1.获取元素
    let all = document.querySelector('#checkAll');
    // 注意这里是获取全部的类选择器 用all
    let ck = document.querySelectorAll('.ck')
    let span = document.querySelector('span');
    // 2.事件监听 全选按钮
    all.addEventListener('click', function () {
      // 2.1给下面的小按钮依次遍历添加选中
      for (let i = 0; i < ck.length; i++) {
        ck[i].checked = all.checked;
      }
      // 3.判断 当全选按钮选中 更改文字
      if (all.checked) {
        // 选中为真 则为取消
        span.innerHTML = '取消';
      } else {
        span.innerHTMl = '全选';
      }
    })
    // 4. 通过循环 同时给多个按钮 绑定相同的事件
    for (let i = 0; i < ck.length; i++) {
      ck[i].addEventListener('click', function () {
        // 4.1 只要点击一个按钮 遍历其他按钮
        for (let j = 0; j < ck.length; j++) {
          // 4.2 判断有没有选中
          if (ck[j].checked === false) {
            // 为false就退出循环
            all.checked = false;
            span.innerHTML = '全选';
            return;
          }
        }
        // 5. 循环结束 没有false 说明被选中
        all.checked = true;
        span.innerHTML = '取消';
      })
    }
  </script>
</body>

</html>
```

##### **3.购物车加减案例**

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    div {
      width: 80px;
    }

    input[type=text] {
      width: 50px;
      height: 44px;
      outline: none;
      border: 1px solid #ccc;
      text-align: center;
      border-right: 0;
    }

    input[type=button] {
      height: 24px;
      width: 22px;
      cursor: pointer;
    }

    input {
      float: left;
      border: 1px solid #ccc;

    }
  </style>
</head>

<body>
  <div>
    <input type="text" id="total" value="0" readonly>
    <input type="button" value="+" id="add">
    <input type="button" value="-" id="reduce" disabled>
    <script>
      // 1.获取元素 加 减 值
      let add = document.querySelector('#add');
      let reduce = document.querySelector('#reduce');
      let total = document.querySelector('#total');
      // 2.加号 绑定事件
      add.addEventListener('click', function () {
        // 让 值自增
        total.value++;    // 有隐式转换
        // 启用 减按钮
        reduce.disabled = false;
      })
      // 3. 减号 绑定事件
      reduce.addEventListener('click', function () {
        total.value--;    // 有隐式转换
        // 判断 当值小于等于0 时 关闭按钮
        if (+total.value === 0) {
          reduce.disabled = true;
        }
      })
    </script>
  </div>
</body>

</html>
```

## **2.- 高阶函数**

高阶函数 可以被简单理解为函数的高级应用，JavaScript中函数可以被当成 值 来对待，基于这个特性实现函数的高级应用。

值 就是JavaScript 中的数据，如数值，字符串，布尔，对象等等

### **2.1-函数表达式**

- 函数表达式 和 普通函数 并无本质区别

```javascript
// 函数表达式 和 普通函数本质上是一样的
let counter = function(x,y) {
    return x + y;
}
// 调用函数
let result = counter(5,10);
console.log(result);
```

### **2.2- 回调函数**

- 如果将函数A作为参数 传递 给函数B 时，我们称函数A 为 回调函数 

```javascript
function fn() {
    console.log('我是回调函数');
}
// fn 传递给了 setInterval ,fn 就是回调函数
setInterval(fn,1000)
```

```javascript
box.addEventListener('click',function() {
    console.log('我也是回调函数')
})
```

## **3.- 环境对象**

环境对象指的时函数内部特殊变量this，它代表当前函数运行所处的环境

作用：弄清楚this的指向，可以让代码更简洁

- 函数的调用方式不同，this指代的对象也不同
- 谁调用，this 就指向谁 是判断 this 指向的粗略规则
- 直接调用函数，其实相当于是window.函数，所有this 指向 window

## **4.- ==编程思想==**

- **排他思想**

- - **当前元素为A状态 	其他元素为B状态**

- **使用：**

- - **1.干掉所有人**

  - - **使用for 循环**

  - **2.复活他自己**

  - - **通过this 或者 下标 找到自己或者对应的元素**

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .gold {
            background: gold;
        }
    </style>
</head>

<body>
    <button class='gold'>第1个</button><button>第2个</button><button>第3个</button><button>第4个</button><button>第5个</button>
    <script>
        let btns = document.querySelectorAll('button');
        for (let i = 0; i < btns.length; i++) {
            btns[i].addEventListener('click', function () {
                // 排他思想
                // 1.干点所有人 for循环
                //for (let j = 0; j < btns.length; j++) {
                //    btns[j].classList.remove('gold');
                //}
                // 1.2也可以优化成
                // 找出那个唯一的类 删除
                // 前提默认得有一个类
                document.querySelector('.gold').classList.remove('gold')
                // 2.复活我自己
                this.classList.add('gold')
            })
        }
    </script>
</body>

</html>
```

## **5.- 综合案例**

- Tab栏切换 / 选项卡

1.获取元素  tab栏所有的li 和 对应的div模块

  注意所有的querySelectorAll

2.循环遍历依次给每一个li添加点击事件

 切换 tab栏边框颜色

 排他思想 删除默认选中的 >> 添加他自己

  给每一个li添加对应的div模块

```javascript
<!DOCTYPE html>
<html>

<head lang="en">
  <meta charset="UTF-8" />
  <title></title>
  <style type="text/css">
    * {
      margin: 0;
      padding: 0;
    }

    ul {
      list-style: none;
    }

    .wrapper {
      width: 1000px;
      height: 475px;
      margin: 0 auto;
      margin-top: 100px;
    }

    .tab {
      border: 1px solid #ddd;
      border-bottom: 0;
      height: 36px;
      width: 320px;
    }

    .tab li {
      position: relative;
      float: left;
      width: 80px;
      height: 34px;
      line-height: 34px;
      text-align: center;
      cursor: pointer;
      border-top: 4px solid #fff;
    }

    .tab span {
      position: absolute;
      right: 0;
      top: 10px;
      background: #ddd;
      width: 1px;
      height: 14px;
      overflow: hidden;
    }

    .products {
      width: 1002px;
      border: 1px solid #ddd;
      height: 476px;
    }

    .products .main {
      float: left;
      display: none;
    }

    .products .main.active {
      display: block;
    }

    .tab li.active {
      border-color: red;
      border-bottom: 0;
    }
  </style>
</head>

<body>
  <div class="wrapper">
    <ul class="tab">
      <li class="tab-item active">国际大牌<span>◆</span></li>
      <li class="tab-item">国妆名牌<span>◆</span></li>
      <li class="tab-item">清洁用品<span>◆</span></li>
      <li class="tab-item">男士精品</li>
    </ul>
    <div class="products">
      <div class="main active">
        <a href="###"><img src="imgs/guojidapai.jpg" alt="" /></a>
      </div>
      <div class="main">
        <a href="###"><img src="imgs/guozhuangmingpin.jpg" alt="" /></a>
      </div>
      <div class="main">
        <a href="###"><img src="imgs/qingjieyongpin.jpg" alt="" /></a>
      </div>
      <div class="main">
        <a href="###"><img src="imgs/nanshijingpin.jpg" alt="" /></a>
      </div>
    </div>
  </div>
  <script>
    // 1.获取元素 tab栏四个li 对应的四个div
    // 注意 这里是获取所有的li 和 div
    let lis = document.querySelectorAll('.tab .tab-item');
    let divs = document.querySelectorAll('.products .main');
    // 2.给tab栏的每一个li  循环绑定点击click监听事件
    for (let i = 0; i < lis.length; i++) {
      // 给tab栏的每一个li [i]添加点击事件 所有循环
      lis[i].addEventListener('click', function () {
        // 2.1 切换边框颜色
        // 排他思想 干掉默认选中的 复活他自己
        document.querySelector('.tab .active').classList.remove('active');
        this.classList.add('active');
        // 3.底部显示隐藏模块 每点击对应的li 切换到对应的底部模块
        document.querySelector('.products .active').classList.remove('active');
        // 对应的divs
        divs[i].classList.add('active');
      })
    }
  </script>
</body>
</html>
```

