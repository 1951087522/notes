## 一、TypeScript

### 1.1 Typescript 简介

Typescript 简称ts，是js语法的超集，很多js新的语法就借鉴了ts语法，ts是由微软团队维护的。

在过去，js的出现是为了解决页面中的一些简单交互，因此js被设计非常简单，被很多开发者接受。

> **js特点**：
>
> 弱类型：定义变量没有具体的类型，可以存储任何类型的数据
>
> 动态的：变量存储的数据需要开辟多少内存空间，不是在定义时候说的算，而是运行时候动态开辟的

由于js是弱类型的，因此变量存储的是什么样式的数据，需要多少的内存空间，我们在定义的时候无法获知，只能在js运行的时候，动态的分配，所以js运行的时候，一边处理业务逻辑，一边分配内存空间，对于小型项目来说，运行时临时分配空间的性能消耗是可以接受的，在大型项目中，这种消耗是无法接受的。

所以在一些强类型语言中，为变量在定义的时候指明类型，这样运行前就可以针对变量的类型分配内存空间，这样在程序运行的时候就不需要分配空间了，可以减少不必要的资源消耗，所以ts是一个强类型语言

在大型项目中，为了提高代码可维护性，我们通常采用面向对象编程方式，但是在面向对象编程中，我们势必要使用类，继承，接口，私有属性，共有属性等等，但是这些关键字，诸如：class，extends， implement， interface， private， public等等js都不支持，虽然JS可以模拟实现这些功能，但是为了模拟这些功能势必会产生一些不必要的开销，在大型项目中，这些开销是无法接受的。所以TS基于面向对象编程方式，实现了这些关键字。

TS语法着眼于未来与大型项目。遗憾的是，这些功能并没有一个浏览器实现，也没有一个浏览器宣称要实现（并且IE浏览器都没有实现），所以我们就要将其编译成js语言（ES3.1版本或者是ES5版本）。

TS与JS的关系如右图：

官网：http://www.typescriptlang.org/

中文网站：https://www.tslang.cn/

GitHub地址：https://github.com/Microsoft/typescript

ts文件的拓展名是.ts

![image-20221223204226938](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221223204226938.png)



### 1.2 编译ts

​	1.安装tsc指令 终端执行：

```
npm install typescript -g
```

​	2.安装完成，查看版本号：`tsc -v`

​		在开发目录中，执行`tsc 文件名`指令，即可编译ts文件。

​	3.监听并发布: 通过`tsc -init`创建`tsconfig.json`配置文件。

​		`outDir` 定义js文件发布的位置

​	4.执行`tsc -w`监听并发布文件

注：在后面课程中，会使用工程化工具来自动化的编译ES6与TS（webpack）。

```js
/* 
    1. tsc -init 初始化配置文件 
    2. 在 tsconfig.json 文件中
        找到  Emit 选项中的 "outDir", 将值改为输出 js 文件的路径地址。
    3.监控文件变化  tsc -w
*/
```



### 1.3 数据类型

在ts中所有的数据都要指明类型： `let 变量名:类型 = val;`

在js中的数据类型，ts中都支持

number , string , boolean, function, array, object

并且还拓展了: any(任意类型)、void、never这些类型

### 1.4 类型猜测

如果定义的数据没有指定类型，此时程序运行的时候根据赋值的数据进行类型猜测

> 但是，不要让ts去猜测类型：
>
> 1 程序执行的时候，会对数据进行猜测，会临时分配内存空间，造成消耗性能
>
> 2 类型猜测往往不是我们要的结果

```js
/* 
    1.typescript 要求变量声明时，必须同时声明变量的类型
        语法格式    var 变量名称:数据类型
            声明关键字  var let const
*/
var num1 = 100
num1 = 200
console.log(num1);

// 2.如果在声明时,没有声明类型,会根据第一次赋值得类型，自动猜测并设置变量的类型
// 一旦变量的类型确定就不再允许赋值其他类型的值
// num1 = 'abc'

var num2: string = 'abc'
num2 = 'def'
console.log(num2);

// 3.typescript 在 js 现有的数据类型基础上，扩展了三个新的类型 any(任意类型) never void (函数的返回值)
// 如果期望声明一个可以赋值为任意类的值的变量 需要指定变量类型为 any
var a:any;
a = 1
console.log(a);
a = 'a'
console.log(a);
a = true
console.log(a);

// typescript 声明变量的原则：所有的变量都手动指定变量的类型，而不要让 typescript 自己去做类型猜测。会临时分配内存空间，造成消耗性能
```



### 1.5  数组

在ts中定义数组也要指定类型

语法：`let arr:type[] = []`

此时：

我们传递数组中的数据，必须和初始化定义的数据类型是一致的

如果类型不确定，我们可以将type改变any

### 1.6 元组

定义元素的方式与定义数组很相像

只不过在定义的时候，要指定类型以及指定个数

语法：`let arr:[type1, type2] = [item1, item2];`

此时：

1 传递数据的时候，必须和初始化的类型是一致的

2 传递数据的时候，必须和初始化的数据个数一致

3 在后面添加数组中成员的时候，必须在指定的类型范围之内

```js
// typescript 中的数组
// 1.第一种数组 所有的值类型必须都是相同的
// 声明一个值必须全是 number 类型的数组
var arr1: number[] = [1, 2, 3]
console.log(arr1);
arr1.push(10)
// 追加的值必须是数字类型
// arr1.push('a')
console.log(arr1);

// 2.第二种数组：元组

// 2.1元组是固定格式的数组，必须在声明时就设置好类型并赋值
var arr2: [string, number] = ['abc', 100]
console.log(arr2);

// 2.2元组的值顺序必须与设置的类型的顺序相同
// var arr3: [string, number] = [123, 'abc']

// 2.3元组的值数量与类型设置的数量相同
// var arr4: [string, number] = ['abc', 100, 200]

// 2.4元组的值可以通过索引的方式，赋值相同类型其他值
var arr5: [string, number] = ['def', 200]
console.log(arr5);
arr5[0] = 'ggg'
arr5[1] = 300
console.log(arr5);

// 2.5 不能赋值不同类型的值 不能增加新值
// arr5[0] = true
// arr5[2] = '123'

// 2.6元组允许通过方法 push() 和 unshift() 添加新值
arr5.push('lll')
console.log(arr5);
// 2.7 只能通过方法添加已经设置的类型的值，不能添加其他类型的值
// arr5.push(true)
arr5.unshift('123')
console.log(arr5);
```



### 1.7 类型级联

如果定义的数据没有确定的类型，我们可以将数据的类型改为any

但是，any类型表示的数据范围太大了，为了缩小数据类型的范围，要使用类型级联技术

语法：`type1 | type2 | type3`

此时，定义的数据类型只能在该范围之内

```js
// 1.类型级联 允许给一个变量设置多个类型 ||
var a: string | number
a = 'abc'
console.log(a);
a = 123
console.log(a);
// 只允许设置 在级联范围内的类型值
// a = true
```



### 1.8 枚举类型

> 枚举类型是介于对象和数组之间的数据类型
>

> 语法：enum 枚举类型 {}
>

特点：

既可以像数组那样，通过索引值获取属性名。

又可以像对象那样，通过点语法获取索引值

注意：

1 枚举类型数据的首字母要大写

2 每一个成员之间用逗号分隔

3 我们可以为某个成员改变索引值。此时，后面的成员索引值要递增，前面的不变

```js
/* 
    枚举类型：属于复杂类型
    枚举类型使用 enum 关键字声明 
        > 枚举类型是介于对象和数组之间的数据类型
        既可以像数组那样，通过索引值获取属性名。
        又可以像对象那样，通过点语法获取索引值
*/

enum obj {
    red,
    green,
    blue
}
console.log(obj);
console.log(obj.red);
console.log(obj.blue);
console.log(obj[0]);

enum sex {
    '男',
    '女'
}
console.log(sex['男']);
console.log(sex['0']);

enum obj2 {
    // 1.枚举类型的索引默认从 0 开始自动自增
    a,
    b,
    // 2.可以设置索引自动的起始位置
    c = 10,
    d,
    e,
    f,
    // 3.索引的范围并不要求是连续 可以随时在任意位置修改后面的起始值
    g = 100,
    i
}
console.log(obj2);
console.log(obj2.a);
console.log(obj2.b);
console.log(obj2.c);
console.log(obj2.d);
console.log(obj2.e);
console.log(obj2.f);
console.log(obj2.g);
console.log(obj2[101]);
```



### 1.9 函数

> 在js中定义函数的方式有：
>
> ​	1 构造函数式
>
> ​	2 函数定义式
>
> ​	3 函数表达式
>
> ​	4 箭头函数
>

只有函数定义式，不需要定义var或者是变量来接收

- 在ts中要为每一个函数指明类型


​	语法：`function demo(arg:type, arg1?:type):type {}`

箭头函数的书写格式

```js
	var fn2 = (a: number, b: number): number => a + b
```

- 传递参数：

  ​	1 传递的数据类型要一致

  ​	2 传递的数据个数要一致

- 注意

  1 函数中参数以及返回值要定义类型

  2 如果参数可有可无，后面加上问号即可

- 函数的返回值通常有三类结果：

  1 返回数据，此时函数的返回值类型就是数据类型

  2 没有返回数据，函数的类型是void

  3 如果函数中出现了错误，此时函数的类型是never

```js
function fn1(a: number | string) {
    // 1.as ： 类型断言 断定变量的值一定是某个类型

    // 1.类型断言的两种语法格式
    // 语法格式 变量名称 as 类型
    // return a as number + 10

    // 2.语法格式 <类型>变量名称
    return <number>a + 10
}
console.log(fn1(100));

/*  1.typescript 要求函数的所有形参必须要设置类型并且声明函数有没有返回值
        格式 function 函数名称(形参1：类型，形参2：类型)：返回值类型 {}
        函数中参数以及返回值要定义类型
        没有返回数据，函数的类型是void 
*/


// 2.箭头函数的书写格式
var fn2 = (a: number, b: number): number => a + b
console.log(fn2(10, 20));

// 3.如果函数不需要有返回值 类型应该设置为 void
function fn3(): void {
    console.log('行百里者半九十');
}
fn3()

// 4.函数设置几个形参,就只能传递几个形参
function fn4(a: number, b: number): number {
    return a + b
}
// console.log(fn4(1, 2, 3));
console.log(fn4(1, 2));

// 5.可选参数 形参?类型 如果参数可有可无后面加上问号
function fn5(a: number, b: number, c?: string): number | string {
    if (typeof c !== 'undefined') {
        return a + b + c
    } else {
        return a + b
    }
}
// console.log(fn5(10, 20));
console.log(fn5(10, 20, 'px'));

// 6.如果函数中出现了错误，此时函数的类型是never
function fn6(): never {
    throw new Error('抛出错误')
}
fn6()
```



####  1.9.1 类型断言

当我们比程序更了解数据类型的时候，此时可以使用类型推断技术。让计算机按照某种类型去运行

语法

> 第一种语法：`<type>数据`
>
> 第二种语法：`数据 as 类型`

类型推断并没有改变数据类型，不同于类型转换

```js
function fn1(a: number | string) {
    // as ： 类型断言 断定变量的值一定是某个类型
    // 1.类型断言的两种语法格式
    // 语法格式 变量名称 as 类型
    // return a as number + 10
    // 2.<类型>变量名称
    return <number>a + 10
}
console.log(fn1(100));
```



### 1.10 泛型

> 如果参数的类型是任意的，返回的结果也可以能是任意的，此时我们可以将类型定义成any。
>
> 如果希望参数与返回值的类型是一致的，any类型就不适用。此时可以使用泛型
>

语法: `function demo<T>(arg:T):T {}`

这样的话，参数与返回值的类型就一致了，都是T变量表示的类型

使用函数的时候，有两种方式

第一种 `demo<type>(数据);`

第二种 `demo(数据)`

此时将猜测类型，常用。

```js
/* 
    1.泛型 函数的一种使用格式
    泛型本身不限定形参的具体类型 但可以通过泛型标识符来控制形参和返回值的格式 

    如果参数的类型是任意的，返回的结果也可以能是任意的，此时我们可以将类型定义成any。
    但是希望参数与返回值的类型是一致的，any类型就不适用。此时可以使用泛型

    格式    function fn<T>(a:T):T {}
*/
function fn1<T>(a: T): T {
    return a
}
console.log(fn1(10));

// 2.传入两个值
function fn3<T, U>(a: T, b: U): [T, U] {
    return [a, b]
}
console.log(fn3(10, 20));
console.log(fn3(10, 'ab'));
```

### 1.11 类

语法：`class 类名 {}`

注：类名首字母要大写。

- **构造函数**：

  我们也是通过`constructor`定义构造函数

  我们只能定义参数类型，不能定义返回值的类型。参数可有可无，后面添加?

- **属性**：

  在ts中，我们要将属性在类体中声明类型。

  声明的时候可以赋值，但必须要设置类型。

  在构造函数中，也可以为声明的属性赋值。没有声明属性，在构造函数中，是不能使用，

  我们用构造函数的参数为属性赋值，实现数据由外部流入内部。

  属性必须在声明的时候赋值或者构造函数内部赋值。

- **方法**：

  定义方法的语法与ES6的语法是一样的。要声明参数以及返回值的类型

- **关键字**：

  ts支持`private`，`protected`，`public`，`static`等关键字，

  ​	`private`			私有方法

  ​			*私有方法只能在 类的内部访问 ,并且不会被子类继承*

  ​	`protected`		受保护的方法

  ​			*受保护的方法只能 类的内部访问， 但可以被子类继承*

  ​	`public`			公共方法

  ​	`static`			静态方法

  ​			*只能通过类访问 不会被实例继承*

  但是private，protected，public在js中无法实现，或者实现成本很高，所以编译的时候，直接删除了，

  staitc定义的静态属性可以实现。直接在数据前面加上staitc，就可以得到一个静态的属性了

  ES6中，可以在类的外部，添加静态属性，但是在ts中，不能在外部添加属性，只能修改（在类中声明）

- **实例化**：

  我们在实例化的时候，出现了变量，因此要定义变量的类型

  变量的类型就是类。传递的参数要与构造函数一致。

  ts编译成es3.1版本的语法，因此定义的类是一个闭包类。·

```js
// ts 中的类

class Person {
  // 1.给实例属性定义类型
  name: string;
  age: number;

  // 2.小括号中是给形参定义类型
  constructor(name: string, age: number) {
    // 需要提前定义实例属性类型
    this.name = name;
    this.age = age;
  }
  // 方法不需要返回值需要设置返回值类型为 void
  say(): void {
    console.log(this.name, this.age);
  }
  // 3.方法有参数和返回值，必须指定参数和返回值的具体类型
  fn1(name: string): string {
    return name;
  }

  // 4.公共方法：public
  public fn2(): void {
    console.log("公共方法");

    // 8.私有方法 只能在类的内部访问 并且不会被子类继承
    this.fn3();

    // 9.受保护的方法 类的内部访问 可以被子类继承
    this.fn4()
  }

  // 5.私有方法：private  私有方法只能在 类的内部访问 ,并且不会被子类继承
  private fn3(): void {
    console.log("私有方法");
  }

  // 6.受保护的方法 protected   受保护的方法只能 类的内部访问， 但可以被子类继承
  protected fn4(): void {
    console.log("受保护的方法");
  }

  // 7.静态方法 static 只能通过类访问 不会被实例继承
  static fn5() {
    console.log("静态方法");
  }
}

// 子类
class Boy extends Person {
  public fn6() {
    console.log("子类的公共方法");
  }
}

var xm = new Person("小明", 18);
var xh = new Boy("小红", 20);
console.log(xm);
console.log(xh);

// 静态方法只能通过类访问
Person.fn5();

/* 
  ts 中的类
    1.需要给实例属性定义类型
    2.小括号中需要给形参定义类型
    3.方法有返回值和参数也要指定具体类型
    4.新增的关键字
      public    公共方法
      private   私有方法
        私有方法只能在类的内部访问并且不会被子类继承
      protected 受保护的方法
        受保护的方法只能在类的内部访问 会被子类继承
      static    静态方法
        只能通过类访问 不会被实例继承
*/

```



### 1.12 继承

TS中的继承：

语法 `class 子类 extends 父类 {}`

继承后，我们可以重写方法，此时优先使用我们重写的方法。

如果重写构造函数，

我们要使用`super`关键字实现构造函数继承

如果传递了参数，要与父类的参数一致（类型一致，个数一致）

属性要在super关键字之后赋值。

ts中的继承是通过：寄生组合式的继承。

```js
class Person {
  // 给实例属性定义类型
  name: string;
  age: number;

  // 小括号中是给形参定义类型
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
  // 方法没有返回值需要指定返回值类型
  say(): void {
    console.log(this.name, this.age);
  }
  // 方法有返回值和参数需要指定具体类型
  fn1(name: string): string {
    return name;
  }
  // 公共方法 public
  public fn2(): void {
    console.log("公共方法");
    // 私有方法只能在类的内部方法 不会被子类继承
    this.fn3();
    // 受保护的方法只能在类的内部访问 会被子类继承
    this.fn4();
  }
  // 私有方法 private
  private fn3(): void {
    console.log("私有方法");
  }
  // 受保护的方法 protected
  protected fn4(): void {
    console.log("受保护的方法");
  }
  // 静态方法 static 只能通过类访问 不会被实例属性继承
  static fn5(): void {
    console.log("静态方法");
  }
}

// 1.继承
class Boy extends Person {
  sex: string;
  constructor(name: string, age: number, sex: string) {
    super(name, age);
    this.sex = sex;
  }

  public fn5(): void {
    console.log("公共方法");
    this.fn4();
  }

  public fn2(): void {
    console.log("覆盖父类同名的公共方法");
  }
}

// 在实例对象后面徐州指定是哪一个类的实例对象
var xm: Person = new Person("小明", 19);
console.log(xm);
var xh: Boy = new Boy("小红", 20, "女");
console.log(xh);
xh.fn2()

```

### 1.13 模块

ts允许我们在文件内部定义模块

通过`module`关键字定义模块

在模块内部，通过`export`关键字暴露接口

模块的本质就是对数据添加了命名空间。所以使用数据的时候，要通过命名空间来访问。

ts中的模块是通过闭包实现的。



### 1.14 接口

接口：指的是一种数据结构，用`interface`关键字定义

语法 `interface 接口名称 { 只定义结构，不要去实现。}`

- **函数接口**

在函数表达式中，出现了变量，为了说明变量的结构，我们要定义接口

在函数中，函数的参数以及返回值属于结构，所以我们只需要定义这些数据的类型。

如果参数可有可无，要添加?

```js
interface 函数接口 {
	 (arg?:type):type
}
```

```js
// 接口 interface

// 1.1给对象定义接口
interface obj {
  name: string;
  age: number;
  // 1.2 ?问号表示 属性或方法可选
  c?: boolean;
  say(): void;
}

// 1.3通过接口定义对象,对象的属性和方法受接口约束，模型是固定的不能随意添加
var xm: obj = {
  name: "小明",
  age: 22,
  say() {
    console.log("接口对象");
  },
};
console.log(xm);

// 1.4定义普通对象，对象的属性和方法不受约束，可以任意添加
var xh: object = {
  name: "小红",
  age: 22,
  sex: "女",
  say() {
    console.log("普通对象");
  },
};
console.log(xh);
```



- **对象接口**

在对象中，对象的属性以及方法的参数与返回值属于对象的结构

参数可有可无，后面添加问号，

属性名称和方法可有可无，后面添加问号

```js
interface 对象接口 {
 	key?:type,
	 method(arg?:type):type
}
```

```js
// 2.定义函数接口
interface fn {
  (str: string, num: number): void;
}
// 2.1通过接口定义函数
function fn1(a: string, b: number) {
  console.log(a, b);
}
fn1("a", 123);

var fn2 = function () {
  console.log(123);
};
var fn3: fn = function () {
  console.log(456);
};

// fn2(123, "abc");  // 应该没有参数 但是获得了两个参数 抛出错误
fn2();
// fn3(123, "abc");  // 定义的a类型是string，b类型是number 抛出错误
fn3("abc", 123);
```



- **类的接口**

类接口与对象接口一样，也是定义类的结构：

在类中，类的属性以及方法的参数与返回值属于类的结构

```js
interface 对象接口 {
     key?:type
     method(arg?:type):type
}
```

注意：类实现接口用`implements`关键字

实例化类的时候，变量的类型

如果是类的类型，可以使用类中的所有属性方法。

如果是接口的类型，只能使用接口中声明的属性和方法

```js
// 3.类 类的接口可以多但是不能少
// 3.1定义接口
interface Person {
  name: string;
  age: number;
  c(): void;
}
// 3.2类 通过 implements 关键字来指定接口
class Xm implements Person {
  name: string;
  age: number;
  abc: string;

  constructor() {
    this.name = "小明";
    this.age = 22;
    this.abc = "abc";
  }

  c(): void {
    console.log("c 方法");
  }

  fn2(): void {
    console.log("fn2 新的方法");
  }
}

var xg = new Xm();
console.log(xg);

xg.fn2()
console.log(xg.name);
```













