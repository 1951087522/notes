# Vue	第三天

## 一、Vue

## 案例

### 数组过滤

```html
  <body>
    <div id="app">
      <input type="text" v-model="msg" />
      <button @click="sortNum = 1">升序</button>
      <button @click="sortNum = -1">降序</button>
      <button @click="sortNum = 0">原序</button>

      <!-- 遍历列表 -->
      <ul>
        <li v-for="item in dealPersons" :key="item.id">
          {{item.name}}--{{item.age}}--{{item.sex}}
        </li>
      </ul>
    </div>

    <script src="../js/vue.js"></script>

    <script>
      let vm = new Vue({
        el: "#app",
        data: {
          persons: [
            { id: "01", name: "马冬梅", age: 20, sex: "女" },
            { id: "02", name: "周冬雨", age: 21, sex: "女" },
            { id: "03", name: "周杰伦", age: 22, sex: "男" },
            { id: "04", name: "温兆伦", age: 23, sex: "男" },
          ],
          // 搜索关键词
          msg: "",
          // 排序关键词
          sortNum: 0,
        },

        // 计算属性数据
        computed: {
          dealPersons() {
            // 过滤数组
            let newArr = this.persons.filter((item) => {
              // 是否包含匹配
              return item.name.indexOf(this.msg) >= 0;
            });

            // 排序
            if (this.sortNum) {
              newArr.sort((a, b) => {
                return (a["age"] - b["age"]) * this.sortNum;
              });
            }
            // 返回结果
            return newArr;
          },
        },
      });
    </script>
  </body>
```



### key 的重要性

```html
  <body>
    <div id="app">
      <button @click="addItem">添加成员</button>
      <!-- 遍历列表 -->
      <ul>
        <!-- 将index作为key -->
        <!-- <li v-for="(item,index) in persons" :key="index">
          {{item.name}}--{{item.age}}
          <input type="text" />
        </li> -->

        <!-- 将 id 作为key -->
        <li v-for="(item,index) in persons" :key="item.id">
          {{item.name}}--{{item.age}}
          <input type="text" />
        </li>
      </ul>
    </div>

    <script src="../js/vue.js"></script>

    <script>
      let vm = new Vue({
        el: "#app",
        data: {
          persons: [
            { id: "01", name: "张三", age: 20 },
            { id: "02", name: "李四", age: 21 },
            { id: "03", name: "王五", age: 22 },
            { id: "04", name: "赵六", age: 23 },
          ],
        },
        methods: {
          addItem() {
            // 创建对象
            const p = { id: "05", name: "老王", age: 24 };

            // 向后追加成员
            // this.persons.push(p);

            // 向前添加成员 (破坏数组顺序)
            this.persons.unshift(p);
          },
        },
      });

      /* 
        index 作为key 需要保证列表不会破坏原有的顺序操作情况下使用  
        in 作为 key 此时可以保证 Vnode 虚拟Dom的唯一性
      */
    </script>
  </body>
```



### 注册列表

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      html,
      body {
        background-color: #eee;
      }
      .alipay {
        position: relative;
        max-width: 600px;
        min-height: 300px;
        margin: 50px auto;
        background-color: #fff;
        padding: 20px;
      }
      label {
        margin-right: 20px;
        font-size: 16px;
      }
      input {
        padding: 10px;
        width: 300px;
      }
      ul,
      li {
        list-style: none;
        margin: 0;
        padding: 0;
      }
      ul {
        position: absolute;
        width: 321px;
        border: 1px solid #ccc;
        top: 59px;
        left: 109px;
      }
      li {
        font-size: 13px;
        line-height: 28px;
        color: #666;
        padding: 0 10px;
      }
      li:hover,
      li.choose {
        background-color: #eee;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <div class="alipay">
        <label for="">电子邮箱</label>
        <input
          type="text"
          v-model="msg"
          @keydown.up.prevent="preveChooseLi"
          @keydown.down="nextChooseLi"
          @keydown.enter="enterChooseLi"
        />
        <!-- 列表的显影与 msg 和 数组的 长度相关 -->
        <ul v-show="msg && dealArr.length">
          <li
            v-for="(item,index) in dealArr"
            @click="clickCurrentLi"
            :class="cls[index]"
          >
            {{dealMsg}}@{{item}}<template v-if="item == 189">.cn</template
            ><template v-else>.com</template>
          </li>
        </ul>
      </div>
    </div>

    <script src="../js/vue.js"></script>

    <script>
      let vm = new Vue({
        el: "#app",
        data: {
          arr: [
            "qq",
            "163",
            "126",
            "189",
            "sina",
            "hotmail",
            "gmail",
            "sohu",
            "21cn",
          ],
          // 搜索关键词
          msg: "",
          // 信号量
          num: -1,
          // 维护choose类
          cls: [],
          // 定义开关
          lock: false,
        },
        // 计算属性数据
        computed: {
          // 处理搜索词
          dealMsg() {
            // 查找是否出现 @ 符号
            let index = this.msg.indexOf("@");
            // 判断是否出现
            if (index > 0) {
              // 截取符号之前的内容
              return this.msg.slice(0, index);
            }
            // 没有输入符号正常返回数据
            return this.msg;
          },
          // 处理数组
          dealArr() {
            // 查找是否出现@符号
            let index = this.msg.indexOf("@");
            // 判断是否出现
            if (index >= 0) {
              // 获取@符号之后的内容
              let str = this.msg.slice(index + 1);

              // 过滤
              return this.arr.filter((item) => {
                // 补全长度
                item += item == "189" ? ".c" : ".co";

                // 判断输入的内容是否在开头
                return item.indexOf(str) === 0;
              });
            }

            // 正常返回
            return this.arr;
          },
        },
        // 定义方法
        methods: {
          // 获取索引值
          getIndex(len) {
            /* 
              无论一个数字大小 对某一个固定数字取余 结果都将保持在固定数字范围区间之内
              该数字的范围是 -len ~ len 之间 如果不需要出现负数 加上 len，此时结果 0 ~ 2len
              之后 再次取余
            */
            // let num = this.num & len;
            // num = num + len;
            // num = num % len;

            // 简写:
            return ((this.num % len) + len) % len;
          },
          // 改变样式
          changestyle() {
            // 获取数组的长度
            let len = this.dealArr.length;
            // 根据数据长度获取每一个li对应的真实索引值
            let idx = this.getIndex(len);

            // 先删除上一个的背景颜色
            this.cls = [];
            // 设置
            this.$set(this.cls, idx, "choose");
          },
          // 选中上一个li
          preveChooseLi() {
            // 判断是否第一次按下
            if (this.lock) {
              this.num--;
            }

            // 开锁
            this.lock = true;

            // 改变样式
            this.changestyle();
          },
          // 选中下一个li
          nextChooseLi() {
            this.num++;
            this.lock = true;
            this.changestyle();
          },
          // 回车选中当前li
          enterChooseLi() {
            // 获取数组的 长度
            let len = this.dealArr.length;
            // 根据数组的长度获取每一个li对应的真实元素
            let idx = this.getIndex(len);
            let item = this.arr[idx];

            // 数据驱动视图
            this.msg =
              this.dealMsg + "@" + item + (item == "189" ? ".cn" : ".com");
          },
          // 点击选中当前li
          clickCurrentLi(e) {
            this.msg = e.target.innerHTML;
          },
        },
      });
    </script>
  </body>
</html>

```



## 1.1 过渡

我们学习v-show指令和v-if指令，这些指令可以显示隐藏元素或者创建删除元素。

vue允许我们在显示隐藏元素以及创建删除元素过程中，**添加过渡效果**。

![image-20230209152959070](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230209152959070.png)

`transition`元素(组件)是由vue提供的。可以为内部的元素添加过渡效果。

​		 可以通过`name`属性设置过渡名称，之后就会根据该名称创建六个类， 

​		 例如 name=”demo”

​				 表示显示的过程（由隐藏的状态变成显示的状态）

​						 `.demo-enter` 	`.demo-enter-to` 	`.demo-enter-active`

​				 表示隐藏的过程（由显示状态变成隐藏的状态）

​						 `.demo-leave` 	`.demo-leave-to` 		`.demo-leave-active`

​		我们基于这六个类，实现css过渡或者动画（借助于css3实现的）



`.demo-enter-active`

`.demo-leave-active`

```css
.box {
  position: relative;
  width: 100px;
  height: 50px;
  background-color: gold;
  font-size: 20px;
}

// 动画
@keyframes donghua {
  from {
    transform: translateX(600px);
  }
  to {
    transform: translateX(0);
  }
}

// 显示过程
.demo-enter-active {
  animation: donghua 1s;
}

// 隐藏过程
.demo-leave-active {
  animation: donghua 1s reverse;
}
```



`.demo-enter` 	`.demo-enter-to` 

`.demo-leave` 	`.demo-leave-to` 

```css
// 过渡
// 显示态
.demo-enter-to,
.demo-leave {
  transform: translateX(0);
}

// 隐藏态
.demo-enter,
.demo-leave-to {
  transform: translateX(600px);
}

// 过程
// .demo-enter-active,
// .demo-leave-active {
//   transition: all 1s;
// }
```





```html
<body>
    <div id="app">
      <button @click="isShow = !isShow">切换</button>

      <!-- 
          通过 transition 组件设置过渡 
              可以通过name设置过渡名称
          表示显示的过程（由隐藏的状态变成显示的状态）
              .v-enter 	.v-enter-to 	.v-enter-active
          表示隐藏的过程（由显示状态变成隐藏的状态）
              .v-leave 	.v-leave-to 		.v-leave-active
      -->
      <transition name="demo">
        <div class="box" v-if="isShow"></div>
      </transition>
    </div>

    <script src="../js/vue.js"></script>

    <script>
      let vm = new Vue({
        el: "#app",
        data: {
          isShow: true,
        },
      });
    </script>
  </body>
```

**入场过渡**

​		 我们通过为transition组件添加`appear`属性，实现入场过渡：当元素加载的时候，执行动画。



```html
      <transition name="demo" appear>
        <div class="box" v-if="isShow"></div>
      </transition>
```



**过渡事件**

css3过渡和动画有事件，我们可以通过DOM监听到。

​		 过渡事件：`webkitTransitionStart`，`webkitTransitionEnd`

​		 动画事件：`webkitAnimationStart`, `webkitAnimationEnd`.

vue实现的过渡也可以监听动画开始与结束的事件，

​		显示过程：

​			    `before-enter`：处于隐藏状态；

​				`after-enter`：处于显示状态；

​				`enter`：显示过程

​		隐藏过程

​				 `before-leave`：处于显示状态；

​				`after-leave`：处于隐藏状态；

​				`leave`：隐藏过程

​		我们可以通过v-on指令或者是@语法糖来监听这些事件。



```html
  <body>
    <div id="app">
      <button @click="isShow = !isShow">切换</button>
      <!-- 
        显示过程：
          before-enter：处于隐藏状态；
          after-enter：处于显示状态；
          enter：显示过程
        隐藏过程
            before-leave：处于显示状态；
            after-leave：处于隐藏状态；
            leave：隐藏过程
      -->
      <transition
        appear
        @before-enter="beforeEnter"
        @after-enter="afterEnter"
        @enter="enter"
        @before-leave="beforeLeave"
        @after-leave="afterLeave"
        @leave="Leave"
      >
        <div class="box" v-if="isShow"></div>
      </transition>
    </div>

    <script src="../js/vue.js"></script>

    <script>
      let vm = new Vue({
        el: "#app",
        data: {
          isShow: true,
        },
        methods: {
          beforeEnter() {
            console.log("beforeEnter");
          },
          afterEnter() {
            console.log("afterEnter");
          },
          enter() {
            console.log("enter");
          },
          beforeLeave() {
            console.log("beforeLeave");
          },
          afterLeave() {
            console.log("afterLeave");
          },
          Leave() {
            console.log("Leave");
          },
        },
      });
    </script>
```





**多元素过渡**

我们可以在transition中，定义多个元素，实现多个元素之间的过渡。

​		 内部的元素必须设置`key`属性，属性值是唯一的。

我们通过`mode`属性定义切换模式：

​		 `in-out` 新元素先执行，再执行当前的元素

​		 `out-in` 当前的元素先执行，再执行新元素。

​		 默认两个元素同时执行。



```html
  <body>
    <div id="app">
      <button @click="isShow = !isShow">切换</button>
      <!-- 
          在transition中，定义多个元素，实现多个元素之间的过渡。

          内部的元素必须设置key属性，属性值是唯一的。

          通过mode属性定义切换模式：
            in-out 新元素先执行，再执行当前的元素
            out-in 当前的元素先执行，再执行新元素。
            默认两个元素同时执行。  
      -->
      <!-- 默认两个元素同时执行。  -->
      <!-- <transition name="demo">
        <div class="box" v-if="isShow" key="1">1</div>
        <div class="box1" v-else key="2">2</div>
      </transition> -->

      <!-- in-out 新元素先执行，再执行当前的元素 -->
      <!-- <transition name="demo" mode="in-out">
        <div class="box" v-if="isShow" key="1">1</div>
        <div class="box1" v-else key="2">2</div>
      </transition> -->

      <!-- out-in 当前的元素先执行，再执行新元素。 -->
      <transition name="demo" mode="out-in">
        <div class="box" v-if="isShow" key="1">1</div>
        <div class="box1" v-else key="2">2</div>
      </transition>
    </div>

    <script src="../js/vue.js"></script>

    <script>
      let vm = new Vue({
        el: "#app",
        data: {
          isShow: true,
        },
      });
    </script>
  </body>
```

```css
.box {
  position: relative;
  width: 100px;
  height: 50px;
  background-color: gold;
  font-size: 20px;
}

// 多元素过渡
.box1 {
  position: relative;
  width: 1000px;
  height: 50px;
  background-color: skyblue;
  font-size: 20px;
}

// 过渡
// 显示态
.demo-enter-to,
.demo-leave {
  transform: translateX(0);
}

// 隐藏态
.demo-enter,
.demo-leave-to {
  transform: translateX(600px);
}

// 过程
.demo-enter-active,
.demo-leave-active {
  transition: all 1s;
}
```



## 1.2 列表过渡

我们通过`v-for`指令渲染列表。

使用v-for指令创建列表元素的时候，如果需要过渡，要使用`transition-group`组件。

​		 与transition组件的区别是：transition-group会渲染成一个真实的元素，

​				 默认是span，通过`tag`属性可以自定义渲染的结果。

使用列表过渡的时候，每一个元素都要添加一个值是唯一的并且稳定的`key`属性。

transition与transition-group的区别：

​		 transition 控制一个元素

​		 transition-group 控制多个元素



- 案例 随机位置 随机添加数字

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link rel="stylesheet" href="./style/transtion.css" />
  </head>
  <body>
    <div id="app">
      <button @click="random">随机位置</button>
      <button @click="addItem">随机添加成员</button>
      <!-- 
        通过v-for指令渲染列表。

        与transition组件的区别是：transition-group会渲染成一个真实的元素，
        transition与transition-group的区别：
            transition 控制一个元素
            transition-group 控制多个元素
      -->

      <!-- 默认是span，通过tag属性可以自定义渲染的结果。 -->
      <!-- 使用v-for指令创建列表元素的时候，如果需要过渡，要使用transition-group组件。 -->
      <transition-group tag="div" class="contain">
        <!-- 使用列表过渡的时候，每一个元素都要添加一个值是唯一的并且稳定的key属性。 -->
        <span v-for="item in arr" :key="item">{{item}}</span>
      </transition-group>
    </div>

    <script src="../js/vue.js"></script>

    <script>
      let vm = new Vue({
        el: "#app",
        data: {
          arr: [0, 1, 2, 3, 4, 5],
        },
        methods: {
          // 随机位置
          random() {
            this.arr.sort(() => {
              // 乱序 随机返回正负1
              return Math.random() > 0.5 ? 1 : -1;
            });
          },
          addItem() {
            // 追加元素
            this.arr.splice(
              // 操作的位置
              parseInt(Math.random() * this.arr.length),
              // 删除的个数
              0,
              // 从第三个参数开始表示插入(替换)的元素
              this.arr.length
            );
          },
        },
      });
    </script>
  </body>
</html>

```



## 1.3 自定义指令

指令是对DOM元素的拓展，使其具有一定的行为特征（功能）。

我们已经学习的指令：

​		 v-bind, v-text, v-html, v-once, v-model, v-cloak, v-on, v-show, v-if, v-else-if, v-else, v-for等等。

内置的指令是有限的，有时候我们需要对元素拓展更多的功能，我们要自定义指令。



自定义指令只需要两步，



​	 第一步，在模板中使用：指令都是以v-为前缀，字母小写，横线分割单词。

​	 第二步，在js中定义指令。有两种定义方式

​			 第一种：全局定义：`Vue.directive(name, fn | {} )`

​					 全局定义的指令可以在所有vue实例化对象（组件）中使用

​					 directive方法不能解构，要在vue实例化对象之前定义。

​			 第二种：局部定义：`directives: { name: fn | {} }`

​					 局部定义的指令只能在当前vue实例化对象(组件)中使用



​					 `name`表示指令的名称：省略v-前置，使用驼峰式命名

​					

​					 `fn`表示指令函数，处理方法。

​					 `{}` 表示指令对象，可以定义一些方法

​							 `bind` ：将指令绑定给元素时候执行的方法；

​							 `update`：指令的属性值发生改变的时候执行的方法

​							 `unbind`：指令从元素上解除绑定时候执行的方法；

​							 `inserted`：指令所在的元素从页面中删除时候，执行的方法；

​							 `componentUpdated`：指令所在的组件更新的时候执行的方法。



​					 不论是指令对象中的方法，还是指令函数，都有四个参数

​							 第一个参数表示指令所在的元素。

 							第二个参数 表示指令对象，包含指令的一些信息

​									 例如：指令名称，属性值表达式，当前的属性值，上一个属性值等等

​							 第三个参数表示当前的虚拟DOM对象。

​							 第四个参数表示上一个虚拟DOM对象



注意：当多次使用指令的时候，一个指令的属性值改变，所有指令相关的回调函数都会执行。

为了提高性能，我们可以在回调函数中判断当前的值与上一个值是否相同，不同再执行。



 注：工作中，我们自定义的指令通常带有命名空间前缀。



```html
<body>
    <div id="app">
      <!-- 自定义指令 -->
      <input type="text" v-model="a" />
      <h1 v-demo-html="'实现的v-html:'+a"></h1>
      <!-- 使用相同的指令 -->
      <h1 v-demo-html="title"></h1>

      <!-- 实现 v-once 指令 -->
      <h1 v-demo-once="a"></h1>

      <!-- 实现的 v-show 指令 -->
      <button @click="isShow = !isShow">切换</button>
      <h1 v-demo-show="isShow">v-show</h1>
    </div>

    <script src="../js/vue.js"></script>

    <script>
      /*
        定义全局指令
          Vue.directive(name, fn | {} )
            name表示指令的名称：省略 v- ，使用驼峰式命名
          不论是指令对象中的方法，还是指令函数，都有四个参数
            第一个参数表示指令所在的元素。
            第二个参数 表示指令对象，包含指令的一些信息
                  例如：指令名称，属性值表达式，当前的属性值，上一个属性值等等
            第三个参数表示当前的虚拟DOM对象。
            第四个参数表示上一个虚拟DOM对象
      */
      // 函数的形式
      // Vue.directive("demoHtml", (...args) => {
      //   console.log(args);
      // });

      /*
        对象的形式
          bind ：将指令绑定给元素时候执行
          update ：指令的属性值发生改变的时候
          unbind：指令从元素上解除绑定时候
          inserted：指令所在的元素从页面中删除时候
          componentUpdated：指令所在的组件更新的时候
      */
      // Vue.directive("demoHtml", {
      //   bind(h1, obj) {
      //     console.log("bind");
      //   },
      //   update(h1, obj) {
      //     console.log("update");
      //   },
      // });

      // 以函数的形式 实现v-html
      Vue.directive("demoHtml", (h1, obj) => {
        // 优化
        /*
          注意：当多次使用指令的时候，一个指令的属性值改变，所有指令相关的回调函数都会执行。
            为了提高性能，我们可以在回调函数中判断当前的值与上一个值是否相同，不同再执行。
        */
        if (obj.value !== obj.oldValue) {
          console.log(h1, obj);
          h1.innerHTML = obj.value;
        }
      });

      let vm = new Vue({
        el: "#app",
        data: {
          a: '<a href="">我是a标签</a>',
          title: "nihao",
          isShow: true,
        },
        methods: {},
        // 局部指令
        directives: {
          // 使用对象的形式
          demoOnce: {
            bind(h1, obj) {
              h1.innerHTML = obj.value;
            },
          },
          // 实现 v-show
          demoShow(h1, obj) {
            h1.style.display = obj.value ? "" : "none";
          },
        },
      });
    </script>
  </body>
```


