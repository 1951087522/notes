## 1.Symbol类型

### 1.1 symbol介绍

> 在js中有6种数据类型：数字、字符串、布尔值、undefined、null、对象。
>
> 在ES6中又添加了一中数据类型： symbol类型，用来创建独一无二的值。

symbol类型通过`Symbol()`函数来创建，每调用一次`Symbol()`，都会创建一个独一无二的Symbol类型的值。

引入symbol类型是为了解决对象在添加新属性时可能与现有属性名称冲突的问题。

1. 设计 Symbol()类型的初衷 是为了解决对象的属性名称有可能重复的问题
2. 虽然 Symbol() 首字母大写，但它并不是一个构造函数，不能使用 new 运算符调用
3. Symbol() 可以接收一个参数作为标识符，这个标识符仅仅作为外面识别不同的 Symbol() 代码的描述，对 Symbol() 的值没有任何影响;Symbol() 的参数标识只是为了让开发者能区分不同的 Symbol 值，所以给不同的 Symbol() 值设置相同的标识是没有意义的
4. Symbol() 类型作为对象属性名称的使用方式:
   1. 如果要单独访问 Symbol() 类型的属性,属性名称必须提前使用变量保存
   2. 使用变量保存之后，就可以间接使用 [] 访问

```js
    <script>
        /* 
            简单类型：
                number / string / boolean / undefined / null / symbol
            复杂类型
                object / array / regexp / date / function / error
        */

        // Symbol 类型是 es6 新增的简单类型
        // Symbol() 每次调用都会产生一个独一无二的值
        var a = Symbol()
        var b = Symbol()
        console.log(a);
        console.log(b);
        console.log(a === b);

        // 1.设计 Symbol()类型的初衷 是为了解决对象的属性名称有可能重复的问题
        var obj = {
            a: 100,
            [Symbol()]: 200
        }
        // 本意是想给 obj 添加一个新属性 a ，但是却与已有的 a 属性重名导致属性覆盖
        obj.a = 300
        console.log(obj);
        // Symbol() 类型每一个值都是独一无二的，不可能产生属性覆盖的问题
        obj[Symbol()] = 400
        console.log(obj);

        // 2.虽然 Symbol() 首字母大写，但它并不是一个构造函数，不能使用 new 运算符调用
        /* var c = new Symbol()
        console.log(c); */

        // 3.Symbol() 可以接收一个参数作为标识符，这个标识符仅仅作为外面识别不同的 Symbol() 代码的描述，对 Symbol() 的值没有任何影响
        var d = Symbol('abc')
        // Symbol() 的参数标识只是为了让开发者能区分不同的 Symbol 值，所以给不同的 Symbol() 值设置相同的标识是没有意义的
        var e = Symbol('abc')
        console.log(d);
        console.log(e);

        // 4.Symbol() 类型作为对象属性名称的使用方式
        // 如果要单独访问 Symbol() 类型的属性,属性名称必须提前使用变量保存
        var sym = Symbol()
        var obj2 = {
            a: 100,
            b: 200,
            [sym]: 300
        }
        console.log(obj2.a);
        console.log(obj2['b']);
        // Symbol() 类型的属性，不能直接使用 . 和 [] 语法访问
        // console.log(obj2.Symbol());
        // console.log(obj2[Symbol]);
        // 如果要单独访问 Symbol() 类型的属性,属性名称必须提前使用变量保存
        // 使用变量保存之后，就可以间接使用 [] 访问
        console.log(obj2[sym]);
    </script>
```



### 1.2.symbol类型特征

symbol类型有如下特征：

- 无法转换为nubmer类型， 尝试将一个symbol值转换为一个number值时，会抛出错误。
- 可以转换为boolean类型，结果一定为true。
- 无法使用 `+` 运算符或`concat()`方法进行字符串拼接，但可以通过 `String()` 转换（不能使用`new`关键字），结果为固定的 `Symbol()`，如果传递了参数，转换结果也会附带参数，比如：`Symbol(abc)`。
- 调用 `JSON.stringify()` 转换对象时，使用 symbol 类型的属性会被忽略。
- 对象的symbol 类型属性无法被 `for...in` 语句或 `Object.keys()、getOwnPropertyNames()` 方法枚举。需要使用专属的 `getOwnPropertySymbols()` 方法获取。

```js
    <script>
        // Symbol 类型的特征
        var sym = Symbol('abc')
        console.log(sym);
        console.log(typeof sym);

        // 1.Symbol 类型不能转换为 number 类型
        // var num = +sym
        // var num = Number(sym)
        // console.log(num);

        // 2.Symbol 类型可以转换为 Boolean 类型，结果一定为 true
        // var bool = !!sym
        // var bool = Boolean(sym)
        // console.log(bool);

        // 3.Symbol 类型本身不能转换成字符串，但是使用 String() 方法转换，不会报错，会得到一个固定的带标识的字符串
        // var str = '' + sym
        // var str = String.prototype.concat(sym)
        // console.log(str);
        // 使用 String() 方法转换，不会报错，会得到一个固定的带标识的字符串
        // var str = String(sym)
        // console.log(str);

        var obj = {
            a: 100,
            b: 200,
            [Symbol()]: 300,
            d: Symbol(),
            e: 123
        }

        // 4.使用 JSON.stringify() 将对象转换成 JSON 字符串时，会忽略属性名称为 Symbol 类型的属性
        console.log(JSON.stringify(obj));

        Object.defineProperty(obj, 'c', {
            value: 400
        })

        // 5.属性名称为 Symbol 类型的属性，不能被 for...in循环遍历，也不能被 Object.keys() 和 Object.getOwnPropertyNames() 方法枚举
        console.log(obj);
        for (var i in obj) {
            console.log(i, obj[i]);
        }
        console.log(Object.keys(obj));
        console.log(Object.getOwnPropertyNames(obj));

        // 6.属性名称为 Symbol 类型的属性，需要通过 getOwnPropertySymbols() 方法获取
        console.log(Object.getOwnPropertySymbols(obj));
        var arr = (Object.getOwnPropertySymbols(obj));
        console.log(obj[arr[0]]);
    </script>
```

## 2.Proxy

> **Proxy** 用于创建一个对象的代理，以间接的方式对对象的指定属性进行自定义控制。

**语法：** `new Proxy(obj, {get, set})；`

第一个参数：obj 表示被代理的对象

第二个参数：{get, set} 表示操作被代理对象的对象

- get(target, key, proxy) 用来获取指定属性的方法。

  - target: 被代理的对象
  - key: 表示要获取的属性名称
  - proxy: 代理对象

  get() 的返回值就是要获取的属性的值。

- set(obj, key, value, proxy) 表示赋值方法

  - target: 被代理的对象
  - key: 表示要设置的属性名称
  - value 表示要设置的新属性值
  - proxy: 代理对象

  不需要返回值

> 注意：获取和修改proxy对象的key属性会导致回调溢出错误。

```js
        // 代理对象：通过第三方对象来控制指定对象的属性获取和修改行为
        // proxy 表示不直接修改对象本身，而是以代理的方式修改，可以在获取和修改时进行条件控制

        var xiaoming = {
            name: '小明',
            age: '18',
            sex: '男'
        }

        // 代理应用实例 控制人物年龄和性别的获取和设置
        // 第一个参数：表示被代理的对象
        // 第二个参数：{get,set} 表示操作被代理对象的对象

        var proxy = new Proxy(xiaoming, {
            // 配置获取代理对象的属性
            // target   被代理的对象
            // key      表示要获取的属性名称
            // proxy    代理对象
            get(target, key, proxy) {
                // 进行条件控制
                // 如果获取年龄就在后面加上 '岁' 字
                if (key === 'age') {
                    return target[key] + '岁'
                }
                return target[key]
            },
            // 配置设置代理对象的属性
            // target   被代理的对象
            // key      表示要获取的属性名称
            // value    表示要设置的新属性值
            // proxy    代理对象
            set(target, key, value, proxy) {
                // 进行条件控制
                // 性别只允许设置为 '男' '女' '保密'
                if (key === 'sex') {
                    switch (value) {
                        case '男':
                            target[key] = '男'
                            break;
                        case '女':
                            target[key] = '女'
                            break;
                        case '保密':
                            target[key] = '保密'
                            break;
                        default:
                            throw new Error('性别设置有误')
                    }
                    return
                }
                // 限制年龄的有效范围 0 - 120
                if (key === 'age') {
                    if (value < 0 || value > 120) {
                        throw new Error('年龄设置有误')
                        return
                    }
                }
                target[key] = value;
            }
        })
        console.log(proxy.name);
        console.log(proxy.age);
        console.log(proxy.sex);
        console.log(xiaoming);

        // proxy.sex = 1;
        proxy.sex = "女";
        console.log(proxy.sex);

        // proxy.age = 999
        proxy.age = 30;
        console.log(proxy.age);
    </script>
```

## 3.Reflect

> Reflect 是 ES6 为了操作对象而提供的新内置对象。

Reflect 对象的设计目的有以下几点：

- 将Object对象的一些明显属于语言内部的方法（比如Object.defineProperty），放到Reflect对象上。现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。
- 修改某些Object方法的返回结果，让其变得更合理。
- 让Object操作都变成函数行为。

### 3.1.修改 Object 现有方法的返回结果

| 方法名称                   | 原Object构造函数                                             | 新Reflectct对象                                              |
| -------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| defineProperty()           | 如果未在对象上成功定义属性，则返回TypeError                  | 如果在对象上成功定义了属性，则返回true，否则返回false。      |
| getOwnPropertyDescriptor() | 如果传入的对象参数上存在，则会返回给定属性的属性描述符，如果不存在，则返回undefined； | 如果给定属性存在于对象上，则返回给定属性的属性描述符。如果不存在则返回undefined，如果传入除对象（原始值）以外的任何东西作为第一个参数，则返回TypeError |
| getPrototypeOf()           | 返回给定对象的原型。如果没有继承的原型，则返回null。在 ES5 中为非对象抛出TypeError，但在 ES2015 中强制为非对象。 | 返回给定对象的原型。如果没有继承的原型，则返回 null，并为非对象抛出TypeError。 |
| setPrototypeOf()           | 如果对象的原型设置成功，则返回对象本身。如果设置的原型不是Object或null，或者被修改的对象的原型不可扩展，则抛出TypeError。 | 如果在对象上成功设置了原型，则返回 true，否则返回 false（包括原型是否不可扩展）。如果传入的目标不是Object，或者设置的原型不是Object或null，则抛出TypeError。 |
| isExtensible()             | 如果对象是可扩展的，则返回 true，否则返回 false。如果第一个参数不是对象（原始值），则在 ES5 中抛出TypeError。在 ES2015 中，它将被强制为不可扩展的普通对象并返回false。 | 如果对象是可扩展的，则返回true，否则返回false。如果第一个参数不是对象（原始值），则抛出TypeError。 |
| preventExtensions()        | 返回被设为不可扩展的对象。如果参数不是对象（原始值），则在 ES5 中抛出TypeError。在 ES2015 中，参数如为不可扩展的普通对象，然后返回对象本身。 | 如果对象已变得不可扩展，则返回true，否则返回false。如果参数不是对象（原始值），则抛出TypeError。 |

```js
    <script>
        var obj = {
            a: 100,
            b: 200,
            c: 300
        }

        // 1. Object.defineProperty()
        Object.defineProperty(obj, "c", {
            writable: false,
            configurable: false
        });
        Object.prototype.abc = '12345';
        // 不能重新配置，抛出错误
        /* Object.defineProperty(obj, 'c', {
            value: 2000
        }) */
        // console.log(obj);
        // 1.不能重新配置，返回结果 false
        /* var res = Reflect.defineProperty(obj, 'c', {
            value: 2000
        })
        console.log(res); */

        // 2.如果第一个参数不是对象类型，返回 undefined
        // console.log(Object.getOwnPropertyDescriptor(1, 'a'));
        // 2.如果第一个参数不是对象类型，抛出错误
        // console.log(Reflect.getOwnPropertyDescriptor(1, 'a'));

        // 3.如果参数非对象类型，es6之前抛出错误，es6之后强制转换为 object 类型
        // es6之后强制转换为 object 类型
        // console.log(Object.getPrototypeOf(1));
        // 3.如果参数为非对象类型，抛出错误
        // console.log(Reflect.getPrototypeOf(1));

        // 4.返回设置成功的原对象
        var res1 = Object.setPrototypeOf(obj, {})
        console.log(res1);
        // 4.返回表示设置成功的布尔值 true
        var res2 = Reflect.setPrototypeOf(obj, {})
        console.log(res2);
    </script>
```

### 3.2.新增方法

- `**Reflect.deleteProperty(obj, key)**`

删除对象的指定属性，和 `delete` 运算符的功能完全相同。

- `**Reflect.get(obj, key)**`

获取对象的指定属性的值，如果 obj 不是对象，则会抛出错误。

- `**Reflect.set(obj, key, value)**`

设置或修改对象的指定属性的值，如果成功，返回true，否则返回false。如果 obj 不是对象，则抛出错误。

- `**Reflect.has(obj, key)**`

判断对象有没有指定属性，和 `in` 运算符的功能完全相同。

- `**Reflect.ownKeys(obj)**`

返回一个包含对象所有自身属性（不包含继承属性）的数组。

```js
    <script>
        var num = 100
        var obj = {
            a: 10,
            b: 20,
            c: 30,
            d: 40,
            [Symbol()]: 999
        }
        Reflect.defineProperty(obj, 'e', {
            value: 50
        })

        // 1.删除指定属性   Reflect.deleteProperty()
        // 使用 delete 运算符删除
        delete obj.b
        // 使用 Reflect.deleteProperty() 删除
        Reflect.deleteProperty(obj, 'c')

        // 2.获取指定属性的值   Reflect.get()
        console.log(obj.a);
        console.log(num.a);
        // 使用 Reflect.get()   获取
        console.log(Reflect.get(obj, 'a'));
        // 只能用在对象上，不能用在简单类型上
        // console.log(Reflect.get(num, 'a'));

        // 3.修改指定属性的值
        obj.d = 400
        // 使用 Reflect.set()
        Reflect.set(obj, 'd', 500)
        // 只能用在对象上，不能用在简单类型上
        // Reflect.set(num, 'd', 500)
        console.log(obj);

        // 4.判断对象有没有指定的属性(会查找原型链)
        Object.prototype.f = '000'
        console.log('b' in obj);
        console.log('f' in obj);
        // 使用 Reflect.has()
        console.log(Reflect.has(obj, 'b'));
        console.log(Reflect.has(obj, 'f'));

        // 5.获取对象自身的所有属性
        Object.prototype.g = 888
        // 只能获取自身的可枚举属性(不能获取属性名为 Symbol 类型的属性)
        console.log(Object.keys(obj));
        // 可以获取自身的所有属性(包括可枚举属性和属性名为 Symbol 类型的属性)
        console.log(Reflect.ownKeys(obj));
    </script>
```

## 4.数据结构

> 在ES5中的数据结构有： 对象和数组
>
> 在ES6中又添加了四种数据结构： Set、WeakSet、Map、WeakMap

### [4.1 Set](https://www.aichuangleyu.com/#/docs/web/ES5_ES6/lesson5/lesson49?id=_41-set)

> Set是类似于数组，但是成员的值都是唯一的，没有重复值的对象类型。

利用Set对象值是唯一的特性，可以实现数组去重功能。

由于Set对象实现了数组迭代器接口，所以可以使用for of语句遍历该对象。

`Set`本身是一个构造函数，用来生成 Set 数据结构。

Set类型的常用方法：

- size: 获取数据的长度（属性）
- has: 判断是否包含某个属性
- add: 添加数据
- delete: 删除某项数据
- clear: 清空数据
- forEach: 以回调函数形式遍历数据
- keys、values、entries：以迭代对象方式遍历对象

```js
    <script>
        var set1 = new Set()
        // 1.添加值 add()
        // 相同的值 只能存储一次，也就是说 set 的值是唯一的
        set1.add(10)
        set1.add(20)
        set1.add(30)
        set1.add(40)
        set1.add(10)
        console.log(set1);
        // set 只有值，没有索引
        // set 类型没有提供单独获取指定值的方式
        console.log(set1[0]);

        // 2.判断set中有没有指定的值    has()
        console.log(set1.has(10));
        console.log(set1.has(11));

        // 3.查看set中的数据数量
        console.log(set1.size);
        // set对象不能使用 for 循环
        /* for (var i = 0; i < set1.size; i++) {
            console.log(set1[i]);
        } */
        // set 对象通过 forEach() 遍历
        set1.forEach(function (val, index, set) {
            // console.log(val.index, set);
            // console.log(this);
            // 因为 set 没有索引,所有 forEach 的第一个形参和第二个形参都是值
            console.log(val, index);
        }, document)

        // 4.删除 set 对象中的指定值
        set1.delete(30)
        console.log(set1);

        // 5.清空 set 对象中的所有数据
        set1.clear()
        console.log(set1);

        // 6.利用数组快速存储值到 set 对象中
        var set2 = new Set([1, 2, 3, 4, 5])
        var set3 = new Set([1, 2, ['a', 'b', ['c', 'd']]])
        console.log(set2);
        console.log(set3);

        // 7.利用set对象的值的唯一性,实现数组去重
        var arr1 = [12, 342, 4, 2, 43, 2, 4, 2, 3, 2, 4, 2, 3, 33, 4, 3, 4];
        var result = [];
        // 前面不能省略分号;
        (new Set(arr1)).forEach(val => result.push(val));
        console.log(result);

        // 8.通过set对象的值 生成可迭代对象
        var set4 = new Set([10, 20, 30, 40, 50])
        var keys = set4.keys()
        var values = set4.values()
        var entries = set4.entries()
        console.log(set4);
        console.log(keys);
        console.log(values);
        console.log(entries);

        console.log(keys.next());
        console.log(keys.next().value);

        console.log(values.next());
        console.log(values.next().value);

        console.log(entries.next());
        console.log(entries.next().value);
    </script>
```



### 4.2 WeakSet

WeakSet 结构与 Set 类似，也是不重复的值的集合。但是，它与 Set 有两个区别。

> - 首先，WeakSet 的成员只能是对象，而不能是其他类型的值。
>
> - 其次，WeakSet 中的对象都是弱引用，即垃圾回收机制不考虑 WeakSet 对该对象的引用，也就是说，如果其他对象都不再引用该对象，那么垃圾回收机制会自动回收该对象所占用的内存，不考虑该对象还存在于 WeakSet 之中。

注意：不能添加空对象null

WeakSet只有add，delete，has三个方法：

- delete: 删除数据
- has: 判断是否包含某个属性
- add： 添加数据

由于weakset不能被垃圾回收机制自动回收，因此要慎用

```js
    <script>
        var set1 = new Set()
        // set 对象的值可以是任意类型
        set1.add(10);
        set1.add('123');
        set1.add(true);
        set1.add(undefined);
        set1.add(null);
        set1.add([1, 2, 3]);
        set1.add({ a: 123 });

        console.log(set1);

        var wk = new WeakSet()
        var arr1 = [1, 2, 3, 4]
        // wk.add(10)
        // wk.add('123')
        // wk.add(true)
        // wk.add(undefined)
        // wk.add(null)
        wk.add([1, 2, 3])
        wk.add({ a: 123 })
        wk.add(arr1)

        console.log(wk.has(arr1));
        wk.delete(arr1)
        console.log(wk.has(arr1));

        console.log(wk);

        /*
            WeakSet()   只能存储对象类型的值
            WeakSet()   是一个弱引用的对象类型,WeakSet 对象中的数据随时有可能被垃圾回收机制回收
            WeakSet() 只有三个方法:
                add()       添加值
                delete()    删除值
                has()       判断有没有指定的值
        */
    </script>
```

### 4.3 Map

Map是特殊的对象类型，与普通对象一样，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

传统的对象所有属性名称都必须是字符串，但是Map对象中，定义的属性名称可以是任意类型（7种类型都可以）

通过 `new Map()` 创建map对象，map对象实现了迭代器接口对象，因此可以使用for of循环遍历。

map类型的常用方法：

- size: 获取数据的长度（属性）
- has: 判断是否包含某个属性
- delete: 删除某项数据
- clear: 清空数据
- get: 获取数据
- set: 设置数据
- forEach: 以回调函数形式遍历数据
- keys、values、entries：以迭代对象方式遍历数据

```js
    <script>
        var mp = new Map()
        var arr = [1, 2, 3, 4]
        // 1.map 对象的属性名称和数学值都可以说任意数据类型
        mp.set(0, 100)
        mp.set('abc', 200)
        mp.set([1, 2, 3], { a: 'abc' })
        mp.set(undefined, { a: 'abc' })
        mp.set('abc', 2000)
        mp.set([1, 2, 3], 3000)
        mp.set(arr, 400)
        // 2.单独获取复杂类型为属性名称的属性，需要提前使用变量保存
        console.log(mp.get([1, 2, 3]));
        console.log(mp.get(arr));
        // 3.属性名称为简单类型的属性，可以直接获取 get()
        console.log(mp.get('abc'));
        // 4.判断map对象中有没有指定的属性  has()
        console.log(mp.has(arr));
        console.log(mp.has('ab'));
        // 5.查看map对象的数据数量  size
        console.log(mp.size);
        // 6.删除指定属性   delete()
        mp.delete(0)
        console.log(mp);
        // 7.清空map对象    clear()
        // mp.clear()
        // console.log(mp);
        // 8.遍历map对象    forEach()
        mp.forEach((val, index, map) => {
            // console.log(val, index, map);
        })
        // 9.将map对象的属性名称转换成可迭代对象
        var keys = mp.keys()
        var values = mp.values()
        var entries = mp.entries()

        console.log(keys);
        console.log(values);
        console.log(entries);
        // 10.利用数组快速创建一个map对象
        // 数组格式是有要求的
        // 数组必须是二维数组
        var mp2 = new Map([[1, 100], [2, 200], ["a", 'abc'], [{ a: 123 }, new Date()]]);
        console.log(mp2);
    </script>
```



### 4.4 WeakMap

`WeakMap`结构与`Map`结构类似，也是用于生成键值对的集合。

`WeakMap`与`Map`的区别有两点：

首先，`WeakMap`只接受对象作为键名（`null`除外），不接受其他类型的值作为键名。

其次，`WeakMap`的键名所指向的对象，不计入垃圾回收机制。

注意：不能添加空对象null

`WeakMap`只有四个方法可用：

- delete: 删除某项数据
- has: 判断是否包含某个属性
- get: 获取数据
- set: 设置数据

由于weakmap不能被垃圾回收机制自动回收，因此要慎用。

```js
    <script>
        // WeakMap 也是一个不稳定的对象类型
        var wkMap = new WeakMap()
        // wkMap.set(0, '123')
        // wkMap.set(false, '123')

        var arr = [1, 2, 3]
        var obj = { a: 123 }
        var re = /\d+/
        var fn = () => { }

        // 1.WeakMap 对象的属性名称必须是复杂类型，不能使用简单类型
        wkMap.set(arr, '123')
        wkMap.set(obj, 100)
        wkMap.set(re, 2000)
        wkMap.set(fn, 3000)

        // 2.只要属性名称对应的对象没有被其他地方引用，就随时又可能被垃圾回收机制回收
        wkMap.set([1, 2, 3], '123')
        wkMap.set({ a: 123 }, 100)

        console.log(wkMap.has(arr));
        console.log(wkMap.get(arr));
        wkMap.delete(arr)
        console.log(wkMap);
    </script>
```

## 5.迭代器接口

> 迭代器是ES6中新增的数据访问接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。

一个数据结构只要具有`Symbol.iterator`属性，就可以认为是“可遍历的”（iterable）。`Symbol.iterator`属性本身是一个函数，就是当前数据结构默认的遍历器生成函数。执行这个函数，就会返回一个遍历器。至于属性名`Symbol.iterator`，它是一个表达式，返回`Symbol`对象的`iterator`属性，这是一个预定义好的、类型为 Symbol 的特殊值。

原生具备 Iterator 接口的数据结构如下：

- Array （keys， values， entries）
- Map
- Set
- String
- TypedArray
- 函数的 arguments 对象
- NodeList 对象

> 通过给对象添加`Symbol.iterator`属性和next方法，可以让原本不支持迭代的对象也可以实现迭代功能。

```js
<script>
        var str = '123456'
        console.log(str);
        console.log(new String(100));
        // 字符串本身也可以使用 for...of 循环遍历
        for (var i of str) {
            console.log(i);
        }
        var obj1 = {
            a: 10,
            b: 20,
            c: 30
        }
        console.log(obj1);
        // 对象不被 for...of 遍历
        // obj is not iterable
        /* for (var j of obj) {
            console.log(j);
        } */

        // 模拟自定义的可迭代对象
        // 1.通过给对象添加`Symbol.iterator`属性和next方法，可以让原本不支持迭代的对象也可以实现迭代功能。
        var obj2 = {
            0: 100,
            1: 'abc',
            3: true,
            4: [1, 2, 3],
            length: 5,
            // 引用任意一个可迭代对象的 Symbol.iterator 属性的值
            [Symbol.iterator]: Array.prototype[Symbol.iterator],
            next: (function () {
                var index = 0
                return function () {
                    // 如果可迭代对象的值已经取完
                    if (index >= this.length) {
                        return { value: undefined, done: true }
                    } else {
                        return { value: this[index++], done: false }
                    }
                }
            })()
        }
        console.log([1, 2, 3, 4]);
        console.log(obj2);
        for (var i = 0; i < obj2.length; i++) {
            console.log(i, obj2[i]);
        }
        for (var k of obj2) {
            console.log(k);
        }

        // 可迭代对象中还有值 {value:值，done:false}
        // 可迭代对象中没有值 {value:undefined,done:true}
        console.log(obj2.next());
        console.log(obj2.next());
        console.log(obj2.next());
        console.log(obj2.next());
        console.log(obj2.next());
        console.log(obj2.next());
        console.log(obj2.next());
    </script>
```

## 6.Promise

### 6.1 Promise介绍

> Promise是将异步写法变为同步写法的规范
>
> 只是写法的改变，操作并没有改变
>
> 异步操作：在回调函数中，一层嵌套一层
>
> 同步操作：将方法写在外部

[js执行机制-同步与异步](L:\AIC\notes\03-JavaScript\03-BOM)

```js
    <script>
        // 1.同步执行：上面的代码执行结束，才会执行下面的代码
        for (var i = 0; i < 10; i++) {
            console.log(i);
        }
        console.log('同步执行');
        // 2.异步执行: 上面的代码只要解析完成,就可以继续执行下面的代码,不需要等上面代码执行
        // 定时器属性异步执行
        // 所有HTTP请求默认也都是异步
        setTimeout(() => {
            for (var j = 0; j < 10; j++) {
                console.log(j);
            }
        }, 0);
        console.log('异步执行');
    </script>
```



- **三个状态**

`pending`： 表示操作正在执行

`fulfilled`： 表示操作执行成功

`rejected`：表示操作执行失败

- **状态的流向**：

在Promise中状态有两个方向的流动：

状态由pending流向fulfilled, 说明操作执行成功完毕

状态由pending流向rejected, 说明操作执行失败完毕

- **语法**

> new Promise((resolve, reject) => { 回调函数中执行异步操作 })

如果操作执行成功 执行resolve方法

如果操作执行失败 执行reject方法

在外部通过then方法监听状态的改变

then(success, fail) 该方法接收两个参数

success: 表示成功时候执行的回调函数，参数是由 resolve方法执行的时候传递的参数（只能传递一个）

fail：表示失败时候执行的回调函数，参数是由 reject方法执行的时候传递的参数（只能传递一个）

then方法的返回值是Promise对象，因此，可以链式调用该方法

上一个then方法的输出，将作为下一个then方法参数的输入。

如果操作已经执行完毕，then方法也会立即执行

```js
  <script>
    /*
        Promise 对象
          1. 对象的状态不受外界影响 只有异步操作的结果可以决定当前是哪一种状态
            pending(进行中)
            fulfilled(成功)
            rejected(失败)
          2. 只有两种情况 状态发生就不会再改变
            从penging到fulfilled 进行中到成功
            从pending到rejected 进行中到失败
          
          Promise 对象是一个构造函数 用来生成promise实例 带有一个回调函数
            参数是 reslove(成功) reject(失败) 参数是函数

          then() 方法第一个参数是reslove状态的回调函数
          第二个参数是reject状态的回调函数
    */

    function time(ms) {
      // Promise 对象 参数为回调函数 第一个参数是resolve成功的状态 第二个参数是reject失败的状态 
      return new Promise((resolve, reject) => {
        // 模拟异步操作
        let res = {
          code: 200,
          msg: 'hello Promise',
          err: '读取失败'
        }
        setTimeout(() => {
          if (res.code === 201) {
            resolve(res.msg)
          } else {
            reject(res.err)
          }
        }, ms);
      })
    }

    // then() 方法监听 第一个参数是resolve成功的回调函数 第二个参数是可选的 是reject失败的回调函数
    /* time(1000).then((val) => {
      console.log(val);
    }), (err) => {
      console.log(err);
    } */

    // then() 方法返回新的Promise实例 采用链式调用 但一般不采用这种写法
    /* time(1000).then((val1) => {
      console.log(val1);
    }).then((val2) => {
      console.log(val2);
    }) */

    /* time(1000).then((val) => {
      console.log(val);
    }).then(null, err => {
      console.log(err);
    }) */

    // then(null,err=>{}) 等价于 catch() 用于指定发送错误时的回调函数
    time(1000).then((val) => {
      console.log(val);
    }).catch(err => {
      console.log(err);
    })
  </script>
```



### 6.2 实例方法

有三个方法可以监听Promise状态：

- `then`: 可以监听状态成功或者是失败的方法

可以定义多个then方法，此时后一个then方法可以监听前一个then的成功与失败。

- `catch`: 可以监听状态失败时候的方法

失败只能被监听一次，但是可以被后面的then继续监听。

- `finally`: 无论成功还是失败都会执行的方法

无法接收数据

```js
    <script>
        function loadImage(src) {
            return new Promise(function (resolve, reject) {
                var img = new Image()
                img.onload = function () {
                    resolve('加载成功')
                }
                img.onerror = function () {
                    reject('加载失败')
                }
                img.src = src
            })
        }

        // 传入成功：
        // var p = loadImage('../1.jpg')
        // 传入失败：
        var p = loadImage('../2.jpg')
        // 1.所有的 Promise 实例都有一个 then() 方法，用来获取执行状态
        // 当 Promise 的成功回调函数执行时，会自动调用 then() 的第一个参数
        // 当 Promise 的失败回调函数执行时，会自动调用 then() 的第二个参数

        // 2.then() 方法的两个参数必须都是函数
        /* p.then(function (data) {
            console.log(data, 1111);
        }, function (data) {
            console.log(data, 2222);
        }); */

        // 3.失败的回调函数可以单独使用 catch() 方法
        /* p.then(function (data) {
            console.log(data, 111);
        }).catch(function (data) {
            console.log(data, 'catch');
        }) */

        // 4.如果要使用 catch() 方法来执行失败的回调函数，就不能给 then() 方法指定第二个参数(因为 then() 的执行优先级高于 catch() )
        /* p.then(function (data) {
            console.log(data, 'then');
        }, function (data) {
            console.log(data, '222');
        }).catch(function (data) {
            console.log(data, 'catch');
        }); */

        // 5.单独绑定失败的回调函数(单独只绑定失败的回调是没有意义的)
        /* p.catch(function (data) {
            console.log(data, 'catch');
        }) */

        // 6.finally: 无论成功还是失败都会执行的方法
        p.then(function (data) {
            console.log(data, 'then');
        }).catch(function (data) {
            console.log(data, 'catch');
        }).finally(function (data) {
            console.log('执行完成', 'finally');
        })


        /* 
            有三个方法可以监听Promise状态：
                1.then(): 可以监听状态成功或者是失败的方法
                    可以定义多个then方法，此时后一个then方法可以监听前一个then的成功与失败。
                2.catch(): 可以监听状态失败时候的方法
                    失败只能被监听一次，但是可以被后面的then继续监听。
                3.finally(): 无论成功还是失败都会执行的方法
                    无法接收数据 
            */
    </script>
```

### 6.3 `all()`

> all方法用于监听多个Promise对象
>
> 参数是一个数组，数组中的每一项都是一个Promise对象
>
> 我们可以通过then方法监听状态的改变

==如果所有的操作都执行成功，才会执行suceess方法==

如果有一个操作执行失败，则会执行fail方法

不论是成功还是失败，返回值是数组，数组中的每一个成员对应每一个promise返回的数据。

```js
    /* all()  Promise.all接受一个promise对象的数组，待全部完成之后，统一执行success;
      方法提供了并行执行异步操作的能力并且在所有异步操作执行完成之后才执行回调  应用;网站素材比较多的 等待图片 静态资源加载完成之后才可以进行页面初始化 */
    let p3 = new Promise((resolve, reject) => { })
    let p4 = new Promise((resolve, reject) => { })

    let pAll = Promise.all([p3, p4])
      .then(() => {
        // 所有都加载成功 才执行
      }).catch(err => {
        // 如果有一个加载失败则失败
      })

    // 案例:从两个不同的URL获取用户信息和好友
    let p5 = new Promise((resolve, reject) => {
      // 模拟资源加载
      setTimeout(() => {
        resolve('p1')
      }, 1000);
    })

    let p6 = new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve('p2')
      }, 2000);
    })

    Promise.all([p5, p6])
      .then((result) => {
        console.log(result);
      })
```

### 6.4 `race()`

> race 方法用于监听多个Promise对象
>
> 参数是一个数组，数组中的每一项都是一个Promise对象
>
> 我们可以通过then方法监听状态的改变（监听第一个promise对象状态的改变）

==race()  : 返回所有 promise 对象中,最先执行完成的 promise 对象的结果(不管成功还是失败,只看谁哪个 promise 对象最先执行完成)==

返回值是状态改变的时候传递的数据

```js
    // race()  接受包含多个Promise对象的数组 只要有一个完成 就执行success
    // 案例 当我们请求某个资源文件时 会导致时间过长 给用户反馈 用race 给某个请求设置超时时间 并且在超时后执行相应操作

    // 请求图片资源
    function requestImg(imgSrc) {
      return new Promise((resolve, reject) => {
        const img = new Image()
        // 加载图片资源
        img.onload = function () {
          resolve(img)
        }
        img.src = imgSrc
      })
    }

    // 延时函数 用于给请求计时
    function timeOut() {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          reject(new Error('请求超时'))
        }, 1000);
      })
    }

    // 如果1S之内加载完成就执行then() 否则执行catch()
    Promise.race([requestImg('https://profile.csdnimg.cn/F/0/7/2_weixin_57847059'), timeOut()])
      .then((data) => {
        document.body.appendChild(data)
      }).catch(err => {
        console.log(err);
      })
  </script>
```

### 6.5 `resolve()`

resolve 是Promise的静态方法，返回一个可以监听resolved状态的promise对象

- resolve()方法将现有对象转换成Promise对象
- 该实例的状态为 fulfilled 成功
- 只会执行成功的回调函数

### 6.6 `reject()`

reject 是Promise的静态方法，返回一个可以监听rejected状态的对象

- reject()方法将现有对象转换成Promise对象
- 该实例的状态为 rejected（失败）
- 只会执行失败的回调函数

```js
    // 1. resolve()  将现有任何对象转换成Promise对象 该实例状态为fulfiled 成功
    // let p = Promise.resolve('foo')

    // 等价于
    let p = new Promise(resolve => resolve('foo'))

    p.then((val) => {
      console.log(val);
    })

    // 2.reject() 该方法和resolve()一样返回一个新的Promise实例  该实例状态为reject 失败
    // let p2 = Promise.reject(new Error('出错了'))

    // 等价于
    let p2 = new Promise((resolve, reject) => reject(new Error('出错了')))
    p2.catch(err => {
      console.log(err);
    })
```

### 6.7 resolve 的参数

- js数据，此时then方法会立即执行（then方法接收的数据就是该数据）
- promise对象
- thenable参数 （带有then方法的对象）

```js
    <script>
        // 1.普通参数
        /* var p = Promise.resolve(12345)
        p.then(d => {
            console.log(d);
        }) */

        // 2.参数为 promise 对象
        /* var p1 = new Promise((resolve, reject) => {

            // resolve("调用了 resolve 回调");
            reject('调用了 reject 回调')
        });

        var p = Promise.resolve(p1)

        p.then(
            d => {
                console.log(d);
            },
            d => {
                console.log(d);
            },
        ) */

        // 3.参数为 对象
        /* var p = Promise.resolve({
            a: 123
        });

        p.then(
            d => {
                console.log(d);
            },
            d => {
                console.log(d);
            },
        ) */

        // 4.对象带有 then() 方法
        var p = Promise.resolve({
            then: function (a, b) {
                a(111);
                // b(222);
            }
            /* then1: function (a, b) {
                // a(333);
                b(444);
            } */
        });
        p.then(
            d => {
                console.log(d);
            },
            d => {
                console.log(d);
            },
        )

        /*
            Promise.resolve() 参数的三种情况：
                1.参数是一个任意类型的值
                    resolve() 会将这个值作为 promise 对象的成功回调的参数传入。

                2.参数是一个 promise 对象
                    resolve() 创建的 promise 对象的执行结果，由参数指定的 promise 对象的执行结果决定
                    
                3.参数是一个 有 then 方法的对象
                    会忽略掉这个对象的除 then() 方法之外的其它属性，并将这对象作为一个 promise 对象解析。
        */
```

