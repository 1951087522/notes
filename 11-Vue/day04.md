# Vue 	第四天

## 一、Vue

### 案例 

#### 表单验证

```html
  <body>
    <div id="app">
      <p>
        <label for="">用户名:</label>
        <input type="text" v-model="username" />
        <!-- 使用自定义指令 -->
        <span
          v-check-data="username"
          error-text="用户名是4-8位数字字母下划线"
          reg-text="\w{4,8}$"
        ></span>
      </p>
      <p>
        <label for="">密&emsp;码:</label>
        <input type="text" v-model="password" />
        <!-- 使用自定义指令 -->
        <span
          v-check-data="password"
          error-text="密码是字母或者是数字开头和结尾"
          reg-text="^\d.*[a-z]|[a-z].*\d$"
        ></span>
      </p>
    </div>

    <script src="./vue.js"></script>

    <script>
      let vm = new Vue({
        el: "#app",
        data: {
          username: "",
          password: "",
        },
        // 自定义指令
        directives: {
          // 函数的形式
          // checkData(span, obj) {
          //   // 优化 判断当前值与上一次输入的值不相同 再去执行
          //   if (obj.value !== obj.oldValue) {
          //     // 获取提示文本
          //     let errorText = span.getAttribute("error-text");
          //     // 设置内部文本
          //     span.innerHTML = errorText;
          //     // 设置样式
          //     span.style.color = "red";
          //     // 初始隐藏
          //     span.style.display = "none";

          //     // 获取正则
          //     let regText = span.getAttribute("reg-text");
          //     // 创建正则
          //     let reg = new RegExp(regText);

          //     // 验证
          //     if (!reg.test(obj.value) && obj.value !== "") {
          //       span.style.display = "inline-block";
          //     }
          //   }
          // },

          // 对象的形式
          checkData: {
            // 指令绑定到元素执行时候
            bind(span, obj) {
              // 获取提示文本
              let errorText = span.getAttribute("error-text");
              // 设置提示文本
              span.innerHTML = errorText;
              // 设置样式 初始隐藏
              span.style.color = "red";
              span.style.display = "none";
            },
            // 更改时候执行的方法
            update(span, obj) {
              if (obj.value !== obj.oldValue) {
                // 获取正则
                let regText = span.getAttribute("reg-text");
                // 设置正则
                let reg = new RegExp(regText);

                // 验证
                if (!reg.test(obj.value)) {
                  span.style.display = "inline-block";
                } else {
                  span.style.display = "none";
                }
              }
            },
          },
        },
      });
    </script>
  </body>
```



#### 随机变化位置 列表过渡

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      ul {
        display: flex;
        flex-wrap: wrap;
        padding: 0;
        margin: 0;
        list-style: none;
        width: 300px;
        height: 300px;
        border: 1px solid #ccc;
        transition: all 1s;
      }
      li {
        width: 30px;
        height: 30px;
        border: 1px solid #ccc;
        box-sizing: border-box;
        text-align: center;
        font-size: 13px;
        line-height: 30px;
        transition: all 500ms;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <button @click="random">随机变化位置</button>
      <!--使用列表过渡 -->
      <transition-group tag="ul">
        <li v-for="obj in arr" :key="obj.id">{{obj.item}}</li>
      </transition-group>
    </div>

    <script src="./vue.js"></script>

    <script>
      let vm = new Vue({
        el: "#app",
        data: {
          arr: new Array(100)
            .fill(0)
            .map((item, index) => ({ item: index % 10, id: index })),
        },
        methods: {
          random() {
            this.arr.sort(() => {
              return Math.random() > 0.5 ? 1 : -1;
            });
          },
        },
      });
    </script>
  </body>
</html>

```



### 1.1 组件

html：    组件就是一段可以被复用的结构代码

css： 	 组件就是一段可以被复用的样式代码

js:   		组件就是一段可以被复用的功能代码

vue：     组件是一个包含独立的结构，样式和脚本的代码。我们可以复用。



使用组件分成三步

​	第一步 在模板中使用组件

​		 	组件名称字母小写，横线分割单词。注意：首字母不区分大小写。



​	第二步 在脚本中，定义组件类：`Vue.extend( { } )`

​			 参数对象跟new Vue时候传递的参数对象是一样的 。

​			 例如：可以传递data, compted, methods, watch 等属性。

​			 这些属性的功能都是一样的 ，但是有些属性的写法是特殊的。

​					 `data`属性：

​							 是一个函数，返回值是绑定的数据。

> *data为什么是一个函数? js中的对象是引用类型 如果被多个程序引用 改变数据 导致互相受到影响 vue完整的独立的个体 为了彼此之间不受到影响 定义为函数 每一次返回新的数据对象 避免彼此影响* 						

​							 this指向组件实例（由于该方法执行完毕，才能绑定数据，因此当前的this无法访问模型数据）

​					 `template` 定义模板的，有两种用法

​							 第一种：属性值是模板字符串（将组件的模板写在脚本中了）

​							 第二种：属性值是css选择器，此时会将页面中对应元素的内容作为组件的模板。

​							 html定义模板有两种方式：

​									1 通过script模板标签定义 

​									2 通过template标签定义（vue建议）

​							 ==注意==：模板中最外层有且只有一个根元素。



​	第三步 注册组件，有两种方式

​			 1 全局注册

​					`Vue.component(name, Comp)`：全局注册的组件可以在任何组件中使用。

​			 2 局部注册

​					`components: { name: Comp }`：局部注册的组件只能在当前组件中使用

​			 `name`表示组件名称，横线分割单词要使用驼峰式命名（首字母不区分大小写）

​			 `Comp`表示组件类。

​	注意：组件是完成独立的，彼此之间数据，方法等不会共享。



**父子组件**

由于组件类继承了Vue类，因此组件实例化对象可以看成是vue实例化对象，反之vue实例化对象也可以看成是组件实例化对象。

​	 在vue实例中使用组件，我们就可以将vue实例看成是父组件，将自定义的组件看成是子组件。



```html
<body>
    <div id="app">
      <h1>{{msg}}</h1>
      <hr />

      <!-- 1.使用组件 字母小写 首字母不区分大小写-->
      <demo></demo>
      <!-- 复用组件 -->
      <Demo></Demo>

      <!-- 横线分割单词 -->
      <!-- <de-mo></de-mo> -->
    </div>

    <!-- html中定义模板的方式 1： script -->
    <!-- <script type="text/template" id="tpl">
      <div>
        <h1>hello demo</h1>
      </div>
    </script> -->

    <!-- html中定义模板的方式 2: template标签 -->
    <template id="tpl">
      <div>
        <h1>hello demo {{msg}}</h1>
      </div>
    </template>

    <script src="./dist/01.js"></script>
  </body>
```

```js
// 引入Vue
import Vue from 'vue'

// 关闭生产提示
Vue.config.productionTip = false

// 2.定义组件类
let Demo = Vue.extend({
  /*   
    定义模板  注意 模板最外层只能有一个根元素 
    第一种：属性值是模板字符串（将组件的模板写在脚本中了）
    第二种：属性值是css选择器，此时会将页面中对应元素的内容作为组件的模板。
      html定义模板有两种方式：
        1 通过script模板标签定义 
        2 通过template标签定义（vue建议）
  */

  // 方式一：
  // template: `
  //   <div>
  //       <input type='text' v-model='num'>
  //       <h1>hello demo {{num}}</h1>
  //   </div>
  // `,

  // 方式二：
  template: '#tpl',


  // 定义数据 data是一个函数 返回值定义数据
  data() {
    // this指向组件实例（由于该方法执行完毕，才能绑定数据，因此当前的this无法访问模型数据）
    // console.log(this.num);
    return {
      num: 100,
      msg:'hello msg'
    }
  }
})

/* 
3.注册组件
  1 全局注册
    Vue.component(name, Comp)：全局注册的组件可以在任何组件中使用。
  2 局部注册
    components: { name: Comp }：局部注册的组件只能在当前组件中使用
    
    name表示组件名称，横线分割单词要使用驼峰式命名（首字母不区分大小写）
    Comp表示组件类。
*/
Vue.component('demo', Demo)
// 横线分割单词要使用驼峰式命名
Vue.component('deMo', Demo)
// 实例化
new Vue({

  // 局部注册组件
  // components: { 'demo': Demo },
  // 因为不区分大小写 故简写省略
  comments: { Demo },

  el: '#app',
  data: {
    msg: 'hello msg'
  },
})
```



### 1.2 动态组件

我们目前使用的组件都是静态的：一个元素对应一个组件类。

想使用动态组件（一个元素可以对应多个组件类），可以通过`components`组件实现。

​		 通过`is`属性定义渲染哪个组件类，

​				 默认是属性值是字符串

​				 我们可以通过v-bind指令动态绑定。



```html
    <div id="app">
      <!-- 
        想使用动态组件（一个元素可以对应多个组件类），可以通过components组件实现。通过is属性定义渲染哪个组件类，
          默认是属性值是字符串
          我们可以通过v-bind指令动态绑定。
        -->
      <components is="List"></components>

      <button @click="page = 'home' ">home</button>
      <button @click="page = 'List' ">List</button>
      <button @click="page = 'Detail' ">Detail</button>
      <!-- 还可以动态设置 -->
      <components :is="page"></components>
    </div>
```

```js
import Vue from 'vue'

// 关闭生产提示
Vue.config.productionTip = false

// 定义组件
let Home = Vue.extend({
  template: `
    <div>
      <h1>hello Home</h1>
    </div>
  `
})
let List = Vue.extend({
  template: `
    <div>
      <h1>hello List</h1>
    </div>
  `
})
let Detail = Vue.extend({
  template: `
    <div>
      <h1>hello Detail</h1>
    </div>
  `
})

new Vue({
  // 注册组件
  components: { Home, List, Detail },
  el: '#app',
  data: {
    msg: 'hello msg',
    page: 'home'
  }
})
```



### 1.3 生命周期

对于一个组件来说，存在三个过程：**创建**，**存在**和**销毁**。因此vue为组件定义生命周期来说明这三个过程。



**创建期**

 当我们创建组件的时候（如在页面中使用），组件将执行创建期的方法，

​		 `beforeCreate`  组件创建前，此时数据，事件等还没有初始化

​		 `created`  组件创建后，此时组件已经绑定了数据，事件等

​		 `beforeMount`  组件构建前，此时确定了组件的模板，以及渲染的容器元素等。

​		 `mounted`  组件构建后，此时组件上树了

 一个组件在一生中只能创建一次，因此创建期的方法只能执行一次。

 一旦组件的模型数据发生改变，组件将会执行存在期的方法



**存在期**

当组件创建完成，将进入存在期，共分两个阶段：

​		 `beforeUpdate` 组件更新前，此时数据改变了，视图尚未更新

​		 `updated`  组件更新后，此时数据和视图都更新了

 在一个组件中，我们可以不停的改变模型数据，因此存在期的方法会不停的执行。

 存在期的方法执行完毕，只是说一次更新的结束，存在期仍然继续。



**销毁期**

当组件从页面中删除（执行了`$destroy`方法）,组件将进入销毁期，执行销毁期的方法，共分两个阶段：

​		 `beforeDestroy` 组件销毁期，此时组件中的数据等还有监听器。

​		 `destroyed`  组件销毁后，此时组件的数据所具有的监听器以及子组件等被销毁了。

 一旦组件被销毁，我们就再也无法使用组件了，想使用只能创建新的组件了。

以上周期方法this都指向组件实例，没有参数，



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
      <button @click="page = 'home'">home</button>
      <button @click="page = 'list'">list</button>

      <!-- <components :is="page"></components> -->
      <keep-alive>
        <components :is="page"></components>
      </keep-alive>
    </div>

    <script src="./dist/03.js"></script>
  </body>
</html>
```

```js
import Vue from 'vue'

// 关闭生产提示
Vue.config.productionTip = false

// 定义组件类
let Home = Vue.extend({
  template: `
    <div>
      <input type="text" v-model="msg">
      <h1>hello home {{msg}}</h1>
    </div>
  `,
  // data 是一个函数  返回值是绑定的数据
  data() {
    return {
      msg: 'hello msg'
    }
  },
  // 方法
  methods: {
    abc() {
      console.log('abc');
    }
  },

  // 创建期
  // 创建之前 无法访问/设置数据
  beforeCreate() {
    console.log(111, 'beforeCreate', this.msg, this.abc);
  },
  // 创建之后 可以访问数据
  created() {
    console.log(222, 'created', this.msg, this.abc);
  },
  // 构建之前 编译好之后未挂载上树
  beforeMount() {
    console.log(333, 'beforeMount', this.$el);
  },
  // 构建之后 渲染上树
  mounted() {
    console.log(444, 'mounted', this.$el);
  },

  // 存在期
  // 更新之前 数据更新 视图未更新
  beforeUpdate() {
    console.log(555, 'beforeUpdate', this.msg);
  },

  // 更新之后 数据视图同步更新
  updated() {
    console.log(666, 'updated');
  },

  // 销毁期
  // 销毁之前 此阶段做收尾工作 比如关闭定时器 解绑事件等
  beforeDestroy() {
    console.log(777, 'beforeDestroy');
  },

  // 销毁之后
  destroyed() {
    console.log(888, 'destroyed');
  },

  /* 
    当组件从页面中销毁的时候 会执行销毁期的方法
    如果从页面中移除的时候 不想将组件销毁 可以使用 keep-alive组件
    由于组件没有被销毁 将不执行销毁期的方法
    由于组件没有被销毁 被缓存到内存中 保留最后一个状态 因此当组件再次显示的时候 会显示被保留的最后一个状态
    拓展了两个方法
  */
  // keep-alive 组件被激活 可以在页面中看到
  activated() {
    console.log(999, 'activated');
  },

  // 组件被禁用 从页面中移除
  deactivated() {
    console.log(1000, 'deactivated');
  },

})
let List = Vue.extend({
  template: `
    <div>
      <h1>hello List</h1>
    </div>
  `
})

new Vue({

  // 注册局部组件
  components: { Home, List },

  el: '#app',
  data: {
    page: 'Home'
  }
})
```



### 1.4 keep-alive

当组件从页面中删除，组件将执行销毁期的方法，如果从页面中移除的时候，不想将组件销毁，可以使用`keep-alive`组件。

​		 该组件会将从页面中移除的组件暂存到内存中，没有被销毁，再次显示的时候，直接从内存中读取。

​		 由于组件没有被销毁，因此不会执行销毁期的方法。为了表示显示和隐藏的过程，keep-alive组件为内部的组件拓展了两个方法：

 				`activated` 组件被激活，可以在页面中看到

​				 `deactivated` 组件被禁用，从页面中移除了

 注意：由于组件被缓存到内存中，因此没有被销毁，保留最后一个状态，因此当组件再次显示的时候，会显示被保留的最后一个状态。



```html
      <!-- 
          当组件从页面中销毁的时候 会执行销毁期的方法
          如果从页面中移除的时候 不想将组件销毁 可以使用 keep-alive组件
          由于组件没有被销毁 将不执行销毁期的方法
          拓展了两个方法
      -->
      <keep-alive>
        <components :is="page"></components>
      </keep-alive>
```



### 1.5 组件通信

组件是一个完整独立的个体，彼此之间的数据，方法等不会共享，想在组件之间共享数据，我们可以使用组件通信的技术。

通常有三个方向：

​		 父组件向子组件通信

​		 子组件向父组件通信

​		 兄弟组件间通信。



### 1.6 父组件向子组件通信

父组件向子组件通信就是将父组件的数据，方法传递给子组件。



需要两步

​		 第一步 在父组件模板中，为子组件元素传递数据。

​				 属性值默认是字符串，想传递变量或者方法，要使用`v-bind`指令动态传递。

​				 命名规范：字母小写，横线分割单词



​		 第二步 在子组件中，接收数据或者方法。

​				 在props属性中接收，有两种接收方式

​				 第一种 属性值是数组，每一个成员代表一个接收的属性名称。

​				 第二种 属性值是对象

​						  key  表示接收的属性名称

​						 value 有三种方式

​								 可以是类型的构造函数：Number, String等

 								可以是数组，每一个成员代表一种类型（定义多种类型）

 								可以是对象：

​								 		`type`：表示类型的构造函数，

​								 		`required`：是否是必须的，

​								 		`default`：默认值，属性值可以是数据，属性值还可以是方法，返回值就是默认的数据。

​								 		`validator`：校验方法，在方法中，校验数据是否合法。

​		 注意：接收属性的时候，属性命名规范：驼峰式命名。

props中接收的数据与模型中的数据一样，都添加给实例化对象自身，并设置了特性。

因此我们既可以在JS中使用，也可以在模板中使用。



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
      <h1>{{msg}}</h1>
      <hr />

      <!-- 
        第一步 在父组件模板中，为子组件元素传递数据
        直接传递数据 属性值默认是字符串
        传递变量或者方法，要使用v-bind指令动态传递。
        命名规范：字母小写，横线分割单词
      -->
      <!-- <demo
        title="nihao"
        num="100"
        :msg="msg"
        :send-method="parentMethod"
      ></demo> -->
      <demo title="nihaoya"></demo>
    </div>
    <script src="./dist/04.js"></script>
  </body>
</html>
```



```js
import Vue from 'vue'

Vue.config.productionTip = false

let Demo = Vue.extend({

  /* 
    第二步 在子组件中，接收数据或者方法。
    在props属性中接收，有两种接收方式
    第一种 属性值是数组，每一个成员代表一个接收的属性名称。
    第二种 属性值是对象
        key  表示接收的属性名称
        value 有三种方式
            可以是类型的构造函数：Number, String等
              可以是数组，每一个成员代表一种类型（定义多种类型）
              可以是对象：
                  type：表示类型的构造函数，
                  required：是否是必须的，
                  default：默认值，属性值可以是数据，属性值还可以是方法，返回值就是默认的数据。
                  validator：校验方法，在方法中，校验数据是否合法。
    */

  // 1.属性值是数组  横线分割的驼峰命名接收
  // props: ['title', 'num', 'msg', 'sendMethod'],

  // 2.属性值是对象
  props: {
    // key  表示接收的属性名称  value 有三种方式
    // 1. 可以是类型的构造函数：Number, String等
    // title: String

    // 2.可以是数组 每一个成员代表一种类型
    // title: [Number, String, Boolean]

    // 3.可以是对象
    title: {
      type: String,

      // required: true,

      // default：默认值，属性值可以是数据，属性值还可以是方法，返回值就是默认的数据。
      // default: 'hello',
      // default() {
      //   return '你好'
      // }

      validator(value) {
        return value.length > 5
      }
    }
  },



  template: `
    <div>
      <!--  <h1>hello demo {{title}} {{num}} {{msg}}</h1> -->
      <h1>接收到的数据： {{title}}</h1>
    </div>
  `,
  // 组件创建完毕
  created() {
    // this.sendMethod()
  },
})

new Vue({

  // 注册局部组件
  components: { Demo },

  el: '#app',
  data: {
    msg: 'hello msg'
  },
  methods: {
    parentMethod() {
      console.log('parentMethod');
    }
  }
})
```



### 1.7 $parent 与 依赖注入

#### $parent

子组件中可以通过`$parent`属性访问父组件，因此就可以间接的获取父组件中的数据。

 		但是工作中，不建议这么使用，因为这种方式与父组件==耦合并且无法校验数据==，

​		 所以我们要使用父组件向子组件传递属性的方式实现通信。



```js
  // 组件创建完毕
  created() {
    // this.sendMethod()

    // 通过 $parent 访问父组件的数据  工作中不建议这么做因为这种方式与父组件耦合并且无法校验数据，
    console.log(this.$parent.msg);
  },
```



#### 依赖注入

  父组件中通过  `provide`  传递数据	返回值就是传递的数据

```js
new Vue({

  // 注册局部组件
  components: { Demo },

  el: '#app',
  data: {
    msg: 'hello msg'
  },
    
  // 依赖注入
  provide() {
    // 返回值就是传递的数据
    return {
      color: 'red',
      num1: 100
    }
  }

})
```

子组件中 通过 `inject` 接收

```js
let Demo = Vue.extend({

    template: `
        <div>
  	      <h1>接收到的数据：{{num1}} {{color}}</h1>
        </div>
	`,
    
  // 接收父组件传递的数据
  inject: ['num1', 'color']

})
```



### 1.8 自定义事件

vue也实现了观察者模式，提供了订阅消息，发布消息，注销消息等方法。

​	 `$on(type, fn)`  订阅消息方法

​			 type：消息名称，

​			 fn：消息回调函数，参数是由$emit方法传递的。

​	 `$emit(type, ...args)`  发布消息方法

​			 type：消息名称，

​			 ...args：从第二个参数开始，表示传递的数据

​	 `$off(type, fn)` 注销消息方法

​			 type：消息名称

​			 fn：消息回调函数

 组件是一个完整独立的个体，因此彼此之间数据不会共享，所以发布消息与订阅消息必须是同一个组件。



```html
<div id="app">
    <Child></Child>
</div>
```



```js
import Vue from 'vue'

// 定义事件总线
const EventBus = new Vue()

Vue.config.productionTip = false

let Child = Vue.extend({
  template: `
    <div>
      <h1>hello Child</h1>
    </div>
  `,
  // 组件创建完毕
  created() {
    // 通过事件总线发布消息
    EventBus.$emit('message', 100, 200)
  }
})


new Vue({

  // 注册局部组件
  components: { Child },

  el: '#app',
  // 创建完毕
  created() {
    /* 
      $on(type, fn)  订阅消息方法
          type：消息名称，
          fn：消息回调函数，参数是由$emit方法传递的。
      $emit(type, ...args)  发布消息方法
        type：消息名称，
        ...args：从第二个参数开始，表示传递的数据
      $off(type, fn) 注销消息方法
          type：消息名称
          fn：消息回调函数
    */

    // 通过事件总线订阅消息
    EventBus.$on('message', (...args) => {
      console.log(args);
    })
  }
})
```



### 1.9 子组件向父组件通信

子组件向父组件通信的实现

​		 在父组件中，订阅消息，

​		 在子组件中，通过this.$parent访问父组件，发布消息并传递数据

​		 在父组件中，接收并存储数据

在通信过程中，我们访问了$parent，因此产生了耦合，所以在工作中，这种方式不常用。



子组件向父组件通信有两种常用方式：

​		 第一种：**模拟DOM事件**

​		 第二种：**传递属性方法**



### 1.10 模拟DOM事件

绑定DOM事件：v-on:click=”fn”

模拟DOM事件：`v-on:demo=”fn`” demo事件名称就是自定义事件名称



子组件向父组件通信

​		 1在父组件模板中，为子组件元素绑定自定义事件。

​				 消息名称：字母小写横线分割单词。==注意==：绑定事件的时候，不要添加参数集合

​						 如果没有添加参数集合，可以接收所有的数据

​						 如果添加了参数集合，不能接收数据，即使传递了$event也只能接收一条数据

​		 2 在子组件中，通过`$emit`发布消息，并传递子组件中的数据。

​				 消息名称：字母小写横线分割单词。==注意==：这是唯一一个不需要做驼峰式命名转换的地方。

​		 3 在父组件事件回调函数中，接收数据并存储数据。



```html
<div id="app">
    <h1>app:{{msg}}</h1>
    <hr>

    <!-- 1.为子组件准备好自定义事件 -->
    <Child @demo-method="parentMethod"></Child>
</div>
```

```js
import Vue from 'vue'

Vue.config.productionTip = false

let Child = Vue.extend({
  template: `
    <div>
      <input type='text' v-model='msg'>
      <h1>hello Child {{msg}}</h1>
    </div>
  `,
  data() {
    return {
      msg: 'child msg'
    }
  },
  // 组件创建完毕
  created() {
    // 2.在子组件中，通过$emit发布消息，并传递子组件中的数据。
    // 消息名称：字母小写横线分割单词。注意：这是唯一一个不需要做驼峰式命名转换的地方。
    this.$emit('demo-method', this.msg)
  },
  watch: {
    msg(newValue) {
      this.$emit('demo-method', newValue)
    }
  }
})


new Vue({

  // 注册局部组件
  components: { Child },
  data: {
    msg: ''
  },
  el: '#app',
  // 创建完毕
  created() {

  },
  methods: {
    // 3. 在父组件事件回调函数中，接收数据并存储数据。
    parentMethod(msg) {
      // 接收并存储
      this.msg = msg
    }
  }
})
```



### 1.11 传递属性方法

我们可以向子组件传递父组件的方法，让子组件执行父组件的方法并传递数据的形式，来实现通信。

​		 第一步 在父组件模板中，为子组件传递父组件中的方法。

​		 第二步 在子组件中，接收方法，执行方法并传递数据，

​		 第三步 在父组件的方法中，接收数据，并存储数据。



```html
<div id="app">
    <h1>app:{{msg}}</h1>
    <hr />
    <!-- 第一步 在父组件模板中，为子组件传递父组件中的方法。 -->
    <!-- 1.动态传递属性方法 -->
    <Child :send-method="parentMethod"></Child>
</div>
```



```js
import Vue from 'vue'

Vue.config.productionTip = false

let Child = Vue.extend({

  // 2.props 接收
  // 第二步 在子组件中，接收属性方法，执行方法并传递数据
  props: ['sendMethod'],

  template: `
    <div>
      <input type='text' v-model='msg'>
      <h1>hello Child {{msg}}</h1>
    </div>
  `,
  data() {
    return {
      msg: 'child msg'
    }
  },
  // 组件创建完毕
  created() {
    // 执行方法并传递数据
    this.sendMethod(this.msg)
  },
  watch: {
    msg() {
      this.sendMethod(this.msg)
    }
  }
})


new Vue({

  // 注册局部组件
  components: { Child },
  data: {
    msg: ''
  },
  el: '#app',
  // 创建完毕
  created() {

  },
  methods: {
    // 3. 在父组件事件回调函数中，接收数据并存储数据。
    parentMethod(msg) {
      this.msg = msg
    }
  }
})  
```



### 1.12 兄弟组件间通信

两个兄弟组件之间没有必然的联系，但是它们都与父组件有联系，因此要实现兄弟组件间通信就是综合使用以上两种通信方式。

 		父组件向子组件通信

 		子组件向父组件通信

流程：

​		 1 将一个子组件的数据传递给父组件。

​		 2 父组件存储数据，再将新的数据传递给另一个子组件。

对于不相干的两个组件之间的通信，后面会学习VueX来解决。



```html
    <div id="app">
      <h1>app:{{msg}}</h1>
      <hr />

      <!-- 1.模拟DOM事件 -->
      <!-- <First @child-method="parentMethod"></First> -->

      <!-- 2.传递属性方法方式 -->
      <First :child-method="parentMethod"></First>

      <!-- 直接传递属性数据 -->
      <Last :msg="msg"></Last>
    </div>
```

```js
import Vue from 'vue'

Vue.config.productionTip = false

let First = Vue.extend({

  props: {
    childMethod: {
      type: Function
    }
  },

  template: `
    <div>
      <input type='text' v-model='msg'>
    </div>
  `,
  data() {
    return {
      msg: 'First msg'
    }
  },
  created() {
    // 发布消息
    // this.$emit('child-method', this.msg)

    // 传递属性方法方式
    this.childMethod(this.msg)
  },
  watch: {
    msg() {
      // this.$emit('child-method', this.msg)

      // 传递属性方法方式
      this.childMethod(this.msg)
    }
  }

})
let Last = Vue.extend({
  props: ['msg'],
  template: `
    <div>
      <h1>展示 {{msg}}</h1>
    </div>
  `,
})


new Vue({

  // 注册局部组件
  components: { First, Last },
  data: {
    msg: 'hello msg'
  },
  el: '#app',
  // 创建完毕
  created() {

  },
  methods: {
    parentMethod(msg) {
      this.msg = msg
    }
  }
})  
```




