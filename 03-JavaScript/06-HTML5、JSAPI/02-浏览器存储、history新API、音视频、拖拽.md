## **1.- 浏览器存储：**

[L:\AIC\notes\03-JavaScript\03-BOM]: 

## **2.- history新API**

**pushState： 通过JS方式向历史记录中添加新的记录。（注：状态改变不需要刷新页面）**

使用方式：history.pushState(obj, str, url)

- obj: 添加的数据 是一个对象
- str: 一个字符串参数，目前所有浏览器都不会处理该参数
- url: 新的网页的url (不指定该参数，默认使用当前页面的url地址)

replaceState： 替换当前的历史记录

使用方式：history.pushState(obj, str, url)

- obj: 添加的数据 是一个对象
- str: 一个字符串参数，目前所有浏览器都不会处理该参数
- url: 新的网页的url (不指定该参数，默认使用当前页面的url地址)

注意： pushState() / replaceState() 必须是在服务器环境中使用，在本地环境使用会抛出异常。（vscode中通过 live server 打开页面，就是在服务器环境中

```js
    <script>

        /* 
            history 的记录默认有访问 a 链接产生
            pushState() 往历史记录中添加一条新纪录
            replaceState()  使用指定的记录替换当前页面的记录

            pushState() 与 replaceState() 的参数
                第一个参数 跟记录一起附带添加的数据
                第二个参数 无效参数 目前所有浏览器都不会使用这个参数的数据(虽然无效 但不能省略)
                第三个参数 要添加记录的页面地址
        */
        var btns = document.querySelectorAll('input')
        btns[0].onclick = function () {
            // history.pushState({ abc: 123 }, '', 'demo.html')
            
            history.replaceState({ abc: 123 }, '', 'demo.html')
            
            console.log(history)
        }
    </script>
```



```js
<body>
    <h2>popstate事件</h2>
    <a href="./prev.html">prev.html</a>
    <a href="./next.html">next.html</a>
    <input type="button" value="demo.html">
    <script>


        /* 
            onpopstate ： 当访问由 pushState() 或 replaceState() 方法添加的历史记录页面时，自动触发的事件。
            popstate 事件的事件对象中的 state 属性可以获取到 pushState() / replaceState() 附带的数据。

            访问通过 a 链接产生的历史记录页面，不会触发 popstate 事件，但会自动刷新和加载页面。

            访问通过 pushState() / replaceState() 产生的历史记录页面，会触发 popstate 事件，但不会自动刷新和加载页面。

            注意： onpopstate 事件需要在服务器环境运行使用。
        */
        console.log(history);
        var btn = document.querySelector('input')
        btn.onclick = function () {
            history.pushState({ name: 'demo' }, '', 'demo.html')
            console.log(history);

        }
        window.onpopstate = function (ev) {
            console.log('触发了', ev);
        }
    </script>
</body>
```



## **3.- 多媒体API**

方法：

- play() 播放
- pause() 暂停

属性：

- duration 总时长	只读属性
- currentTime 当前时长
- volume 音量（0-1之间的数）
- muted 是否静音
- paused 是否暂停
- 音频 构造函数 	

```js
var audio = new Audio()
```

- 视频 标签

```js
<video src="./video/1.mp4"></video>
```

## **4.- 拖拽事件**

drag: 拖拽（高频事件）

dragenter: 拖拽进入

dragleave: 拖拽离开

dragstart: 拖拽开始

dragend: 拖拽结束

dragover：悬浮

drop: 丢弃事件。

- 注：dragover 事件中的默认行为阻止会 drop 事件触发，导致 drop 事件默认不能够执行，所以要给一个元素添加 drop 事件，必须要阻止该元素 dragover 事件的默认事件

```js
<script>
        var div1 = document.querySelector('#div1')
        var div2 = document.querySelector('#div2')

        // 拖拽事件 drag 需要配合标签属性 draggable=true
        // div1.ondrag = function () {
        //     console.log('div1 ondragover');
        // }

        // 当拖拽元素进入元素触发的事件 悬停事件
        // div1.ondragover = function () {
        //     console.log('div1 ondragover');
        // }
        // div2.ondragover = function () {
        //     console.log('div2 ondragover');
        // }

        // 进入触发事件
        // div1.ondragenter = function () {
        //     console.log('div1 ondragenter');
        // }
        // div2.ondragenter = function () {
        //     console.log('div2 ondragenter');
        // }

        // 离开触发事件
        // div1.ondragleave = function () {
        //     console.log('div1 ondragleave');
        // }
        // div2.ondragleave = function () {
        //     console.log('div2 ondragleave');
        // }

        /*// 拖拽开始
        div1.ondragstart = function () {
            console.log('div1 ondragstart');
        }
        // 拖拽结束
        div1.ondragend = function () {
            console.log('div2 ondragend');
        }
        div2.ondragstart = function () {
            console.log('div1 ondragstart');
        }
        div2.ondragend = function () {
            console.log('div2 ondragend');
        } */

        // drop: 丢弃事件。 会受到 dragover 影响
        div1.ondrop = function () {
            console.log('div1 ondrop');
        }
        // 取消被触发元素的默认行为
        div2.ondragover = function (e) {
            e.preventDefault()
        }
        div2.ondrop = function () {
            console.log('div2 ondrop');
        }
    </script>
```

