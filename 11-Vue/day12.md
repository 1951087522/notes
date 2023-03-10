# Vue 第十二天

## 一、Vue3

### 1.0 计算属性数据

`computed`方法定义计算属性数据

​	 参数

​			 函数 只定义取值器方法

​	 对象

​			 get  定义取值器方法

​			 set  定义赋值器方法



```js
<template>
  <div>
    <input type="text" v-model="msg" />
    <h1>{{ dealMsg }}</h1>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
// 定义ref数据
let msg = ref("hello msg");

// 定义计算属性数据

// 函数的形式
// let dealMsg = computed(() => {
//   // ref 定义的数据通过.value 访问
//   return msg.value.toUpperCase();
// });

// 对象的形式
let dealMsg = computed({
  // get 定义取值器方法
  get() {
    return msg.value.toUpperCase()
  },
  // set 定义赋值器方法
  set(val) {
    msg.value =val
  },
});
</script>
```



### 1.1 数据监听

`watch` 监听数据变化（同watch）

​		 监听ref数据 						 `watch(num, callback)`

​		 监听多个ref数据 				  `watch([num1, num2], callback)`

​		 监听reactive数据 				 `watch(() => obj.key, callback)` 	

​		 配置项 第三个参数

​				immediate：立即执行 

​				deep：深度监听

​		深度监听 watch(() => obj.data => callback, { deep: true }) 问题：无法获取前一个值 



`watchEffect` 立即执行回调函数，并监听内部响应式数据的变更



```vue
<template>
  <div>
    <!-- .number 修饰符 -->
    <input type="text" v-model.number="num" />
    <h1>{{ num }}</h1>
    <hr />
    <input type="text" v-model.number="msg" />
    <h1>{{ msg }}</h1>
    <hr />
    <h2>姓名:{{ person.name }}</h2>
    <h2>薪资:{{ person.job.salary }}</h2>
    <button @click="change">改变信息</button>
  </div>
</template>

<script setup>
import { ref, reactive, computed, watch, watchEffect } from "vue";

// 定义ref数据
let num = ref(199);
let msg = ref("hello msg");

// 定义proxy对象
let person = reactive({
  name: "小王",
  job: {
    salary: 200,
  },
});

// 定义方法
const change = () => {
  person.name = "小张";
  person.job.salary++;
};

// 监听数据的变化
// 单个数据
// ref
/* 
  第一个参数 当前值
  第二个参数  上一个值
*/
// watch(num, (now, prev) => {
//   console.log("msg改变", now, prev);
// });

// 同时监听多个数据 数组的形式
// watch([num, msg], (now, prev) => {
//   console.log("数据改变", now, prev);
// });

// 监听active
// 第一个参数要作为方法的返回值使用
// watch(
//   () => person.job.salary,
//   (now, prev) => {
//     console.log("salary 改变", now, prev);
//   }
// );

// 监听多层级数据
// 第三个参数表示配置项
// watch(
//   () => person.job,
//   (now, prev) => {
//     console.log("salaey改变", now, prev);
//   },
//   // 开启深度监听 deep 问题：无法获取前一个值
//   // 立即执行 immediate
//   { deep: true, immediate: true }
// );

// watchEffect 参数是回调函数 监视的回调中用到哪个属性 那就监视哪个属性
watchEffect(() => {
  // console.log(msg.value);
  console.log(num.value);
});
</script>
```



### 1.2 生命周期

v2																	v3

beforeCreate 		组件创建之前 				setup方法模拟

created 				 组件创建完成  				setup方法模拟

beforeMount 		组件构建之前 				onBeforeMount

mounted 			  组件构建完成 				 onMounted

beforeUpdate 	  组件更新之前 				  onBeforeUpdate

updated 			   组件更新完成 				  onUpdated

activated 			  组件被激活 					 onActivated

deactivated 		  组件被禁用 					 onDeactivated							(搭配 keep-active)

beforeUnmount    组件销毁之前 				  onBeforeUnmount			 		(搭配 keep-active)

unmounted  		 组件销毁完成 				  onUnmounted



```js
<template>
  <div>
    <button @click="isShow = !isShow">切换组件</button>
    <hr />
    <Demo v-if="isShow"></Demo>
  </div>
</template>

<script setup>
import { ref } from "vue";
import Demo from "./Demo.vue";

let isShow = ref(true);
</script>
```



```vue
<template>
  <div class="demo">
    <input type="text" v-model="msg" />
    <h1>Demo part {{ msg }}</h1>
  </div>
</template>

<!-- vue3 -->
<script setup>
import {
  ref,
  onBeforeUpdate,
  onUpdated,
  onBeforeMount,
  onMounted,
  onActivated,
  onDeactivated,
  onBeforeUnmount,
  onUnmounted,
} from "vue";

// 定义数据
let msg = ref("hello msg");

// vue2中的创建期 beforeCreate created 在vue3中被setup替代了

console.log("000", "setup");

// 构建之前
onBeforeMount(() => {
  console.log(111, "onBeforeMount");
});

// 构建之后
onMounted(() => {
  console.log(222, "onMounted");
});

// 更新之前
onBeforeUpdate(() => {
  console.log(333, "onBeforeUpdate");
});

// 更新之后
onUpdated(() => {
  console.log(444, "onUpdated");
});

// 组件激活
onActivated(() => {
  console.log(555, "onActivated");
});

// 组件被禁用
onDeactivated(() => {
  console.log(666, "onDeactivated");
});

// 销毁之前
onBeforeUnmount(() => {
  console.log(777, "onUnmounted");
});

// 销毁之后
onUnmounted(() => {
  console.log(888, "onUnmounted");
});
</script>

<!--  
<script>
export default {
  data() {
    return {
      msg: "hello msg",
    };
  },

  // vue2生命周期
  // 创建之前 无法访问数据
  beforeCreate() {
    console.log(111, "beforeCreate");
  },
  // 创建之后 可以访问数据
  created() {
    console.log(222, "created");
  },
  // 构建之前 渲染上树 但数据视图未同步更新
  beforeMount() {
    console.log(333, "beforeMount");
  },
  // 构建之后 数据视图更新
  mounted() {
    console.log(444, "mounted");
  },
  // 存在期
  beforeUpdate() {
    console.log(555, "beforeUpdate");
  },
  updated() {
    console.log(666, "updated");
  },

  // 组件被激活 不再支持销毁期的方法
  activated() {
    console.log(999, "activated");
  },
  // 组件被禁用
  deactivated() {
    console.log(100, "deactivated");
  },

  // 销毁之前 (更改) 此阶段进行收尾工作
  // beforeDestroy() {
  beforeUnmount() {
    console.log(777, "beforeDestroy");
  },
  // 卸载之后 (更改)
  // destroyed() {
  unmounted() {
    console.log(888, "destroyed");
  },
};
</script>
-->
```



### 1.3 错误冒泡

解决子组件出现错误，冒泡到父级组件，导致整个页面无法渲染的问题

当前层级无法捕获



`onErrorCaptured` 

​	用于捕获内部组件的错误信息

​	参数是回调函数

​		err						表示错误信息

​		return false	 	表示阻止传递错误



组件内部阻止冒泡

 		import { `onErrorCaptured` } from 'vue’;

​		 onErrorCaptured(() => { return false })



```js
<template>
  <div>
    <h1>parent part</h1>
    <hr />
    <Demo></Demo>
  </div>
</template>

<script setup>
import { onErrorCaptured } from "vue";

import Demo from "./Demo.vue";

// 捕获内部组件的错误
onErrorCaptured((err) => {
  console.log(err);

  // 阻止错误向上传递
  return false
});
</script>
```



全局捕获错误

​		 app.config.errorHandler = () => {}



```js
import { createApp } from 'vue'
import App from './App.vue'

// 获取应用
let app = createApp(App)
// 全局捕获错误 app.config.errorHandler = () => {}
app.config.errorHandler = function (err) {
  console.log(err);
}
app.mount('#app')
```



### 1.4 hooks

组件间数据，方法复用的一种方式

​	 hooks是一个工厂函数

​			 内部定义数据，方法，生命周期等

​			 返回值：对外提供的数据

​	 使用：执行工厂函数获取数据



```js
// 引入Vue
import { reactive, onMounted, onBeforeUnmount } from 'vue'

// 本质是一个函数
export default () => {
  // 定义响应式数据对象
  let point = reactive({
    x: 0,
    y: 0
  })

  // 定义方法
  const changePoint = e => {
    point.x = e.pageX
    point.y = e.pageY
  }

  // 组件构建完毕
  onMounted(() => {
    window.addEventListener('click', changePoint)
  })

  // 组件即将销毁
  onBeforeUnmount(() => {
    window.removeEventListener('click', changePoint)
  })

  // 返回结果对象
  return point
}
```



```js
<template>
  <div>
    <h2>展示坐标:x:{{ point.x }} y:{{ point.y }}</h2>
    <h1>app part</h1>
    <hr />
    <Parent></Parent>
  </div>
</template>

<script setup>
import { ref } from "vue";
// 引入hooks
import hooks from "./hooks.js";
// 引入组件
import Parent from "./Parent.vue";

// 获取数据
let point = hooks();
console.log(point);
</script>

```



### 1.5 依赖注入

`provide`和`inject`实现祖先与后代组件通信

​		 功能类似父组件向子组件通信，但更直接



​		 provide(key, value)	  向后代组件提供数据

​		 inject(key) 					后代组件获取数据



==注==：只能由父组件向后代组件注入，后代组件不能向父组件注入



```vue
<template>
  <div>
    <h1>app part</h1>
    <hr />
    <Parent></Parent>
  </div>
</template>

<script setup>
import { ref, provide } from "vue";
// 引入组件
import Parent from "./Parent.vue";

// 注入数据
provide("msg", "hello msg");
provide("num", 100);

</script>
```

```js
<template>
  <div>
    <h1>parent part</h1>
    <h2>{{ msg }} -- {{ num }}</h2>
    <hr />
    <Demo></Demo>
  </div>
</template>

<script setup>
import { inject } from "vue";
import Demo from "./Demo.vue";

// 引入数据
let msg = inject("msg");
let num = inject("num");
</script>
```



全局注入：app.use(app => {app.provide(key,value))

应用：在全局注入axios服务 在任意后代组件发送请求



```js
import { createApp } from 'vue'
import App from './App.vue'

// 引入axios
import axios from 'axios'

// 获取应用
// let app = createApp(App)
// 提供全局数据
// app.use(app => {
// })
// app.mount('#app')

// 简写
createApp(App)
  // 提供数据
  .provide('axios', axios)
  // 上树
  .mount('#app')
```

```js
<template>
  <h1>Demo part</h1>
</template>

<script setup>
import { inject } from "vue";

inject("axios")
  .get("/data/home", { params: { a: 1, b: 2 } })
  .then((res) => console.log(res));
</script>

<style></style>
```



### 1.6 组件挂载

内置组件：`Teleport`

​		Teleport是一种能够将我们的组件html结构移动到指定位的技术

​		使用方式：将内容写入内置组件中

​						通过`to`属性指定移动的目标元素中



```vue
<template>
  <div>
    <button @click="isShow = !isShow">打开</button>
    <hr />

    <!-- 将组件的html结构移动到指定位置
          通过 to属性指定移动位置  
        -->
    <Teleport to="body">
      <div class="modal" v-if="isShow">
        <h1>啦啦啦</h1>
        <button @click="isShow = !isShow">关闭</button>
      </div>
    </Teleport>
  </div>
</template>

<script setup>
import { ref } from "vue";

let isShow = ref(false);
</script>
```



### 1.7 异步组件

定义异步组件：`defineAsyncComponent`

​	 参数是回调函数，回调函数的返回值 两种方式

​			 import(url) 直接异步加载组件

​			 new Promise(() => import(url)) 异步操作中（延迟）加载组件



新增组件：`Suspense`

​	 用于加载异步组件，常用插槽： 

​			 \#default  渲染异步组件

​			 \#fallback 组件加载完，提示的内容



```vue
<template>
  <div>
    <!-- <Demo></Demo> -->

    <!-- 
      Suspense 用于加载异步组件
        常用插槽 
          #default 渲染异步组件
          #fallback 组件加载完提示内容
    -->
    <Suspense>
      <template #default>
        <Demo></Demo>
      </template>

      <template #fallback>
        Loading>>>
      </template>
    </Suspense>
  </div>

</template>

<script setup>
import { ref, defineAsyncComponent, Suspense } from "vue";

/* 
  参数是回调函数 回调函数的返回值
    直接异步加载组件
    异步操作中的延迟加载组件
*/
// 直接异步引入组件
// let Demo = defineAsyncComponent(() => import("./Demo.vue"));

// 还可以返回promise
let Demo = defineAsyncComponent(
  () =>
    new Promise((resolve) => {
      setTimeout(() => {
        resolve(import("./Demo.vue"));
      }, 2000);
    })
);
</script>
```



