# vue第2天

# 一、Vue

### 1.1 单选框

我们将input元素的type属性设置为`radio`就可以得到单选框，通过`checked`属性设置其选中状态。

对单选框实现数据双向绑定，也是用v-model指令。

特点：

​		 1 一组单选框绑定同一份数据。

​		 2 选框的值通过value属性定义。

​		 3 此时checked属性失效了。

​		 4 一组单选框的默认值就是绑定数据的值。



```html
    <div id="app">
      <div>
        <!-- 
          单选框
            通过将input元素的type设置为radio  通过checked属性设置其选中状态
            特点
              1.一组单选框绑定同一份数据
              2.选框的值通过value属性定义
              3.此时checked属性失效
              4.一组单选框的默认值就是绑定数据的值
        -->
        <input type="radio" v-model="sex" value="man" checked />男
        <input type="radio" v-model="sex" value="woman" />女
        <h1>{{sex}}</h1>
      </div>
    </div>
```

```js
import Vue from 'vue'

new Vue({
  el: '#app',
  data: {
    // 单选框数据
    // 4.一组单选框的默认值就是绑定数据的值
    sex: 'man',
  }
})
```



### 1.2 多选框

我们将input元素的type属性设置为`checkbox`就可以得到多选框，通过`checked`属性设置其选中状态。

对多选框实现数据双向绑定，也是用v-model指令。

特点：

​		 1 一组多选框绑定不同的数据，为了访问方便，我们将其放在同一个命名空间下。

​		 2 选框的值==默认是布尔值 ==

​				通过v-bind:true-value以及v-bind:false-value可以自定义其值：	内部是JS环境

​				 :true-value：定义选中时候的值 

​				:false-value：定义未选中时候的值。

​		 3 checked属性失效了

​		 4 一组多选框的默认值就是绑定数据的值，如果自定义了:true-value以及:false-value属性，就是自定义数据值。



```html
      <div>
        <!-- 
          多选框 type类型设置为checkbox 
          特点
            1.一组多选框绑定不同的数据 为了访问方便 将其放在同一个命名空间下
            2.选框的默认值是布尔值 
              通过 v-bind:true-value v-bind:false-value 可以自定义其值
              :true-value 定义选中时候的值
              :fasle-value 定义未选中时候的值
                注意内部是js环境
            3.此时绑定数据后 checked属性就失效了
            4.一组多选框的默认值就是绑定数据的值  
              如果自定义了 :true-value以及:false-value属性 就是自定义数据值
        -->
        <label for="">请选择兴趣爱好：</label>
         <input type="checkbox" v-model="interset.basketball"  :true-value="'选中了'"  :false-value="'未选中'" checked />篮球 	    <input type="checkbox" v-model="interset.football" />足球
        <input type="checkbox" v-model="interset.pingpang" />乒乓球
        <h1>{{interset}}</h1>
      </div>
```

```js
import Vue from 'vue'

new Vue({
  el: '#app',
  data: {
    // 多选框
    interset: {
      /* 一组多选框的默认值就是绑定数据的值
              如果自定义了 :true-value以及:false-value属性 就是自定义数据值 */
      basketball: '选中了',
      football: true
    }
  }
})
```



### 1.3 下拉框

我们通过`select`元素定义下拉框，通过`option`元素定义选项，

​		 选项的值默认是内容值，设置了value就是value值。

我们通过为select元素添加v-model指令实现数据双向绑定。

​		 单选下拉框绑定的是字符串。

​		 多（复）选下拉框绑定的是数组。

我们通过为select元素设置`multiple`属性，实现单选下拉框与多选下拉框的切换。



```html
      <div>
        <!-- 
        通过 select 定义下拉框  通过option 定义选项
          选项的值默认是内容值 设置了value就是value值
          通过select添加v-model实现数据双向绑定
            单选下拉框绑定的是字符串
            多选下拉框绑定的是数组
      -->
        <!-- 单选下拉框 -->
        <select v-model="colors1">
          <option value="isRed">red</option>
          <option value="isGreen">green</option>
          <option value="isBlue">blue</option>
        </select>

        <h1>{{colors1}}</h1>

        <!-- 多选下拉框 通过为select元素设置multiple属性-->
        <select v-model="colors2" multiple>
          <option value="isRed">red</option>
          <option value="isGreen">green</option>
          <option value="isBlue">blue</option>
        </select>

        <h1>{{colors2}}</h1>
      </div>
```

```js
import Vue from 'vue'

new Vue({
  el: '#app',
  data: {
    // 下拉框 单选下拉框绑定的是字符串
    colors1: 'blue',

    // 多选下拉框 绑定的是数组
    colors2: ['isRed', 'isGreen']
  }
})
```





### 1.4 数据监听器

vue实现了数据双向绑定，当模型中的数据改变，会被视图监听到。进而同步到视图中。

如果想在js中，监听模型中数据的改变，可以使用监听器技术

​	 我们在实例化对象中的`watch`属性中，定义监听器，是一个对象

​			 key   被监听的数据名称

​			 value  当数据改变的时候，执行的回调函数。

​				 第一个参数表示新的值， 

​				 第二个参数表示旧的值。

​				  this指向vue实例

 注意：watch不仅仅可以监听模型中的数据，实例化对象中，设置了特性的数据都可以监听，例如：路由数据等。

​		**==不应该使用箭头函数来定义== method 函数** (例如 `plus: () => this.a++`)。理由是箭头函数绑定了父级作用域的上下文，所以 `this` 将不会按照期望指向 Vue 实例，`this.a` 将是 undefined。



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <input type="text" v-model="msg" />
      <h1>{{msg}}</h1>
    </div>
    <script src="./dist/02.js"></script>
  </body>
</html>
```

```js
import Vue from 'vue'

new Vue({
  el: '#app',
  data: {
    msg: 'hello msg',
  },
  // 监听数据的变化
  watch: {
    // key 被监听的数据
    // value  表示处理函数
    msg(newValue, oldValue) {
      console.log(newValue, oldValue);
    }
  }
})
```



### 1.5 状态监听

状态监听就是当数据改变的时候，我们平滑的将其过渡到某个值。

​	 我们在监听器中，监听数据的变化。

​	 我们再启动循环定时器，改变数据。



```js
import Vue from 'vue'

// 声明定时器句柄
let timer

new Vue({
  el: '#app',
  data: {
    total: 0,
    num: 0
  },
  // 监听数据的变化
  watch: {
    // key 被监听的数据
    // value  表示处理函数

    // 监听total
    total() {
      // 开启定时器之前关闭定时器
      clearInterval(timer)

      // 赋值定时器
      timer = setInterval(() => {
        // 拦截判断
        // 注意 默认数据都是number 一旦数据改变之后就变成string类型 所有使用==
        if (this.num == this.total) {
          return clearInterval(timer)
        }

        // this.num = this.total > this.num ? this.num + 1 : this.num - 1
        // 简写
        this.num += this.total > this.num ? 1 : -1

      }, 200);
    }
  }
})
```

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app">
      <!-- 状态过渡 -->
      <input type="text" v-model="total" />
      <h1>{{num}}</h1>
    </div>

    <script src="./dist/02.js"></script>
  </body>
</html>

```



### 1.6 DOM 事件

- 事件绑定：vue为了绑定DOM事件，提供了`v-on`指令。

  ​		 语法：`v-on:click=”fn()”`

- v-on指令的语法糖是`@`

  也可以写成 `@click=”fn()”`

  fn代表事件回调函数，定义在vue实例的`methods`属性中。

  ​	methods中定义的方法跟data中的定义的数据一样，添加实例化对象自身了。

  ​	methods中定义的方法，内部的this指向vue实例化对象。

  ​	methods中定义的方法不仅仅可以在事件中使用，所有可以获取实例化对象的地方，都可以通过实例化对象来使用该方法。

- **参数集合**

  ​	`( )`表示参数集合，可有可无。

  

​			 如果没有定义参数集合，

​					 默认有一个参数，就是事件对象



​			 如果添加了参数集合，

​					 默认没有参数，此时使用什么数据可以在参数集合中传递。

​					 想使用事件对象，要传递`$event`。



vue中事件绑定技术，没有使用事件委托技术，是直接绑定给元素的。

因此事件对象是源生的事件对象。



```html
  <body>
    <div id="app">
      <!-- 通过 v-on 指令绑定事件 -->
      <!-- v-on:click = fn() -->

      <!-- 没有传递参数集合 默认有一个原生事件对象 -->
      <button v-on:click="clickBtn1">clickBtn1</button>

      <!-- 传递参数集合 此时使用什么数据就传递什么数据 
            使用事件对象 要传递$event -->
      <button v-on:click="clickBtn2(100,true,'abc',$event)">clickBtn2</button>

      <!-- 语法糖 @click=fn -->
      <button @click="clickBtn3">clickBtn3</button>
    </div>

    <script src="./dist/03.js"></script>
  </body>
```

```js
import Vue from 'vue'

new Vue({
  el: '#app',
  data: {

  },
  // 方法定义在 methods中
  methods: {
    clickBtn1() {
      console.log(this, arguments);
    },
    clickBtn2() {
      console.log(this, arguments);
    },
    clickBtn3() {
      console.log(this, arguments);

      // 可以访问其他方法
      this.clickBtn2()
    }
  },
})
```



### 1.7 深度监听 与 立即执行

`deep`			该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深

`immediate`	该回调将会在监听开始之后被立即调用



```html
  <body>
    <div id="app">
      <button @click="n++">点我n++</button>
      <h1>{{n}}</h1>

      <hr />

      <button @click="number.n++">点我number.n++</button>
      <h2>{{number}}</h2>
    </div>

    <script src="./dist/04.js"></script>
  </body>
```



```js
import Vue from 'vue'

new Vue({
  el: '#app',
  data: {
    n: 1,
    // 深层次数据
    number: {
      n: 1
    }
  },
  // 定义数据监听器
  watch: {
    // 函数的形式
    // n() {
    //   console.log('n以函数的形式改变了');
    // }

    // 对象的形式
    // n: {
    //   handler: function () {
    //     console.log('n以对象的形式改变了');
    //   },
    //   // 1. immediate 该回调将会在监听开始之后被立即调用
    //   immediate: true
    // }

    // 监听 number 无法被监听
    // number() {
    //   console.log('number以函数的形式改变');
    // }

    // 支持多层级
    // 'number.n'() {
    //   console.log('number.n支持多层级以函数的形式改变');
    // }

    // 2. 对象的形式
    number: {
      handler() {
        console.log('number以对象的形式改变');
      },
      // 该回调会在任何被侦听的对象的 property 改变时被调用，不论其被嵌套多深
      deep: true
    }
  }
})
```



### 1.8 computed 和 watch 区别

computed和watch区别:

​	computed: 1 computed默认是走缓存 只有当依赖的数据发生变化 才会重新计算   2 不支持异步监听

​	watch:   1 没有缓存性的 更多的是观察的作用  只要数据改变 就会执行对应的函数  2 支持异步监听的



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <div id="app1">
      姓：<input type="text" v-model="firstName" />
      <br />
      名：<input type="text" v-model="lastName" />
      <h1>全称：{{fullName}}</h1>
    </div>

    <div id="app2">
      <div id="app1">
        姓：<input type="text" v-model="firstName" />
        <br />
        名：<input type="text" v-model="lastName" />
        <h1>全称：{{fullName}}</h1>
      </div>
    </div>

    <script src="./dist/05.js"></script>
  </body>
</html>

```



```js
import Vue from 'vue'

// computed
new Vue({
  el: '#app1',
  data: {
    firstName: '张',
    lastName: '三',
  },
  // 计算属性数据
  computed: {
    fullName() {
      // 异步函数
      setTimeout(() => {
        return this.frstName + '' + this.lastName
      }, 1400);
    }
  }
})

// watch
new Vue({
  // 绑定视图
  el: '#app2',
  // 绑定数据
  data: {
    firstName: '张',
    lastName: '三',
    fullName: ''
  },
  watch: {
    firstName(newValue) {
      setTimeout(() => {
        this.fullName = newValue + '-' + this.lastName;
      }, 3000);
    },
    lastName() {
      setTimeout(() => {
        this.fullName = this.firstName + '-' + this.lastName;
      }, 3000);
    }
  }
})

// computed和watch区别:
    // computed: 1 computed默认是走缓存 只有当依赖的数据发生变化 才会重新计算    2 不支持异步监听
    // watch:    1 没有缓存性的 更多的是观察的作用  只要数据改变 就会执行对应的函数  2 支持异步监听的
```



### 1.9 事件修饰符

我们使用jQuery的时候，想判断事件绑定的元素与事件触发的元素是同一个元素的功能，要获取事件对象，再通过判断事件对象的e.target是否等于e.currentTarget，来确定结果，很麻烦。

```js
import Vue from 'vue'

new Vue({
  el: '#app',
  methods: {
    parentClick(e) {
      console.log('parent');

      //  我们使用jQuery的时候，想判断事件绑定的元素与事件触发的元素是同一个元素的功能，要获取事件对象，再通过判断事件对象的e.target是否等于e.currentTarget，来确定结果，很麻烦.
      //  如果 触发事件元素 和 绑定事件元素是同一个才执行
       if (e.target === e.currentTarget) {
         console.log('parent');
       }

    },
    clickBtn1() {
      console.log('clickBtn1');
    }
  },
})
```



​		 vue简化了这些流程，让我们在绑定事件的时候就实现这些功能。通过事件修饰符实现。

​		 语法： `v-on:click.修饰符=”fn”`

​				 “`.修饰符`”就表示事件修饰符，我们可以通过多个“.”来同时应用多个修饰符。



**通用修饰符**

​		 `stop`：实现阻止冒泡的修饰符。 

​		 `prevent`：实现阻止默认行为的修饰符。

​		 `once`：表示单次触发的修饰符。 

​		 `self`：表示绑定事件的元素与触发事件的元素必须是同一个元素。

​		 这些修饰符还可以混合使用。



```html
    <div id="app">
      <!-- 
        通用修饰符
          .stop  实现阻止冒泡
          .prevent  实现阻止默认行为
          .once 单次渲染
          .self 绑定的元素与触发的元素必须是同一个
      -->

      <!-- <div class="parent" @click="parentClick"> -->
      <div class="parent" @click.self="parentClick">
        parent
        <button @click="clickBtn1">冒泡</button>
        <button @click.stop="clickBtn1">阻止冒泡</button>

        <a @click="clickBtn1" href="https://baidu.com">百度</a>
        <!-- 修饰符可以混合使用 -->
        <a @click.prevent.stop="clickBtn1" href="https://baidu.com"
          >百度-阻止默认行为</a
        >

        <a @click.once.prevent.stop="clickBtn1" href="https://baidu.com"
          >百度-单次渲染</a
        >
      </div>
    </div>
```



**鼠标键修饰符**

​		 left：点击鼠标左键

​		 right：点击鼠标右键

​		 middle：点击鼠标中间件



```html
      <!-- 
          鼠标修饰符
            .left 鼠标左键修饰符
            .right  右键修饰符
            .middle 中键修饰符
      -->
      <button @click.left="clickBtn1">鼠标左键修饰符</button>
      <button @click.right="clickBtn1">鼠标右键修饰符</button>
      <button @click.middle="clickBtn1">鼠标中键修饰符</button>
```



**辅助键修饰符**

​		 ctrl：点击ctrl辅助键

​		 shift：点击shift辅助键

​		 alt：点击alt辅助键

​		 meta：点击window|command辅助键



```html
      <!-- 
        辅助键修饰符
          .ctrl Ctrl辅助键修饰符
          .shift shift辅助键修饰符
          .alt alt辅助键修饰符
          .meta 点击windows|(Mac)command辅助键修饰符
      -->
      <button @click.ctrl="clickBtn1">Ctrl辅助键修饰符</button>
      <button @click.shift="clickBtn1">shift辅助键修饰符</button>
      <button @click.alt="clickBtn1">alt辅助键修饰符</button>
      <button @click.meta="clickBtn1">meta辅助键修饰符</button>
```



**键盘事件修饰符**

​		键盘事件：`keydown`，keyup，keypress（输入有效键）

​		 当我们绑定键盘事件的时候，可以使用键盘事件修饰符。

​		 有效修饰符：esc， tab， space， `enter`， delete（delete|backspace），up, down, left, right, 以及所有字母键。



```html
      <!-- 
        键盘修饰符
          键盘事件 keydown  keyup   keypress(输入有效键)
        有效修饰符
          esc tab space enter delete(|backspace)  up  down  left  right 以及所有字母键
      -->
      <input type="text" placeholder="esc" @keydown.esc="clickBtn1" />
      <input type="text" placeholder="tab" @keydown.tab="clickBtn1" />
      <input type="text" placeholder="space" @keydown.space="clickBtn1" />
      <input type="text" placeholder="enter" @keydown.enter="clickBtn1" />
      <input type="text" placeholder="delete|backspace" @keydown.delete="clickBtn1"
      />
      <input type="text" placeholder="left" @keydown.left="clickBtn1" />
      <input type="text" placeholder="right" @keydown.right="clickBtn1" />
      <input type="text" placeholder="a" @keydown.a="clickBtn1" />
```



`.lazy`  当改变数据完毕之后 一次性更新数据(默认是边写边改)

```html
      <!-- .lazy  当改变数据完毕之后 一次性更新数据(默认是边写边改) -->
      <input type="text" v-model="msg" />
      <input type="text" v-model.lazy="msg" />
      <h1>{{msg}}</h1>
```



`.number`  保证所有的数字都是number  默认改变了数据之后是string

```html
      <!-- .number  保证所有的数字都是number  默认改变了数据之后是string -->
      <input type="text" v-model="number" />
      <input type="text" v-model.number="number" />
      <h1>{{typeof number}}</h1>
```



`.trim`  去掉左右多余的空白字符

```html
      <!-- .trim  去掉左右多余的空白字符 -->
      <input type="text" v-model="title" />
      <input type="text" v-model.trim="title" />
      <h1>{{title}}</h1>
```



### 1.10 类的绑定

在vue中，为元素绑定类有三种方式

​		 `:class=”{}”`

​				 key   表示一组类的名称（可以包含空格，切割成多个类）

​				 value 表示是否保留这组类



​		 `:class=”[]”`

​				 每一个成员代表一组类（可以包含空格，切割成多个类）



​		 `:class=”str”`

​				 类名之间用空格隔开。



```html
    <div id="app">
      <!-- 对象的形式 -->
      <!-- 
        key 表示一组类的名称(可以包含空格 切割成多个类)
        value 布尔值  表示是否保留这组类
      -->
      <!-- <h1
        :class="{
        red:true,
        'fz-50':true,
        'bg-pink':false
      }"
      >
        hello world
      </h1> -->

      <!-- 也可以写成： 在js中定义出来-->
      <h1 :class="classObject">对象的形式</h1>

      <!-- 数组的形式 -->
      <!-- <h1 :class="['red','fz-50']">数组的形式</h1> -->

      <!-- 也可以写成：在js中定义出来 -->
      <h1 :class="classArray">数组的形式</h1>

      <!-- 字符串的形式 当中是变量  设置多个类 字符串拼接 引号空格-->
      <h1 :class="cls + ' red' ">字符串的形式</h1>
    </div>
```

```js
import Vue from 'vue'

// 引入样式文件
import './demo.scss'

new Vue({
  el: '#app',
  data: {
    classObject: {
      red: true,
      'fz-50': true,
      'bg-pink': false
    },
    classArray: ['red', 'fz-50', 'bg-pink'],
    cls: 'bg-pink'
  }
})
```



- 案例显示隐藏

```html
      <!-- 案例 -->
      <!-- 对象的形式 -->
      <!-- :class="{choose:cls2}" -->
      <!-- 数组的形式 -->
      <!-- :class="[cls2]" -->
      <!-- 字符串的形式 -->
      <!-- :class="cls2" -->
      <div
        class="blog"
        :class="cls2"
        @mouseenter="showBLog"
        @mouseleave="hideBlog"
      >
        <span>博客</span>
        <ul>
          <li>微博评论</li>
          <li>未读提醒</li>
        </ul>
      </div>
    </div>
```

```js
import Vue from 'vue'

// 引入样式文件
import './demo.scss'

new Vue({
  el: '#app',
  data: {
    cls2: false
  },
  methods: {
    showBLog() {
      // 对象的形式
      // this.cls2 = true
      // 数组的形式
      this.cls2 = ['choose']
    },
    hideBlog() {
      // 对象的形式
      // this.cls2 = false
      // 数组的形式
      this.cls2 = ['']
    }
  },
})
```



### 1.11 样式的绑定

与绑定类一样，绑定样式也有三种方式

​		 `:style=”{}”` 绑定样式对象

​				 key   表示样式名称，建议使用驼峰式命名

​				 value  表示样式属性值



​		 `:style=”[]”` 绑定多组样式对象

​				 每一个成员是一个对象，代表一组样式

​				 key   表示样式名称，建议使用驼峰式命名

​				 value  表示样式属性值



​		 `:style=”str”` 做样式字符串拼接。



```html
    <div id="app">
      <!-- 
	对象的形式
              :style="{}" 绑定样式对象
                key   表示样式名称，建议使用驼峰式命名
                value  表示样式属性值
      -->
      <h1 :style="{color:'red',fontSize:'50px'}">对象的形式</h1>

      <!-- 
        数组的形式
              :style="[]" 绑定多组样式对象
          每一个成员是一个对象，代表一组样式
          key   表示样式名称，建议使用驼峰式命名
          value  表示样式属性值
      -->
      <h2
        :style="[
      {background:'pink'},
      {color:'green'}
      ]"
      >
        数组的形式
      </h2>

      <!-- 
        字符串的拼接
          :style="str" 做样式字符串拼接。 str是变量
      -->
      <h3 :style=" 'color:' + str">字符串的拼接</h3>
    </div>
```

```js
import Vue from 'vue'

// 引入样式文件
import './demo.scss'

new Vue({
  el: '#app',
  data: {
    str: 'pink'
  },
})
```



- 案例

```html
      <div class="jd">
        <div>
          <label for="">颜色</label>
          <img
            :style="{ borderColor:imgs[0]}"
            @click="changeStyle(41)"
            src="../img/01.jpg"
            alt=""
          />
          <img
            :style="{ borderColor:imgs[1]}"
            @click="changeStyle(42)"
            src="../img/02.jpg"
            alt=""
          />
          <img
            :style="{ borderColor:imgs[2]}"
            @click="changeStyle(43)"
            src="../img/03.jpg"
            alt=""
          />
        </div>

        <div>
          <label for="">尺码</label>
          <span :class="{choose:spans[0]}">41</span>
          <span :class="{choose:spans[1]}">42</span>
          <span :class="{choose:spans[2]}">43</span>
        </div>
      </div>
```

```js
import Vue from 'vue'

// 引入样式文件
import './demo.scss'

new Vue({
  el: '#app',
  data: {
    imgs: [],
    spans: []
  },
  methods: {
    changeStyle(num) {
      switch (num) {
        case 41:
          // 使用#set
          this.$set(this.imgs, 0, 'red')
          this.$set(this.spans, 0, true)
          break;
        case 42:
          this.imgs = [, 'red'];
          this.spans = [true, true];
          break;
        case 43:
          this.imgs = [, , 'red'];
          this.spans = [];
          break;
        default:
          break;
      }
    }
  },
})
```



### 1.12 条件模板指令

模板指令就是控制元素创建和删除的指令。

条件模板指令

​		vue模拟了js中的if条件语句，提供了条件模板指令v-if

​				 if条件语句：`if () {} else if () {} else {}`

​		所以vue也为v-if指令提供了组合指令：`v-if`，`v-else-if`， `v-else`。

​				 v-if指令的属性值是布尔值

​						 true  表示创建这个元素

​						 false  表示删除这个元素

​				 是真正的创建和删除，不是显示和隐藏。



### 1.13 v-show

用来显示或者隐藏元素的指令，属性值是布尔值

​		 true  显示元素

​		 false  隐藏元素

 通过控制样式“display: none”实现的。

与v-if相比，有两点不同

​	  1 原理不同

​			 v-if是真正的创建和删除。 `v-show`是通过样式实现的显示和隐藏。

​	 2 组合指令

​			 v-if有组合指令（v-else-if，v-else）。v-show没有组合指令



​	由于v-show是通过修改样式实现的，因此性能更高。



```html
    <div id="app">
      <button @click="isShow1 = !isShow1">点我创建和删除元素</button>
      <h1 v-if="isShow1">hello world</h1>

      <!-- 组合指令 -->
      <button @click="n++">点我n++</button>
      <h1 v-if="n===1">Angular</h1>
      <h1 v-else-if="n===2">React</h1>
      <!-- 在使用组合指令时 中间不能打断 -->
      <!-- <span></span> -->
      <h1 v-else-if="n===3">Vue</h1>
      <h1 v-else>啦啦啦</h1>

      <hr />
      <!-- v-show 显影指令 -->
      <button @click="isShow = !isShow">点我控制显影</button>
      <h1 v-show="isShow">hello world</h1>
    </div>

    <!-- 在没有频繁操作元素的情况下使用 v-if
          反之使用v-show-->
```

```js
import Vue from 'vue'

new Vue({
  el: '#app',
  data: {
    isShow: true,
    isShow1: true,
    n: 1
  },
  // 方法定义在 methods中
  methods: {
  },
})
```



### 1.14 循环模板指令

vue模拟js中的for in循环语句实现了`v-for`循环模板指令：

有两种用法：

​		 第一种：`v-for=”item in data”`

​		 第二种：`v-for=”(item, index) in data”`

​				 v-for和in都是关键字

​				 data表示循环的数据，可以是数字，对象和数组

​						 如果是数字：item 表示数值（从1计数）。 index 表示索引值（从0计数）

​						 如果是对象：item 表示value（属性值）。 index 表示key（属性名称）

​						 如果是数组：item 表示成员值。 index 表示索引值



使用v-for指令的时候，我们要==设置key属性==，要求属性值时候唯一的。

​		 我们可以设置成员值item中的唯一属性，如id等

​		 我们还可以设置索引值index。

​				 建议用成员中的唯一属性。

注意：

​		 1 在vue cli 脚手架中，必须设置`key`属性

​		 2 循环中的变量item和index只在循环中生效，循环结束后被销毁。



```html
    <div id="app">
      <!-- 
        循环模板指令
          v-for="(item,index) in data"
            data 表示循环的数据 可以是数字 对象 数组
              数字  item表示数值从1开始 index表示索引值从0开始
              数组  item表示成员值  index表示索引值
              对象  item表示value属性值 index表示Key属性

          设置key属性 
        -->

      <ul>
        <label for="">数字</label>
        <!-- 数字  item表示数值从1开始 index表示索引值从0开始 -->
        <li v-for="(item,index) in num" :key="item">{{item}} {{index}}</li>
        <hr />
        <!-- 数组   item表示成员值  index表示索引值 -->
        <label for="">数组</label>
        <li v-for="(item,index) in colors" :key="item">{{item}} {{index}}</li>
        <hr />
        <!-- 对象   item表示value属性值   index表示key属性 -->
        <label for="">对象</label>
        <li v-for="(value,key) in obj" :key="key">{{value}} {{key}}</li>
      </ul>
    </div>
```

```js
import Vue from 'vue'

new Vue({
  el: '#app',
  data: {
    num: 10,
    obj: {
      title: 'nihao',
      color: 'red',
      abc: 'hello abc'
    },
    colors: ['red', 'green', 'blue', 'pink', 'purple']
  },
})
```



### 1.15 模板元素

当我们想渲染多个兄弟元素的时候，那么我们就要将指令放在父元素上，这样会导致引入其它的元素。

为了避免引入其它的元素，vue建议我们使用模板元素。

​		 在html中定义模板元素有两种语法：

​				 第一种：通过script模板标签定义

​				 第二种：html5提供了`tempalte`模板元素。

​		 vue建议我们使用template模板元素。

模板元素跟普通的元素一样，可以包含子元素，可以添加属性，可以设置指令等等，但就是不会渲染到页面中，我们将循环指令添加给模板元素，就不会引入其它的元素了。

​		 注意：使用模板元素template，==不能设置key属性==。



```html
      <!-- 将循环指令给外层使用 -->
      <!-- <div v-for="(item,index) of colors" :key="item">
        <p>{{item}}-{{index}}</p>
        <hr />
      </div> -->

      <!-- 
        当我们想渲染多个兄弟元素的时候，那么我们就要将指令放在父元素上，这样会导致引入其它的元素。
          为了避免引入其它的元素，vue建议我们使用模板元素
            第一种：通过script模板标签定义
            第二种：html5提供了tempalte模板元素。
          注意：使用模板元素template，不能设置key属性。
      -->
      <template v-for="(item,index) of colors">
        <p>{{item}}-{{index}}</p>
        <hr />
      </template>
```


