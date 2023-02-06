#  css预编译

## 一、sass

### 1.1 sass

跟less一样，sass也是一种css预编译语言，由于sass出现的比less晚，因此支持的语法更多，例如**循环语句**，**条件语句**等等。sass是通过**ruby**编译的，因此我们使用sass要首先安装ruby

​	安装完成，输入gem -v可以查看版本号

​	sass属于gem的一个模块，我们可以通过gem安装sass：

​			`gem isntall sass`

​	安装完成，提供了sass指令，我们输入sass -v可以查看版本号，我们可以手动安装sass：

​			gem install 将sass文件拖入进来。我们开发的时候，通常写中文注释，

但是ruby是日本人开发的，因此编码不涵盖中文字符串，所以我们要更改字符集：

​	进入C:\Ruby23-x64\lib\ruby\gems\2.3.0\gems\sass-3.4.22\lib\sass目录，打开engine.rb文件，

​	在54行，添加

```js
Encoding.default_external = Encoding.find('utf-8')
```



### 1.2 拓展名

sass支持两种拓展名

一种是.sass

​		 这种拓展名文件，支持的语法比较新，跟css差距很大，因此开发者很少使用

**一种是.scss**

​		 这种拓展名文件，支持的语法跟css语法很相似，因此被很多开发者接受

我们以.scss语法讲解课程

sass跟less一样。浏览器不识别，所以我们要编译



### 1.3 编译

#### **命令行编译**

​		 我们安装完成sass，可以通过sass指令来编译我们的代码

​		 sass sass文件 css文件 配置

​		 scss scss文件 css文件 配置

- sass

```css
.box
  .box1
    a
      color: red
      background-color: pink
```

- 编译

```js
sass style.sass style.css -C  --sourcemap=none
```

- scss

```css
.box {
  .box1 {
    a {
      color: gold;
      background-color: red;
    }
  }
}
```

- 编译

```js
scss style.scss style.css -C --sourcemap=none
```



​		 配置：

​				-C 避免输出缓存文件 

​				--sourcemap=none 避免输出suorcemap文件



#### **工程化编译**

​	 更推荐在项目在使用

​	 对`webpack`来说sass文件也是资源，是资源我们就可以模块化打包加载

​	 我们要装`sass-loader`，使用方式跟less一样

​	 `style-loader!css-loader!sass-loader`

​	 注意：本地安装sass，sass-loader依赖node-sass

​			==node-sass 版本  "node-sass@6.0.1",==

- webpack.config.js

```js
// 定义配置对象
const config = {
  // 配置环境
  mode: 'development',
  // 入口文件
  entry: './modules/main.js',
  // 出口配置
  output: {
    filename: './style.js'
  },
  // 配置加载机
  module: {
    // 定义规则
    rules: [
      {
        // css
        test: /\.css$/,
        loader: 'style-loader!css-loader'
      },
      {
        test: /\.scss$/,
        loader: 'style-loader!css-loader!sass-loader'
      }
    ]
  }
}

// 暴露接口
module.exports = config
```



#### 插件编译

安装插件 Easy Sass



- 关闭scss文件自动生成的css文件和min.css文件

https://developer.aliyun.com/article/1092949

```js
"easysass.excludeRegex": "false",
"easysass.formats": [
    {
      "format": "expanded",
      "extension": ""
    },
    {
      "format": "compressed",
      "extension": ""
    }
  ],
```

- 关于报错	EasySass: could not generate CSS file: No extension specified for easysass.formats[1].

  ![image-20230206161356034](C:\Users\19510\AppData\Roaming\Typora\typora-user-images\image-20230206161356034.png)

- 设置配置文件

```js
    "easysass.formats": [
        {
            "format": "expanded",
            "extension": ".css"
        },
    ],
```





### 1.4 嵌套语法与`&`

嵌套式语法

​	 使用预编译语言最大的特点就是可以使用嵌套式语法，也就是说直接在一个选择器中使用另一个选择器并书写样式

​		 编译的时候会在将内外选择器同时保留，外部选择器类似命名空间，确保选择器名称不会相互覆盖。

嵌套的时候，我们可以通过`&`符号获取当前选择器



```css
// 嵌套语法
.box {
  .box1 {
    a {
      color: gold;
      background-color: blueviolet;
      // & 当前选择器
      &:hover {
        background-color: red;
      }
    }
  }
}

```



### 1.5 属性嵌套

在sass中支持`复合属性嵌套定义`

复合属性：一个属性可以代表多个属性的属性

​		 margin: 

​				margin-top, margin-bottom, marign-right, margin-left

​		 border: 

​				border-width, border-style, border-color，

​				border-top, border-right, border-bottom, border-left，

​				border-top-width, border-top-style, border-top-color，

​				border-right-width, border-right-style, border-right-color，

​				border-bottom-width, border-bottom-style, border-bottom-color

​				border-left-width, border-left-style, border-left-color

复合属性定义的语法：

​	 `parentProps: { childProps: value; }`



```scss
.box {
  margin-top: 10px;
  margin-bottom: 20px;
  margin-left: 30px;
  margin-right: 40px;
}

// 为了避免多次使用父属性 可以使用复合属性的语法
.box1 {
  margin: {
    top: 10px;
    bottom: 20px;
    left: 30px;
    right: 40px;
  }
}
```



### 1.6 变量

sass支持变量 `$`

​	 语法 `$key: value;`

​		 全局的变量，任何模块都能使用，

​		 局部的变量，只能在当前模块中使用，模块外部无法使用

​		 ==不支持声明前置==

工作中，我们尽量将变量定义在最前面。



```scss
// 定义全局变量
$size: 100px;

.box {
  width: $size;
  height: $size;
  background-color: red;
  .box1 {
    a {
      // 定义局部变量
      $color: #fff;
      color: $color;
    }
  }
}

.box1 {
  // 无法使用局部变量
  // color: $color;
  width: $size;
  height: $size;
}
```



### 1.7 运算

#### 数学运算

​	 sass支持加减乘除运算，但是运算是有物理意义的

​			 加减法:**运算时候会做单位转换。保留是第一个加数或者被减数的单位**

​			 乘法：**只能有一个乘数带单位**

​			 除法：**默认不会执行除法**。

​				在css中，font属性的属性值可以是字号比行高，所以/默认不执行除法想执行除法，必须满足三个条件之一

​				1 必须有() 

​				2 必须出现变量或者方法的返回值 

​				3 是表达式的一部分（除了除法，还有其它的运算）

 					 单位：10 / 2  10是被除数， 2是除数

​					 如果被除数有单位，除数单位可有可无，有单位单位抵消。

​					 如果被除数没有单位，除数一定不能有单位



```css
// 数学运算
// 加减运算

.box {
  //  保留第一个加数或者被减数的单位
  width: 10px - 1px;
  height: 10px + 2px;
  margin: 10px + 3;
  padding: 10px - 4;

  // 运算时会做单位转换
  border: 10in + 1px solid #000;
}

// 乘法运算
.box2 {
  // 只能有一个乘数带单位
  // width: 10px * 20px;

  width: 10px * 2;
}

.box3 {
  // 除法 默认不会执行除法 因为font属性可以是字号/行高 所以默认不执行
  // width: 10px / 2px;

  // 想要执行必须满足以下三者条件之一
  // 1.带括号
  width: (10px / 2);

  // 2.必须出现变量或者方法的返回值
  $size: 100px;

  // 变量的返回值
  height: $size / 2;

  // 方法的返回值
  margin-top: max(100px, 200px);

  // 3.是表达式的一部分(运算符号之间空格隔开)
  padding-left: 200px / 2 + 10px;

  // 如果被除数有单位 除数有单位会互相抵消
  margin-bottom: (100px / 20px);

  // 如果被除数没有单位 除数一定不能有单位 报错
  // margin-right: (100 / 2px);
}
```



#### 色彩运算

在sass中，色彩可以进行运算

​		 必要条件是：==色彩的透明度必须一致==

​		 运算的时候，每个通道单独运算的，透明度通道不运算

​		 运算过程中，通道之间不会进位或者借位

​		 一个通道的值只能在0-255（00-ff）之间



```css
.box4 {
  // 色彩运算
  $color1: rgba(90, 150, 200, 0.5);
  $color2: rgba(50, 200, 230, 0.9);
  $color3: rgba(255, 50, 100, 0.5);

  // 不同的透明度通道值是不能计算
  // color: $color3 - $color2;

  color: $color3 - $color1;
}
```



#### 字符串运算

​		 sass允许字符串，就是字符串拼接

​				在 Sass 中可以通过加法符号 “+” 来对字符串进行连接

​				除了在变量中做字符连接运算之外，还可以直接通过 +，把字符连接在一起

​				注意，如果有引号的字符串被添加了一个没有引号的字符串,结果会是一个有引号的字符串。

​						  相反如果一个没有引号的字符串被添加了一个有引号的字符串。结果将是一个没有引号的字符串。



```css
.box5 {
  // 在 Sass 中可以通过加法符号 “+” 来对字符串进行连接
  $content: "Hello" + " " +"Sass";

  &::before {
    content: #{$content};
  }

  // 除了在变量中做字符连接运算之外，还可以直接通过 +，把字符连接在一起
  cursor: e + -resize;
}

.box6 {
  // 注意，如果有引号的字符串被添加了一个没有引号的字符串,结果会是一个有引号的字符串。
  &::before {
    content: "Foo" + Bar;
  }

  // 相反如果一个没有引号的字符串被添加了一个有引号的字符串。结果将是一个没有引号的字符串。
  &::after {
    content: Foo + "Bar";
  }
}
```



### 1.8 混合

sass中的混合也是为了复用一组样式(方法)



#### 定义混合

​	 语法 

​			`@mixin 混合名称() {`

​					`// 定义样式`

​			 `}`



 使用混合 `@include` 混合名称;



```css
// 定义混合
@mixin size() {
  width: 100px;
  height: 100px;
}

// 使用混合 通过 @include
.box {
  @include size();
}

```

 特点

​		 1 混合在编译的时候，删除了，当多次使用混合的时候，并没有合并选择器，是真正的复制。

​		 2 不支持声明前置，外部的混合内部不能使用

​		 所以sass中的混合相当于less中的方法



#### **混合传参**

 定义参数的语法，跟定义变量的语法是一样的，并且可以传递默认参数

 	语法 

​			`@mixin 混合名称 ($arg1: val1 ) {`

​					 `// 定义样式`

​			 `}`



**使用混合** 

​	`@include 混合名称(val1, val2);`

​		 如果定义了默认参数，使用混合的时候，可以不用传递参数

​		 如果是一个参数都不传递，我们可以省略参数集合

​		 如果没有定义默认参数，使用混合的时候，必须传递参数



```css
// 混合传参
@mixin size2($w: 100px, $h: $w) {
  width: $w;
  height: $h;
}

// 使用混合
.box2 {
  // 如果定义了默认参数 使用混合的时候可以不传递参数
  // @include size2();

  // 如果一个参数都不传递 可以省略参数集合
  // @include size2;

  @include size2(200px, 300px);
}

// ==========================

@mixin size3($w, $h) {
  width: $w;
  height: $h;
}

.box3 {
  // 没有定义默认参数 使用混合必须传递参数
  @include size3(300px, 400px);
}
```



**获取剩余参数**

​		 在less中获取所有参数用@argumetns

​		 在`sass`中获取剩余参数语法跟ES6语法相似，使用三个点语法

​				 sass中，三个点语法在变量之后

​				 `@mixin demo($arg1, $arg2, $arg...) {}`

​						 arg获取从第三个参数后面所有的参数。前面的参数可以正常使用



 		兼容浏览器，有时候，通常我们为了将css3样式兼容所有浏览器，而添加浏览器前缀，要书写很多遍，此时我们可以通过混合复用样式，提高开发效率，通常将混合名称定义成css样式名称，并且在混合中，只设置一个样式



```scss
// 获取剩余参数 $arg...
// 定义制作盒子阴影的方法
@mixin box-shadow($args...) {
  box-shadow: $args;
  // chrome
  -webkit-box-shadow: $args;
  // ff
  -moz-box-shadow: $args;
  // ie
  -ms-box-shadow: $args;
  // opera
  -o-box-shadow: $args;
}

.box4 {
  @include size1;
  background-color: gold;
  @include box-shadow(1px 2px 3px red);
}
```



### 1.9 继承

#### 全局继承

为了复用选择器的样式，sass提供了继承，跟less中的混合是一样的

​		 less中的方法是sass中的混合

​		 less中的混合是sass中的继承

定义继承就是定义选择器，sass支持四类继承选择器：

​		 类选择器，id选择器，元素名称选择器，自定义元素名称选择器

使用继承：`@extend` 继承选择器;

 特点： 

​		 1 继承的选择器在编译的时候会保留

​		 2 编译的时候，合并了选择器，而不是复制样式，相对来说效率更高



```css
// 定义继承就是定义选择器
// sass支持四类选择器 id选择器 类选择器 元素名称选择器 自定义选择器
.demo {
  width: 100px;
}
#text {
  height: 100px;
}
a {
  margin: 10px;
}
abc {
  padding: 10px;
}

// 使用继承选择器
.box {
  @extend .demo;
  @extend #text;
  @extend a;
  @extend abc;
}
```



如果不想在编译的时候保留选择器样式，我们可以使用选择器`占位符%`

​		 注意：只有元素名称和自定义元素名称选择器可以使用%占位符，id选择器和类选择器无法使用

​		 ==一旦添加占位符，@extend 继承选择器的时候，也要添加占位符==



```css
/* 
  如果不想在编译的时候保留选择器样式 可以使用占位符%
    注意 只有元素名称选择器和自定义选择器支持占位符
    一旦使用占位符 使用的时候也要添加占位符
*/
%a {
  margin: 20px;
}
%abc {
  padding: 30px;
}

.box1 {
  @extend %a;
  @extend %abc;
}
```



sass中的继承支持声明前置，所以工作中，我们尽量将他们定义在最前面



#### 局部继承

​		 当前模块可以使用

​		 其他模块可以直接使用，但是有意想不到的结果

​		 所以工作中不建议使用局部的继承



```scss
// 局部继承

// .box2 {
//     // 定义局部继承
//     #demo {
//         color: red;
//     }
//     // 当前模块可以使用
//     // @extend #demo;
// }


// 其他模块可以直接使用，但是有意想不到的结果

// .box3 {
//     @extend #demo;
// }



// 分析原理:
// a {
//     b {
//         c {
//             d {
//                 color: red;
//             }
//         }
//     }
// }

// e {
//     f {
//         g {
//             h {
//                 @extend d;
//             }
//         }
//     }
// }
// 一开始是a b c d自己内部拥有color 
// 用下面的一组(e f g h)选择器 替换掉上面使用的选择器（d）
// 用上面的(a b c d) 选择器 替换掉下面使用的选择器 （h）




a {
    b {
        c {
            d {
                e {
                    color: blue;
                }
            }
        }
    }
}



h {
    j {
        k {
            l {
                m {
                    @extend e;
                }
            }
        }
    }
}

// 结果: a b c d e, a b c d h j k l m, h j k l a b c d m
// 注意: 工作中不建议使用局部的继承
```



### 1.10 插值与引入文件

#### 插值

sass插值语法

​		定义插值	`$key : val`

​		使用插值	`#{ $key }`

 跟less一样，sass中的插值有三种用法

​		 1 在字符串中插值

​		 2 在选择器上插值

​		 3 在属性名称上插值



```css
// 定义插值
$day: "周一";
$num: 1;
$dir: top;

// 1.在字符串中使用插值
.box::after {
  content: "今天是"+"#{$day}";
}

// 2.在选择器上使用插值
.box#{$num} {
  color: red;
}

// 3.在属性名称上使用插值
.box3 {
  border-#{$dir}-color: red;
}

```



#### 引入文件

​		 `@import url` 				sass引入方式



```css
// 通过sass方式引入
@import "./common.scss";

// 使用
.box {
  width: $size;
  background-color: $bg-pink;
  font-size: $fz-50;
}

```





### 1.11 条件语句

sass支持if条件语句，语法跟js中的if条件语句类似

​		 js中条件语句 if () {} else {}

​		 sass中条件语句 `@if {} @else {}`

对比js语法

​		 相同点：

​				1 关键字（js： if, else if, else。sass: @if, @else if, @else）；

​				2 条件体：都是通过{}定义；

​				3 比较运算符：>, <, >=, <=, ==

​		 不同点：

​				1 js中，条件定义在条件集合()中, ==sass不需要()==。直接写条件；

​				2 逻辑运算符

​						 **js 中 “与”用“&&”表示，“或”用“||”表示**

​						 **sass 中 “与”用“and”表示， “或”用“or”表示**



```scss
// sass中的条件语句
/* 
  @if {} else {}
    与js不同 不需要 ()

  逻辑运算符
    js 中 “与”用“&&”表示，“或”用“||”表示
    sass 中 “与”用“and”表示， “或”用“or”表示
*/

// 封装制作三角形的方法 兼容行内元素和块元素
@mixin arrow($w: 50px, $dir: top, $color: red, $width: 100px) {
  // 兼容行内元素和块元素
  width: 0;
  font-size: 0;
  border: #{$w} solid transparent;
  border-#{$dir}-color: #{$color};

  // 使用条件语句
  @if $dir == top or $dir == bottom {
    border-top-width: $width;
    border-bottom-width: $width;
  }
  else {
    border-left-width: $width;
    border-right-width: $width;
  }
}

// 使用混合
.box1 {
  @include arrow(10px, left, gold, 30px);
}

span {
  @include arrow(30px, left, green);
}
```



### 1.12 循环语句

跟js中的for循环一样，sass也支持循环语句，也用@for关键字定义，有两种语法

​		 `@for $i from start through end {}`

​		 `@for $i from start to end {}`

​				 @for：表示循环语句关键字 

​				 $i：表示循环变量 

​				 from，through，to：都是循环关键字

​				 start：表示循环的起始值 

​				 end：表示循环的终止值 默认每次循环增量都是1

through与to的区别是：

​		 through：最后一次循环包含end值

​		 to：最后一次循环不包括end值

循环语句通过{}定义了代码块，因此循环语句中定义的变量，外部无法使用



```scss
div {
  height: 20px;
  margin-bottom: 10px;
  background-color: pink;
}

// 使用循环语句
/* 
@for $i from start through end {}
  @for：表示循环语句关键字 
  $i：表示循环变量 
  start：表示循环的起始值 
  end：表示循环的终止值 默认每次循环增量都是1

through与to的区别是：
    through：最后一次循环包含end值
    to：最后一次循环不包括end值
*/

// through：最后一次循环包含end值
@for $i from 1 through 20 {
  .box#{$i} {
    background-color: red;
  }
}

// to：最后一次循环不包括end值
@for $i from 1 to 20 {
  .box#{$i} {
    background-color: red;
  }
}
```



### 1.13 while 循环

在for循环中，==增量==始终是1，无法改变，自定义循环增量，使用while循环

​	语法

​		 `定义循环变量`

​		 `@while 循环条件 {`

 				`定义循环体（定义样式）`

​				 `改变循环变量`

​		 `}`

由于循环变量定义在了循环外部，因此在循环外部可以继续使用循环变量

需要注意的是 必须循环变量指定完成之后 再去编译

```css
div {
  height: 20px;
  margin-bottom: 10px;
  background-color: pink;
}

// 使用while循环 自定义增量
$i: 1;
// 定义循环语句
// 注意 指定好条件好再打开while循环 否则会卡死
@while ($i <= 20) {
  .box#{$i} {
    background-color: pink;
  }
  // 修改循环变量
  $i: $i + 2;
}
```





### 1.14 枚举循环

for循环的循环增量是1，while循环中可以自定义循环增量，但是循环增量始终是固定的，如果想循环一个没有规律规律的聚合数据，我们可以使用枚举循环

​	语法

​		`@each $i in 枚举体 {}`

​			 `@each  枚举循环关键字`

​			 `$i  是循环变量`

​			 `in  循环关键字`

​			 枚举体 多个个体用逗号隔开

注意：如果枚举的个体命中了sass内置的数据变量，如red，green，blue等，使用的时候，要添加引号

```js
div {
  height: 20px;
  margin-bottom: 10px;
  background-color: pink;
}

// 枚举循环 循环没有规律的聚合数据
/* 
		@each $i in 枚举体 {}
      @each  枚举循环关键字
      $i  是循环变量
      in  循环关键字
    枚举体 多个个体用逗号隔开

    注意：如果枚举的个体命中了sass内置的数据变量，如red，green，blue等，使用的时候，要添加引号
*/
@each $i in red, green, gold {
  .#{$i} {
    background-color: $i;
  }
}
```



### 1.15 三元运算符

在js中，?:是一个三元运算符（是对if语句的简写）

sass提供了if方法，可以实现该功能

​	 `if(condition, trueValue, falseValue)`

​		 condition值为真，结果是truevalue

​		 condition值为假，结果是falsevalue

```scss
// 三元运算符
/* 
    if(condition, trueValue, falseValue)
    condition值为真，结果是truevalue 
    condition值为假，结果是falsevalue 
    */
.box {
  width: if(true, 100px, 290px);
  height: if(false, 200px, 300px);
}
```

