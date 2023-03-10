# Vue	第五天

## 一、Vue

### 1.0 数据双向绑定的实现

```html
    <div id="app">
      <input type="text" v-model="msg" />
      <h2>{{msg}}</h2>
      <hr />

      <!-- 数据双向绑定的原理 -->
      <!-- 元素： -->

      <label for="">两个方向</label>
      <!-- 一个方向是由模型进入视图：通过为元素的value属性绑定数据实现 -->
      <input type="text" v-bind:value="msg" />
      <!-- 一个方向是由视图进入模型：通过为元素定义input事件实现 -->
      <input type="text" @input="e => msg = e.target.value" />

      <hr />
      <!-- v-model 的实现 -->
      <label for="">v-model 的实现</label>
      <input
        type="text"
        v-bind:value="msg"
        @input="e => msg = e.target.value"
      />
      <hr />

      <!-- 多层级数据 使用 $set实现 避免数据丢失-->
      <input
        type="text"
        v-bind:value="size.width"
        @input="e => size.width = this.$set(this.size,'width',e.target.value)"
      />
      <h2>{{size.width}}</h2>
      <hr />

      <!-- 组件 -->
      <demo v-bind:msg="msg" @input="msg => this.$set(this,'msg',msg)"></demo>
    </div>
```

```js
import Vue from 'vue'

Vue.config.productionTip = false

let Demo = Vue.extend({
  props: ['value'],
  template: `
    <div>
      <input type="text" @input="e => $emit('input',e.target.value)">
      <h2>hello demo {{value}}</h2>
    </div>
  `
})

new Vue({
  components: { Demo },
  el: "#app",
  data: {
    msg: 'hello msg',
    size: {}
  }
})
```



### 1.1 ref

想在脚本中，获取模板中的**元素**或者**组件**，可以使用ref属性。

分成两步

​		 第一步 为元素或者组件添加`ref`属性。属性值代表它的名称

​		 第二步 在脚本中，通过`this.$refs`属性，即可获取对应的元素或者组件。



​				 为元素设置ref，获取的是源生的DOM对象，要使用源生的API操作

​				 对组件设置ref，获取的是组件实例化对象。



```html
<body>
    <!-- 定义视图 -->
    <div id="app">
        <h1 ref="h1">{{msg}}</h1>
        
        <!-- 使用组件 -->
        <Demo ref="demo"></Demo>
    </div>
    <!-- 引入发布之后的文件 -->
    <script src="./dist/02.js"></script>
</body>
```

```js
// 引入vue
import Vue from 'vue';

// 关闭生产提示
Vue.config.productionTip = false;

// 定义组件
let Demo = Vue.extend({
    // 接收数据
    template: `
        <div>
            <h1>hello demo</h1>
        </div>
    `
})

// 实例化
new Vue({
    // 注册
    components: { Demo },
    el: '#app',
    data: {
    },

    // 组件创建完成
    created() {
        // 获取对应的元素 或者组件
        console.log(this.$refs.h1);
        console.log(this.$refs.demo);
    },

    // 构建完成之后
    mounted() {
        // 获取到的是源生的元素
        // console.log(this.$refs.h1);
        // 源生的api操作
        this.$refs.h1.style.color = 'red';

        console.log(this.$refs.demo);
    }
})
```



### 1.2 组件与语法糖

语法糖

​		 : => v-bind:

​		 @ => v-on:

​		 \# => v-slot:

组件

​		 component， slot， transition， transtion-group， keep-alive



### 1.3 插槽

让我们在组件的模板中，使用组件元素内部的其它元素（内容）。



使用插槽分成两步：

​		 第一步 为组件元素内部的子元素设置`slot`属性，定义插槽名称

​		 第二步 在组件模板中，通过slot组件，插入内容。

​				 通过`name`属性定义插槽名称，如果没有定义name属性，将插入默认的的元素。



如果不想引入slot属性所在的元素，我们可以使用`template`模板元素。

​		 此时slot属性要改成`v-slot`指令，

​				 使用方式： `v-slot:name`

​				 v-slot指令语法糖：# 可以写成 `#name`

 ==注意==：我们不能在普通的元素上使用v-slot指令。



```html
    <div id="app">
      <Demo>
        <!-- 1.为组件元素内部的子元素设置slot属性 定义插槽名称 -->
        <!-- <div class="header" slot="header">
          <h1>header</h1>
        </div> -->

        <!-- 如果不想引入slot属性所在的元素 可以使用template模板元素
              此时slot属性要改成v-slot指令  v-slot:name 语法糖  #name
              注意:不能在普通元素设置v-slot指令
        -->
        <template #header>
          <h1>
            header
          </h1>
        </template>
      <Demo>
        <div class="body">
          <h1>body</h1>
        </div>
      <Demo>
        <!-- 注意:不能在普通元素设置v-slot指令 -->
        <div class="footer" slot="footer">
        <!-- <div class="footer"  v-slot="footer"> -->
          <h1>footer</h1>
        </div>
      </Demo>
    </div>

    <!-- 定义模板 -->
    <template id="tpl">
      <div>
        <!-- 2.在组件模板中通过slot组件插入内容 通过name指定插槽 -->
        <slot name="header"></slot>

        <!-- 如果没有定义name属性 将插入默认的元素 -->
        <slot></slot>

        <slot name="footer"></slot>
      </div>
    </template>
    
    <script src="./dist/03.js"></script>
  </body>
```

```js
// 引入vue
import Vue from 'vue';

// 关闭生产提示
Vue.config.productionTip = false;

// 定义组件
let Demo = Vue.extend({
    // 接收数据
    template: '#tpl'
})

// 实例化
new Vue({
    // 注册
    components: { Demo },
    el: '#app',
})
```



### 1.4 插槽作用域

插槽作用域就是说，我们在组件元素内部的子元素中，使用的数据存储在哪里。

在父组件模板中，组件元素内部的子元素仍然使用父组件中的数据。



插槽作用域技术就是让我们在组件元素内部的子元素中，使用子组件中的数据。

共分两步

​		 第一步 在子组件模板中，为`slot`组件传递属性数据。

​				 命名规范：字母小写，横线分割单词。

​				 想动态传递数据，要使用`v-bind`指令。

​		 第二步 在子组件元素内，通过`v-slot`指令定义数据的命名空间

​				 命名空间中的数据名称：要使用驼峰式命名。



```html
    <div id="app">
      <demo>
        <!-- 2.在子组件元素内 通过v-slot指令定义数据命名空间 -->
        <template #header="childScope">
          <h1>
            {{childScope.childMsg}} -- {{childScope.title}} --
            {{childScope.num}}
          </h1>
        </template>
      </demo>
    </div>

    <template id="tpl">
      <div>
        <!-- 通过slot组件使用插槽 -->
        <!-- 1.在子组件模板中 为slot组件传递属性数据 -->
        <slot name="header" title="nihao" num="123" :child-msg="msg"></slot>
      </div>
    </template>
```

```js
// 引入vue
import Vue from 'vue';

// 关闭生产提示
Vue.config.productionTip = false;

// 定义组件
let Demo = Vue.extend({
    // 接收数据
    template: '#tpl',
    data() {
        return {
            msg: 'child msg'
        }
    }
})

// 实例化
new Vue({
    // 注册
    components: { Demo },
    el: '#app',
    data: {
        msg: 'parent msg'
    }
})
```



## 二、VueX

### 2.1 Flux

vuex是一个解决组件之间通信，实现组件之间共享数据的框架。

参考了flux思想实现的框架。

flux实现了单一数据源，数据单向流动等特征。

flux包含了四个模块

​	 action  消息对象

​	 dispatcher   捕获消息的

​	 store  存储数据的

​	 views  组件视图

通信流程

​	 一个组件发布了一个action对象。action被dispatcher捕获，根据消息类型处理数据。store会存储新的数据，并将新的数据传递给另一个组件。

![](./assets/flux.png)



### 2.2 VueX

vuex就是基于flux思想实现的框架，也实现了单一数据源，数据单向流动等特征。

​		 单一数据源：一个应用程序中，只能有一个store对象（用来存储数据的）

​		 数据单向流动：数据始终朝着一个方向传递（形成环路）。

优势：应用中可能有很多环，但是彼此之间没有联系，所以在一个环中添加一个成员或者删除一个成员，只影响当前的环，不影响其它的环，因此我们可以将系统中的所有环抽象成一个环。

vuex由三部分组成：

​		 action  消息对象

​		 state  用来存储数据的

​		 views  组件视图

<img src="./assets/vuex.png" style="zoom:50%;" />



通信流程

​		一个组件发布了消息。

​		消息被action捕获，并根据消息类型，处理数据

​		数据改变了，存储在state中，

​		state数据更新了，将新的数据传递给另一个组件。



### 2.3 action 分类

在vuex中，action共分两类：

​	 同步action：`mutations`

​			 处理同步的消息对象（只能执行同步操作）

​			 可以被测试

​	 异步action: `actions`

​			 处理异步的消息对象（可以执行异步操作）

​			 无法测试，所以为了测试，要再发布一个同步消息。

注意：如果没有异步的操作，我们是可以直接发布同步action的（mutations）。



### 2.4 store的组成

在vuex中，我们要创建store对象（代表vuex部分）

在一个应用程序中，只能有且只有一个store对象：store由以下几部分组成：

​	 `state` 			 用来存储静态数据的。store中的数据放在state中存储。

​	 `getters` 	      用来存储计算属性数据的。getters与state的区别与组件中data与computed区别是一样的。

​	 `mutations` 	  用来订阅同步消息的。用commit方法提交同步消息。

​	 `actions` 		  用来订阅异步消息的。用dispatch方法提交异步消息。

​	 `mudules` 		用来切割store的

规范：只能在mutations中修改state，不能在actions中修改state。



<img src="./assets/通信流程.png" style="zoom: 150%">



### 2.5 使用vuex

安装vuex：我们通过npm安装vuex模块，npm install vuex

使用vuex大致分成五步：

​	 第一步 

​			引入vuex:  `import Vuex from 'vuex'`

​			安装vuex：`Vue.use(Vuex)`。vue家族的插件，都可以使用Vue.use方法来安装。

​		 	注意：在模块化开发中要安装（除了seajs），不使用模块化开发规范，不需要安装

​	 第二步 创建store对象，并传递各个组成部分。创建：`new Store()`

​			 组成部分：

​					静态数据：`state`, 

​					计算属性数据：`getters`, 

​					同步消息：`mutations`, 

​					异步消息：`actions`, 

​					切割store：`mudules` ......

​	 第三步 在vue实例化对象中，通过`store`属性注册store实例化对象。

​			 注册目的：让所有的组件都可以获取$store对象。

​	 第四步 在一个组件中发布消息

​	 第五步 在一个组件中使用数据



```html
    <div id="app">
      <button @click="reduceNum"></button>
      <hr />

      <add-Num></add-Num>
      <show-Num></show-Num>
    </div>
```

```js
import Vue from 'vue'
// 1.引入 Vuex
import Vuex, { Store } from 'vuex'
// 安装
Vue.use(Vuex)

// 2.创建store对象
let store = new Store({
  // 静态数据
  state: {
    num: 0
  },
  // 同步消息队列
  muations: {

  },
  // 异步消息队列
  actions: {

  }
})


// 定义组件
// 增加数字
let AddNum = Vue.extend({
  template: `
  <div>
    <button>增加数字</button>
  </div>
  `
})

// 展示结果
let showNum = Vue.extend({
  template: `
  <div>
    <h2>展示结果:{{}}</h2>
  </div>
  `
})

new Vue({
  // 3.注册store
  store,

  components: { AddNum, showNum },
  el: "#app",
  methods: {
    reduceNum() {

    }
  }
})
```



### 2.6 同步消息

同步消息定义在`mutations`中，是一个对象

​	 `key` 	表示消息名称

​	 `value`  表示消息回调函数

​			 第一个参数表示`state`数据对象

​			 第二个参数表示`commit`方法提交的数据

​				 在方法中，我们用新的数据，更新state数据对象。

`commit`方法：用来提交同步消息的方法

​	 第一个参数表示消息类型

​	 第二个参数表示提交的数据

注意：数据只能在第二个参数中传递，想传递多个数据，可以放在数组或者对象中。



```js
import Vue from 'vue'
// 1.引入 Vuex
import Vuex, { Store } from 'vuex'
// 安装
Vue.use(Vuex)

// 2.创建store对象
let store = new Store({

  // 静态数据
  state: {
    num: 0
  },
  // 同步消息队列
  mutations: {
    /* 
      key 表示消息名称
      value 表示回调
        第一个参数state数据对象
        第二个参数commit提交的数据
    */
    // 订阅消息
    // 减少数字
    reduceNum(state, num) {
      state.num -= num
    },
    // 增加数字
    addNum(state, num) {
      state.num += num
    }
  },
  // 异步消息队列
  actions: {

  }
})


// 定义组件
// 增加数字
let AddNum = Vue.extend({
  template: `
  <div>
    <!-- <button @click="addNum">增加数字</button> -->
    <!-- 方法体中只有一句话可以简写 -->
    <button @click="$store.commit('addNum', 10)">增加数字</button>
  </div>
  `,
  methods: {
    addNum() {
      this.$store.commit('addNum', 10)
    }
  }
})

// 展示结果
let showNum = Vue.extend({
  template: `
  <div>
    <h2>展示结果:{{$store.state.num}}</h2>
  </div>
  `
})

new Vue({
  // 3.注册store
  store,

  components: { AddNum, showNum },
  el: "#app",
  methods: {
    // 减少数字
    reduceNum() {
      // 默认只能传递一个数据
      /* 
        commit 方法
          第一个参数消息类型
          第二个参数提交的数据
      */
      this.$store.commit('reduceNum', 5)
      // 想要传递多个数据可以放在数组或对象中
      // this.$store.commit('reduceNum', [5, 7, 8])
    }
  }
})
```



### 2.7 异步消息

我们通过`actions`定义异步消息，是一个对象

​	 key  表示消息名称

​	 value  表示消息回调函数

​		 第一个参数表示store对象

​		 第二个参数表示`dispatch`方法提交的数据

​				 在方法中，我们实现异步操作，但是不要修改state

`dispatch`：用来提交异步消息的方法。

​	 第一个参数表示消息名称

​	 第二个参数表示提交的数据

注意：数据只能在第二个参数中传递，想传递多个数据，可以放在数组或者对象中。



```js
import Vue from 'vue'

// 1.引入Vuex
import Vuex, { Store } from 'vuex'
// 安装
Vue.use(Vuex)

// 2.创建Store
let store = new Store({

  // 严格模式
  strice: true,

  // 静态数据
  state: {
    num: 0
  },
  // 同步消息队列
  mutations: {
    // 订阅减少数字消息
    reduceNum(state, num) {
      state.num -= num
    },
    addNum(state, num) {
      state.num += num
    },
    clearNum(state, num) {
      state.num = num
    }
  },
  // 异步消息队列
  actions: {
    clearNum(store, num) {
      // 只允许在mutations中修改数据  通过commit笑死的提交mutations中的数据做修改
      setTimeout(() => {
        // 同步消息
        store.commit('clearNum', num)
      }, 2000);
    }
  }
})

// 定义组件
let addNum = Vue.extend({
  template: `
    <div>
      <button @click="$store.commit('addNum',4)">增加数字4</button>
      <button @click="$store.dispatch('clearNum',0)">3s之后清除数字</button>
    </div>
  `,
})
let showNum = Vue.extend({
  template: `<h1>{{$store.state.num}}</h1>`
})

new Vue({
  // 3.注册store
  store,
  components: { addNum, showNum },
  el: "#app",
  methods: {
    reduceNum() {
      /* 
        commit()
          第一个参数  消息类型
          第二嘎参数  提交的数据
      */
      this.$store.commit('reduceNum', 3)
    }
  }
})
```



### 2.8 严格模式

vuex的规范：

​	只能在`mutations`修改state数据，不能在其它的位置修改state数据。

​	只能在commit显示的提交mutations中的数据 做修改

vuex为了严格限制这一规范，提供了严格模式：

​	 在store实例化对象中，设置`strict: true`。

​	 这样当我们在其它位置修改state数据的时候，就会抛出警告错误。



```js
let store = new Store({
    // 开启严格模式
    strict: true,
)}
```



### 2.9 getters

`getters`：

store中存储的计算属性数据，可以对state中的数据动态处理；

​	是一个对象

​		 key 表示数据名称

​		 value  计算的方法

​			 第一个参数表示当前的state数据

​			 第二个参数表示当前的getters数据

​			 必须有返回值，表示计算的结果。



```html
    <div id="app">
        <button @click="reduceNum">减少数字</button>
        <hr>

        <!-- 定义组件 -->
        <add-num></add-num>
        <show-num></show-num>
    </div>
```



```js
  // 计算属性数据
  getters: {
    /* 
      key 数据名称
      value 计算方法
        第一个参数  当前state数据
        第二个参数  当前getter数据
      必须有返回值 表示计算结果
    */
    double(state, getter) {
      return state.num * 2
    }
  },
```



### 2.11 模块切割

在一个非常复杂的应用中，共享的数据会有很多，所有的数据都直接写在state中，就可能会产生命名空间冲突的问题，为了解决冲突问题，vue提供了`mudules`属性，允许我们对模块切割。

​	 属性值是一个对象：

​			key表示命名空间，

​			value表示该命名空间对应的子store对象。

​	 在store中，数据作为state来存储的，因此切割store的本质就是切割state，因此切割后，只有state需要携带命名空间，其它的的数据正常访问。

切割的子store对象理论上也是store对象，因此可以设置store中的所有组成部分：mutations， actions， state， getters， modules等。切割后，各个部分如果

==重名的影响==：

​	 1 state：由于具有命名空间，因此不受影响。

​	 2 getters: 各个store之间的getters数据不允许同名。所有的getters都添加在全局。

​	 3 actions和mutations：可以正常访问：全局的消息修改全局的数据，局部的消息修改局部的数据。



```js
let store = new Store({
    // 开启严格模式
    strict: true,
    // 传递各个组成部分
    state: {
        num: 0
    },
    // 定义getters
    getters: {
        doubleNum(state, getter) {
            // console.log(333, arguments);
            // 必须有返回值
            return state.num * 2;
        }
    },
    // 同步消息队列
    mutations: {
        // 订阅消息
        reduceNum(state, num) {
            state.num -= num;
        },
        // 订阅增加数字的消息
        addNum(state, num) {
            state.num += num;
        },
        // 订阅清空数字的消息
        clear(state, num) {
            state.num = num;
        }
    },
    // 异步消息的队列
    actions: {
        // 定义情况数字的消息
        clearNum(store, num) {
            // console.log(this, arguments);

            // 异步操作
            setTimeout(() => {
                // 不要在actions消息队列中修改state的值
                // store.state.num = num;

                // 要发布一个同步消息
                store.commit('clear', num);
            }, 3000);
        }
    },

    // 切割store
    modules: {
        // key 表示命名空间
        demo: {
            // 传递各个组成部分
            state: {
                num: 10
            },
            // 定义getters
                // 定义的getters数据不允许同名的
            getters: {
                doubleNum222(state, getter) {
                    // console.log(333, arguments);
                    // 必须有返回值
                    return state.num * 2;
                }
            },

            // actions和mutations：可以正常访问：全局的消息修改全局的数据，局部的消息修改局部的数据。

            // 同步消息队列
            mutations: {
                // 订阅消息
                reduceNum(state, num) {
                    state.num -= num;
                },
                // 订阅增加数字的消息
                addNum(state, num) {
                    state.num += num;
                },
                // 订阅清空数字的消息
                clear(state, num) {
                    state.num = num;
                }
            },
            // 异步消息的队列
            actions: {
                // 定义情况数字的消息
                clearNum(store, num) {
                    // console.log(this, arguments);

                    // 异步操作
                    setTimeout(() => {
                        // 不要在actions消息队列中修改state的值
                        // store.state.num = num;

                        // 要发布一个同步消息
                        store.commit('clear', num);
                    }, 3000);
                }
            }
        }
    }
})
```



### 2.10 数据映射

`mapState`

对state数据做处理，并返回映射的结果

​	 参数是对象

​			 key   结果对象的属性名称

​			 value  结果对象的属性值

​					 可以是字符串：与state中的数据名称对应

​					 可以是函数：对state做处理并返回新结果。

​							 第一个参数是state

​							 第二个参数是getters

解决了访问数据的时候，每次都要写this.$store的重复书写问题。简化了对state数据的访问。



```js
let showNum = Vue.extend({
  template: `
    <div>
      <h2>展示结果：{{$store.state.num}} 子store:{{$store.state.demo.num}}</h2>
      <h2>双倍结果：{{$store.getters.double}}</h2>
      <!-- 简写 -->
      <h3>{{num}} getters数据:{{double}}</h3>
    </div>
      `,
  // 映射 state数据
  computed: {
    // 使用扩展运算符
    ...mapState({
      // key 自定义数据的名称
      // value state中的数据名称
      num: 'num',

      // 默认是不能映射getters数据的
      // 数据值可以是函数的形式 间接映射getters数据
      double(state, getter) {
        return getter.double
      }
    }),

    // 此时不会再影响处理其他数据
    // xxx() {}
  }
})
```



`mapMutations`

使用在方法中

```js
// 定义组件
let addNum = Vue.extend({
  template: `
    <div>
      <button @click="$store.commit('addNum',4)">增加数字4</button>
      <button @click="$store.dispatch('clearNum',0)">3s之后清空数字</button>

      <!-- 简写 -->
      <button @click=addNumber>增加数字10</button>
    </div>
  `,
  methods: {
    // 映射mutations消息
    ...mapMutations({
      addNum: 'addNum'
    }),
    addNumber() {
      // 此时可以直接执行方法
      this.addNum(10)
    }
  }
})
```



### 2.12 插件

为了测试vuex，vuex提供了测试插件：vuex/dist/logger.js.

该插件可以帮助我们测试同步消息（mutations），不能测试异步消息（actions）

是一个方法，执行后才能被使用。



使用插件：

​		 导入测试文件	`import logger from 'vuex/dist/logger.js';`

​	 	在store对象中的`plugins`中定义插件。

​		 使用插件的目的：为了复用插件对store拓展的功能。



`subscribe` ：store对象提供了subscribe方法，可以监听store的变化。

`store.subscribe((obj, state) => console.log(obj.payload, obj.type))`

​	 参数是回调函数，表示当发布同步消息的时候，执行的方法

​			 第一个参数表示消息对象：消息类型，传递的数据，

​			 第二个参数表示state对象。

​			 根据state数据来判断执行的结果是否符合预期。

 	只能监听同步消息，无法监听异步消息。



#### **自定义插件**

自定义插件就是让我们将对store的拓展封装起来，在不同的项目中复用。

插件是一个方法，

 	参数是store对象。

​	 在方法中，我们对store拓展



- 自定义插件

```js
export default (Store) => {
  Store.subscribe((obj, state) => { console.log(obj.payload, obj.type) })
}
```



```js
import Vue from 'vue'

// 引入Vuex
import Vuex, { Store, mapState, mapMutations } from 'vuex'

// 1. 引入测试文件
// import logger from 'vuex/dist/logger.js'

// 引入自定义插件
import tools from './tools.js'

// 安装Vuex
Vue.use(Vuex)

// 创建 Store
let store = new Store({

  // 2.在store对象中的plugins中定义插件
  plugins: [
    // logger()
    tools
  ],

  // 静态数据
  state: {
    num: 0
  },
  // 计算属性数据
  getters: {
    /* 
      key 数据名称
      value 计算方法
        第一个参数  当前state数据
        第二个参数  当前getter数据
      必须有返回值 表示计算结果
    */
    double(state, getter) {
      return state.num * 2
    }
  },
  // 同步消息队列
  mutations: {
    // 订阅减少数字消息
    /* 
      key 消息名称
      value 回调
        第一个参数  数据对象
        第二个参数  commit提交的数据
    */
    reduceNum(state, num) {
      state.num -= num
    },
    addNum(state, num) {
      state.num += num
    },
    clearNum(state, num) {
      state.num = num
    }
  },
  // 异步消息队列
  actions: {
    clearNum(store, num) {
      setTimeout(() => {

        // 只能在mutations中修改数据  通过commit显示提交的mutations中修改数据
        // 同步消息
        store.commit('clearNum', num)

      }, 2000);
    }
  },

  // 模块切割
  /* 
    key 表示命名空间
    value 表示命名空间对应的字store对象
      state 携带命名空间
      getters 不允许同名
  */
  modules: {
    demo: {
      // 静态数据
      state: {
        num: 0
      },
      // 计算属性数据
      getters: {
        /* 
          key 数据名称
          value 计算方法
            第一个参数  当前state数据
            第二个参数  当前getter数据
          必须有返回值 表示计算结果
        */
        double1(state, getter) {
          return state.num * 2
        }
      },
      // 同步消息队列
      mutations: {
        // 订阅减少数字消息
        /* 
          key 消息名称
          value 回调
            第一个参数  数据对象
            第二个参数  commit提交的数据
        */
        reduceNum(state, num) {
          state.num -= num
        },
        addNum(state, num) {
          state.num += num
        },
        clearNum(state, num) {
          state.num = num
        }
      },
      // 异步消息队列
      actions: {
        clearNum(store, num) {
          setTimeout(() => {

            // 只能在mutations中修改数据  通过commit显示提交的mutations中修改数据
            // 同步消息
            store.commit('clearNum', num)

          }, 2000);
        }
      }
    }
  }
})

// 定义组件
let addNum = Vue.extend({
  template: `
    <div>
      <button @click="$store.commit('addNum',4)">增加数字4</button>
      <button @click="$store.dispatch('clearNum',0)">3s之后清空数字</button>

      <!-- 简写 -->
      <button @click=addNumber>增加数字10</button>
    </div>
  `,
  methods: {
    // 映射mutations消息
    ...mapMutations({
      addNum: 'addNum'
    }),
    addNumber() {
      // 此时可以直接执行方法
      this.addNum(10)
    }
  }
})



let showNum = Vue.extend({
  template: `
    <div>
      <h2>展示结果：{{$store.state.num}} 子store:{{$store.state.demo.num}}</h2>
      <h2>双倍结果：{{$store.getters.double}}</h2>
      <!-- 简写 -->
      <h3>{{num}} getters数据:{{double}}</h3>
    </div>
      `,
  // 映射 state数据
  computed: {
    // 使用扩展运算符
    ...mapState({
      // key 自定义数据的名称
      // value state中的数据名称
      num: 'num',

      // 默认是不能映射getters数据的
      // 数据值可以是函数的形式 间接映射getters数据
      double(state, getter) {
        return getter.double
      }
    }),

    // 此时不会再影响处理其他数据
    // xxx() {}
  }
})

// 3.store 对象提供 subscribe() 监听store变化
/* 
  参数是回调 表示当前发布同步消息的时候 执行的方法
    第一个参数  消息对象：消息类型 传递的数据
    第二个参数  state对象
  只能监听同步消息
*/
// store.subscribe((obj, state) => { console.log(obj.payload, obj.type) })

new Vue({
  // 注册
  store,
  components: { addNum, showNum },
  el: '#app',
  // 定义方法
  methods: {
    // 增加数字方法
    reduceNum() {
      /* 
        commit()
          第一个参数  消息类型
          第二个参数  传递的数据
            默认只能传递一个数据  传递多个数据使用数组
      */
      this.$store.commit('reduceNum', 3)
    }
  }
})
```



### 2.13 state代理

当我们在表单中，想对state中的数据实现数据双向绑定，我们可以使用state代理技术。

​	 由于我们不能在store的外部修改state，因此我们要通过计算属性数据代理state；

​	 为计算属性数据定义特性对象

​			 在取值器方法中，获取state数据

​			 在赋值器方法中，修改state数据



```html
    <div id="app">
        <input type="text" v-model="dealTitle">
        <h1>app part -- {{$store.state.title}}</h1>
    </div>
```



```js
// 引入Vue
import Vue from 'vue';
// 引入vuex并安装
import Vuex from 'vuex';
// 安装
Vue.use(Vuex);

// 创建store
let store = new Vuex.Store({
    // 开启严格模式
    strict: true,
    state: {
        title: 'hello title'
    },
    // 同步消息队列
    mutations: {
        // 订阅消息
        update(state, title) {
            state.title = title;
        }
    },
    // 异步消息队列
    actions: {}
})



// 实例化
new Vue({
    // 注册
    store,
    // 绑定视图
    el: '#app',
    // 定义计算属性数据
    computed: {
        dealTitle: {
            // 取值器
            get() {
                return this.$store.state.title;
            },

            // 赋值器
            set(val) {
                // 提交同步消息 在mutations中修改
                this.$store.commit('update', val);
            }
        }
    }
})
```
