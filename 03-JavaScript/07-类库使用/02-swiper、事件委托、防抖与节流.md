## **1.- Swiper 类库**

- 插件: 就是别人写好的一些代码,我们只需要复制对应的代码,就可以直接实现对应的效果

- 学习插件的基本过程

- - 1.熟悉官网,了解这个插件可以完成什么需求 	https://www.swiper.com.cn/ 
  - 2.看在线演示,找到符合自己需求的demo		https://www.swiper.com.cn/demo/index.html
  - 3.查看基本使用流程 					https://www.swiper.com.cn/usage/index.html
  - 4.查看APi文档,去配置自己的插件 			https://www.swiper.com.cn/api/index.html
  - 5.注意: 多个swiper同时使用的时候, 类名需要注意区分

## **2.事件委托**

### **2.1.事件委托**

**事件委托：** 不直接在元素自身处理事件触发逻辑，而是委托给父元素，根据事件冒泡捕获机制，在父元素绑定事件处理函数中处理。

优点：

 减少绑定事件处理函数的数量

 预言未来元素（新增的元素也可以绑定事件），并可针对不同元素实现不同的功能。

 避免内存泄

### **2.2.jQuery中的事件委托**

**delegate：**

 第一个参数是被委托的元素（子元素）

 第二个参数是事件类型

 		（这里还可以传递自定数据）

 第三个参数是事件回调函数

本质上是调用的on方法，只不过他的委托语义化更强

```js
<script>
        $('button').click(function () {
            var text = $('textarea').val()
            $('.list').prepend(`<li>${text}<a href='#'>删除</a></li>`)
        })
        // jQuery 中的事件委托
        /* $('.list').click('a', function (e) {
            var target = e.target
            var tagName = target.nodeName.toLowerCase()
            if (tagName === 'a') {
                $(target).parent().remove()
            }
        }) */

        /*
            1. on()方法 事件委托
                第一个参数  事件类型
                第二个参数  真正要触发事件的dom元素
                第三个参数  事件处理函数
        */
        /* $('.list').on('click', 'a', function () {
            $(this).parent().remove()
        }) */

        /*
            2.delegate()    jQuery 中专门用来做事件委托的方法
                第一个参数  真正要触发事件的dom元素
                第二个参数  事件类型
                第三个参数  事件处理函数
        */
        $('.list').delegate('a', 'click', function () {
            $(this).parent().remove()
        })
    </script>
```

## **3. 节流与防抖**

### **3.1.防抖	类似于动画累计**

抖动： 当快速重复执行相同动画效果时，元素有可能会出现的抖动（尺寸或位置变化）或频闪（显示隐藏）现象。

防抖：防止抖动或频闪的出现。

方案一：先结束当前正在执行的动画，再开启新的动画。

方案二：检测如果有动画正在执行，就不允许执行新动画，直到动画执行结束。

### **3.2.节流**

节流是对防抖的进一步优化，当频繁触发事件时，在事件触发阶段，如果频繁执行事件处理函数，会造成不必要的内存占用。

节流：在事件触发阶段，先不执行代码，当事件触发停止时，再执行代码。

节流通常通过延迟定时器来实现。

- 返回顶部案例

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
            height: 3000px;
        }
        
        .toTop {
            width: 50px;
            font-size: 25px;
            background: red;
            color: #fff;
            padding: 5px 8px;
            cursor: pointer;
            position: fixed;
            bottom: 50px;
            right: 50px;
            display: none;
        }

        p {
            font-size: 24px;
            font-family: '楷体';
            line-height: 2;
            text-indent: 2em;
        }

        .safe {
            width: 1200px;
            margin: 0 auto;
        }
    </style>
</head>

<body>
    <div class="box safe">
        <h2>走近北京古桥｜高梁桥·琉璃河大桥·玉带桥·十七孔桥</h2>
        <p>北京从前水域众多，修筑有大小各式桥梁几百座。随着时光流逝，有些水域可能不复存在，部分桥梁也已消失，那些留存至今的古桥因而显得愈加宝贵。它们是古代劳动人民智慧的体现，也为今人了解过去提供了线索。文旅君今天邀请各位走近北京古桥梁，了解高粱桥、琉璃河大桥、玉带桥和十七孔桥的风采。
        </p>
    </div>
    <div class="toTop">返回顶部</div>

    <script src="./js/jquery-3.6.1.min.js"></script>
    <script>
        var $top = $('.toTop')
        var timer;

        $(window).scroll(function () {
            var y = $(window).scrollTop()

            // 节流：   减少不必要的代码执行
            // 当滑动滚动条停止时 才会显示返回顶部按钮
            clearTimeout(timer)
            timer = setTimeout(function () {
                // 1.解决抖动方案一： 在动画执行期间 不允许开启新的动画
                if (!$top.is(':animated')) {
                    if (y > 500) {
                        $top.fadeIn()
                    } else {
                        $top.fadeOut()
                    }
                }

                // 2.方案二：在开启新的动画之前 先关闭之前的动画    有弊端滑动过程中还是会显示
                /* if (y > 500) {
                    $top.stop(true).fadeIn(1200)
                } else {
                    $top.stop(true).fadeOut(1200)
                } */
            }, 200)
        })

        // 返回顶部
        $top.click(function () {
            $('html').animate({
                scrollTop: 0
            }, 200)
        })
    </script>
</body>

</html>
```

