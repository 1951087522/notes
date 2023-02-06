## **1.对象**

- 对象（object）：JavaScript里的一种数据类型

- 是一种**无序**的数据集合

- 用来详细的描述某个事物

- - 静态特征：可以使用数字/字符串/数组/布尔类型等表示
  - 动态行为：使用函数表示

### **1.对象使用**

#### **1.1 对象声明语法**

```javascript
let 对象名 = {
    //属性都是成对出现的，包括属性名和属性值，之间用:分隔
    属性名：属性值,        //多个属性之间用，分隔
    方法名：函数
}

//示例
let person = {
    uname = 'andy',
    age = 18,
    sex = '男'
}
```

#### **1.2 对象由属性和方法组成**

- 属性：信息或叫特征（名词）
- 方法：功能或叫行为（动词）

#### **1.3 属性**

- 数据描述性的信息称之为属性，如人的姓名，身高，年龄，性别等。

- - 属性都是成对出现的，包括属性名和属性值，它们之间用英文：分隔
  - 多个属性之间使用英文，分隔
  - 属性就是依附在对象上的变量，（外面是变量，对象内是属性）
  - 属性名可以使用""或‘’，一般情况下，除非名称遇到特殊符号如空格/中横线等

#### **1.4 属性访问**

- 声明对象，并添加了若干属性后，可以属于 . 或  [] 获得对象中属性对应的值
- 简单理解就是获得对象里的属性值

方式一：**对象名.属性**

```javascript
let person = {
    name: 'andy',
    age: 18,
    sex: '男'
}
//访问属性    对象名.属性
console.log(对象名.属性)

console.log(person.name)
```

方式二：对象名['属性名']

```javascript
let person = {
    name: 'andy',
    age: 18,
    sex: '男'
}
//访问属性    对象名['属性名']
console.log(对象名['属性'])

console.log(person['name'])
```

#### **1.5 对象中的方法**

- 数据行为性的信息称为方法，跑步，唱歌等，一般是动词性的，其本质是函数

```javascript
let person = {
    name: 'andy',
    sayHi: function() {
        document.write('hi')    
    }
}
```

- 方法是由方法名和函数两部分构成，它们之间使用：分隔
- 多个属性之间用，分隔
- 方法是依附在对象中的函数
- 方法名可以使用“”或‘’，一般情况下省略，除非名称遇到特殊符号如空格，中横线等

```javascript
let person = {
    name: 'andy',
    age: 18,
    sex: '男',
    //方法名：function () {}
    sayHi: function () {
        console.log('hi')    
    }
}
```

#### **1.6 对象中的方法访问**

- 声明对象，并添加了若干方法后，可以使用，调用对象中的函数

```javascript
let person = {
    name: 'andy',
    age: 18,
    sex: '男',
    //方法名：function () {}
    sayHi: function () {
        console.log('hi')    
    }
}

// 对象名.方法名()
person.sayHi()
```

### **2.操作对象**

- 对象本质就是 无序的数据集合，操作数据就是 增 删 改 查 语法：

![image-20221218154743869](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218154743869.png)

- - 改：

```javascript
let obj = {
    uname: '小明',
    age: 18
}

//修改    对象.属性 = 新值
// 或者    对象['属性'] = 新值
obj.age = 20
obj['age'] = 20
console.log(age)    //20
```

- - 增：

```javascript
//增加
//会去对象里面找是否有sex这个属性，如果有则更新值修改，如果没有，则新增这个属性

//对象.属性 = '属性值'
//或者对象['属性'] = '属性值'

obj.sex = '男'
obj['sex'] = '男'
obj.sing = function () {}
```

- - 删：

```javascript
//delete 对象名.属性
//或者 delete 对象名['属性']
delete obj.uname
```

**.运算符 和 [] 运算符的区别：**

- **.运算符后面只能写属性名称**
- **[] 既可以写属性名称 也可以写变量**

```javascript
        var xiaoming = {
            name: '小明',
            height: 182,
            weight: 120,
            age: 12
        };
        
        xiaoming.attr = '一个属性名为attr的属性';
        
        // 变量
        var attr = 'name';
        attr = 'height';
        attr = 'weight';
        // [] 里面使用变量
        console.log(xiaoming[attr]);    //120
        // [] 里面使用属性名称
        console.log(xiaoming['attr']);    //一个属性名为attr的属性

        // .运算符后面只能使用属性名称
        console.log(xiaoming.attr);    //一个属性名为attr的属性
        // .运算符后面不能使用引号
        // console.log(xiaoming.'attr');
```

### **3.遍历对象**

- 对象没有length属性
- 对象使用for in循环遍历

```javascript
        var obj = {
            a: 123,
            b: 'abc',
            c: true
        }
        
         for (var i in obj) {
            console.log(i, obj[i]);
        }
```

in 运算符：

- 判断对象中是否有指定属性
- 如果有指定属性 返回true；否则返回false

```javascript
console.log( "a" in obj ); // true
console.log( "d" in obj ); // false
```

数组既可以使用for循环遍历 **也可以使用for in循环遍历**

```javascript
for (var i=0; i<arr.length; i++) {
    console.log(arr[i]);
}
---
for (var j in arr) {
    console.log(j, arr[j]);
}
```

### **4.类数组对象**

- **定义：**类数组对象 具有部分数组特征的对象（有下标 和 length 属性）

- - 类数组不是数组，不能使用数组方法。
  - 类数组对象无法通过修改length属性来修改对象中的数据总数。
  - 函数中的 arguments 对象就是内置的类数组对象。

```javascript
function fn1() {
    console.log(arguments[2]);
    console.log(arguments.length)

    // 类数组对象没有数组方法
    // arguments.pop()
}
fn1(1, 2, 3, 4, 5);
```

- **自定义类数组对象：** 通过模拟数组的特征，可以实现自定义的类数组对象。

```javascript
        // 定义自定义数组对象
        // 通过模拟数组的特征，可以创建自定义的类数组对象
        var obj = {
            // 对象的属性名称也可以是数字，但需要使用引号包含。
            // 数字必须像数组的下标一样，是从0开始的有序数字
            '0': 10,
            '1': 20,
            '2': 30,
            '3': 40,
            // 通过 length 属性指定数据的总数
            length: 4
        };

        // 类数组对象可以像数组对象一样使用 for 循环
        for (var i = 0; i < length; i++) {
            console.log(i, obj[i]);
        }

        console.log(obj);
```

## **2.this 关键字**

- this 是 JavaScript 中的一个特殊的内置对象。

- - **调用当前对象里的对象，不会对外面产生影响**
  - **直接调用，显示window**
  - **函数调用，显示函数的值**

- 在浏览器端，全局作用域中的 this 指向 window 对象。

```javascript
// 全局作用域中的 this 指向 window 对象
// window 是浏览器中内置的对象，属于 BOM。
console.log( this );
```

- 函数中的 this 值并不是固定的，当函数被调用时才会确定 this 的值。
- 调用方式不同，this 指向的值也不同。

```javascript
// 直接加 () 调用，this 指向 window 对象
fn1();
// 通过定时器调用，this 指向 window 对象
setTimeout(fn1, 1000);
// IIFE 函数中的 this 也指向 window 对象
(function() {
 console.log(this);
})();

// 通过触发事件调用， this 指向触发事件的对象 （这里指向document对象）
// 事件是后续课程中的内容
document.onclick = fn1;
var obj = {
 name: "小明",
 age: 18,
 say: fn1
};

// 如果函数属于对象的方法， this 指向这个对象
obj.say();
```

- this 通常在函数内部使用。

```javascript
// this 通常在函数内部使用
function fn1() {
 // 函数中的this 指向谁，要看函数是通过什么方式调用的
 // 也就是说，函数中的this的值是在函数调用的时候确定的
 console.log(this);
}
```

**通常情况下：**

1. 当函数是通过使用 () 直接调用时， this 指向 window 对象。
2. IIFE 函数中的 this 指向 window 对象。
3. 当函数属于某个对象的方法时，this 指向这个对象。

```javascript
// this 使用场景
var person = {
 name: "小明",
 age:18,
 say: function() {
   // 直接使用对象的变量名称
   // console.log(person.name + ', ' + person.age);
   // 使用 this 对象
   console.log(this.name + ', ' + this.age);
 }
}
person.say();

var p1 = person;
// person 指向对象时，可以正常执行 say 方法。
p1.say();
// 修改 person 的值， person 不再存的是对象，而是字符串。
person = "小明";
// 再使用 say() 方法，如果是通过对象获取属性值，将获取不到正确的值。
// 如果通过 this 对象获取属性值，不受影响。 
p1.say();
```

## **3.JSON**

JSON (JavaScript Object Notation, JS 对象表示法)

JSON 是轻量级的文本数据交换格式，是具有特定格式的字符串。

JSON 作用是方便进行前后端数据交互。

**说明：**

1. JSON格式与对象类似，但本质上是一个字符串值。
2. JSON数据的**所有属性名称必须使用双引号包裹，字符串值也必须使用双引号包裹**
3. JSON格式文件一般是由后端返回，或者前端通过 ajax 请求获取。

```javascript
    var json = {
        "name": "小明",
        "age": 18,
        "isStudent": true
    };
```

### **3.1 -数组、对象转换成JSON字符串**

JSON.stringify(obj)

- 将数组或对象转换成JSON字符串
- **参数:** obj 表示要转换的对象或数组。
- JSON.stringify() 应该只用来转换标准的数组或对象，但实际可以转换所有JS中支持的数据类型。

**所有数据类型在转换时的规则如下 ：**

1. null, NaN, 正负Infinity 值会转换成 "null"
2. 函数，undefined，symbol值 在对象中时转换后被忽略，在数组中时转换为 "null"
3. 单独的 函数，symbol值 和 undefined 会转换成 "undefined"
4. **布尔值，数字，字符串按类型转换规则转换成字符串。**
5. **对象转换成字符串之后，数据的顺序是不固定的。**

```javascript
var arr = [1,2,3,4];
var obj = {name: "小明", age: 18};

// 将数组转换成JSON字符串
console.log( JSON.stringify(arr) );

// 将对象转换成JSON字符串
console.log( JSON.stringify(obj) );

// 普通值转换成JSON字符串： 除了数组 和 对象，其它类型的值，转换之后就是一个普通字符串。
// JSON.stringify() 一般用来将数组或对象转成json字符串，其他类型数据转成字符串没有实际意义。
console.log( JSON.stringify(1) );
console.log( JSON.stringify(true) );

// null, NaN, 正负Infinity 值会转换成 "null"
console.log( JSON.stringify( [null, NaN, +Infinity, -Infinity] ) ); // '[null,null,null,null]'
console.log( JSON.stringify( {a: null, b: NaN, c: +Infinity, d: -Infinity} ) ); // '{"a":null,"b":null,"c":null,"d":null}'

// 单独的 函数，symbol值 和 undefined 会转换成 "undefined"
// symbol 是 ES6 新增的数据类型，后面课程再介绍
console.log( JSON.stringify(function() {}) ); // "undefined"
console.log(JSON.stringify(Symbol()));    // "undefined"
console.log( JSON.stringify(undefined) ); // "undefined"

// 数组转换成字符串，数据的顺序跟之前的数组一致
console.log( JSON.stringify([1,2,3,4]) ); // '[1,2,3,4]'

// 对象转成字符串， 数据的顺序是不固定的，与原来的可能不一样
console.log( JSON.stringify( {a: 123, "0": 'abc', "1": true} ) ); // '{"0":"abc","1":true,"a":123}'

// 函数，undefined，symbol值 在对象中时转换后被忽略  
var obj2 = {
    a: 123,
    b: "abc",
    c: function() {},
    d: undefined,
    e: true
};
console.log( JSON.stringify(obj2) ); // '{"a":123,"b":"abc","e":true}'

// 函数，undefined，symbol值 在数组中时转换为 "null" 
var arr2 = [10, function(){ console.log(111)}, undefined, 'hello'];
console.log( JSON.stringify(arr2) ); // '[10,null,null,"hello"]'
```

### **3.2- JSON字符串转换成数组或对象**

JSON.parse(string)

- 将一个JSON字符串转换成对象或数组，转换结果是对象还是数组，取决于字符串的格式

**// 为了避免出错，JSON.parse() 方法应该只用来将对象或数组格式的字符串。**

**注意：**

1. JSON字符串的外层使用单引号，里面的属性名称和属性值使用双引号。
2. 对象格式的属性名称必须有双引号。
3. 对象或数组格式的最后一个值后面不允许有逗号。
4. 非对象或数组格式的字符串值无法转换成对象或数组，甚至可能会直接报错。

```javascript
var json = '{"a": 123, "b": "abc"}';
var arr = '[1,20,"abc", true, "false"]';

// 如果字符串是对象格式，会被转换成对象
console.log( JSON.parse(json) ); // {a: 123, b: 'abc'}

// 如果字符串是数组格式，会被转换成数组
console.log( JSON.parse(arr) ); // [1, 20, 'abc', true, 'false']

// 这样写会报错，因为JSON格式字符串要求对象的属性名称必须使用双引号
console.log( JSON.parse( '{a: 123, b: true}') );

// 这样也会报错，因为JSON格式字符串中，数组或对象的最后一项后面不允许有逗号
console.log( JSON.parse( '{"a": 123, "b": 100,}' ) );
console.log( JSON.parse( '[1,2,3,4,]' ) );

// 数字使用 JSON.parse() 返回的还是123
console.log( JSON.parse(123) );

// 会报错，无法正常转换
// var str4 = '{"a": Infinity, "b": -Infinity, "c": undefined, "d": Symbol()}'
// console.log(JSON.parse(str4))

// 数组或对象字符串中的值不允许为函数，会报错
console.log( JSON.parse( '[1,2,function(){}, 10,20]' ) ); 
console.log( JSON.parse( '{"a": 100, "b":"hello", "f": function(){}}' ) ); 

// 为了避免出错，JSON.parse() 方法应该只用来将对象或数组格式的字符串。

```

### **3.3- eval函数**

- eval() 是 JS 内置的一个全局函数，**用来将一段字符串值解析成 JS 语句。**
- eval() 函数在**直接调用**和**间接调用**时的作用域环境是不同的。

- 直接使用eval函数	**会在当前作用域** 	解析字符串中的变量或函数
- 间接使用eval函数	所有的变量或函数都**会在全局作用域中声明**

```javascript
        // eval()   是内置的全局函数    作用是将普通字符串解析成js语句

        var str1 = 'var a = 100';
        console.log(str1);
        // console.log(a);  //访问不到的

        // 通过eval 将这个字符串转换成语句/表达式
        eval(str1);
        console.log(a);

        // eval()   的返回值由被解析字符串最终解析成的js表达式决定
        console.log(eval(str1));//undefined
        console.log(eval('1+2'));//3
------------------------------------------------------------
        /* 
            eval 函数的两种使用场景
                1.直接使用  直接调用eval()函数
                2.间接使用  通过表示eval()函数的变量或通过window对象调用
        */

        //直接使用eval函数
        // 会在当前作用域 解析字符串中的变量或函数
        //全局作用域下的
        eval("var num1 = 200; function fn2() { console.log('fn2')}");
        console.log(num1);

        //局部作用域下的
        function fn1() {
            eval("var num2 = 500; function fn3() { console.log('fn3')}");
            console.log(num2);
            fn3();
        }
        fn1();

        //全局作用域下的    可以访问
        // console.log(num1);
        //局部作用下的  无法访问
        // console.log(num2);
        //全局作用域下的    可以访问
        // fn2();
        //局部作用下的  无法访问
        // fn3();   

        //间接使用eval函数
        //间接使用时,所有的变量或函数都会在全局作用域中声明
        function fn4() {
            var f = eval;
            f("var num3 = 1000;function fn5() { console.log('fn5')}");
            // window.eval("var num3 = 1000;function fn5() { console.log('fn5')}");

            console.log(num3);
            fn5()
        };
        fn4()
        // 全局访问
        console.log(num3);
        fn5()
```

### **4.代码规范**

```javascript
// 1，使用 2个 或 4 个空格做为一个缩进层级
// 所有的缩进统一使用空格代替制表符
// vscode 的设置中可以设置默认缩进大小

// 2，switch 下的 case 和 default 增加一个缩进层级
    switch (true) {
        case 1:
            break;
        case 2:
            break;
        default:
            break;
    }

// 3，二元运算符两侧有一个空格，一元运算符与操作对象之间没有空格。
    12 + 34
    4 % 5
    +10
    !true
    ~12
    var a = 1;
    a ++;
    ++a

// 4，代码块起始的左花括号 { 前有一个空格。
  for (;;) {}
  function fn1() {}
  if (true) {}

// 5，if / else / for / while / function / switch / do / try / catch / finally 关键字后有一个空格。
  if (true) {} else {}
  for (;;) {}
  while (1) {}

// 6，在对象创建时，属性中的 : 之后有一个空格，: 之前没有空格。
  var obj = {
    a: 123,
    b: true
  };

// 7，函数声明、匿名函数表达式、函数调用中，函数名和 () 之间没有空格。
  function fn1() {}
  var fn2 = function() {};
  fn1();

// 8，语句以分号结束，分号前没有空格。
  console.log(123);
  var abc = 1234;
  var fn2 = function() {};

// 9，在函数调用、函数声明、括号表达式、属性访问、if / for / while / switch / catch 等语句中，单行声明的数组与对象，如果包含元素，() 和 [] 内紧贴括号部分没有空格。
  function fn1(a, b, c) {}
  if (1 > 2) {}
  var arr = [1, 2, 3, 4];
  var obj = {a: 123, b: 'abc', c: true};

// 10，每个独立语句结束后换行。

// 11，函数声明、函数表达式、函数调用、对象创建、数组创建、for语句等场景中的 , 或 ; 前不换行。
// fn1();
/* var obj = {
    a: 123,
    b: "abc"
    ,
}
; */

// 12，不同行为或逻辑的语句集，使用空行隔开，更易阅读。

// 13，在 if / else / for / do / while 语句中，即使只有一行，也不省略大括号 {}。
  if (123) { console.log(456); }
  for (;;) { console.log('abcd'); }

// 14，函数定义结束不添加分号，IIFE 函数表达式外添加 () 。
  function abc() {}
  (function() {})();

// 15，页面中 script 标签与左侧缩进一层，script 标签内部的代码不缩进，与script开始标签左对齐。
```

