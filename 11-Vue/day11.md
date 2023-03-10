# Vue3 第十一天

## 一、Vue3

### 1.0 setup

Vue3.0中一个新的配置项，值为一个函数

==组件中所用到的：数据、方法等等，均要配置在setup中。==



参数: 

​	props 组件中的属性属性

​	context 组件中其它的数据



setup函数的**两种返回值**：

	1. 若返回一个对象，则对象中的属性、方法, 在模板中均可以直接使用

```js
<template>
  <div id="div">
    <h1>app part</h1>
    <hr />
    <h2>{{ num }}</h2>
  </div>
</template>

<script>
export default {
  // 组件中所有的数据方法 都应该定义在setup中
  setup() {
    // 定义数据
    let num = 10;

    // setup函数的两种返回值
    // 若返回一个对象 则对象中的属性、方法、在模板中均可以直接使用
    return { num };
  },
};
</script>
```



	2. 若返回一个渲染函数：则可以自定义渲染内容。（import { h } from 'vue’;） 不常用

```js
<script>
import { h } from "vue";
export default {
  // 组件中所有的数据方法 都应该定义在setup中
  setup() {
    // setup函数的返回值 
    // 返回一个渲染函数 则可以自定义渲染内容 会将现有的模板视图覆盖 不常用
    /* 
      第二个参数 表示属性数据
      第三个参数开始 表示子节点
    */
    return () => h("h1", { class: "box" }, "hello h1", h("p", null, "hello p"));
  },
};
</script>
```



注意: 在项目中可以为script 传递setup属性的形式简写使用 内部定义的数据可以直接在模板中使用

```js
<!-- 传递 setup 属性 可以简写 内部定义的数据 可以直接在模板中使用 -->
<script setup>
    
</script>
```



### 1.1 ref 函数

作用：定义响应式的数据

语法：let demo = ref(value)

 		创建一个包含响应式数据的引用对象（reference对象，简称ref对象)

​		 响应式：视图更新模型亦更新，模型更新视图亦更新

 注：

​		**ref定义的数据：操作数据需要.value**

​		**模板中使用的时候可以直接读取不需要.value。**



```vue
<template>
  <div id="div">
    <input type="text" v-model="msg" />
    <h2>{{ msg }}</h2>
    <hr />
    <input type="text" v-model="person.name" /> <span>{{ person.name }}</span>
    <br />
    <input type="text" v-model="person.age" /> <span>{{ person.age }}</span>
    <br />
    <input type="text" v-model="person.job.salary" />
    <span>{{ person.job.salary }}</span> <br />
    <button @click="changeData">修改数据</button>
  </div>
</template>

<!-- 传递 setup 属性 可以简写 内部定义的数据 可以直接在模板中使用 -->
<script setup>
import { ref } from "vue";

// 通过ref定义响应式数据
let msg = ref("hello msg");

// 通过ref定义引用类型数据
let person = ref({
  name: "张三",
  age: 22,
  job: {
    salary: 555,
  },
});

// 定义方法
function changeData() {
  // 操作数据需要通过 .value
  msg.value = "hello world";

  // 对象中的数据 也要通过.value操作
  person.value.name = "小王";
  person.value.age = 33;
  person.value.job.salary = 99;
}
</script>
```



### 1.2 reactive 函数

作用：定义对象类型的响应式数据 

​	 无需.value即可修改数据

​	 解决数据丢失问题

 	==注：基本类型不要用它，要用ref函数==



语法：let 代理对象= reactive(源对象)

​		 参数：对象或者数组等

​		 返回值：代理对象（proxy实例）



reactive定义的响应式数据是“深层次的”

内部基于 ES6 的 Proxy 实现，通过代理对象，对源对象内部数据进行操作



```vue
<template>
  <div>
    <input type="text" v-model="msg" />
    <h1>hello app -- {{ msg }}</h1>
    <hr />

    <input type="text" v-model="person.name" /> {{ person.name }} <br />
    <input type="text" v-model="person.age" /> {{ person.age }} <br />
    <input type="text" v-model="person.job.salary" /> {{ person.job.salary }}
    <br />
    <button @click="changeData">点我修改信息</button>
  </div>
</template>

<!-- 传递 setup 属性 可以简写 内部定义的数据 可以直接在模板中使用 -->
<script setup>
import { ref, reactive } from "vue";

// 定义数据
let msg = ref("hello msg");

// 定义引用类型
let person = reactive({
  name: "张三",
  age: "22",
  job: {
    salary: 999,
  },
});

// 基本类型不能使用 reactive
// let num = reactive(100);

// 定义方法
function changeData() {
  person.name = "李四";
  person.age = 23;
  person.job.salary = 333;
}

console.log(msg);
console.log(person);

/* 
    对于 ref 和 reactive 
      语法层面 ：
        ref .value 修改数据
        reactive 无效.value即可修改数据
*/
</script>

```



### 1.3 响应式实现

Vue 2.0

 		通过ES5提供的特性实现的

Vue 3.0

​		 ref  通过特性实现响应式

​		 reactive  通过代理实现响应式



```vue
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
      <h1>{{name}} -- {{age}}</h1>
    </div>
    <script>
      // vue2中的响应式：通过设置特性方式实现 会造成数据丢失
      // let data = {
      //   // 备份数据对象
      //   _data: {},
      // };

      // // 定义特性
      // Reflect.defineProperty(data, "name", {
      //   // 取值器
      //   get() {
      //     return this._data.name;
      //   },
      //   // 赋值器
      //   set(val) {
      //     this._data.name = val;
      //     // 执行渲染方法
      //     render(this._data);
      //   },
      // });

      // // 获取内部文本
      // let tpl = document.getElementById("app").innerHTML;
      // // 定义渲染方法
      // function render(data) {
      //   // 格式化模板
      //   let html = tpl.replace(/{{(\w+)}}/g, (match, $1) => {
      //     return data[$1] || "";
      //   });
      //   // 替换模板内容
      //   document.getElementById("app").innerHTML = html;
      // }

      // // 修改数据
      // data.name = "李四";
      // data.age = 26;

      // --------------------------------------------------------------------------------------

      // vue3中的响应式：通过代理实现 防止数据丢失
      // let obj = { a: 1 };

      // // 定义代理对象
      // let data = new Proxy(obj, {
      //   get(obj, key) {
      //     // return obj[key];

      //     return Reflect.get(obj, key);
      //   },
      //   set(obj, key, value) {
      //     // obj[key] = value;

      //     Reflect.set(obj, key, value);

      //     // 渲染方法
      //     render(obj);
      //   },
      // });

      // // 获取内部文本
      // let tpl = document.getElementById("app").innerHTML;
      // // 定义渲染方法
      // function render(data) {
      //   // 格式化模板
      //   let html = tpl.replace(/{{(\w+)}}/g, (match, $1) => {
      //     return data[$1] || "";
      //   });
      //   // 替换模板内容
      //   document.getElementById("app").innerHTML = html;
      // }

      // // 修改数据
      // data.name = "李四";
      // data.age = 26;
      // data.sex = "男";

      // --------------------------------------------------------------------------------------

      // 实现ref函数
      // function ref(value) {
      //   return new RefImpl(value);
      // }
      // // 实现ref类
      // class RefImpl {
      //   constructor(value) {
      //     // 数据存储在_value中
      //     this._value = value;
      //   }
      //   // 取值器
      //   get value() {
      //     return this._value;
      //   }
      //   // 赋值器
      //   set value(value) {
      //     this._value = value;
      //     // 更新视图
      //     render();
      //   }
      // }

      // // 定义数据
      // var data = {
      //   name: ref(),
      //   age: ref(),
      //   msg: ref(),
      // };
      // // 获取容器元素
      // let app = document.getElementById("app");
      // // 获取模板
      // let tpl = app.innerHTML;
      // function render() {
      //   var html = tpl.replace(/{{(\w+)}}/g, (match, $1) => {
      //     return data[$1].value || "";
      //   });
      //   app.innerHTML = html;
      // }
      // data.name.value = "张三";
      // data.age.value = 20;
    </script>
  </body>
</html>
																									
```



### 1.4 响应式 API

#### **toRef**  

​		将proxy对象（reactive）的属性转化成ref对象

​		拆分数据

#### **toRefs** 

​		将proxy对象转成ref对象

​		批量拆分数据



```js
<template>
  <div>
    <label for="">简化数据的访问</label><br />
    <input type="text" v-model="name" /> {{ name }} <br />
    <input type="text" v-model="age" /> {{ age }} <br />
    <input type="text" v-model="job.salary" /> {{ job.salary }} <br />
  </div>
</template>

<!-- 传递 setup 属性 可以简写 内部定义的数据 可以直接在模板中使用 -->
<script setup>
import { ref, reactive, toRef, toRefs } from "vue";

// 定义 reactive 对象
let person = reactive({
  name: "小王",
  age: 20,
  job: {
    salary: 199,
  },
});

// 利用 toRef 实现拆分数据
// let name = toRef(person, "name");
// let age = toRef(person, "age ");
// let salary = toRef(person.job, "salary ");

// 利用 toRefs 批量拆分
let { name, age, job } = toRefs(person);
</script>
```



#### **shallowRef** 

​		能让值类型响应式，对象不可以 

​		注：ref参数可以是对象，此时一级属性是ref对象，后代属性是代理

#### **shallowReactive**  

​		浅层响应式，后代属性无法响应式



```js
<template>
  <div>
    <input type="text" v-model="msg" /> {{ msg }}
    <hr />
    <input type="text" v-model="person.name" /> {{ person.name }} <br />
    <input type="text" v-model="person.age" /> {{ person.age }} <br />
    <input type="text" v-model="person.job.salary" /> {{ person.job.salary }}
    <br />
  </div>
</template>

<!-- 传递 setup 属性 可以简写 内部定义的数据 可以直接在模板中使用 -->
<script setup>
import { ref, reactive, shallowRef, shallowReactive } from "vue";

let person = {
  name: "小王",
  age: 20,
  job: {
    salary: 199,
  },
};

// shallowRef 只能让值类型是响应式 引用类型不可以 (参数只能是值类型)
// person = shallowRef(person);

// 值类型
let msg = shallowRef("hello msg");

// 改为 shallowReactive 对象 浅层次响应式 后代属性无法响应
person = shallowReactive(person);
</script>
```



#### **toRaw**  

​		将proxy对象转成普通对象（取消代理）

#### **markRaw** 

​		处理的对象无法再被代理（代理后设置，无效）



```js
<template>
  <div>
    <input type="text" v-model="person.name" /> {{ person.name }} <br />
    <input type="text" v-model="person.age" /> {{ person.age }} <br />
    <input type="text" v-model="person.job.salary" /> {{ person.job.salary }}
    <br />
  </div>
</template>

<!-- 传递 setup 属性 可以简写 内部定义的数据 可以直接在模板中使用 -->
<script setup>
import { ref, reactive, toRaw, markRaw } from "vue";

let person = {
  name: "小王",
  age: 20,
  job: {
    salary: 199,
  },
};

// markRaw  处理的对象永远无法再被代理(放在最前面使用)
person = markRaw(person);

// 将普通对象转换成响应对象
person = reactive(person);

// toRaw 将proxy对象转换成普通对象(取消代理)
// person = toRaw(person);
</script>
```



**readonly** 

​		响应数据是只读的（不能修改）

**shallowReadonly** 

​		值类型的响应数据是只读的，对象类型的响应数据是可操作的



```vue
<template>
  <div>
    <input type="text" v-model="person.name" /> {{ person.name }} <br />
    <input type="text" v-model="person.age" /> {{ person.age }} <br />
    <input type="text" v-model="person.job.salary" /> {{ person.job.salary }}
    <br />
  </div>
</template>

<!-- 传递 setup 属性 可以简写 内部定义的数据 可以直接在模板中使用 -->
<script setup>
import { ref, reactive, readonly,shallowReadonly } from "vue";

let person = {
  name: "小王",
  age: 20,
  job: {
    salary: 199,
  },
};

// 将普通对象转换成响应对象
person = reactive(person);

// readonly 响应数据只读(不能修改)(深只读)
// person = readonly(person);

// shallowReadyonly 值类型的响应数据是只读的 对象类型的响应数据是可以操作的(浅只读)
person = shallowReadonly(person)
</script>
```



### 1.5 判断响应式数据

**isRef**(data) 

​		判断data是否是ref对象

**isReactive**(data) 

​		判断data是否是reactive创建的proxy对象

**isReadonly**(data) 

​		判断data是否是readonly创建的proxy对象

**isProxy**(data) 

​		判断data是否是由reactive或readonly创建的proxy对象



```vue
<template>
<div>
    <input type="text" v-model="person.name"> <span>person.name: {{person.name}}</span> <hr>
    <input type="text" v-model="person.job.salary"> <span>person.job.salary: {{person.job.salary}}</span> <hr>
</div>
</template>
<script setup>
import { ref, reactive, toRef, toRefs, shallowRef, shallowReactive, toRaw, markRaw, readonly, shallowReadonly, isRef, isReactive, isReadonly, isProxy } from 'vue';
let msg = ref('hello');
let person = reactive({
    name: 'zhangsan',
    age: 20,
    job: {
        salary: 100
    }
})
let colors = readonly(['red', 'green']);
console.log(msg, person, colors);
// 是否是ref对象
// console.log(isRef(msg));
// console.log(isRef(person));
// console.log(isRef(colors));
// 是否是reactive创建的代理对象
// console.log(isReactive(msg));
// console.log(isReactive(person));
// console.log(isReactive(colors));
// 判断是否是readonly创建的代理对象
// console.log(isReadonly(msg));
// console.log(isReadonly(person));
// console.log(isReadonly(colors));
// 是否是代理对象
console.log(isProxy(msg));
console.log(isProxy(person));
console.log(isProxy(colors));
</script>
```



 *总结响应式API方法*

  *ref         					 定义响应式数据 可以接受值类型和引用类型*

  *refs         					定义响应式数据 只能处理引用类型数据*

  *toRef        				单个拆分数据*

  *toRefs        				批量拆分数据*

  *shallowRef      			定义浅层次数据 只能接受值类型*

  *shallowReactive   		定义浅层次的reactive数据对象 后代属性无法响应式*

  *toRaw       					 取消代理*

  *markRaw       				用于无法转为代理*

  *readonly    				   将响应式数据定义为只读*

  *shallowReayonly   		浅只读 后代属性任然可以被修改*

  *isRef     						   检查一个值是否为ref对象*

  *isReactive      					检查一个对象是否由reactive创建的响应式代理*

  *isReadonly     					 检查一个对象是否由readonly 创建的只读代理*

  *isProxy    						  检查一个对象由reactice或者readonly方法创建的代理*



### 1.6 自定义 ref

customRef：创建一个自定义的 ref 并对其依赖项跟踪和更新触发进行显式控制。



​	 参数工厂函数

​	 参数

​			  track  跟踪数据

​			  trigger 触发更新

​	 返回值

​			 get  取值器方法

​			 set  赋值器方法



```vue
<template>
  <div>
    <input type="text" v-model="msg" />
    <h2>{{ msg }}</h2>
  </div>
</template>

<script setup>
import { ref, reactive, customRef } from "vue";

function newRef(value) {
  // 开启句柄
  let timer;
  /*
  参数
    track 跟踪数据
    trigger 触发更新
  返回值
    get 取值器方法
    set 赋值器方法
  */
  return customRef((track, trigger) => {
    return {
      get() {
        // track 跟踪数据
        track();
        return value;
      },
      set(val) {
        // 为了防止开启多个定时器 开启之前关闭定时器
        clearTimeout(timer);

        setTimeout(() => {
          // trigger 触发更新
          trigger();
          value = val;
        }, 1000);
      },
    };
  });
}

// 自定义ref
let msg = newRef("hello");
</script>

<style></style>
```



### 1.7 setup 参数

props 接收到的数据 并且通过props声明的数据

context  组件相关数据

​	 attrs  			接收的是外部传递过来的 但没有在props配置中声明的(捡漏)

​	 slots 			接收组件内部的插槽

​	 emit  			发布消息方法（同$emit）

​	 expose  		向外暴露数据的方法





```vue
<template>
  <div class="Demo">
    <h1>Demo part {{ msg }} {{ num }}</h1>
    <button @click="submit">发布</button>
  </div>
</template>

<!-- v3 -->
<script setup>
// 引入vue
import { defineProps, defineEmits, defineExpose } from "vue";

// 接收数据
defineProps(["msg", "num"]);

// 暴露接口
defineExpose({ abc: 12 });

// 触发消息
let emits = defineEmits();
emits("hello", 200, false, "cc");
</script>

<!-- v2 -->
<!-- <script>
export default {
  props: ["msg", "num"],

  // v2暴露数据
  // expose: ["num"],

  setup(props, context) {
    /* 
      props 接收到的数据 并且通过props声明的数据

      context  组件相关数据
        attrs   接收的是外部传递过来的 但没有在props配置中声明的(捡漏)
        slots   接收组件内部的插槽
        emit    发布消息方法（同$emit）
        expose  向外暴露数据的方法
    
    */
    // console.log(context.attrs);

    //  插槽
    // console.log(context.slots);

    // 暴露接口
    context.expose({ a: 123 });

    // 定义触发消息的方法
    function submit() {
      context.emit("hello", 100, true, "abc");
    }

    return { submit };
  },
};
</script> -->

<style></style>

```

