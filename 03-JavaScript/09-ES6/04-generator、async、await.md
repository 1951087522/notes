## 一、 Gennerator 生成器

### 1.1 Generator 函数

> generator函数为处理异步编程提供了解决方法（异步函数），内部封装了大量的状态，允许我们逐条遍历

语法：`function *demo() { 函数中定义状态 }`

- 在函数内部通过yield关键字定义状态，yield表示暂停的意思

注意：yield关键字只能出现在generator函数中

- 通过return定义最后一个状态，return后面的状态不会执行

- generator函数的返回值为一个迭代对象，使用next()方法获取迭代对象的每一个值

- next方法的返回值是一个对象，包含两个属性：
  - done属性： 表示是否遍历完成，遍历完成值为true, 否则为false
  - value属性： 表示当前的状态值，如果遍历完成（done为true时），value始终为undefined.

- generator函数的返回值为迭代对象，通过for of方式遍历内部的状态

- 注意不要同时使用两种方式去遍历内部的状态，因为一方遍历完成，另一方就得不到状态了。

generator函数没有遍历完成的时候，状态为`<suspended>`；遍历完成之后，状态变为 `<closed>`

```js
    <script>
        // 1.定义 generator 函数的星号标识四种书写形式
        // function* fn1() {}
        // function *fn2() { }
        // function*fn3() { }
        // function * fn4 () {}

        // 2.function 函数，又称状态机
        function* fn1() {
            yield 10
            yield 20
            yield 30
            yield 40
        }
        var a = fn1()

        // 3.generator 函数的返回结果是一个状态对象(可迭代对象的一种)
        // 在可迭代对象还有值时，状态为 <suspended>
        console.log(a);
        console.log(a.next());
        console.log(a);
        console.log(a.next());
        console.log(a);
        console.log(a.next());
        console.log(a);
        console.log(a.next());  // {value: 40, done: false}
        console.log(a);         // fn1 {<suspended>}
        console.log(a.next());  // {value: undefined, done: true}
        // 在可迭代对象的值取完之后 状态为 {<closed>}
        console.log(a);         // fn1 {<closed>}


        function* fn2() {
            console.log(111);
            yield 1
            console.log(222);
            yield 2
            console.log(333);
            yield 3
            console.log(444);
            yield 4
            console.log(555);
            yield 123456
        }

        // 4.在执行 generator 函数本身，函数内部的代码并不会执行
        var b = fn2()

        /*
            只有调用 next() 方法，才会执行函数内部的代码
                并且调用一次next() 只会执行对应的一个 yield 关键字之前的代码，
                碰到 yield 关键字，执行会自动暂停，
                直到再次调用 next() ，才会执行到下一个 yield 关键字之前的代码，
                并且 yield 关键字后面的结果作为当前的 next() 返回值
        */
        console.log(b.next());
        console.log(b.next());
        console.log(b.next());
        console.log(b.next());
        console.log(b.next());
        // 当所有的 yield 之前的代码执行完成，和 yield 和结果都获取结束之后，再次调用 next() 如果有 return 会返回return 后面的值，否则返回 undefined 作为值
        console.log(b.next());
    </script>
```

### 1.2 数据传递

> 在generator函数中数据传递有两个方向：
>
> 1 数据由generator函数的内部流向外部
>
> 2 数据由generator函数的外部流向内部

数据由内部流向外部

- 1 通过yield表达式定义状态值

- 2 在外部通过next方法获取返回的对象，通过value属性获取值

数据由外部流向内部

- 1 在外部通过next方法传递数据

- 2 在内部通过yield表达式接收数据

```js
    <script>
        function* fn1() {
            // 1.yield 是将 函数内部的值传输到函数外部
            var a = yield 1
            var b = yield 2
            var c = yield 3
            var d = yield 4
            console.log(a, b, c, d);
            return [a, b, c, d]
        }

        var d = fn1()

        /* console.log(d.next());
        console.log(d.next());
        console.log(d.next());
        console.log(d.next());
        console.log(d.next());
        console.log(d.next()); */

        /* 
            2.next() 方法是将外部的值传输到函数内部
                可以将值传递给generator内部的上一个 yield
                因为第一次调用 next() 没有对应的上一个 yield，所以第一个 next() 方法的参数是无效的       
        */
        // 外部数据传递到内部
        function* demo(...args) {
            console.log(1, args);

            var data;

            // yield关键字接收数据
            // 1 向外传递数据（后面的），2 接收外部传递的数据（放在前面的变量中）
            data = yield 111;
            console.log(2, data);
            data = yield 222;
            console.log(3, data);
            data = yield 333;
            console.log(4, data);
            return 444;
            console.log(5, data);
        }
        
        var d = demo(100, 200);
        // 向内部传递
        // 第一次执行next表示启动状态机，因此传递数据无效
        d.next(300);   // 到111处暂停了
        d.next(400);   // 到222处暂停了， 被111yield接收
        d.next(500);   // 到333处暂停了， 被222yield接收
        d.next(600);   // 到444处暂停了， 被333yield接收
        d.next(700);   // 已经到了return后面就不会在执行了
    </script>
```

### 1.3 return

在generator函数的提供了两种提前终止取值的方式：

1，在外部调用`return()`方法提前终止；

​		在外部使用时，可以多次使用

2，在内部调用`return`关键字提前终止；

​		在内部使用时，return 只能使用一次

如果 Generator 函数内部有`try...finally`代码块，且正在执行`try`代码块，那么`return()`方法会导致立刻进入`finally`代码块，执行完以后，整个函数才会结束。

```js
    <script>
        function* fn1() {
            yield 1
            yield 2
            yield 3
            yield 4
            // 1.在函数内部使用 return
            // return: 提前结束 generator 函数的执行
            // 在内部使用时，return 只能使用一次
            return 100
            yield 5
            yield 6
            yield 7
            yield 8
        }
        var g = fn1()

        console.log(g.next());
        console.log(g.next());
        console.log(g.next());
        console.log(g.next());
        console.log(g.next());
        // 在外部使用 return
        // 在外部使用时，可以多次使用
        console.log(g.return());
        console.log(g.return(200));
        console.log(g.return(300));
        console.log(g.next());
        console.log(g.next());

        /* 
            return: 提前结束 generator 函数的执行
                在内部使用时，return 只能使用一次
                在外部使用时，可以多次使用
        */
    </script>
```

### 1.4 throw

generator函数生成的可迭代对象都有一个`throw()`方法，可以在函数体外抛出错误，然后在 Generator 函数体内捕获。

1，必须至少调用一次next()方法，才能调用`throw()`，否则直接抛出错误。

2，`throw()` 与 `yield` 是一一对应的，必须使用 `try...catch`语句捕获`throw()`方法抛出的错误。

3，`throw()`方法返回值与`next()`相同，并且会自动执行一次`next()`操作。

4，`try...catch`语句可以在函数内部使用，也可以在外部使用。在内部时，`yield`应该写在`try...catch`中； 在外部时，`throw()`方法应该写在`try...catch`中。

5，每个`try...catch`语句只会捕获一次`throw()`方法。

6，全局的 `throw` 关键字 与 `generator` 没有关系， 抛出的错误不会被内部的 `tr...catch` 捕获。

```js
    <script>
        function* fn1() {
            yield 1
            yield 2

            /* 
                在函数内部必须 try...catch() 语句处理 throw() 的错误信息
                    如果没有错误，会执行 try 分支，而不执行 catch 分支
            */
            try {
                yield 3
                yield 4
                yield 5
                // 如果有错误，在try分支中的抛出错误语句之后的语句都不执行，执行catch分支的语句
            } catch (error) {
                console.log('错误');
                yield 6
                yield 7
                // 不管有没有错误，finally 分支中的语句都会执行
            } finally {
                yield 8
                yield 9
            }
            // 如果有错误，在try分支中的抛出错误语句之后的语句都不执行，执行catch分支的语句
            yield 10
            yield 11
        }

        var g = fn1()
        console.log(g.next());
        console.log(g.next());
        console.log(g.next());
        console.log(g.throw());
        console.log(g.next());
        console.log(g.next());

        // 2.全局的 throw 是一个关键字 
        // generator 中的 throw() 是一个方法
        try {
            throw new Error('全局的错误信息')
        } catch (err) {
            console.log(err);
        }

        console.log(g.next());
        console.log(g.next());
        console.log(g.next());
        console.log(g.next());
        console.log('这是函数外部的执行语句');


        // throw() 往 generator 函数内部输出一条错误信息
    </script>
```

### 1.5 yield*

> yield *语法可以引用另一个generator函数（当前函数暂停执行，等引入的函数执行完成，再执行当前函数）。

```js
    <script>
        function* fn1() {
            yield 10
            yield 20
            yield 30
            yield 40
        }
        function* fn2() {
            yield 'aaa'
            yield 'bbb'
            yield 'ccc'
            yield 'ddd'
        }
        // 分别执行两个 generator 函数
        var g1 = fn1()
        var g2 = fn2()

        console.log(g1.next());
        console.log(g1.next());
        console.log(g1.next());
        console.log(g1.next());
        console.log(g1.next());

        console.log(g2.next());
        console.log(g2.next());
        console.log(g2.next());
        console.log(g2.next());
        console.log(g2.next());

        // 1.yield *语法可以引用另一个generator函数（当前函数暂停执行，等引入的函数执行完成，再执行当前函数）
        function* fn3() {
            yield 1

            // 在函数内部引用另一个 generator函数 yield*
            yield* fn4()

            yield 2
            yield 3
        }
        function* fn4() {
            yield 'a'
            yield 'b'
            yield 'c'
        }
        var g3 = fn3()
        console.log(g3.next());
        console.log(g3.next());
        console.log(g3.next());
        console.log(g3.next());
        console.log(g3.next());
        console.log(g3.next());
        console.log(g3.next());

        // 2.通过两个 generator 函数相互引用 实现无限循环取值
        function* fn5() {
            yield 'q'
            yield 'w'
            yield 'e'
            // 在函数内部引用另一个 generator 函数
            yield* fn6()
        }
        function* fn6() {
            yield 'r'
            yield 't'
            yield 'y'
            // 在函数内部引用另一个 generator 函数
            yield* fn5()
        }

        var g5 = fn5()
        console.log(g5.next());
        console.log(g5.next());
        console.log(g5.next());
        console.log(g5.next());
        console.log(g5.next());
        console.log(g5.next());
        console.log(g5.next());
    </script>
```

### 1.6 扩展运算符与for...of循环

使用扩展运算符和`for...of`可以提取或遍历`generator`函数返回的迭代对象的所有值。

```js
    <script>
        function* fn1() {
            yield 1
            yield 2
            yield 3
            yield 4
        }

        // 普通的取值方式
        var g = fn1()
        /* console.log(g.next());
        console.log(g.next());
        console.log(g.next());
        console.log(g.next());
        console.log(g.next()); */

        // 1.使用 for...of 遍历取值
        /* for (var val of g) {
            console.log(val);
        } */

        // 2.使用扩展运算符 取值
        var result = [...g]
        console.log(result);

        // 不论哪一种取值方式 迭代对象的值只能取一次
    </script>
```



### 1.7 this

在generator函数中的this默认指向window。

### 1.8 new

generator函数不能作为构造函数使用，无法使用 new 运算符调用。

```js
    <script>
        function* fn1() {
            // 1.generator 函数中的 this 默认执行window
            console.log(this);
        }
        fn1().next()

        // 2.可以通过 call() / apply() / bind() / 修改this 的值

        // call() / apply() 修改this 的值
        fn1.call(document).next()
        fn1.apply(document).next()

        var g = fn1.call({ a: 123 })
        g.next()
        var g = fn1.apply({ a: 123 })
        g.next()

        // bind()修改this 的值
        var g = fn1.bind({ a: 123 })()
        g.next()

        // 3.generator 函数不能作为构造函数使用
        // var obj = new fn1
    </script>
```

## 二、Co模块

### 2.1 co 模块

> Co模块是为了简化状态函数的启动过程
>
> 在ES6中想要使用co模块的话， 需要下载该模块：`npm install co`
>
> 下载完毕之后，将`co.js`文件引入到页面中即可

虽然，当前的方式确实可以实现我们的需求，但是状态与函数之间是强耦合，于是ES6为了解决这个问题

提供了co模块，又提供了co方法，用于简化状态函数的启动

当调用了co方法之后，可以通过then方法监听状态的改变

```js
<!-- 
        Co模块是为了简化状态函数的启动过程
            在ES6中想要使用co模块的话， 需要下载该模块：npm install co
            下载完毕之后，将co.js文件引入到页面中即可
            虽然，当前的方式确实可以实现我们的需求，
            但是状态与函数之间是强耦合，于是ES6为了解决这个问题,提供了co模块，又提供了co方法，用于简化状态函数的启动.当调用了co方法之后，可以通过then方法监听状态的改变
    -->
    <script src="../co.js"></script>
    <script>
        function* fn1() {
            var val1 = yield Promise.resolve(100);
            var val2 = yield Promise.resolve(200);
            return [val1, val2];
        }

        // 普通的方式取值
        /* var ge = fn1();
        // var p1 = ge.next();
        var p1 = ge.next().value;
        var p2 = ge.next().value;

        // console.log( p1 );
        p1.then(data => {
            console.log(data);
        });
        p2.then(data => {
            console.log(data);
        }); */

        // 1.通过 co 模块取值
        co(fn1).then(data => console.log(data))


        // 2. co 模块只能处理 yield 后面为 promise / generator / array / object / function 类型的值
        // 普通的值无法取值
        /* function* fn1() {
            yield 100;
            yield 200;
        }
        co(fn1).then(data => console.log(data)) */
    </script>
```

## 三、异步函数

### 3.1 async 与 await

async和await是ES2016（ES7）中提出的

 可以认为是generator函数的语法糖

语法糖：对一些复杂操作的简化，可以使我们用更简单的方式去操作，提高了开发效率

 async 表示函数中有异步操作，代表了 * 语法

 await 表示等一等的意思，只有当前程序执行完毕之后，后续代码才会执行，代表了 yield关键字

- 特点：

   1 提高了代码的语义化

   2 await返回值是Promise对象

   3 await后面允许是任何数据

   4 generator表示状态机，async定义的是异步函数

   5 在函数中内置状态函数的启动，直接执行函数即可，不需要通过next方法执行

当程序执行到await的时候，会交出程序的控制权，只有当异步操作完毕之后，后续的代码才会执行

 如果await后面出现了其它数据，会返回一个监听resolved状态的promise对象

 如果函数中出现了错误，会将错误信息追踪到错误队列中

- 返回对象

   awiat返回值是一个promise对象

   可以使用then方法监听成功时候状态

   可以通过catch方法监听失败时候的状态

 await与yield一样：

 await只能出现在async中 yield只能出现在generator函数中

```js
 <script>
    async function test() {

    }
    let result = test()
    // 1.凡是在函数前面添加了async 即便函数内部什么都没有 执行后都会自动返回一个 Promise对象
    console.log(result);


    /* 
      2.await 
        1).await 必须放在async函数中 (async函数可以没有await)
        2).await 右侧的表达式一般为promise对象
        3).await 返回的是promise成功的值
        4).如果 async 函数中有多个 await , 那么 then 函数 会等待所有的 await 指令运行完才去执行
        5).await 右侧的 promise失败了 就会抛出异常 那么整个async函数都会中断执行。 需要通过 try...catch 解决
    */

    // 2).如果 async 函数中有多个 await, 那么 then 函数 会等待所有的 await 指令运行完才去执行
    async function f() {
      let s = await 'hello async'
      let data = await s.split('');
      return data;
    }
    f().then(v => { console.log(v) })
      .catch(e => { console.log(e) });

    // 创建Promise对象
    const p = new Promise((resolve, reject) => {
      // 成功的值
      // resolve('hello async')

      // 如果值是失败的值 需要通过 try...catch处理
      reject('失败')
    })

    async function main() {
      try {
        // await 必须放在async函数中 (async函数可以没有await)
        // await 右侧的表达式返回的是promise成功的值
        let result = await p
        console.log(result);

      } catch (e) {
        console.log(e);
      }
    }
    main()
  </script>
```





















