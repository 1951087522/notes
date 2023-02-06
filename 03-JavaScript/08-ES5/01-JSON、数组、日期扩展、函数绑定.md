## **1.JSON拓展**

### **1.1** **parse(str, fn)**

**str：** 处理的字符串

**fn：** 回调函数

parse() 方法的第二个参数为一个回调函数，在处理每一个属性时，都会调用一次该回调函数，并将当前的属性名称和属性值作为参数传入到回调函数中。

如果指定了回调函数，JSON字符串str的每一个属性都会被回调函数处理，并且属性的值被替换为回调函数的返回值。

调用回调函数时会自动传入两个参数：

第一个参数表示当前的属性名称

第二个参数表示当前的属性值

回调函数调用中的this指向当前属性所属的对象。

回调函数返回 undefined 时，当前属性会被忽略。

parse() 方法是从子节点到根节点的方向遍历的，从最深层向外遍历。

```js
    <script>
        var str = '["a","b","c","d","e"]'
        // 1.parse()    第二个参数 可以指定一个函数作为参数
        var arr = JSON.parse(str, function (index, val) {
            if (index !== '') {
                return val + val
            }
            return val
        })
        console.log(arr);


        var str2 = '{"a": 100, "b": 200, "c": 300, "d": {"e": 1, "f": 2}}';
        var obj = JSON.parse(str2, function (key, val) {
            // 2.如果返回值为 undefined 表示忽略指定的属性
            if (key === 'b') {
                return undefined
            }
            if (key !== '') {
                return val / 10
            }
            return val
        })
        console.log(obj);
    </script>
```



### **1.2** **stringify(obj, fn)**

将js对象转换成json字符串

**obj：** 处理的对象

**fn：** 执行函数

**stringify()** 方法的第二个参数为一个回调函数，在处理每一个属性时，都会调用一次该回调函数，并将当前的属性名称和属性值作为参数传入到回调函数中。

如果指定了回调函数，obj对象的的每一个属性都会被回调函数处理，并且属性的值被替换为回调函数的返回值。

调用回调函数时会自动传入两个参数：

第一个参数表示当前的属性名称

第二个参数表示当前的属性值

回函数调用中的this指向当前属性所属的对象。

处理对象类型的值时，如果返回 undefined，当前属性会被忽略。

处理数组类型的值时，如果返回 undefined，会使用null代替。

```js
    <script>
        var arr = [100, ['aa', 'bb'], 300]
        var str = JSON.stringify(arr, function (key, val) {
            if (key !== '') {
                return val * 10
            }
            return val
        })
        console.log(str);

        var obj = { a: 100, b: 200, c: 300 }
        var str2 = JSON.stringify(obj, function (key, val) {
            if (key === 'b') {
                return undefined
            }
            return val
        })
        console.log(str2);
    </script>
```



## **2.数组拓展**

### **2.1 判断数组**

第一种方式 判断对象类型是数组

 	Object.prototype.toString.call(obj)

第二种方式 判断构造函数是否是Array

 obj.constructor === Array

第三种方式 判断是否是实例化对象

 obj instanceof Array

第四种方式 判断数组的静态方法isArray		*

 Array.isArray(obj)

```js
    <script>
        var a = 123;
        var b = 'abc';
        var c = true;
        var d = [1, 2, 3, 4];
        var e = { a: 100, b: 200 };
        var f = function () { console.log(1234); };

        // 判断数组
        // 方式一
        // 所有数据类型都有自身的 tostring() 方法
        /* console.log(a.toString());
        console.log(b.toString());
        console.log(c.toString());
        console.log(d.toString());
        console.log(e.toString());
        console.log(f.toString()); */

        /* console.log(e.toString());
        console.log(e.toString.call(a));
        console.log(e.toString.call(b));
        console.log(e.toString.call(c));
        console.log(e.toString.call(d));
        console.log(e.toString.call(f)); */

        // 利用对象的 toString() 方法的特殊性获取变量的类型
        /* console.log(e.toString.call(a).slice(8, -1).toLowerCase());
        console.log(e.toString.call(b).slice(8, -1).toLowerCase());
        console.log(e.toString.call(c).slice(8, -1).toLowerCase());
        console.log(e.toString.call(d).slice(8, -1).toLowerCase());
        console.log(e.toString.call(e).slice(8, -1).toLowerCase());
        console.log(e.toString.call(f).slice(8, -1).toLowerCase()); */
        

        // 方式二： 利用 constructor 属性判断是不是数组
        /* console.log(a.constructor);
        console.log(b.constructor);
        console.log(c.constructor);
        console.log(d.constructor);
        console.log(e.constructor);
        console.log(f.constructor); */

        /* if (a.constructor === Array) {
            console.log('数组');
        } else {
            console.log('不是数组');
        }

        if (d.constructor === Array) {
            console.log('数组');
        } else {
            console.log('不是数组');
        } */

        // 方式三 利用 instanceof   运算符 检测是不是数组
        /* console.log(a instanceof Array);
        console.log(b instanceof Array);
        console.log(c instanceof Array);
        console.log(d instanceof Array);
        console.log(e instanceof Array);
        console.log(f instanceof Array); */

        // 方式四 利用es5 新增的 isArray() 判断
        /* console.log(Array.isArray(a));
        console.log(Array.isArray(b));
        console.log(Array.isArray(c));
        console.log(Array.isArray(d));
        console.log(Array.isArray(e));
        console.log(Array.isArray(f)); */
    </script>
```



### **2.2 数组索引值**

ES5为数组拓展了两个方法：indexOf， lastIndexOf。

与字符串的同名方法功能相同，indexOf， lastIndexOf 的功能是查找数组中是否有指定的值，

如果有，返回这个值在数组中第一次出现的位置，

如果没有，返回 -1。

两个方法都接收两个参数：

第一个参数： 要查找的值

第二个参数：开始查找的位置

查找成员的时候，不会做数据类型的转换，是真正的全等查找。

```js
    <script>
        var arr = [1, 2, 3, 4, 'a', 'b', 3, 'c', '5']
        // indexOf 从左往右查找
        // 一个参数 要查找的值 返回查找值的位置
        console.log(arr.indexOf(3));    // 2
        // 未查找到返回 -1
        console.log(arr.indexOf(5));    // -1
        // 第二个参数 查找的开始位置 返回查找值的位置
        console.log(arr.indexOf(3, 3));     // 6

        // lastIndexOf  从右往左查找
        console.log(arr.lastIndexOf(3));    // 6
        // 第二个参数 查找的开始位置 返回查找值的位置
        console.log(arr.lastIndexOf(3, 5));     // 2

        /* 
            indexOf 从左往右查找数组中有没有指定的值
                如果查找到 返回这个值在数组中第一次出现的索引
                如果没有查找到  返回 -1

                第一个参数  要查找的值
                第二个参数  查找的起始位置

            lastIndexOf 从右往左查找数组中有没有指定的值
                如果查找到 返回这个值在数组中第一次出现的索引
                如果没有查找到 返回 -1

                第一个参数  要查找的值
                第二个参数  查找的起始位置
        */
    </script>
```



### **2.3** **forEach()	数组遍历**

ES5 新增的数组遍历方法，有两个参数：

第一个参数：每次遍历时的回调函数

第二个参数：指定回调函数的this值

回调函数被调用时，自动传入三个参数：

第一个参数：当前循环的值

第二个参数：当前循环的索引

第二个参数：循环数据本身

forEach() 不会修改原数组。

```js
    <script>
        var arr = [10, 20, 30, 40]
        /* for (var i = 0; i < arr.length; i++) {
            console.log(i, arr[i]);
        } */

        // 1.遍历数组   第一个参数是值 第二个参数是索引
        arr.forEach(function (val, index) {
            console.log(this);

            console.log(val, index);
            // 2.更改 this 指向
        }, document)

        /*
            forEach()   遍历数组
                第一个参数  在每一个值上都会调用的回调函数
                第二个参数  用来更改第一个参数的内部 this 指向

                回调函数
                    自动传入两个参数
                    第一个参数  当前循环数组的值
                    第二个参数  当前循环数组的索引

            与 jQuery 的 each() 方法不同点
                each():
                    1. 可以遍历数组 和 对象
                    2. 回调函数的第一个形参为索引
                                第二个形参为值
                forEach():
                    1. 只能遍历数组
                    2. 回调函数的第一个形参为值
                                第二个形参为索引
        */
    </script>
```



### **2.4** **map()	数组****遍历 有返回值**

ES5 新增的数组遍历方法，有两个参数：

第一个参数：每次遍历时的回调函数

第二个参数：指定回调函数的this值

回调函数被调用时，自动传入三个参数：

第一个参数：当前循环的值

第二个参数：当前循环的索引

第二个参数：循环数据本身

**与 forEach() 方法不同的是，forEach() 没有返回值，map() 方法的返回值为一个新数组，新数组的值为回调函数的返回值。**

map() 方法不会修改原数组。

```js
    <script>
        var arr = [10, 20, 30, 40]
        var result = arr.map(function (val, index) {
            console.log(this);

            // return 100
            // return val * 10
            return val * index

            // 2. 更改 this 指向 
        }, document)
        // 1.返回值为 由每一次回调函数的返回值组成的数组
        console.log(result);

        var arr2 = arr.forEach(function (val, index) {

            // console.log( val, index );

            console.log(this);

            // return 100;
            // return val * 10;
            return val * index;

        }, document);
        console.log(arr2);

        /* 
            map()   遍历并处理数组的每一个值
                第一个参数  在每一个值上调用的回调函数
                第二个参数  更改第一个擦拭你的内部 this 的值

                返回值  由每一次回调函数的返回值组成的数组
        
        */
    </script>
```



### **2.5** **fill() 指定值填充数组**

用指定的值填充数组，覆盖数组原来的值。

三个参数：

第一个参数：用来填充的新值

第二个参数：开始填充的位置，支持负数（可选）

第三个参数：填充的结束位置，支持负数（可选）

fill() 方法会修改原数组。

```js
    <script>
        var arr = [1, 2, 3, 4, 5, 6, 7, 8, 9]
        console.log(arr);
        // fill() 用指定值填充数组
        // 第一个参数 被填充的值
        // 第二个参数 填充的起始位置
        // 第三个参数 填充的结束位置

        // 1.只指定一个参数，填充数值的所有值
        // arr.fill('a')

        // arr.fill('a', 3, 6)
        
        // 2.指定两个参数 从开始位置填充后面所有的值
        arr.fill('a', 3)

        // 3.参数可以为负数 表示从后往前数组位置
        // arr.fill('a', - 3)

        // 4.开始位置必须小于结束位置 否则不填充
        // arr.fill('a', 6, 3)

        // 5.不指定参数 则使用 undefined 填充
        // arr.fill()

        console.log(arr);

        /*
            fill()  用指定的值填充参数
                第一个参数  被填充的数值
                第二个参数  填充的起始位置
                第三个参数  填充的结束位置

            1.只指定一个参数，填充数值的所有值
            2.指定两个参数，从开始位置填充后面所有的值    
            3.参数可以为负数    表示从后往前数值的位置
            4.起始位置必须小于结束位置，否则不填充
            5.不指定参数，则使用 undefined 填充
        */
    </script>
```



### **2.6** **some()	判断数组中的值是否符合指定条件**

ES5 新增的数组方法，判断数组中的值是否符合指定条件。

第一个参数：每次遍历时的回调函数

第二个参数：指定回调函数的this值

回调函数被调用时，自动传入三个参数：

第一个参数：当前循环的值

第二个参数：当前循环的索引

第二个参数：循环数据本身

`只要有一个值满足条件，some() 的返回值 true，否则为false。`

some() 不会修改原数组。

```js
    <script>
        var arr = [12, 32, 43, 45, 21, 4, 5, 76, 8, 23, 54];

        var result = arr.some(function (val, index) {
            console.log(this);

            // 1.直接返回真假
            // return true
            // return false

            // 2.根据条件返回真假
            // 只要有一个值的返回值为 true 最终结果就为 true
            return val > 50

            // 3.更改 this 指向
        }, document)
        console.log(result);

        /* 
            some()  根据回调函数的结果，返回对象的布尔值
                第一个参数  在每一个值上调用的回调函数
                第二个参数  更改第一个参数 内部 this 的值

            1.如果每一个值得回调函数都返回 true ，最终结果为 true
            1.如果每一个值得回调函数都返回 false ，最终结果为 false
            3.只要有一个返回值为 true ， 最终结果为 true
        */
    </script>
```



### **2.7** **every()**	**判断数组中的值是否符合指定条件**

ES5 新增的数组方法，判断数组中的值是否符合指定条件。

第一个参数：每次遍历时的回调函数

第二个参数：指定回调函数的this值

回调函数被调用时，自动传入三个参数：

第一个参数：当前循环的值

第二个参数：当前循环的索引

第二个参数：循环数据本身

`数组中的所有值都必须满足条件，every() 返回值才为 true，否则为false。`

every() 方法不会修改原数组。

```js
    <script>
        var arr = [12, 32, 43, 45, 21, 4, 5, 76, 8, 23, 54];

        var result = arr.every(function (val, index) {
            console.log(this);

            // return true
            // return false

            // return val > 50
            // return val < 100

            if (index === 2) {
                // return true
                // return false
            }

            return true

            // 更改 this 指向
        }, document)
        console.log(result);

        /*
            every()     根据回调函数的结果，返回对应的布尔值
                第一个参数  在每一个值上调用的回调函数
                第二个参数  更改第一个参数内部 this 的值

            1.  如果每一个值的回调函数都返回 true，最终结果为 true
            2.  如果每一个值的回调函数都返回 false，最终结果为 false
            3.  只要有一个值得返回值为 false 最终结果就为 false
                必须所有值得回调函数都返回 true 最终结果才为 true
        */
    </script>
```



### **2.8** **filter()	筛选满足条件的值**

ES5 新增的数组方法，判断数组中的值是否符合指定条件。

第一个参数：每次遍历时的回调函数

第二个参数：指定回调函数的this值

回调函数被调用时，自动传入三个参数：

第一个参数：当前循环的值

第二个参数：当前循环的索引

第二个参数：循环数据本身

filter() 方法用来过滤数组的值，当回调函数的返回值为true时，值被保留；返回值为false时，值被忽略。

filter() 的返回值为过滤之后的新数组，不会修改原数组。

```js
    <script>
        var arr = [12, 32, 43, 45, 21, 4, 5, 76, 8, 23, 54];

        var result = arr.filter(function (val, index) {
            // console.log(this);

            // 2.如果每一个回调函数都为 true ，数组中所有的值都会被选中
            // return true
            // 3.如果每一个回调函数都为 false ，数组中所有的值都会被忽略，最终返回空数组
            // return false

            // 1.filter()    从数组中筛选出满足条件的值，并返回包含筛选结果的新数组
            // return val > 20
            return val > 20 && val < 50


        }, document)

        console.log(arr);
        console.log(result);

        /*
            filter()    从数组中筛选出满足条件的值，并返回包含筛选结果的新数组
                第一个参数  在每一个值上调用的回调函数
                第二个参数  更改第一个参数 内部 this 的值

            1.如果每一个回调函数都为 true ，数组中所有的值都会被选中
            2.如果每一个回调函数都为 false ，数组中所有的值都会被忽略，最终返回空数组
            3.如果每一个回调函数都不返回值，默认返回空数组
        */
    </script>
```



### **2.9 回调函数的第三个参数**

- forEach() / map() / some() / every()  / filter() : 

  	第三个参数都为遍历的数组本身。

```js
    <script>
        var arr = [12, 32, 43, 45, 21, 4, 5, 76, 8, 23, 54];

        /* arr.forEach(function (val, index, a) {
            console.log(val, index, a);
        }) */

        /* arr.map(function (val, index, a) {
            console.log(val, index, a);
        }) */

        /* arr.some(function (val, index, a) {
            console.log(val, index, a);
        }) */

        /* arr.every(function (val, index, a) {
            console.log(val, index, a);
            return true
            // return false
        }) */

        /* arr.filter(function (val, index, a) {
            console.log(val, index, a);
        }) */

        // forEach() / map() / some() / every()  / filter() : 
        // 第三个参数 都为数组本身
    </script>
```



### **2.10 **reduce 与 reduceRight**

这两个是累加方法，reduce是从前向后累加，reduceRight是从后向前累加，对所有数组成员逐一处理，并将结果返回

reduce 与 reduceRight 有两个参数：

第一个参数：每次累加的回调函数

第二个参数：开始累加的初始值

> **reduce：**如果指定第二个参数，第二个参数将作为累加的初始值，reduce 从第一个值开始累加；如果不指定第二个参数，数组的第一个值将作为累加的初始值，reduce 从第二个值开始累加。
>
> reduceRight: 如果指定第二个参数，第二个参数将作为累加的初始值，reduceRight 从最后一个值开始累加；如果不指定第二个参数，数组的最后一个值将作为累加的初始值，reduceRight 从倒数第二个值开始累加。

第一个参数指定的回调函数每次调用时，都会自动传入四个参数，分别为：

- 第一个参数：上一次回调的返回值
- 第二个参数：当前的值
- 第三个参数：当前的索引值
- 第四个参数：原数组

返回值： 最后一次回调函数的返回值。

1.如果数组为空数组，并且没有指定第二个参数，reduce 与 reduceRight 将抛出错误。

2.如果数组只有一个值，不论这个值在数组的第几个位置，都直接返回这个值，不会执行回调函数。

3.如果数组为空，但指定了第二个参数，直接返回第二个参数，不会执行回调函数。

```js
    <script>
        var arr = [12, 32, 43, 45, 21, 4, 5, 76, 8, 23, 54];

        var result = arr.reduce(function (total, value, index, arr) {
            // console.log(arguments);
            // console.log(total, value, index, arr);
            // console.log(total, value, index);

            // 使用 reduce 方法实现累加
            // return total + value

            // 使用 reduce 实现累乘
            return total * value;

            // 第二个参数  运算的初始值
            // }, 0)
        })

        console.log(result);

        /*
            reduce()    对数组的每一个值 从左往右顺序运算
                第一个参数  每次运算的回调函数
                第二个参数  运算的初始值
        */
     </script>
```

```js
<script>
        var arr = [12, 32, 43, 45, 21, 4, 5, 76, 8, 23, 54];

        var result = arr.reduceRight(function (total, val, index, arr) {
            console.log(total, val, index, arr);
            return total + val
        }, 100)
        console.log(result);

        /*
            reduceRight()   对数组的每一个值从右往左顺序运算
                第一个参数  每次运算的回调函数
                第二个参数  运算的初始值

                回调函数的形参
                    第一个形参：    上一次回调函数的运算结果
                        第一个调用回调函数时：
                            如果指定 reduceRight 的第二个参数，第一个形参的值为 reduceRight 的第二个参数
                            如果没有指定 reduceRight 的第二个参数，第一个形参为数组的最后一个值
                    第二个形参：    数组当前循环的值
                    第三个形参:     当前值的索引
                    第四个形参      数组本身
        */
    </script>
```

```js
    <script>
        // var arr = []
        var arr = [10]
        var result = arr.reduce(function (total, val, index) {
            console.log(total, val, index);
        })
        var result = arr.reduceRight(function (total, val, index) {
            console.log(total, val, index);
        })
        var result = arr.reduce(function (total, val, index) {
            console.log(total, val, index);
        }, 10)
        var result = arr.reduceRight(function (total, val, index) {
            console.log(total, val, index);
        }, 10)

        console.log(result);

        /*
            reduce() 和  reduceRight()
                1.如果数组是一个空数组，并且 reduce() / reduceRight 没有指定第二个参数。会抛出错误
                2.如果数组是一个空数组，并且 reduce() / reduceRight 指定了第二个参数，直接将第二个参数作为结果返回，不会调用回调函数
                3.如果数组只有一个值，并且 reduce() / reduceRight 没有指定第二个参数，直接将数组的第一个值作为结果返回，不会调用回调函数
                4.如果数组只有一个值，并且 reduce() / reduceRight 指定了第二个参数，会调用一次回调函数，并且将回调函数的结果作为返回值返回
        */
    </script>
```



## **3.日期拓展**

- toJSON

toJSON 将日期转化成json格式，（标准化格式）

它返回 UTC 时区的 ISO 格式日期字符串（由后缀 Z 表示）。

是ES5新增的方法，增强对日期格式的可读性。

```js
    <script>
        var date = new Date()

        console.log(date.toString());

        // 1.返回一个固定格式的世界世界字符串
        console.log(date.toJSON());

        // 跟 toISOString() 的返回结果相同
        console.log(date.toISOString());
    </script>
```



## **4.函数绑定**

ES5 新增方法，克隆数组，并将数组的 this 绑定为指定值。

- call()与apply()的区别

  都是改变函数this的方法，都是在调用该方法的时候，执行函数并改变this的值。

  call 从第二个参数开始，表示传递给函数的参数

  apply 第二个参数是数组，每一个成员表示传递给函数的参数

- bind()跟call()类似

  第一个参数表示改变的this值

  从第二个参数开始，表示传递的参数

- 区别：

  call() | apply() 调用即执行

  bind() 克隆函数，但不立即执行

```js
    <script>
        function fn1() {
            console.log(this);
        }
        fn1()

        fn1.call(1)
        fn1.call(document)

        fn1.apply(2)
        fn1.apply(document)

        var fn2 = fn1.bind(1)
        fn1()
        fn2()

        function fn3(a, b) {
            console.log(this, a, b);
        }
        var fn4 = fn3.bind(document, 10, 20)
        var fn4 = fn3.bind(document)
        fn4()
        fn4(100, 200)

        /*
            bind()  克隆当前函数，并且将克隆函数的 this 绑定为指定的值
                1.在进行的 bind() 绑定 this 的值时，不会调用原函数
                2.克隆函数传参方式有两种：
                    第一种：在 bind() 时传递参数
                        特点：克隆函数的参数始终是固定的，就是bind() 时传递的值
                    第二种：在调用克隆函数时传递参数
                        特点：每次调用都可以传递不同的参数

                注意：bind()    只会改变克隆函数的 this 值，原函数不受影响
        
        */
    </script>
```

