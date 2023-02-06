## **1. jQuery 的使用**

```js
<body>
    <div></div>
    <!-- 1.导包 -->
    <script src="./jQuery.min.js"></script>
    <!-- 测试 -->
    <script>
        // 2.方式一：等页面DOM加载完毕再执行js代码
        $(document).ready(function(){
            $('div').hide()
        })

        // 2.方式二：等页面DOM加载完毕再执行JS代码
        $(function(){
            $('div').hide()
        })

        // 隐藏 div
        // $('div').hide()
    </script>

</body>
```

## **2. jQuery 的顶级对象**

```js
<body>
    <div></div>
    <script>
        // $ jQuery的顶级对象

        // 使用 jQuery    都可以
        jQuery(function () {
            jQuery('div').hide()
        })

        // 使用 $    都可以
        $(function () {
            $('div').hide()
        })
    </script>
</body>
```

## **3. jQuery 对象 和 DOM 对象**

```js
<body>
    <div></div>
    <span></span>
    <script>
        // 1.DOM 对象   用原生js获取过来的对象就是DOM对象
        var div = document.querySelector('div')
        console.dir(div)
        // 2.jQuery 对象 用jQuery的方式获取过来的对象就是jQuery对象 本质是通过$ 把DOM元素进行包装
        $('div')
        console.dir($('div'))
        // 3.jQuery 对象只能使用jQuery方法  DOM对象则使用原生的JavaScript的属性和方法
        // div.hide()  // div 是一个DOM 对象，不能使用jquery 的对象
        $('div').style.display = 'none'     //$('div') 是jQuery 对象 不能使用JavaScript的属性和方法
    </script>
</body>
```

## **4. jQuery 对象转换为 Dom 对象**

```js
方式一：$('video')[index]   index 是索引号
方式二：$('video').get(index)   index 是索引号

<body>
    <video src="./video/1.mp4" muted></video>
    <script>
        // 1.Dom 对象转换为jQuery对象
        // (1)  直接获取视频元素 得到的就是jQuery 对象
        // $('video')
        // (2)  使用原生js 获取
        var video = document.querySelector('video')
        // 2. jQuery 对象转换为 DOM对象
        // 方式一：$('video')[index]   index 是索引号
        // $('video')[0].play()
        // 方式二：$('video').get(index)   index 是索引号
        // $('video').get(0).play()
    </script>
</body>
```

## **5.- jQuery 常用API**

- jq 1.x API 	http://hemin.cn/jq/
- jq 3.x API		https://jquery.cuishifeng.cn/

### **5.1 jQuery 选择器**

![image-20221218172149875](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218172149875.png)

![image-20221218172156810](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218172156810.png)

```js
    <script>
        console.log($('ul li'));
        console.log($('.checked'));
    </script>
```

- **隐式迭代**

- - 给匹配到的所有的元素进行循环遍历 执行相应的方法 	不需要我们再进行循环

```js
<script>
        // 给匹配到的所有元素进行循环遍历   执行相应的方法  不需要我们再进行循环
        $('ul li').css('background', 'pink')
        $('p').css('color', 'gold')
</script>
```

#### **筛选择器**

![image-20221218172235289](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218172235289.png)

```js
    <script>
        $('ul li:first()').css('color', 'red')

        $('ul li:last()').css('color', 'pink')

        $('ul li:eq(3)').css('background', 'gold')

        $('ol li:odd()').css('background', 'hotpink')

        $('ol li:even()').css('background', 'green')
    </script>
```

#### **筛选方法**

![image-20221218172302933](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20221218172302933.png)

### **5.2** **jQuery 内容获取与设置**

- **size() ： 返回 jQuery 对象的 length 值。**
- **html() : 获取第一个普通元素的内容，或者修改所有普通元素的内容。**

- **val() : 获取第一表单元素的内容，或者修改所有表单元素的内容。**

```js
    <script>
        // 1.html()   获取普通标签的inner HTML 内容 只能获取第一个
        console.log($('h2').html());

        // 2.val()  获取表单元素的 value 内容
        console.log($('input').val());

        // 3.获取元素的数量 length
        console.log($('li').length);

        // 4.window.onlonad 用法
        // 等页面加载完毕执行的
        $(function () {
            console.log(123);
        })
    </script>
```

### **5.3** **jQuery 属性操作**

**addClass() :** 给所有元素添加className。

**removeClass() :** 移除所有元素的className，不传递参数会清空所有class。

**hasClass() :** 所有元素都没有指定class，返回 false, 否则返回 true。

**toggleClass() :** 切换执行 addClass() 和 removeClass()。

**attr() :** 获取第一个元素的指定属性的值，或修改所有元素的指定属性值。

**attr()** 获取和设置的是字符串值， 布尔类型的值使用 prop() 方法。

**removeAttr() :** 删除元素的指定属性，不指定参数则不执行该方法（不用处理关键字）。

由 prop() 方法添加的属性，建议使用 **removeProp()** 方法删除。

```js
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #div1 {
            width: 200px;
            height: 200px;
            background: red;
        }

        #div1.font {
            font-size: 30px;
            color: #fff;
        }

        #div1.active {
            text-align: center;
            background: orange;
            line-height: 200px;
        }

        #div1.pos {
            position: relative;
            left: 100px;
            top: 50px;
        }
    </style>
</head>

<body>
    <div id="div1" class="font pos" abc="12345">jQuery学习</div>
    <input type="text" value="文本框">

    <script src="./js/jquery-3.6.1.min.js"></script>
    <script>
        // $('input').prop('disabled',true)

        // 1.prop()   添加的自定义属性不会显示在行间
        $('input').prop('abc', '1000')

        $('#div1').click(function () {
            // 所有事件处理函数 this 都是原生DOM对象
            // console.log(this);

            // 2.添加class
            // $(this).addClass('active')

            // 3.可以同时添加多个 class
            // $(this).addClass('active pos');

            // 4.删除指定 class
            // $(this).removeClass('font')

            // 5.删除多个 class
            // $(this).removeClass('font pos')

            // 6.判断有没有指定的 class
            // console.log($(this).hasClass('font'));
            // console.log($(this).hasClass('pos'));

            // 7.根据判断 添加或删除 class
            /* if ($(this).hasClass('active')) {
                $(this).removeClass('active')
            } else {
                $(this).addClass('active')
            } */

            // 8.有则删无则加
            // $(this).toggleClass('active')

            // 9.同时对多个class 进行toggle() 操作
            // $(this).toggleClass('active pos')

            // console.log('----------------------------');

            // 10.添加或修改属性(包括自定义属性)
            // $(this).attr('abc', 789)
            // $(this).attr('id', 'div2')

            // 11.删除指定属性
            // $(this).removeAttr('class')

            // 12.removeAttr 与 removeClass 不同处在于 
            // 前者直接删除整个属性 后置只删除属性的指定值
            // $(this).removeClass('font pos')

            // 13.获取属性的值  
            // console.log($(this).attr('id'));
            // console.log($(this).attr('disabled'));

            // 14.prop()    获取或设置是布尔类型的属性
            // console.log($('input').prop('disabled'));
            // console.log($('input').prop('abc'));

            // 15.删除由prop()  方法添加的自定义属性
            $('input').removeProp('abc')
            console.log($('input').prop('abc'));
        })
    </script>
</body>

</html>
```

### **5.4** **jQuery 尺寸、位置操作**

**尺寸**

**width() ：** 计算后样式 width 的值 （getComputed().width）

**height() :** 计算后样式 height 的值 (getComputed().height)

**innerWidth() ：** 包含滚动条的 clientWidth 值

**innerHeight ():** 包含滚动条的 clientHeight 值

**outerWidth() :** 默认获取的是 innerWidth + 边框

- **outerWidth(true)** 传递参数 true , 会额外加上margin的值

**outerHeight() :** 默认获取的是 innerHeight + 边框

- **outerHeight(true)** 传递参数 true , 会额外加上margin的值

```js
    <script>
        // jQuery 对象
        var $div1 = $('#div1')
        // 从jQuery 对象中获取DOM 对象
        var div1 = $div1[0]

        console.log($div1);

        // 1.元素自身的 不包含滚动条的宽高
        console.log($div1.width(), $div1.height());

        // 2.原生的 可视区域 + 滚动条的 宽高
        console.log($div1.innerWidth(), $div1.innerHeight());

        console.log(div1.clientWidth, div1.clientHeight);

        // 3.innerWidhth() / innerHeight()  +border 尺寸
        console.log($div1.outerWidth(), $div1.outerWidth());
        console.log(div1.offsetWidth, div1.offsetHeight);

        // 4.传入参数   width() / height() + padding + border + margin 的尺寸
        console.log($div1.outerWidth(true), $div1.outerHeight(true));

    </script>
```

**位置**

**position() :** 元素到 offsetParent 最近一级定位之间的距离 （{left: 0, top: 0}）

该方法对于 display: none; 和 position: fixed; 的元素计算不准确

**offset() :** 元素到整个文档的左边或顶边之间的距离 （{left: 0, top: 0}）

该方法对设置了 display: none; 的元素无效。

```js
    <script>
        // 1.position()     当前元素到 offsetParent() 最近一级定位之间的距离
        console.log($(div3).position());

        // 2.offset()   当前元素到文档边缘的距离
        console.log($(div3).offset());

        // position()   / offset()  当元素设置 display :none 时 值为0
    </script>
```

### **5.5** **jQuery 绑定事件**

**on() ：** 绑定事件处理函数.

语法： on(事件名称, 事件处理函数)

事件名称不需要 'on' 前缀，如： $('#div1').on('click', function(){});

on() 是绑定事件的通用方法，jquery 还提供直接以事件名作为方法来绑定对应事件函数，如:

**click() :** 给点击事件绑定处理函数。

**mouseover() :** 给鼠标移入事件绑定处理函数。

**keydown()：** 给键盘按下事件绑定处理函数。

所有常用事件都有对应的以事件名称为方法名称的方法来绑定处理函数。

**one() :** 绑定只执行一次的事件处理函数。

```js
<body>
    <input type="button" value="点击">
    <script src="./js/jquery-3.6.1.min.js"></script>

    <script>
        var $btn = $('input')

        // 1.jquery 默认使用的是 addEventListener()   方式绑定事件处理函数 所以同名事件都会触发
        /* $btn.on('click', function () {
            console.log('123');
        })
        $btn.on('click', function () {
            console.log('456');
        }) */


        // 2.绑定只执行一次的事件处理函数
        /* $btn.one('click', function () {
            console.log('789');
        }) */

        // 3.给特定的事件类型绑定事件处理函数
        /* $btn.click(function () {
            console.log('点击');
        })
        $btn.mousedown(function () {
            console.log('悬停');
        })
        $btn.mouseover(function () {
            console.log('离开');
        }) */

        // 4.组合事件
        /* $btn.hover(function () {
            console.log('123');
        }), function () {
            console.log('456');
        } */

        /* 
            jQuery 绑定事件的方式
                1.通用方法
                    on(事件类型，事件处理函数)：给指定的事件类型绑定事件处理函数

                    one(事件类型，事件处理函数)：绑定只执行一次的事件处理函数

                2.根据事件类型名称，绑定事件处理函数
                    click()
                    mouseover()
                    mouseleave()
                    ...
        */
    </script>
</body>
```

### **5.6** **jQuery 注销事件**

**off():** 注销事件处理

传递参数表示注销指定类型事件的处理函数，可以使用空格分隔来注销多个类型的事件的处理函数。

不传递参数会注销所有事件处理函数

```js
<body>
    <input type="button" value="点击">
    <script src="./js/jquery-3.6.1.min.js"></script>
    <script>
        var $btn = $('input')
        function fn1() {
            console.log('fn1 函数被调用');
        }
        function fn2() {
            console.log('fn2 函数被调用');
        }
        function fn3() {
            console.log('fn3 函数被调用');
        }
        function fn4() {
            console.log('fn4 函数被调用');
        }

        $btn.click(fn1)
        $btn.click(fn2)
        $btn.mousedown(fn3)
        $btn.mouseleave(fn4)

        // off()    不能单独柱形绑定的匿名函数
        /* $btn.on('click', function () {
            console.log(123);
        }) */

        // 注销事件处理函数
        // 1.不设置参数 表示注销所有事件处理函数
        // $btn.off()

        // 2.只指定一个参数 注销指定事件类型的所有事件处理函数
        // $btn.off('click')

        // 3.指定两个参数 注销事件类型的特定事件处理函数
        // $btn.off('click', fn1)

        // 4.同时注销多个事件类型的事件处理函数(空格分隔)
        $btn.off('click mouseout')
    </script>
</body>
```

### **5.7 节点操作**

#### **5.7.1 获取节点**

**$(this) :** 将原生 this 对象 包装成 jQuery 对象。

**parent() :** 获取已选中的所有元素的直接父节点，可以传递参数来筛选父节点。

**children() :** 获取已选中的所有元素的直接子节点，可以传递参数来筛选子节点。

**siblings()** **:** 获取已选中的所有元素的所有同级兄弟节点，可以传递参数来筛选兄弟节点。

**first() :** 返回 jQuery 对象中的第一个元素 （jQuery对象）。

**last()** **:** 返回 jQuery 对象中的最后一个元素 （jQuery对象）。

**next()** **:** 获取与已选中的所有元素的相邻的下一个兄弟节点，可以传递参数来筛选兄弟节点。

**nextAll() :** 获取已选中的所有元素后面的所有兄弟节点，可以传递参数来筛选兄弟节点。

**prev() :** 获取与已选中的所有元素的相邻的上一个兄弟节点，可以传递参数来筛选兄弟节点。

**prevAll()** **:** 获取已选中的所有元素前面的所有兄弟节点，可以传递参数来筛选兄弟节点。

**parents() :** 获取已选中的所有元素的所有祖先节点，可以传递参数来筛选祖先节点。

**find() :** 获取已选中的所有元素的指定后代节点，不传递参数将不会选中任何后代节点。

**eq(index) :** 从 jQuery 对象中选中指定的元素，index 表示要选中的元素的位置，正数表示从前往后查找，负数表示从后往前查找。

以上所有方法都支持链式操作，并且可以使用 end() 方法回退链式操作。

```js
<script>
        // 1.parents()    选中元素的所有祖先节点
        // console.log($('li.focus').parents());

        // 2.chidlren()     获取选中元素的所有子节点
        // console.log($('div').children());

        // 3.获取选中元素的第一个元素   返回值是jQuery 对象
        // console.log($('li').first());

        // 4.获取选中元素的最后一个元素 返回值是 jQuery 对象
        // console.log($('li').last());

        // 5.获取选中元素所有兄弟节点
        // $('li.focus').siblings().css('background', 'pink')

        // 6.获取选中元素的上一个兄弟节点
        // $('li.focus').prev().css('background', 'gold')

        // 7.获取选中元素的下一个兄弟节点
        // $('li.focus').next().css('color', 'red')

        // 8.获取选中元素上面所有的兄弟节点
        // $('li.focus').prevAll().css('color', 'gold')

        // 9.获取选中元素下面所有的兄弟节点
        // $('li.focus').nextAll().css('color', 'hotpink')

        // 10.获取选中元素的子节点
        // $('div').children().css('border', '1px solid #333')

        // 11.获取选中元素所有的后代节点
        // $('div').find('*').css('border', '1px dashed #333')

        // 12.获取选中元素的所有后代节点中的li节点
        // $('div').find('li').css('color', 'red')

        // 13.find() 必须传递参数指定 获取哪些后代节点
        // $('div').find().css('color', 'gold')

        // 14.获取所有选中元素中的指定元素 (参数就是要选择的元素位置 支持负数)
        $('li').eq(2).css('border', '1px dashed #333')
        $('li').eq(-2).css('border', '1px dashed #333')
    </script>
```

**end() ：** 回退到上一个链式操作的节点操作，如果end() 前面没有链式操作，返回空的 jquery 对象。

```js
    <script>
        // 1.链式操作 基于上一个选择器，串联地获取节点或操作节点样式和属性的写法
        $('.main').css('border', '1px solid #333').find('ul:first').css('border', '1px dashed blue').children().css('border', '2px dotted gold').find('ul').css('border', '1px solid red')

        // 2.链式操作格式化写法
        $('.main').css('border', '1px solid #333')
            .find('ul:first')
            .css('border', '1px dashed blue')
            .children()
            .css('border', '2px dotted gold')
            .find('ul')
            .css('border', '1px solid red')

        // 3.end()  返回到链式操作中的上一个选择器
        $('.main').css('border', '1px solid #333')
            .find('ul:first')
            .css('border', '1px dashed blue')
            .children()
            .css('border', '2px dotted gold')
            .find('ul')
            .css('border', '1px solid red')
            // 一个end()    返回一级 多个返回多次
            // end() 前面没有链式操作，返回空的 jquery 对象。
            .end()
            .end()
            .css('background', 'skyblue')
    </script>
```

**get(index) :** 返回指定jQuery对象的原生DOM对象，不指定参数则返回所有原生DOM对象。

```js
    <script>
        var $lis = $('li')

        // 1.eq() 返回的是 jQuery 对象
        console.log($lis.eq(2));
        $lis.eq(2).css('border', '1px solid #333')
        // eq 必须传递参数 来指定要获取第几个 jQuery 对象
        console.log($lis.eq());

        // 2.get(index) 返回的是原生 DOM 对象
        console.log($lis.get(2));
        // 原生DOM 不能使用 jQuery的方法
        // $lis.get(2).css('color', red)
        // get() 不指定参数 返回包含所有原生 DOM 对象的数组
        console.log($lis.get());
    </script>
```

**元素遍历：**

**each(fn) :** 遍历已选中的所有 jQuery 对象。

参数： fn 遍历时自动调用的函数。

**each()** 方法会自动传递两个参数给 fn 函数：

第一个参数： 当前遍历元素的索引

第二个参数： 当前遍历元素的原生 dom 对象。

```js
    <script>
        // each() 方法 是对 for 和 for...in 语句的封装
        var arr = [1, 2, 3, 4]
        var obj = { a: 100, b: 200, c: 300 }

        // 原生遍历数组和对象
        /* for (var i = 0; i < arr.length; i++) {
            console.log(i, arr[i]);
        }

        for (var j in obj) {
            console.log(j, obj[j]);
        } */

        // 1. jQuery 遍历 数组和对象
        $.each(arr, function (index, val) {
            console.log(index, val);
        })
        $.each(obj, function (index, val) {
            console.log(index, val);
        })

        // 2.jQuery 遍历dom 对象
        var $lis = $('li')
        console.log($lis.length);

        $.each($lis, function (index, val) {
            $(val).css('border', '1px solid red')
        })
    </script>
```

**元素索引 index()**

**index()** 方法返回指定元素的相对于兄弟节点的下标，如果没有匹配的元素，则返回 -1。

该方法接收一个参数，也可以不传递参数。

**不传递参数：**返回第一个 jq 对象在它的所有兄弟节点中的位置。

**传递参数：** 返回 第一个 jq 对象在所有具有参数指定的选择器的兄弟节点中的位置。

```js
    <script>
        var $lis = $('li');
        /* 
            index()
                1.设置参数
                    在参数指定的元素范围之中的索引
                        如果选中元素不再参数指定范围之内 返回-1
                2.不设置参数
                    在所有同级元素的索引
        */
        $lis.click(function () {
            // 1.index()    默认表示在所有同级节点中的索引
            console.log($(this).index());
        })

        $('h2').click(function () {
            // 2.设置参数 在参数指定范围之中的索引
            console.log(($(this).index('h2')));
        })
    </script>
```

#### **5.7.2 添加、删除、替换、克隆节点**

- 创建元素

jquery中创建元素非常简单，只需要将标签作为参数传递给 $() 函数即可。

**$("") : 创建一个空的div标签\**$("这是一个div")\** ： 创建一个带有id属性和内容的div标签**

```js
<script>
        // 1.创建空标签 只需要写开始标记
        $('#div1').append($('<div>'))

        // 2.创建带内容的标签
        $('#div1').append($('<div>创建一个内容标签</div>'))
        $('#div1').append($('<div class="focus">创建一个类标签</div>'))

        // 3.批量创建标签
        $('#div1').append($('<ul><li>1</li><li>1</li><li>1</li><li>1</li></ul>'))
    </script>
```

- 添加元素

父元素选择子元素:

** $(father).append(child)**：在father的后面追加child元素

** $(father).prepend(child)**: 在father的前面追加child元素

子元素选择父元素:

** $(child).appendTo(father):** 将child追加到father的最后去

** $(child).prependTo(father):** 将child追加到father的前面去

兄弟选择兄弟:

 **$(dom).after(element):** 在dom的后面追加element元素

** $(dom).before(element)：** 在dom的前面追加elment元素

** $(dom).insertBefore(element):** 在element的前面追加dom元素

** $(dom).insertAfter(element):** 在element的后面追加dom元素

```js
    <script>
        // 1.往选中元素的最后添加子节点
        $('.list').append('<li>最后</li>')
        // 往选中元素的最前添加子节点
        $('.list').prepend('<li>最前</li>')


        // 2.appendTo()  和 append()  功能相同，只是创建元素和选择父节点的顺序颠倒
        $('<li>appendTo最后</li>').appendTo('.list')
        // prependTo() 和 prepend   功能相同，只是创建元素和选择父节点的顺序颠倒
        $('<li>prependTo最前</li>').prependTo('.list')


        // 3.在选中元素的后面添加兄弟节点
        $('.list li:nth-child(2)').after('<li>在选中元素的后面添加</li>')
        // 在选中元素的前面添加
        $('.list li:nth-child(2)').before('<li>在选中元素的前面添加</li>')

        // 4.insertAfter()  和  insertBefore()  是颠倒写法
        $('<li>颠倒写法后面</li>').insertAfter('.list li:nth-child(2)')
        $('<li>颠倒写法前面</li>').insertBefore('.list li:nth-child(2)')
    </script>
```

添加父元素：

**wrap() :** 给选中的每一个元素单独添加一个父标签

**wrapAll():** 给选中的所有元素添加一个共同的父标签（如果被选中的元素不是连续的，jquery会将所有元素移动到一起，然后包裹标签）。

**wrapInner() :** 给每个选中元素的内容添加一个父标签

```js
    <script>
        // 1.wrap() 给所有选中的每一个元素单独添加一个父标签
        // $('a').wrap('<div>')

        // 2.wrapAll()  给所有选中的元素添加一个共同的父标签
        // $('a').wrapAll('<div>')

        // 3.wrapInner()    给所有选中的每一个元素的内容添加一个父标签
        $('a').wrapInner('<span>')
    </script>
```

- 替换节点：

**$(dom).replaceAll(selector):** 用 $(dom) 替换掉所有被选中的元素

**$(selector).replaceWith(content):** 将所有选中的元素替换成指定的 content 内容。

```js
    <script>
        // 1.用指定的元素替换掉选中的每一个元素
        $('.main p').replaceWith('<h2>明天会更好</h2>')
    </script>
```

- 清空后代：**empty()**

```js
        // 1.empty()    清空标签的所有内容
        $('input').click(function () {
            $('#div1').empty()
        })
```

- 删除元素：

**unwrap() :** 移除所有选中元素的父节点。

**remove() :** 从页面中删除节点，并注销节点中的所的事件处理函数。

**detach()：**从页面中删除节点，但保留节点的事件处理函数。

```js
        // 2.unwrap() 删除选中元素的父元素
        $('#div1 a').click(function () {
            $(this).unwrap()
        })

        $('a').click(function () {
            // 3.删除被选中的元素
            $(this).remove()

            // 4.删除被选中的元素
            $(this).detach()


            /* 
                remove()    和      detach()
                    remove() 将元素从页面删除的同时，还会注销本身的事件
                    detach() 不会删除本身的事件
            */
        })
```

- 克隆元素

**clone(bool)：**克隆节点和所有后代节点及内容。

- - bool: 是一个布尔值。false 只克隆节点，不克隆事件；true 克隆节点和事件

```js
    <script>
        $("#div1").click(function () {
            console.log("我被点击啦！");
        });

        // 1.clone()    克隆指定的节点
        // 参数表示是否克隆元素的事件

        var cloneDiv = $('#div1').clone()
        // var cloneDiv = $('#div1').clone(true)

        $('.box:last').click(function () {
            $(this).append(cloneDiv)
        })
    </script>
```

## **6. 动画方法**

### **6.1 简单动画**

**show() ：** 显示元素

**hide() ：** 隐藏元素

**toggle() ：** 在 show() 与 hide() 方法之间来回切换调用。

**slideDown()：** 下滑展开显示

**slideUp():** 上拉收缩隐藏

**slideToggle():** 切换调用 slideDown() 和 slideUp()

**fadeIn():** 淡入显示

**fadeOut():** 淡出隐藏

**fadeToggle():** 切换调用 fadeIn() 和 fadeOut()

**说明：** show() / hide() / toggle() 方法默认没有动画效果，可以传递时间参数来开启动画效果，单位为 ms。

slideDown() / slideUp() / slideToggle() / fadeIn() / fadeOut() / fadeToggle() : 默认不设置参数，也有动画效果， 默认的动画持续时间为 450毫秒

```js
    <script>
        var btns = $('input')
        var div1 = $('#div1')

        btns.eq(0).click(function () {
            // 1.显示
            // div1.show()
            // 传入参数 动画 单位毫秒
            // div1.show(1000)

            // 4.sileDown() 展开
            // div1.slideDown()
            // div1.slideDown(1000)

            // 7.fadeIn()   淡入
            // div1.fadeIn()
            div1.fadeIn(1000)
        })

        btns.eq(1).click(function () {
            // 2.隐藏
            // div1.hide()
            // 传入参数 动画 单位毫秒
            // div1.hide(1000)

            // 5.sileUp() 收缩
            // div1.slideUp()
            // div1.slideUp(1000)

            // 8.fadeOut()  淡出
            // div1.fadeOut()
            div1.fadeOut(1000)
        })

        btns.eq(2).click(function () {
            // 3.显示隐藏切换
            // div1.toggle()
            // 传入参数 动画 单位毫秒
            // div1.toggle(1000)

            // 6.展开收缩切换
            // div1.slideToggle()
            // div1.slideToggle(1000)

            // 9.淡入淡出切换
            // div1.fadeToggle()
            div1.fadeToggle(1000)
        })
    </script>
```

### **6.2 自定义动画**

- 参数：**animate(css, speed, easing, fn);**

**css :** 要执行动画的css样式对象

**speed :** 动画速度，可以使用"slow","normal", "fast" 或 毫秒数值。

**easing :** 动画效果，可选值为"linear" 和 "swing（默认）"，可以通过插件扩展。

**fn :** 动画结束之后的回调函数（每个元素的动画结束都会执行一次）。

- 执行顺序：

1.同一元素的不同animate，会按照顺序执行

2.不同元素的animate，会同时执行

- 说明： jq 的所有动画效果都有回调函数。 （show, hide, slideDown, slideUp, fadeIn, fadeOut 等）

```js
    <script>
        var btns = $('input')
        btns.eq(0).click(function () {
            // 1.自定义动画  
            // 第一个参数 要进行动画的css样式
            // 第二个参数 表示动画执行的快慢 有两种参数形式
                // 01.具体数值
                // $('#div1').animate({ width: 500 }, 1200)
                // 02.表示快慢的字符串值
                // normal   正常
                // $('#div1').animate({ width: 500 }, 'normal')
                // slow     慢
                // $('#div1').animate({ width: 500 }, 'slow')
                // fast     快
                // $('#div1').animate({ width: 500 }, 'fast')
            // 第三个参数 表示动画形式
                // swing    初始时加速  快到终点减速
                // linear   匀速
                // $('#div1').animate({ width: 500 }, 'swing')
                // $('#div1').animate({ width: 500 }, 'linear')
            // 第四个参数 表示动画结束之后自动调用的函数
            // $('#div1').animate({width:500},2000,'swing',function(){
            //     console.log('动画执行结束');
            // })
            

            /* $('#div1').animate({
                width: 500,
                height: 500,
                opacity: .6,
                left: 500
            }) */

            // 2.在同一个元素上多次调用 animate()函数,可以实现依次执行动画的效果，被称为动画队列
            // $('#div1').animate({ width: 300 }).
            //     animate({ height: 300 })
        })
    </script>
```

**动画的延迟与终止**

- **delay() :** 在动画方法前面调用，延迟动画执行的开始时间。

参数： 表示延迟时长的毫秒数值

所有动画方法都支持 delay()。

```js
        btns.eq(0).click(function () {
            // 1.延迟执行动画   delay()
            $('#div1').delay(1000).animate({ width: 500 }, 2000)
        })
```

- **stop() :** 提前终止动画执行。

**参数：** 两个

第一个参数 ： 是否清空动画队列， true 清空 	false 不清空

第二个参数 ： 是否让当前正在执行的动画立即完成。

true : 立即让动画跑到终点（不会有动画效果）

false: 直接结束动画 （动画执行到哪，就停止在哪）

参数组合：

**stop(false, false)：** 等价于 stop() ，停止当前动画，继续执行后续的动画。

**stop(false,true)：** 直接让当前动画执行到终点，继续执行后续的动画。

**stop(true, true)：** 清空当前动画队列， 并让当前动画直接跑到终点。

**stop(ture, false)**：等价于stop(true) ， 清空当前动画队列，并立即停止当前动画

```js
    <script>

        var btns = $('input');

        btns.eq(0).click(function () {

            // 1.延迟执行动画   delay()
            $('#div1').delay(1000).animate({ width: 500 }, 2000).animate({ height: 500 })

        })

        btns.eq(1).click(function () {
            // 2.停止动画执行 stop()
            // $('#div1').stop()

            // 第一个参数 停止后,后面没有执行的动画队列 是否继续执行
                // true 不执行  false 执行
            // 第二个参数 是否让当前正在执行的动画直接完毕
                // true 是      false 否
            // $('#div1').stop(true, true)
            // $('#div1').stop(true, false)
            // $('#div1').stop(false, false)
            $('#div1').stop(false, true)
        })
    </script>
```

**动画累积**

动画累积又叫动画堆叠，是指在动画还未执行完成之前，频繁触发相同动画效果产生的现象。

动画累积影响用户体验，可以通过以下两种方式优化：

1，终止旧的动画任务，开启新的动画任务

使用 **stop(true)** 来终止旧的动画。

2，判断动画执行的状态，如果旧动画还在执行，则不执行新的动画。

使用 **:animated** 状态伪类检测 （length值不为0表示动画正在执行）。

:animated 状态伪类：

jquery 会自动给正在执行动画效果的元素添加 :animated 伪类，表示该元素正在执行动画，当动画执行结束，会自动取消 :animated 伪类。该伪类可以作为选择器来选择元素。

```js
    <script>
        // 动画积累 连续点击造成的积累事件
        $('input').click(function () {

            $('#div1').toggle(500)

            // 避免动画积累
            // 方式一:  执行新动画之前 先把之前的动画关闭
            // $('#div1').stop(true).toggle(500)

            // .is(':animated') 检测指定元素当前是不是正在执行动画
            // console.log($('#div1').is(':animated'));

            // 方式二:  在有动画执行过程中 不允许开启新的动画
            if (!$('#div1').is(':animated')) {
                $('#div1').toggle(500)
            }
        })
    </script>
```

