## 一、面向对象

### 1.1 类

在ES6中实现了类。

语法：`class 类名 {}`

ES6 之前定义类的方式：function People(title) { this.title= title; }

**在类中可以定义三类数据：**

- 第一种：实例数据

  可以通过constructor构造函数定义自身属性或者是方法，这类数据会被当前实例化对象所访问

- 第二种： 原型数据

  我们直接在类体中定义原型方法即可。

  如果要定义原型属性数据，则必须要使用get、set设置特性的方式来定义：get 取值器，set 赋值器

  由于对数据设置了特性，在查看对象的时候，这些数据将展示在自身。

- 第三种：静态数据 （通过类直接访问，而实例化对象是不能访问的）

  **定义静态数据的方式有两种：**

  ​	1，直接在类体中，在数据的前面加上static关键字即可

  ​	2，在类体的外部，直接为类添加数据

**区别：**

在类体中添加的静态数据 设置了特性

在类体外部添加的静态数据 没有设置特性

```js
    <script>
        // es6 实现面向对象
        // 通过 class 关键字 创建类
        class Animal {
            // 类的构造函数：用来添加实例属性
            constructor(name, age, sex) {
                this.name = name
                this.age = age
                this.sex = sex
            }
            // 给类的实例对象添加公共方法
            eat() {
                console.log('吃饭');
            }
        }
        class Cat extends Animal {
            // 给子类添加公共方法
            action() {
                console.log('抓老鼠');
            }
        }
        class Dog extends Animal {
            // 给子类添加公共方法
            action() {
                console.log('看家');
            }
        }

        var xm = new Cat('小猫', 3, '公')
        var xg = new Dog('小狗', 4, '母')

        console.log(xm);
        xm.eat()
        xm.action()

        console.log(xg);
        xg.eat()
        xg.action()
    </script>
```

```js
    <script>
        class Person {
            constructor(name, age) {
                this.name = name
                this.age = age
                this.sex = '未知'
            }

            walk() {
                console.log('学习');
            }
        }

        // 1.给类添加属性静态属性和静态方法
        Person.abc = 123
        Person.fn = function () {
            console.log('静态方法');
        }

        class Boy extends Person {
            // 如果资料没有自己的实例属性,可以省略 constructor 不写
            // 如果资料需要添加自己的实例属性 , 必须写 constructor 并且在 constructor 函数的最前面使用 super() 方法继承父类
            constructor(name, age) {
                super(name, age)
                this.sex = '男'
            }
            // 如果子类有与父类同名的公共方法,会覆盖父类的同名方法
            walk() {
                console.log('打篮球');
            }
        }
        class Girl extends Person {
            constructor(name, age) {
                super(name, age)
                this.sex = '女'
            }
        }

        var xm = new Boy('小明', 22)
        var xh = new Girl('小红', 23)

        console.log(xm);
        console.log(xh);
        xm.walk()
        xh.walk()

        // 给实例对象添加私有属性和私有方法
        xm.a = 1000
        xh.f = function () {
            console.log('私有方法');
        }
        console.log(xm.constructor);
        console.log(xh.constructor);

        // 静态属性和静态方法 不能被实例对象继承
        // console.log(xm.abc);
        // xh.fn()

        // 静态方法只能通过类名调用,静态方法也可以被子类继承
        console.log(Person.a);
        console.log(Boy.a);
        console.log(Girl.a);
        Boy.fn()
        Girl.fn()
    </script>
```

## 二、编译Es6

### 2.1 编译 ES6

> 随着es6,es6+等新标准的出现，为了有更好的开发体验而要使用这些新特性，但是在浏览器中又不能直接运行，所以我们就需要一个编译工具来将代码编译成浏览器支持的版本，这就需要babel编译器

1.检查电脑有没有node环境
                在命令提示符中输入 node -v 
                    如果可以看到版本号，说明安装了node环境 
                    如果有错误提示，说明没有安装
                    官网：    https://nodejs.org/en/

2.查看全局环境的安装包
                npm list -g
3.全局安装 babel-cli 工具
                npm install -g babel-cli
4.配置.babelrc文件
5.安装 ECMASCript 语法转换插件
                npm install babel-preset-es2015
                会在根目录生成 node_modules文件夹
6.编译js文件
                注意：在项目目录中运行
                完整命令写法：  babel 要编译的文件名称 --out-file 输出文件名称
                简写： babel 要编译的文件名称 -o 输出文件名称

```js
{
    "presets": ["es2015"]
}
```

## 三、ECMASript 7 新特性

### 3.1.Array.prototype.includes

`Includes()` 方法用来检测数组中是否包含某个元素，返回布尔类型值

### 3.2.指数操作符

在 ES7 中引入指数运算符`**`，用来实现幂运算，功能与 `Math.pow` 结果相同

```js
    <script>
        // https://caniuse.com/   查看特性浏览器支持情况

        var arr = [1, 2, 3, 4, 5]
        // 1.includes:  查看数组中有没有指定的值
        console.log(arr.includes(4));
        console.log(arr.includes('a'));

        // 2.指数运算符
        console.log(2 * 3);
        // 2 的 三次方 
        console.log(2 ** 3);
        console.log(8 ** 3);
        // 指数运算符的优先级高于乘除法和取余运算符
        console.log(4 * 2 ** 3);
    </script>
```

## 四、 ECMASript 8 新特性

### 4.1.async 和 await

async 和 await 两种语法结合可以让异步代码像同步代码一样

#### 4.1.1.async 函数

1. async 函数的返回值为 promise 对象
2. promise 对象的结果由 async 函数执行的返回值决定

#### 4.1.2.await 表达式

1. await 必须写在 async 函数中
2. await 右侧的表达式一般为 promise 对象
3. await 返回的是 promise 成功的值
4. await 的 promise 失败了, 就会抛出异常, 需要通过 try...catch 捕获处理

### 4.2.Object.values和 Object.entries

1. Object.values()方法返回一个给定对象的所有可枚举属性值的数组
2. Object.entries()方法返回一个给定对象自身可遍历属性 [key,value] 的数组

### 4.3.Object.getOwnPropertyDescriptors

该方法返回指定对象所有自身属性的描述对象

```js
<script>
		// ES8 新特性
        // 将对象转换成可迭代对象
        var obj = {
            a: 1,
            b: 2
        }
        Object.defineProperty(obj, 'c', { value: 30 })
        console.log(obj);

        // 1.Object.values() 将对象的可枚举属性的值转换成数组
        var obj2 = Object.values(obj)
        console.log(obj2);

        // 2.Object.entries() 将对象的可枚举属性的属性名和属性值转换成一个二维数组
        var obj3 = Object.entries(obj)
        console.log(obj3);

        // 3.获取对象的指定属性的特性描述符
        console.log(Object.getOwnPropertyDescriptor(obj, 'a'));
        // 4.获取对象的所有属性的特性描述符
        console.log(Object.getOwnPropertyDescriptors(obj));
    </script>
```

## 五、ECMASript 9 新特性

### 5.1.Rest/Spread 属性

Rest 参数与 spread 扩展运算符在 ES6 中已经引入，不过 ES6 中只针对于数组， 在 ES9 中为对象提供了像数组一样的 rest 参数和扩展运算符

### 5.2.正则表达式命名捕获组

ES9 允许命名捕获组使用符号`[?]`,这样获取捕获结果可读性更强

### 5.3.正则表达式反向断言 ES9 支持反向断言

通过对匹配结果前面的内容进行判断，对匹配进行筛选。

### 5.4.正则表达式 dotAll 模式

正则表达式中点.匹配除回车外的任何单字符，标记`『s』`改变这种行为，允许行 终止符出现

```js
    <script>
        // 1.逆运用 对数组操作
        /* let arr = [1, 2, 3, 4, 5];
        // 解构
        let [num1, num2, ...nums] = arr;
        // 逆运用
        let result1 = [...nums, 8, 9]
        console.log(result1); */

        // 2.对象 逆运用
        /* let obj = {
            num: 10,
            color: 'red',
            msg: 'hello'
        }
        // 解构
        let { num, ...others } = obj;
        // ES6不支持对象的三个点语法逆运用，ES2018（ES9）提供了支持
        let result2 = { ...others, num }
        console.log(result2); */

        // 3.命名捕获分组
        /* var str = 'adfa123uiu035k435fdasljfl980fd';
        // 命名捕获分组： 给捕获分组取了一个名字
        // 语法：  (?<分组名称>exp)
        var re = /(?<letter>[a-z])(?<num>\d+)/g;
        var result = re.exec(str);
        console.log(re.exec(str)); */

        // 4.正向肯定断言  ： 后面必须有指定的字符
        /* var str = 'adfa123uiu035k435fdasljfl980fd';
        var re1 = /\d+(?=u)/g;
        console.log(str.match(re1));
        // 正向否定断言： 后面不能有指定字符
        var re2 = /\d+(?!f)/g;
        console.log(str.match(re2)); */

        // 5.反向肯定断言： 前面必须有指定字符
        /* var str = 'adfa123uiu035k435fdasljfl980fd';
        var re2 = /(?<=a)\d+/g;
        console.log(str.match(re2));
        // 反向否定断言：前面不能有指定字符
        var re3 = /(?<!a)\d+/g;
        console.log(str.match(re3)); */

        // 6.全匹配修饰符 s
        var str2 = 'fasdj\fds\nfdajlk\rjfdlf';
        // . 元字符默认不能匹配 换行符 和 回车符
        // var re3 = /.+/g;
        // 使用 s 修饰符，让 . 元字符可以匹配包括换行符和回车符的所有字符。
        var re3 = /.+/gs;
        console.log(str2.match(re3));
    </script>
```

## 六、ECMASript 2019 新特性

#### 6.1.Object.fromEntries

将键值对列表（二维数组或map对象）转换成对象

```js
var arr = [
            [10, 20, 30],
            ["a", "b", "c"],
            [/\d+/, true, 100],
            [12345]
        ];

        // 1. fromEntries() 将一个二维数组转换为一个对象
        // 只会取二维数组的前两个值当作属性名称和属性值，多余的值会被忽略
        var obj = Object.fromEntries(arr)
        console.log(arr);
        console.log(obj);
```



#### 6.2.trimStart 和 trimEnd

trimStart() 去除字符串开头的空白字符

trimEnd() 去除字符串结尾的空白字符

```js
var str = `

            \t这是一个多行字符串,  fadskl jfldajl f fdas
            这是第二行。fsadf;jl;; \n\rjfjda;ljlj

        `;
        console.log("(" + str + ")");
        // 2.trimStart() 去掉字符串开始的空白字符
        console.log("(" + str.trimStart() + ")");
        // 3.trimEnd()  去掉字符串结尾的空白字符
        console.log("(" + str.trimEnd() + ")");
        // 4.trim()     去掉字符串开头和结尾的空白字符
        console.log("(" + str.trim() + ")");
```



#### 6.3.Array.prototype.flat 与 flatMap

flat(n) : 数组降维 n 表示降维层级 支持 Infinity

flatMap() : 以 map() 方法的形式降维

arr.flatMap( (val, index, arr) => {})

```js
// 多维数组
        var arr = [1, 2, [3, 4, [5, 6, [7, 8]]]];
        console.log(arr);
        // 5.flat()     对数组进行降维
        // 参数：表示要降的维数 默认值为1   可以使用具体数字，使用 Infinity
        var result = arr.flat()
        console.log(result);
        // 将数组降两维
        var result = arr.flat(2)
        console.log(result);
        // 将多维数组降一维数组
        var result = arr.flat(Infinity)
        console.log(result);
```



#### 6.4.Symbol.prototype.description

返回 `Symbol`的可选描述的字符串 ，即 Symbol("hello") 中的 "hello"。

```js
// 6.获取 symbol 类型的标识符   description
        var sym1 = Symbol('aaa')
        var sym2 = Symbol('bbb')
        console.log(sym1);
        console.log(sym2);

        console.log(sym1.description);
        console.log(sym2.description);
```

## 七、ECMASript 2020 新特性

#### 7.1.String.prototype.matchAll

返回一个包含所有匹配正则表达式的结果及分组捕获组的迭代器。

必须设置 g 修饰符，否则抛出错误。

```js
    <script>
        var str = 'adf23kjlf432jlkjl980joiuo898fdas';
        var re = /\d+/g;

        console.log(str.match(re));
        console.log(str.matchAll(re));

        // 返回包含所有匹配结果的可迭代对象
        // 必须使用正则的 g 修饰符,否则会抛出错误
        var result = str.matchAll(re)
        console.log(result.next());
        console.log(result.next());
        console.log(result.next());
        console.log(result.next());
        console.log(result.next());
    </script>
```



#### 7.2.类的私有属性 和私有方法

只能在类的内部使用，不能在外部使用的属性和方法

```js
    <script>
        // 私有属性：只能在类中使用，不能在类的外面使用，其它面向对象语言提供了private关键字，js提供#，可以定义私有属性
        class Book {
            // 类体中定义属性
            color = 'red';
            // 私有属性 #
            #num = 10;
            constructor(title) {
                this.title = title;
                // 可以访问
                console.log(this.#num);
            }
            getTitle() {
                return `这本书的名字是《${this.title}》`
            }
            // 私有是属性只能在类的内部访问，外部无法访问，
            // 想在外部访问，定义在类中定义访问私有属性的方法
            getNum() {
                return this.#num;
            }
        }
        // 创建一本书
        // let b = new Book('设计模式');
        // // console.log(b.#num)
        // console.log(b, b.getNum(), b.color);
        // 私有属性不能被继承
        class JsBook extends Book {
            constructor(title) {
                super(title);
                // 尝试访问私有属性， 失败
                // console.log(this.#num);
            }
        }
        // 实例化
        let jb = new JsBook('面试题');

        console.log(jb.color, jb);
    </script>
```



#### 7.3.Promise.allSettled

Promise.all() 碰到失败的 promise 会停止，

`Promise.allSettled` 会执行完所有。

```js
    <script>
        var p1 = new Promise((res, rej) => {
            res(100)
        })
        var p2 = new Promise((res, rej) => {
            res(200)
            // rej('abc')
        })
        var p3 = new Promise((res, rej) => {
            res(300)
        })

        // 所有 promise 对象都执行成功回调 才会返回所有成功的结果
        // 只要有一个失败,就只返回失败的结果
        Promise.all([p1, p2, p3]).then(
            d => console.log(d, '成功的回调'),
            d => console.log(d, '失败的回调')
        )

        // 1.始终返回所有 promise 对象的执行结果
        // 返回值是一个数组 数组的值对应每一个promise对象的结果
        // 执行成功的 promise 对象结果包含两个属性 status 值为 fulfilled 和 value 值为 promise 的执行成功结果
        // 执行失败的 promise 对象的结果包含两个属性 status 值为 rejected 和 reason 值为 promise 的执行失败结果
        Promise.allSettled([p1, p2, p3]).then(
            d => console.log(d, '成功的回调'),
            d => console.log(d, '失败的回调')
        )
    </script>
```



#### 7.4.可选链操作符

`?.`	如果属性存在就返回属性值，不存在返回 undefined

```js
    <script>
        var obj = {
            a: 100,
            b: {
                c: {
                    d: 'abc'
                },
                e: 200
            }
        };

        console.log(obj.a);
        console.log(obj.b);

        /* console.log(obj.a.c);
        console.log(obj.c);
        console.log(obj.c.d); */

        // 1.可选链操作符   ?.
        // 判断对象身上有没有指定的属性,如果有返回属性的值
        // 如果没有返回 undefined
        /* obj?.a?.b    查看obj 有没有 a 属性,如果没有返回 undefined
        如果有再继续查看属性后面有没有b属性 */
        console.log(obj?.a);
        console.log(obj?.c);
        console.log(obj?.b?.c?.d)       // 'abc'
    </script>
```

#### 7.5 golbalThis

Web端：window、self

Web Workers端： self

Node.js端：global

```js
    <script>

        // 1.浏览器环境运行  globalThis window对象
        console.log(globalThis);
        // 2.worker 环境运行    DedicatedWorkerGlobalScope 对象     服务器模式运行
        var wk = new Worker('../js/a.js')
        console.log(globalThis);
        // 3.nodejs 环境运行
        // node
        // this
        /*
            globalThis 全局作用域中的 this 对象，是一个只读对象
            在不同的运行环境中 globalThis 的值不同
                1.浏览器环境运行
                    window对象
                2.worker 环境运行    DedicatedWorkerGlobalScope 对象
                3.  在 nodejs 环境中
                    globalThis 为 全局对象  global
        */
    </script>
```



#### 7.6 BigInt

> 表示大于 2^53 - 1 的整数：100n 或 BigInt(1)
>
> [ES2020](https://github.com/tc39/proposal-bigint) 引入了一种新的数据类型 BigInt（大整数），来解决 数值的精度只能到 53 个二进制位（相当于 16 个十进制位），大于这个范围的整数，JavaScript 是无法精确表示， 并且大于或等于2的1024次方的数值，JavaScript 无法表示 这个问题，这是 ECMAScript 的第八种数据类型。

BigInt 只用来表示整数，没有位数的限制，任何位数的整数都可以精确表示。

为了与 Number 类型区别，BigInt 类型的数据必须添加后缀`n`。

BigInt 可以使用负号（`-`），但是不能使用正号（`+`），因为会与 asm.js 冲突。

JavaScript 原生提供`BigInt`函数，可以用它生成 BigInt 类型的数值。转换规则基本与`Number()`一致，将其他类型的值转为 BigInt。 （不能使用new关键字调用）

```js
    <script>
        var num = 100
        console.log(typeof num);

        // js 中能够表示的最大近似值
        console.log(Number.MAX_VALUE);

        // js中能够准确表示的最大值
        console.log(Number.MAX_SAFE_INTEGER);

        // 1.字面量方式传教 Bight
        var num1 = 100n
        console.log(num1);
        console.log(typeof num1);       // bigint

        // 2.通过构造函数 Bight() 创建(不能使用 new 运算符)
        var num2 = BigInt(200)
        console.log(num2);
    </script>
```

#### 7.5.动态 import 导入

ES Module是在ES6中提出的，但是被纳入了ES2016（ ES7 ）中。

引入模块：通过import引入

-  import {} from ‘’ 引入某些接口，可以直接使用接口
-  import * as M from ‘’ 引入所有的接口，并存储在M命名空间下
-  import M from ‘’ 引入默认接口
-  import "" 引入文件

暴露接口：通过export或者export default暴露

 export 暴露接口，可以被前两种引入接口的方式引入。

 export default 暴露默认接口，可以被第三种引入接口的方式引入。

```js
    <!-- import / export 以模块的方式加载 js 文件 -->
    <!-- 
        以模块的方式加载文件
                每一个 script 标签都是一个单独的作用域
                每个 js 文件 就是一个单独模块
                每个script标签也是一个单独模块
    -->
    <script type="module">
        // 1.使用 import 关键字 来导入其他模块共享的数据
        /* import { a, b, fn1 } from './1.js'
        console.log(a, b);
        fn1() */
        // 2.导入的时候 可以使用 as 关键字对原接口名称进行重命名
        import { a as d } from './1.js'
        console.log(d);
    </script>
```

```js
    <script type="module">
        // 不暴露接口引入整个模块   
        // 1.引入整个 js 模块  import 文件名
        import './2.js'

        // 2.从模块中引入指定接口
        import {
            a,
            b,
            abc as num
        } from './1.js'
        console.log(a);
        console.log(b);
        console.log(num);

        // 4.引入的接口不能与已有的变量名同名   
        // Identifier 'num1' has already been declared
        var num1 = 1
        var num2 = 2
        // import { num1, num2 } from './3.js'
        // 解决方式一 使用 as 关键字重命名
        /* import { num1 as n1, num2 as n2 } from './3.js'
        console.log(n1);
        console.log(n2); */
        // 解决方式二 在导出时 使用export default 导出默认接口
        // 注意 每一个模块只能使用一次默认接口
        // import命令可以为其指定任意名字
        import obj from './3.js'
        console.log(obj);
    </script>


    <!-- 
        模块模式 默认使用严格模式   'use strict'
        import 
            1.引入整个 js 模块  import 文件名
            2，从 js 模块中引入指定的接口
                import {} from 文件名
            3，引入并重命名接口
                import { a as b } from 文件名
            4.引入的接口不能与已有的变量名同名   
                解决方式一 使用 as 关键字重命名
                解决方式二 在导出时 使用export default 导出默认接口
                    注意 每一个模块只能使用一次默认接口
                    import命令可以为其指定任意名字
        export: （接口数据可以是任意数据类型）

            1, 导出时指定名称的接口 (可以使用 var / let / const )
                export var 接口名称 = 接口数据;

            2，导出提前声明的接口 (可以使用 var / let / const )
                var 接口名称 = 接口数据;
                export {接口名称}

            3，导出默认接口 （一个模块只允许有一个默认接口）
                export default 接口数据
    -->
```





