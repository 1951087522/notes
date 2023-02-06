## 0. ES6 简介

> - ECMAScript 6.0（以下简称 ES6）是 JavaScript 语言的下一代标准，已经在 2015 年 6 月正式发布了。它的目标，是使得 JavaScript 语言可以用来编写复杂的大型应用程序，成为企业级开发语言
> - ES6 既是一个历史名词，也是一个泛指，含义是 5.1 版以后的 JavaScript 的下一代标准，涵盖了 ES2015、ES2016、ES2017 等等，而 ES2015 则是正式名称，特指该年发布的正式版本的语言标准。本书中提到 ES6 的地方，一般是指 ES2015 标准，但有时也是泛指“下一代 JavaScript 语言”
> - 90%的浏览器都支持ES6，也可以通过 babel 编译器（一个 js 编译器）将 ES5 代码 转为 ES6 代码

**[详细教程 阮一峰-ES6 入门教程](https://es6.ruanyifeng.com/)**

## 1. 关键字

### 1.1 let 关键字

> ES6 新增了一个新的变量声明关键字 let。

var 定义的变量为函数级作用域：

在函数内部定义的变量，外部无法访问，

在代码块（if，for等）中定义的变量，外部可以访问

let 定义的变量为块级作用域：

在函数内部定义的变量，外部无法访问，

在代码块（if，for等）中定义的变量，外部仍然无法访问

**let 与 var 比较**

1. 作用域

   var 函数级作用域

   let 块级作用域

2. 能否重复定义

   var 可以重复定义变量

   let 不可以重复定义变量

3. 声明提升

   var 支持声明提升

   let 不支持声明提升

4. for 循环中的单独作用域

   var 不会产生单独作用域

   let 会产生单独作用域

5. 全局变量是否会成为window对象的属性

   var 声明的变量自动成为window的属性

   let 声明的变量不会自动成为window的属性

```js
    <script>
        // ES6 之前 ECMAScript 以函数区分作用域
        console.log(a);
        var a = 10;
        function fn1() {
            var b = 20
            console.log(a);
            console.log(b);
        }
        fn1()
        console.log(a);
        for (var i = 0; i < 5; i++) {
            console.log(i);
        }
        if (true) {
            var c = 100
        }
        console.log(c);

        // 使用 let 关键字声明的变量，不再进行声明提升
        let d = 100
        console.log(d);
        function fn2() {
            let e = 200
            console.log(e);
        }
        fn2()
        // 函数外部无法访问
        // console.log(e);

        // for 循环中 使用let 关键字声明的变量，会产生自己的作用域
        for (let j = 0; j < 5; j++) {
            console.log(j);
        }
        // for 语句 外部访问不到这个变量
        // 函数外部无法访问
        // console.log(j);

        if (true) {
            let f = 3000
            console.log(f);
        }
        // console.log(f);

        {
            var aa = 100000
            let bb = 200000

            console.log(aa);
            console.log(bb);
        }
        console.log(aa);
        // 无法访问到
        // console.log(bb);

        var abc = 123
        var abc = 456

        let def = 789
        // 变量名不能重复
        // let def = 789

        console.log(window);
        console.log(window.abc);
        console.log(window.def);

        /* 
        let 关键字  ES6 新增的  用来变量声明的关键字
        与 var 关键字的不同
            1.作用域范围不同
                var 函数作用域
                let 块级作用域
            2.是否允许重复定义同名变量
                var 允许
                let 不允许
            3.是否声明提升
                var 声明提升
                let 不会声明提升
            4.for 循环是否产生单独作用域
                var 不会产生单独作用域
                let 会产生单独作用域
            5.声明的全局变量是否会成为 window 属性
                var 会自动成为 window 属性
                let 不会自动成为 window 属性
        */
    </script>
```

### 1.2 const 关键字

> const关键字是用于定义常量（一旦定义无法改变的变量，通常是表示一些固定不变的数据）

使用const关键字的特点：

1 无法被修改

2 支持块作用域

3 无法重复定义

4 无法声明前置

5 不会自动成为window的属性

6 不能作为for循环体中的变量使用

通常是将常量名称用全大写字母表示，并且横线分割单词。

```js
    <script>
        /* 
            const   用来声明常量的关键字
            常量    固定不变的量
                1.const 声明的常量名称  区分大小写
                2.为了保持代码风格统一，建议所有的常量名称大写
                3.常量必须在声明时赋值
                4.如果常量的值是一个复杂类型，这个变量本身不允许修改，但常量的属性或方法 可以修改

            const 与 let 
                共同点
                    1.都不会有声明提升
                    2.都不允许重复声明相同的名称
                    3.都不会自动称为 window 的属性或方法
                不同点
                    1.let 声明变量
                        const 声明常量
                    2.let 可以作为 for 循环的关键字使用
                    const 不能作为 for 循环的关键字
        */
    </script>
```

## 2.字符串拓展

### 2.1 多行字符串	（模板字符串）

单行字符串：由一组单引号或者双引号定义的字符串

单行字符串的问题：

1 单行字符串不能换行

2 一些特殊的按键要使用转义字符 \n

3 一些特殊的字符要使用转义字符 \x20

4 字符串标志符号不能直接嵌套

- 单引号中不能直接写单引号，要转义 \’
- 双引号中不能直接写双引号，要转义 \”

 ......

ES6为了解决单行字符串中的问题，提供了多行字符串，

通过 ` 定义``，在多行字符串中，只有需要转义 `，其他的字符，都可以直接书写

并且ES6多行字符串支持插值语法 ：${key}

`${}`提供了js环境，可以写js表达式

```js
    <script>
        // es6 之前，字符串不允许直接换行，需要使用 \ 进行转义
        var str1 = '123abc\
        def'

        // es6 新增 多行字符串(模板字符串)
        var str2 = `
        123
        abc`

        console.log(str2);

        // 多行字符串可以使用${}    引用变量 称为插值语法
        var title = '前端开发'
        var content = '努力就会有回报'
        var html = `
        <h2>${title}</h2>
        <p>${content}</p>
        `
        console.log(html);

        // 插值语法可以进行运算
        console.log(`${1 + 2}`);
        console.log(`${false}?'hello':'world'`);
    </script>
```

### 2.2 原始字符串

在使用了转义字符之后，并且在浏览器查看的时候，我们只能看到结果，不能看到原始完整的字符串（包含转义字符），于是ES6中拓展了String.raw方法用于，查看完整的原始字符串

> 使用方式： String.raw``

参数通过多行字符串的形式传递，字符串中的插值会被解析，但转义字符不会被转义。

```js
    <script>
        var str = `
        哈喽~ \n\t123
        ${123 + 123}
        `
        console.log(str);

        // 原始字符串   String.raw
        // 查看字符串转义之前的内容
        console.log(String.raw `
        哈喽~ \n\t123
        ${123 + 123}
        `);
    </script>
```

### 2.3 重复字符串

ES6中拓展了`repeat()`方法用于重复输出字符串

参数就是要重复的次数

返回值就是重复的结果

```js
    <script>
        var str = 'hello'
        console.log(str.repeat(1));
        console.log(str.repeat(2));
        console.log(str.repeat(3));
        console.log(str.repeat(0));
        console.log(str.repeat());

        /* 
            repeat()    重复字符串(将字符串复制几份)
                参数    表示要重复的次数
                设置0或不设置参数 默认返回空字符串
        */
    </script>
```

### 2.4 判断字符串位置

**`startsWith(str, pos)`** 是否以参数字符串开头

截取后面的部分，并且包含截取位置字符

str 参数字符串（子字符串）

pos 字符串截取位置

返回值都是布尔值

**`endsWith(str, pos)`** 是否以参数字符串结尾

截取前面的部分，并且不包含截取位置字符

**`includes(str, pos)`** 是否包含参数字符串

截取后面的部分，并且包含截取位置字符

```js
    <script>
        var str = '路漫漫其修远兮'
        console.log(str);

        // startsWith()     判断字符串是否以指定字符串开头
        console.log(str.startsWith('北京'));
        console.log(str.startsWith('路'));
        // 第二个参数   忽略指定位置之前的字符
        console.log(str.startsWith('修', 4));

        // endsWith()       判断字符串是否以指定字符串结尾
        console.log(str.endsWith('兮'));
        console.log(str.endsWith('天'));
        // 第二个参数   忽略指定位置之后的字符
        console.log(str.endsWith('漫', 2));

        // includes()       判断字符串是否包含指定的子字符串
        console.log(str.includes('漫'));
        // 第二个参数       忽略指定位置之前的字符
        console.log(str.includes('漫', 2));
    </script>
```

## 3.数字拓展

### 3.1 `isNaN`

ES6中为数字拓展了几个方法： isNaN、isFinite、 isInteger

全局中有一个isNaN方法，是用于判断是否是NaN(not a Number)

全局中isNaN在判断的时候，会进行类型转换

而Number拓展的isNaN，在判断的时候不会做类型转换

首先必须是数字

其次才去判断是否是NaN

如果是NaN，则返回true

如果不是NaN，则返回false

```js
    <script>
        var str1 = '1234'
        var num1 = 1000
        var str2 = 'abc'
        var num2 = NaN

        // 全局函数 isNaN()
        console.log(isNaN(str1));   // false
        console.log(isNaN(str2));   // true
        console.log(isNaN(num1));   // false
        console.log(isNaN(num2));   // true

        // 构造函数的静态方法   Number.isNaN()
        console.log(Number.isNaN(str1));    // false
        console.log(Number.isNaN(str2));    // false
        console.log(Number.isNaN(num1));    // false
        console.log(Number.isNaN(num2));    // true

        /* 
            全局函数    isNaN()
                先检测要判断的值，如果不是数字类型，先尝试转换成数字类型，可以转换成数字，返回false；得到 NaN 返回 true
                如果是数字类型，判断是不是 NaN，是返回 true
                                    不是，返回 false
            构造函数    Number.isNaN()
                先检测要判断的值，不是数字类型，直接返回 false
                是数字类型，判断是不是 NaN
                            是，返回 true
                            不是，返回 false
        */
    </script>
```

### 3.2 `isFinite`

全局中有一个isFinite方法，是用于判断是否是有限的

全局中isFinite在判断的时候，会进行类型转换

而Number拓展的isFinite，在判断的时候不会做类型转换

首先必须是数字

其次才去判断是否是有限的

如果是有限的，则返回true

如果不是有限的，则返回false

```js
        var a = 10;
        var b = '123';
        var c = 'abc';
        var d = Infinity;
        var e = -Infinity;
        var f = NaN;

        // 全局函数 isFinite()  判断是否是有效数字
        console.log(isFinite(a));
        console.log(isFinite(b));
        console.log(isFinite(c));
        console.log(isFinite(d));
        console.log(isFinite(e));
        console.log(isFinite(f));

        // 构造函数的静态方法   Number.isFinite()
        console.log(Number.isFinite(a));
        console.log(Number.isFinite(b));
        console.log(Number.isFinite(c));
        console.log(Number.isFinite(d));
        console.log(Number.isFinite(e));
        console.log(Number.isFinite(f));

        /* 
            isFinite()  判断一个值是不是有效数字(有效数字：能够准确描述数量的数字)

            全局函数 isFinite()
                先检测要判断的值 不是数字类型   先尝试转换成数字类型
                    可以转换 返回 true
                    转换结果为 NaN Infinity -Infinity 返回 false
                    如果是 数字类型 值为 NaN Infinity -Infinity 返回 false
                        否则返回 true
            构造函数的静态方法  Number.isFinite()
                先检测要判断的值    不是数字类型    直接返回 false
                是数字类型  值为 NaN Infinity -Infinity 返回 false
                    否则返回 true
        */
```

### 3.3 `isInteger`

Number拓展的isInteger方法，用于判断是否是整型的

在判断的过程中，会校验类型

首先必须是数字

其次才去判断是否是整型的

如果是整型，则返回true

如果不是整型，则返回false

```js
        console.log(Number.isInteger('10'));
        console.log(Number.isInteger(10));
        console.log(Number.isInteger(10.1));
        console.log(Number.isInteger(0));
        console.log(Number.isInteger(-0));
        console.log(Number.isInteger(NaN));
        console.log(Number.isInteger(Infinity));
        console.log(Number.isInteger(-Infinity));

        /* 
            isInteger()     判断一个值是不是整数
                1.要判断的值 如果不是数字类型   直接返回false
                2.如果值为整数 返回true 不是返回 false
                3.正负0 也算是整数
        */
```

### 3.4 数学对象拓展

就是对Math对象的拓展，

ES6为了适应大型项目，解决自身运算的问题，拓展了大量的方法。

> Math.cbrt：计算一个数的立方根。
>
> Math.fround：返回一个数的单精度浮点数形式。
>
> Math.hypot：返回所有参数的平方和的平方根。

自然对数相关方法：

（对数是幂运算的逆运算，就像除法是乘法的逆运算一样。自然对象是以常数E为底数的对数；常数E的近似值为2.718，可以通过 Math.E 获取）

Math.expm1(x)：返回 E^X - 1 的值（常数E的x次方减去1的值），等同于 `Math.exp(x) - 1`。

Math.log1p(x)：返回1 + x的自然对数。如果x小于-1，返回NaN。

Math.log10(x)：返回以10为底的x的对数。如果x小于0，则返回NaN。

Math.log2(x)：返回以2为底的x的对数。如果x小于0，则返回NaN。

**三角函数方法**

Math.sinh(x) 返回x的双曲正弦（hyperbolic sine）

Math.cosh(x) 返回x的双曲余弦（hyperbolic cosine）

Math.tanh(x) 返回x的双曲正切（hyperbolic tangent）

Math.asinh(x) 返回x的反双曲正弦（inverse hyperbolic sine）

Math.acosh(x) 返回x的反双曲余弦（inverse hyperbolic cosine）

Math.atanh(x) 返回x的反双曲正切（inverse hyperbolic tangent）

Math.sign(x) 判断 x 是正数、负数还是零， 传入该函数的参数会被**隐式转换**成数字类型。

一共有五个可能的值： **1, -1, 0, -0, NaN.**

(0, Infinity] =>1，

[-Infinity, 0) => -1，

0 => 0，

0 => -0，

NaN => NaN

```js
    <script>
        // 数学对象扩展 让 Math 对象可以支持更多的数学公式
        // Math.cbrt()  求一个值得立方根
        console.log(Math.cbrt(8));

        // 返回一个小数的单精度值
        console.log(Math.fround(10));
        console.log(Math.fround(0.5));
        console.log(Math.fround(0.01));

        // 求任意多个数字的平分相加的和的平方根
        console.log(Math.hypot(2, 3));
        // 相当于
        console.log(Math.sqrt(2 * 2 + 3 * 3));

        // 常数 E
        console.log(Math.E);
    </script>
```

## 4.数组拓展

### 4.1 Array.of()

> `Array.of()` 是Array()构造函数的一个静态方法，用于创建数组。

通过new Array() 或者是 Array() 创建数组：

1 如果没有传递参数，得到的是一个空数组

2 如果传递了一个数字参数，得到的是带有一个长度的空数组

3 如果传递一个非数字参数，得到的是带有一个成员的数组

4 如果传递了多个参数，得到就是一个带有多个参数的数组

ES6中 拓展的 Array.of() 方法与 new Array() 的区别在于，当只有一个参数，并且参数为number类型时，new Array() 是将参数作为 length 值，而 Array.of() 是将参数仍然作为值。

```js
    <script>
        // 字面量方式传教数组
        var arr1 = [1, 2, 3, 4]
        console.log(arr1);
        // 构造函数方式创建数组
        var arr2 = new Array(1, 2, 3, 4)
        console.log(arr2);

        // 缺点：构造函数方式创建，参数表示的含义不统一
        var arr3 = new Array(3)
        console.log(arr3);

        // Array.of()   修复构造函数的缺陷，让参数指标是要存入新数组的值
        var arr4 = Array.of(3)
        console.log(arr4);
    </script>
```

### 4.2 Array.from()

> 用于遍历类数组对象，或将类数组对象转换成数组，是数组的静态方法。

类数组对象：

可以通过索引值获取属性值，

并且要具备length属性的一类对象

类数组对象不能使用数组的遍历器方法，ES6中拓展的from方法可以将类数组对象转为真正的数组，之后就可以使用数组的常用方法

使用方式： `Array.from(arrLike, fn)`

arrLike: 类数组对象

fn: 执行的函数，

有两个参数：成员值、索引值。

this默认指向window

如果传递的fn参数，此时，fn方法的返回值是from函数的执行结果

总结：from方法不仅可以将类数组转为数组，还可以遍历类数组对象

```js
    <script>
        var lis = document.querySelectorAll('li')
        console.log(lis);

        // 遍历类数组对象
        lis.forEach(function (val, index) {
            console.log(val, index);
        })

        // lis 是一个类数组对象，不能使用数组方法
        // console.log(lis.slice(1));

        // 1.Array.from() 将类数组对象转换成标准数组
        var arrLi = Array.from(lis)
        console.log(arrLi);
        console.log(arrLi.slice(1));

        // 2.如果传递第二个参数，第二个参数为函数，表示将第一个参数的类数组对象转换成标准数组，然后使用 map() 方法处理这个标准数组
        var result = Array.from(lis, function (val, index, arr) {
            console.log(val, index, arr);
            // return 1000
            return val.innerHTML + 'hello';
        })
        console.log(result);

        var arr = [1, 2, 3, 4, 5]
        var res = arr.map(function () {
            return 1000
        })
        console.log(res);
    </script>
```

### 4.3.查找数组

> 在ES5中拓展了查找成员的方法：indexOf、lastIndexOf
>
> 在ES6中拓展了查找成员的方式：`find()`、`findIndex()`

参数就是执行的函数

函数中有三个参数：成员值、索引值、原数组。

this默认指向window

- **find方法在查找成员的时候，如果找到了则返回该成员，如果没有找到则返回undefined**
- **findIndex方法在查找成员的时候，如果找到了则返回该成员的索引值，如果没有找到返回-1**

在查找的过程中，一旦找到则停止遍历

```js
    <script>
        var arr = [123, 23, 12, 18, 56];
        console.log(arr.indexOf(3));

        // 1.find()     查找数组中有没有满足指定条件的值
        // 有 返回第一个满足条件的值
        // 没有 返回 undefined
        var result = arr.find(function (val, index, arr) {
            console.log(val, index, arr);

            console.log(this);

            // 从数组中查找等于 3 的值
            /* if (val === 3) {
                return true
            } */

            /* if (val > 10) {
                return true
            } */

            if (val > 10 && val < 20) {
                return true
            }

            // 更改 this
        }, document)
        console.log(result);

        // 2.findIndex()    查找数组中有没有满足指定条件的值
        // 有 返回第一个满足条件值得下标
        // 没有 返回 -1
        var res = arr.findIndex(function (val, index, arr) {
            console.log(val, index, arr);

            console.log(this);

            /* if (val === 3) {
                return true
            } */

            /* if (val > 10) {
                return true
            } */

            if (val > 10 && val < 20) {
                return true;
            }
        }, document)
        console.log(res);

        /* 
            find()      查找数组中有没有满足指定条件的值
                有      返回 第一个满足条件的值
                没有    返回 undefined
                第一个参数  用来查找的回调函数
                第二个参数  更改第一个参数 this 的值

            findIndex() 查找数组中有没有满足指定条件的值
                有      返回 第一个满足条件值的下标
                没有    返回 -1
                第一个参数  用来查找的回调函数
                第二个参数  更改第一个参数中的 this 值
        */
    </script>
```

### 4.4.数组内部复制

> ES6为了实现数组内部复制成员提供了一个方法： `copyWithin()`

使用方式：arr.copyWithin(pos, start, end)

pos: 要粘贴的位置

start: 要复制的起始位置（包含起始位置）

end: 要复制的结束位置（不包含结束位置）

返回值就是原数组，并且原数组发生变化

例如： [0, 1, 2, 3, 4, 5, 6, 7, 8 ,9]. copyWithin(3, 6, 9)

结果： [0, 1, 2, 6, 7, 8, 6, 7, 8 9]

```js
    <script>
        var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9, 0];

        // copyWithin() 从数组中复制指定范围的值,并覆盖指定范围的值
        // 第一个参数   指定开始黏贴覆盖的位置
        // 第二个参数   指定复制的开始位置
        // 第三个参数   指定复制的结束位置
        // var result = arr.copyWithin(5, 0, 5)
        // var result = arr.copyWithin(8, 0, 5)
        // var result = arr.copyWithin(8)
        var result = arr.copyWithin(1)

        console.log(arr);
        console.log(result);

        /* 
            copyWithin()    从数组中复制指定范围的值,并覆盖指定范围的值
                第一个参数  指定开始黏贴覆盖的位置
                第二个参数  指定复制的开始位置
                第三个参数  指定复制的结束位置

            1.复制的范围包括开始位置,不包括结束位置
            2.不指定参数,数组不会有任何变化
            3.只指定一个参数,复制整个数组,并从指定位置开始黏贴
            4.指定两个参数,从第二个参数指定位置开始,复制到数组最后一个值,然后黏贴到指定位置
            5.数组的 length 不会发生变化,黏贴的值超出数组的 length ,超出部分会忽略

            返回值:进行复制黏贴之后的数组
        
        */
    </script>
```

### 4.5.数组迭代器

> ES6中为了遍历数组中成员，拓展了三个迭代器方法： keys、values、entries

`keys`： 获取索引值

`values`: 获取成员值

`entries`: 获取索引值以及成员值：[index，item]

- **由于实现了数组的迭代器接口方法，就可以使用for of 或者是next方法遍历**

实现了迭代器接口的数据，都有next方法，可以通过next方法来遍历成员。

返回值是一个对象

value: 表示成员值

done: 表示是否遍历完成

如果遍历完成了，此时： done将永远是true value将永远是undefined

```js
    <script>
        var arr = [10, 20, 30, 40, undefined, 50];

        var keys = arr.keys()
        console.log(keys);

        // 查看迭代对象的第一个值
        var val1 = keys.next()
        console.log(val1);
        console.log(val1.value);

        // 查看迭代对象剩下的值
        console.log(keys.next().value);
        console.log(keys.next().value);
        console.log(keys.next().value);
        console.log(keys.next().value);
        console.log(keys.next().value);
        // 当返回结果 value 值为 undefined 并且 done 属性状态为 true 时,表示迭代对象的值已经取完,里面没有可用的值
        console.log(keys.next().value);
        console.log(keys.next().value);

        var values = arr.values();
        console.log(values.next());
        console.log(values.next());
        console.log(values.next());
        console.log(values.next());
        console.log(values.next());
        console.log(values.next());
        /* 当迭代对象的所有值被取出以后,无论调用 next() 几次,都只会返回
        {value: undefined, done: true} */
        console.log(values.next());
        console.log(values.next());

        // 将数组的值和 value 都转换成迭代对象 (迭代对象的一个值,包含数组的一个索引和值)
        var entries = arr.entries()
        console.log(entries.next());
        console.log(entries.next());
        console.log(entries.next());
        console.log(entries.next());
        console.log(entries.next());
        console.log(entries.next());
        console.log(entries.next());

        /* 
            迭代对象:    ES6 新增一直特殊格式的对象
                1.迭代对象的值无法直接查看
                2.迭代对象的值只能通过 next() 方法获取
                3.迭代对象的每一个值只能获取一次
                4.迭代对象的值是一个固定格式的对象,这个对象有且只有两个属性
                    value:表示当前的值(迭代对象中的每一个值)
                    done:表示当前的状态(取出当前值时,迭代对象中有没有剩余值)
                5.如果 next() 方法的返回结果为 {value:undefined,done:true}
                    表示迭代对象中已经没有值
        */
    </script>
```

## 5.循环

### 5.1.for...of 循环

> `for of` 循环是ES6专门为实现了迭代器接口的对象设计的循环结构。

for of是专门为迭代器接口设置的遍历方法。语法： for (let item of data) {}

可以像其它循环一样在内部使用continue、break等关键字

for of也是可以遍历数组的，但是在遍历过程中，无法使用索引值

遍历数组的时候，item表示数组的每一个成员，没有办法访问索引值，但是我们可以在外部定义一个循环变量，在循环体中手动更新。

for of循环遍历数组的时候，不需要通过索引值访问成员，而for循环以及for in循环要通过索引值访问

for in也可以遍历数组，但是有一些问题：遍历的时候，key显示的是字符串，不是数字

总结：

for循环用于遍历数组，

for in循环用于遍历对象，

for of循环遍历实现了迭代器接口的对象（包括数组）

```js
    <script>
        var arr = [10, 20, 30, 40, undefined, 50];

        for (var i = 0; i < arr.length; i++) {
            console.log(i, arr[i]);
        }
        for (var i in arr) {
            console.log(i, arr[i]);
        }

        var values = arr.values()
        // 迭代对象是一种特殊格式的对象,无法使用 for 和 for...in 循环语句
        console.log(values);
        for (var i = 0; i < arr.length; i++) {
            console.log(i, values[i]);
        }
        for (var i in values) {
            console.log(i, values[i]);
        }

        // 1.迭代对象只能使用 for...of 语句遍历
        for (var j of values) {
            //  for...of 语句 会直接获取到 迭代对象的值
            console.log(j);

            // 迭代对象只有值 没有索引 不存在values[j] 的取值方式
            // console.log(values[j]);
        }

        // 迭代对象的每一个值只能被获取一次，所有一个迭代对象最多只能使用一次for..of  循环
        for (var j of values) {
            console.log(j);
        }
        console.log(values.next());

        // for...of 语句只会遍历没有被 next() 取出过的值
    </script>
```

```js
    <script>
        var arr = [1, 2, 3, 4, 5];

        // 使用 for...of 语句遍历数组 只能拿到数组的值，不能拿到数组的索引
        for (var val of arr) {
            console.log(val);
        }

        // 但是可以使用 if 语句
        for (var val of arr) {
            if (val > 2) {
                console.log(val);
            }
        }
        // 也可以使用 跳出与跳过
        for (var val of arr) {
            if (val === 3) {
                // continue
                break
            }
            console.log(val);
        }

        // 对象不能使用 for...of 循环
        var obj = {
            a: 100,
            b: 200,
            c: 300
        }
        /* for (var val of obj) {
            console.log(val);
        } */

        /* 
            只有可迭代对象 才能被 for...of 循环遍历
            一个对象是否是 可迭代对象，就看这个对象原型链中有没有 Symbol.iterator 标识
        */
    </script>
```





