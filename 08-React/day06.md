# React 第六天

## 一、React

### 1.1 高阶组件

在React中，为了保证组件基类的完整性，我们对组件拓展功能的时候，React是不予许我们修改组件基类的。例如：使用axios，直接使用，不能安装。

> 如果对组件拓展功能，React提供了高阶组件的技术：类似装饰者模式，不用直接修改原来的组件，而是定义一个新组件，对原来组件做包装。
>

​	 使用高阶组件技术，需要定义一个高阶函数（方法）

​			 参数是原组件，以及给高阶组件传递的数据。在函数中，我们创建一个新组件，

​					 在render方法中，我们渲染原来的组件并传递相应的属性数据

​				 	我们对新组件进行包装（拓展），以此来实现对原来组件的拓展（没有被修改）。

​			 返回新组件，使用的时候，使用的是新组件

高阶组件技术中，返回的新组件被称之为高阶组件，这个方法被称之为高阶函数（方法）

注意：在高阶函数中，绝对不能直接对原来的组件进行拓展（修改）。

==高阶组件跟混合很相似，它们的区别是：==

​		 混合是对原组件的继承，因此可以访问原组件的属性和方法，所以就有机会覆盖或者修改它们

​				 是为了复用原组件的属性和方法

​		 高阶组件是对原组件的包装，因此访问不到原组件的属性和方法，这样就无法修改它们了，起到了保护的作用，建议使用

​				 是为了对组件拓展，但是又不影响原来组件的使用。

```jsx
import React, { Component } from 'react'
import { render } from 'react-dom'

class App extends Component {
  // 定义构造函数
  constructor(props) {
    super(props);
    this.state = {
      msg: ''
    }
  }

  render() {
    return (
      <div>
        <h1>app part</h1>
        <hr></hr>
        {/* 定义数据双向绑定 */}
        <input type='text' value={this.state.msg} onChange={e => this.setState({ msg: e.target.value })}></input>
        {/* 使用组件 */}
        <Demo msg={this.state.msg}></Demo>
        <Demo msg={this.state.msg}></Demo>
        {/* 以下组件传递数据的时候 不要去影响其他组件数据的改变 */}
        {/* <Demo msg={this.state.msg}></Demo> */}
        {/* 3.使用高阶组件 */}
        <DealDemo msg={this.state.msg}></DealDemo>
      </div>
    )
  }
}

// 定义组件类
class Demo extends Component {
  // // 接收数据
  // componentWillReceiveProps(props) {
  //   console.log('Pack', props);
  // }

  render() {
    return (
      <div>
        <h1>demo-part {this.props.msg}</h1>
      </div>
    )
  }
}

// 1.定义高阶方法
function deal(Comp) {
  // 定义包装组件(新组件)
  class Pack extends Component {
    // 接收数据
    componentWillReceiveProps(props) {
      console.log('Pack', props);
    }

    render() {
      // 渲染的时候 返回值是参数组件
      return <Comp {...this.props}></Comp>
    }
  }

  // 返回的是包装组件
  return Pack
}

// 2.执行高阶方法
let DealDemo = deal(Demo)

// 高阶组件跟混合很相似 (产生覆盖)
// class DealDemo extends Demo {
//   componentWillReceiveProps(props) {
//     console.log('DealDemo', props);
//   }
// }

render(<App></App>, app)
```



### 1.2 ref 转发

ref转发就是让我们在==组件的外部==访问组件内部的元素。

- 我们使用ref有两种方式

  ​	 第一种 使用ref字符串。

  ​			 在组件内部通过this.refs获取对应的元素（组件内部获取）

  ​	 第二种 通过createRef创建ref对象

  ​			 通过ref对象的current属性获取对应的元素（允许在组件外部获取）

  注： ref属性既可以添加给虚拟DOM，也可以添加给组件。

### 1.3 使用 ref

ref既可以指向虚拟DOM，也可以指向组件，但是==只能指向一个，不能重复指向==

​	 如果ref指向虚拟DOM，此时获取的将是真实DOM元素。

​	 如果ref指向组件，此时获取的将是组件实例化对象。

此时ref属性对组件就有了意义（不仅仅是传递数据）。因此ref属性可以看成是组件的非元素属性，与之类似的还有key属性。使用ref转发技术，分成三步

- 第一步 通过`createRef`方法创建一个ref对象


- 第二步 将ref对象传递给组件。

  ​	注：传递的时候不要用ref属性传递ref对象，可以是任何其它的属性，如 icktRef。

- 第三步 在组件中，将ref对象指向内部的虚拟DOM。这样我们就可以在组件外部，通过ref对象，获取组件内部的元素了。 注：ref对象只能指向类组件，不能指向函数组件。可以使用`forwardRef`


### 1.4 forwardRef

`forwardRef` 取消ref对象对组件（包括函数组件也包括类组件）的指向。

​	 是一个方法，参数是回调函数

​			 第一个参数表示属性对象

​			 第二个参数表示被取消指向的ref对象

​			 返回值表示渲染的组件

forwardRef在高阶组件中有很多应用。

```jsx
import React, { Component, createRef, forwardRef } from "react";
import { render } from 'react-dom'

// 定义全局ref对象
let dom = createRef()

class App extends Component {
  render() {
    return (
      <div>
        {/* 1.ref 对象 */}
        <h1 ref={dom}>app part</h1>
        {/* 3.作为属性设置ref */}
        {/* ref既可以指向虚拟DOM，也可以指向组件，但是只能指向一个，不能重复指向 */}
        {/* <h1 ref={this.props.ref}>app part</h1> */}
        {/* 4. 接受更换名字之后的ref */}
        {/* <h1 ref={this.props.abc}>app part</h1> */}
        {/* 5.使用forwardRef */}
        {/* <h1 ref={this.props.aref}>forwardRef</h1> */}
      </div>
    );
  }
}

// 2.指向组件
// render(<App ref={dom}></App>, app, () => {
//   console.log(dom);
//   // 1.在组件的外部操作
//   // dom.current.style.color = 'pink'
// })

// 4. 可以将传递的ref名称更改
// render(<App abc={dom}></App>, app, () => {
//   console.log(dom);
// })

/* 
  5.如果不想更改ref名称 使用forwordref 
    参数是回调函数
        第一个参数表示属性对象
        第二个参数表示被取消指向的ref对象
        返回值表示渲染的组件
*/
// let DealApp = forwardRef((props, ref) => {
//   return <App aref={ref} {...props}></App>
// })
// render(<DealApp ref={dom} msg='hello'></DealApp>, app, () => {
//   console.log(dom);
// })

// 6.函数组件 不能使用ref
let Demo = props => {
  return <h1 ref={props.aref}>函数组件</h1>
}
// 使用forwardRef 
let DealDemo = forwardRef((props, ref) => {
  return <Demo aref={ref}></Demo>
})
render(<DealDemo ref={dom}></DealDemo>, app2, () => {
  console.log(dom);
})
```



### 1.5 hook

hook是React新版本中提供一项技术，用来为函数组件拓展功能的。

> 函数组件是一个十分简单的组件，是对类组件的简化，因此没有继承组件基类，不具有组件的行为。例如：状态数据，生命周期等等。所以为了让函数组件具有这些行为，我们可以用hook方法对函数组件做拓展。
>

- **使用状态数据** 

  ​	语法： `let [ 数据，修改数据的方法 ] = useState(默认数据值);`

  ​			 “修改数据的方法”的参数就是新的状态数据。

    			 在函数组件中，用“修改数据的方法”去修改“数据”，此时函数组件将进入存在期，更新虚拟DOM。

- **使用生命周期方法** 

  ​	语法: `useEffect(fn)`

  - fn代表`componentDidMount`  组件构建完毕 以及`componentDidUpdate`  组件更新完毕*  方法。

    ​	传递第二个参数

    ​	 什么都没有传递 监听所有的数据变

     	传递空数组 不会监听数据变

     	执行要监听的数据 [xxx,xxx,...]

   ... hook为函数组件拓展了大量的功能。

```jsx
import React, { Component, useState, useEffect } from "react";
import { render, unmountComponentAtNode } from 'react-dom'

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      num: 1
    }
  }

  // 组件构建完毕
  componentDidMount() {
    this.timer = setInterval(() => {
      this.setState({ num: this.state.num + 1 })
    }, 1000);
  }

  // 卸载方法
  unMount() {
    // 关闭定时器
    clearInterval(this.timer)
    // 卸载应用程序
    unmountComponentAtNode(app)
  }

  render() {
    return (
      <div>
        <button onClick={e => this.unMount()}>卸载应用程序</button>
        <h1> app part {this.state.num}</h1>
      </div>
    );
  }
}
render(<App></App>, app)

// 1.hooks 基本语法
let Demo1 = props => {
  // 1.定义状态
  const [num, setNum] = useState(5);

  // 2.生命周期方法 useEffect(fn)
  // fn 代表componentDidMount 和 componentDidUpdate
  useEffect(() => {
    console.log(123);
  })

  // 定义 ++方法
  function addNum() {
    // a.改变数据
    // setNum(num + 1)

    // b.还可以是函数的形式
    setNum(num => num + 1);
  }

  // 卸载方法
  function unMount() {
    // 卸载应用程序
    unmountComponentAtNode(app2)
  }

  return (
    <div>
      <button onClick={e => addNum()}>点我++ {num}</button>
      <button onClick={e => unMount()}>卸载应用程序</button>
    </div>
  )
}

render(<Demo1></Demo1>, app2)

// 用函数组件实现类组件功能
let Demo2 = props => {
  // 定义状态
  const [num, setNum] = useState(99);

  /* 
    定义生命周期的方法
      fn 代表 componentDidmount 组件构建完毕 和 componentDidUpdate 组件更新完毕 
      传递第二个参数
        什么都没有传递 监听所有的数据变化
        传递空数组 不会监听数据变化
        执行要监听的数据 [xxx,xxx,...]
  */
  useEffect(() => {
    let timer = setInterval(() => {
      setNum(num => num + 1)
    }, 1000)
    return () => {
      // 卸载的时候清空定时器
      clearInterval(timer)
    };
  }, [])

  // 卸载方法
  function unMount() {
    unmountComponentAtNode(app3)
  }

  return (
    <div>
      <button onClick={e => setNum(num + 1)}>点我++ {num}</button>
      <button onClick={e => unMount()}> 卸载</button>
    </div>
  )
}
render(<Demo2></Demo2>, app3)
```



## 二、服务器端渲染

### 2.1 服务器端渲染

前端渲染的问题：

​	 1 白屏时间长，影响用户体验

​	 2 不利于SEO优化

​	 ... ...

所以我们要使用服务器端渲染，来解决这些问题。

React实现了多端适配，为不同的端提供了不同的渲染库，React核心库只负责创建虚拟DOM和组件。

​	 在web端渲染，使用react-dom渲染库

​	 在服务器端渲染，使用react-dom/server.js渲染库

### 2.2 服务器端渲染库

> react-dom/server.js提供了在服务器端渲染的方法
>

​		 `renderToString` 				将应用程序渲染成模板字符串

​		 `renderToStaticMarkup`  	将应用程序渲染成模板字符串，并且会删除不必要的属性

​				 例如：data-reactroot等属性

​				 该方法减少了文件大小，因此常常用在渲染静态页中。

服务器端渲染的问题是：无法绑定交互。

```js
// 引入express
const express = require('express');
// 引入ejs
const ejs = require('ejs');
// 引入react
const React = require('react')
// 引入服务器端的渲染库
const { renderToString, renderToStaticMarkup } = require('react-dom/server')
// 获取应用
const app = express()

// 配置引擎
app.engine('html', ejs.__express)

// 创建组件实例
class App extends React.Component {
  // 定义状态
  constructor(props) {
    super(props);
    this.state = {
      num: 10
    }
  }

  addNum() {
    this.setState({ num: this.setState.num + 1 })
  }

  // 由于服务器端不识别jsx语法 所以使用原生语法
  render() {
    return React.createElement(
      'div',
      null,
      React.createElement(
        'button',
        {
          style: { fontSize: 42, color: 'red' },
          onClick: e => this.addNum()
        },
        '点我++',
        this.state.num
      )
    )
  }
}

/* 
  将组件渲染成字符串
    renderToString    将应用程序渲染成模板字符串
    renderToStaticMarkUp  将应用程序渲染成模板字符串 会删除不必要的属性
*/
// console.log(111, renderToString(React.createElement(App)));
// console.log(222, renderToStaticMarkup(React.createElement(App)));

// 渲染页面
app.get('/', (req, res) => {
  res.render('index.html', {
    title: '服务器端的渲染',
    content: renderToString(React.createElement(App))
  })
})

// 监听端口号
app.listen(3000, () => console.log(3000));
```



###  2.3 渲染优化

> 在渲染过程中，`renderToString`以及`renderToStaticMarkup`方法会将整个应用程序渲染出来，再返回到页面中。如果应用程序非常复杂，假设需要渲染1000ms，那么用户就要等到1000ms之后，才能看到页面。
>

React为了提高渲染的性能，让用户可以更快速的看到页面，提供了与浏览器渲染页面相似的功能。

​	 浏览器渲染页面优化：每加载8kb渲染一次。

​	 React也是这样的，允许我们渲染一部分组件就返回一些数据。

​			 例如，第一次渲染用了10ms，并返回到浏览器端，这样用户就可以提前990ms看到内容。

### 2.4 渲染优化方法

服务器端渲染库提供了两个方法：

​	 `renderToNodeStream` 			将应用程序渲染成字符串

​	 `renderToStaticNodeStream`  	将应用程序渲染成模板字符串，并且会删除不必要的属性

所以这两个方法的区别与renderToString以及renderToStaticMarkup方法是一样的。

这两个方法都返回一个Strea流，提供了`pipe`方法

​		 第一个参数表示	res对象 

​	 	第二个参数表示	传递的配置对象

​				 end: false  渲染完成不执行end方法，我们可以继续写入。

提供了`on`方法，用来监听事件

​		 data事件：表示每次渲染完成执行的方法。 

​		 end事件：表示已经渲染完成

```js
// 引入express
const express = require('express')
// ejs
const ejs = require('ejs')
// 引入react
const React = require('react')
// 引入服务器端的渲染库
const { renderToStream, renderToStaticNodeStream } = require('react-dom/server.js')

// 获取应用程序
const app = express()

// 配置引擎
app.engine('html', ejs.__express)

// 创建应用程序组件
class App extends React.Component {
  constructor(props) {
    super(props)
    this.state = {
      num: 10
    }
  }

  // 定义方法
  addNum() {
    this.setState({ num: this.state.num + 1 })
  }

  // 由于服务器端不识别jsx语法 所有要转为原生语法
  render() {
    return React.createElement(
      'div',
      null,
      React.createElement(
        'button',
        {
          style:
          {
            fontSize: 24,
            color: 'green',
          },
          onClick: e => this.addNum()
        },
        '点我num++',
        this.state.num
      ))
  }
}

// 路由
app.get('/', (req, res) => {
  // 定义替换的数据
  let data = {
    title: '服务器端的渲染优化',
    // 定义标签
    seo: `<meta name="keywords" content="前端、北京、IT、爱创、古玩城" />`,
  }

  // 即使不使用 render 方法 还可以使用 write 方法返回数据
  res.write(`
  <!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!-- =号是替换变量 -->
  <title>${data.title}</title>`)
  res.write(data.seo)
  res.write(`</head>
    <body>
        <div>`)

  // renderToNodeStream 返回一个Stream对象
  let Stream = renderToStaticNodeStream(React.createElement(App))

  // 提供了pipe方法
  Stream.pipe(res, {
    // 渲染完成不执行end方法 可以继续写入
    end: false
  })

  // 提供了on方法监听事件
  // data事件 表示每次渲染完成 执行的方法
  Stream.on('data', data => console.log(111, '渲染一部分'))

  // end 事件 表示已经渲染完成
  Stream.on('end', (...args) => {
    res.write(`</div>
        </body>
        </html>`);

    // 配置end 方法 中断链接
    res.end()
  })
})
// 监听端口号
app.listen(3000, () => console.log(3000))
```



### 2.5 前后端同步渲染

前端渲染的问题：

​	 1 白屏时间长，

​	 2 无法做SEO优化

​	  ... ...

服务器端渲染的问题：

​	  ==为了解决上述问题，我们要前后端同步渲染。==

### 2.6 前后端渲染环境

前端渲染

​	是为了给用户使用，因此希望资源加载的快（压缩，打包，添加指纹等等）

​	为了更好的效果，我们要加载样式；为了看到页面，我们要处理模板等，......

但是服务器端渲染不需要考虑这些问题，只需要一个应用程序组件，并最终将其渲染成字符串。

因此前后端渲染有不同的入口文件，有不同的webpack配置

​	有不同的入口文件

​		前端入口文件，要渲染应用程序组件；后端入口文件，只需要提供一个应用程序组件

​	不同的webpack配置

​		前端要考虑样式，模板，性能优化（压缩，打包，添加指纹等）等

​		后端只要将ES Module规范，编译成CommonJS规范即可。

### 2.7 目录部署

config 	存储配置文件

home 	前端开发的项目

views 	模板目录

static 	静态资源

app.js 	服务器入口文件

### 2.8 资源发布

将静态资源发布到外面的static目录中

将模板资源发布到外面的views/index.html文件中

发布html文件用html-webpack-plugin插件

​	template 	  定义模板文件位置

​	filename 	  模板文件发布位置

​	hash 		 	是否添加指纹（添加在query上）

​	inject 			是否注入静态资源（默认是注入的）

压缩资源：压缩js，压缩html，压缩css

### 2.9 拆分文件

默认是将样式文件和js文件打包在一块的:

将模块文件打包在一起：main.js

将样式文件打包在一起: style.cssmini-css-extract-plugin插

​	我们使用件

​		css和scss|less样式拆分，使用该插件加载机

​		我们通过插件的filename属性定义文件发布位置。

对资源添加指纹：css资源，js资源等

​	压缩css使用optimize-css-assets-webpack-plugin插件

​	压缩js： mode: ‘production



```js
	optimization: {
        //拆分文件
		splitChunks: {		
            //公用缓存分组	
			cacheGroups: {		
				lib: {
                    // 库文件名称
					name: 'lib',
                    // 初始化的时候
					chunks:	'initial',
                    // 库文件特征
					test: /node_modules/
				}	
            }	
        }	
    }
```

### 2.10 npm 指令

由于webpack配置存储到特定位置，因此启动webpack指令很长

webpack --config 文件位置

为了简化webpack的运行，我们可以配置一些npm指令，运行：

​	npm run client 	运行前端配置

​	npm run server 	运行服务器端配置

​	npm run start 		同时运行前后端配置

​	npm run start:client 	监听并运行前端配置

​	npm run start:server 	监听并运行后端配置

我们要在package.json中，配置scripts项，来定义指令。

### 2.11 服务器端配置

- 服务器端渲染不同点：

  1 不需要模板，不用写html插件

  2 不需要样式，不用写style-laoder，不需要拆分样式

  3 不需要性能优化、拆分模块，添加指纹，服务器端本地引入文件，因此打包在一起就可以了等

  4 通过target: node配置，说明给node使用。

  5 在output.libraryTarget: commonjs2, 将ES Module规范转成commonjs规范

  6 将default接口暴露出来

在参数对象中，通过filename属性，可以自定义json文件的发布位置。

### 2.6 webpack 配置

开发的应用程序使用的是ES Module规范，我们要分别为前后端发布应用程序。

服务器端渲染不同点：

​	 1 是给node环境使用的。

​	 2 node中不需要编译模板，不需要处理样式，不需要压缩，打包等等

​	 3 将ES Module规范编译成CommonJS规范

​	 4 将default接口暴露出来

​	 5 入口文件不同，

​		 浏览器端：渲染应用程序

​		 服务器端：返回应用程序

### 2.7 hydrate

使用前后端渲染的技术，服务器端已经将页面渲染完成了，前端只需要添加交互就可以了，

所以第一次创建页面的时候，不需要渲染虚拟DOM，因此React渲染库提供了`hydrate`方法。

==该方法会判断页面是否被服务器端渲染过，如果渲染过，将直接绑定交互，第一次创建不会渲染虚拟DOM。==

​	 该方法的用法与render是一样的。

注意：使用前后端渲染的技术，一定要保证前后端给组件传递的属性是一致的。

使用hydrate方法的时候，不要使用带有static名字的渲染方法，否则会将服务器端渲染的标志删除，而无法优化。



