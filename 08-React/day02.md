# React 第二天

## 一、React

### 1.1  DOM事件

react中，为虚拟DOM绑定事件与真实的DOM绑定事件的语法是类似的。

​	 真实DOM绑定事件 <div onclick="fn"></div>

​	 虚拟DOM绑定事件 <div onClick={this.fn}></div>

> 注意：
>
> ​	 1 事件名称首字母要大写。
>
> ​	 2 事件回调函数要定义在组件中
>
> ​	 3 事件回调函数不要执行 (后面不要添加参数集合())
>
> ​	 4 默认参数就是事件对象
>
> ​	 5 this指向undefined
>

### 1.2 事件参数

默认参数表示事件对象（是React封装后的）

​	 因为react实现了事件委托：1 减少事件数量，2 预言未来元素，3 避免内存外泄

react框架时时刻刻想的都是优化。

注意：

​	 1 为了让虚拟DOM与真实的DOM元素相对应，react为虚拟DOM和真实DOM设置相同的 data-reactid属性。在之前的版本中，添加在标签中，在当前版本中，添加在dom元素上。

​	 2 使用插值语法时候，我们建议将表达式写在插值语法内部，这样会少创建元素。

### 1.3 ==this 指向==

- this默认指向`undefined`。为了更改this指向，react提供了三种方式：

  - 第一种，在构造函数中，重写原型方法，添加到实例对象自身，并绑定this。此时可以传递数据，传递的数据在事件对象之前。

  ​			 注：由于使用了继承，在构造函数中，要使用`super`关键字实现继承。

  - 第二种，使用事件回调函数的时候直接通过`bind`方法绑定。此时可以传递数据，传递的数据在事件对象之前。	注：第二种方式原则上讲，可以绑定任何对象，但是react不建议，只建议绑定组件实例对象。

    ​	与第一种方式相比：

  ​				 	第一种添加在实例上，第二种添加在原型上。

  ​					 第一种传递的参数是固定的，第二种可以自由的传递数据

  - 第三种， 可以通过箭头函数实现对this指向的更改。this指向render方法中的this，参数可以在任何位置传递（包括事件对象）。

​			由于箭头函数可以简写，非常方便，工作中常用。

```jsx
import React, { Component } from "react";
import { render } from 'react-dom'

class Demo extends Component {
  // 1.实例数据 定义在构造函数中
  constructor() {
    // 使用 super 实现继承
    super()

    // 解决按钮1 this指向undefined问题
    // 第一种，在构造函数中，重写原型方法，添加到实例对象自身，并绑定this
    this.clickBtn1 = this.clickBtn1.bind(this, 1, 2)
  }

  // 2.1 定义方法
  clickBtn1() {
    console.log('clickBtn1', this, arguments);
  }
  clickBtn2() {
    console.log('clickBtn2', this, arguments);
  }
  clickBtn3() {
    console.log('clickBtn3', this, arguments);
  }


  render() {

    // 3.2 定义对象
    let obj = { a: 1 }

    return (
      <div>
        <h2>DOM事件</h2>

        {/* 2.绑定事件使用小驼峰命名 内部this 指向 undefined */}
        <button onClick={this.clickBtn1}>按钮1</button>

        {/* 3. 改变this 
            第二种，使用事件回调函数的时候直接通过bind方法绑定 传递的数据是在事件对象之前*/}
        <button onClick={this.clickBtn2.bind(this, 1,)}>按钮2</button>
        {/* 原则上讲可以绑定任何对象 但是react不建议 只建议绑定组件实例对象 */}
        <button onClick={this.clickBtn2.bind(obj, 2)}>按钮2-obj</button>

        {/* 3.第三种， 可以通过箭头函数实现对this指向的更改。this指向render方法中的this 简化绑定的过程 */}
        <button onClick={e => this.clickBtn3(3)}>按钮3</button>
      </div>
    )
  }
}
render(<Demo></Demo>, app)
```



### 1.4 状态数据

状态与属性一样，都是为组件存储数据的。

​	 属性是从组件外部传递进来的，是组件外部的数据，从组件外部维护。

​	 状态是从组件内部定义出来的，是组件内部的数据，从组件内部维护。

我们在组件的内部维护（定义，获取和修改）状态数据

根据组件是否有状态数据，我们将组件分成两类：无状态组件，有状态组件。

### 1.5 无状态组件

组件创建后，不会因为交互而产生数组，不会发送异步请求获取数据等等，组件从创建到销毁是一成不变的，这类组件就是无状态组件，我们目前定义的组件都是无状态组件。

由于无状态组件中没有状态数据，因此可以将无状态组件简写成==函数组件==。

### 1.6 函数组件 无状态组件

​	 通过函数定义的组件

​		 	第一个参数表示属性数据

 			返回值表示渲染的虚拟DOM

​	 注：函数组件可以用箭头函数来简写，因此函数组件简化了组件的定义。

​	 类组件：通过类定义的组件

```jsx
import React, { Component } from "react";
import { render } from 'react-dom'

class Demo extends Component {
  // 定义渲染列表的方法
  creatList() {
    return this.props.arr.map(item => <li key={item}>{item}</li>)
  }

  render() {
    return (
      <div>
        {/* 渲染列表 */}
        <ul>{this.creatList()}</ul>
      </div>
    )
  }
}
render(<Demo arr={[1, 2, 3]}></Demo>, app)

// 1.类组件改写成函数组件
function App(props) {
  // 2.渲染列表函数
  function createList() {
    return props.arr.map(item => <li key={item}>{item}</li>)
  }
  // 3.返回虚拟DOM
  return <ul>{createList()}</ul>
}
render(<App arr={[4, 5, 6]}></App>, app)

// 简写上面的函数组件
function Demo2(props) {
  return <ul>{props.arr.map(item => <li key={item}>{item}</li>)}</ul>
}
render(<Demo2 arr={[7, 8, 9]}></Demo2>, app)

// 使用箭头函数简写
let Demo3 = props => <ul>{props.arr.map(item => <li key={item}>{item}</li>)}</ul>
render(<Demo3 arr={[9,8,7]}></Demo3>, app)
```



### 1.7 有状态组件

组件创建后，会因为交互而产生数据，会发送异步请求获取数据，这些在组件内部产生的数据就要放在状态中去存储。

- 获取状态

   获取属性：在组件中，通过`this.props`获取属性数据

  获取状态：在组件中，通过`this.state`获取状态数据

- 修改状态

  在组件内部修改状态通过`this.setState`方法.

  参数是一个对象，表示修改的数据集合

   			key：表示修改的状态数据名称 

  ​			value：表示修改的状态数据值

### 1.8 初始化状态

我们在构造函数（`constructor`）中，初始化状态数据: `this.state = {}`

​	 由于继承了组件基类，所以在构造函数中，要使用`super`关键字实现构造函数继承。

构造函数的第一个参数表示传递给组件的属性数据（`props`）。我们可以传递给super方法。

>  	如果没有传递，此时this.props是undefined与参数props不同
>
> ​	 如果传递了，此时this.props与参数props就是相同的了。

注：工作中，建议传递props，这样使用参数props或者this.props就没有区别了。

在构造函数中，我们可以访问到属性数据，==因此可以用属性数据为状态数据赋值==，实现数据由外部（属性数据）流入内部（状态数据）。

在工作中之所以这么做，因为==属性数据只能在外部维护，将属性数据存储到状态中，可以在组件内部维护修改这些数据了。==

==注：工作中，开发组件的时候，一定要分析出哪些是属性数据，哪些是状态数据。==

```jsx
import React, { Component } from "react";
import { render } from 'react-dom'

class App extends Component {

  // 1.在构造函数中定义状态
  constructor(props) {
    /* 
      构造函数式继承 super
        如果没有传递 此时 this.props 是undefined 与参数 props不同
        如果传递 此时 this.props 是undefined 与参数 props相同
    */
    super(props)
    console.log(this.props, props, this.props === props);

    // 2.初始化状态 通过state定义
    this.state = {
      color: 'red',
      num: 100,
      // 3.修改属性数据 将属性数据作为状态数据使用
      // 注：一定要分析出哪些是属性数据，哪些是状态数据
      
      // msg: props.msg
      // 3.1 使用扩展运算符 一次性写入
      ...props
    }

    // 3.修改数据
    // 2s之后修改数据
    setTimeout(() => {
      this.setState({ color: 'gold', msg: 999 })
    }, 2000)
  }

  render() {
    // 2.1 解构数据
    let { color, num } = this.state
    return (
      <div>
        <h2 style={{ color: color }}>{num} {this.props.msg} {this.state.msg}</h2>
      </div>
    )
  }
}
render(<App msg='123'></App>, app)
```

#### 1.8.2 换一换案例

```js
import React, { Component } from "react";
import { render } from 'react-dom'

class Demo extends Component {

  // 1.3 定义构造函数
  constructor(props) {
    super(props)
    this.state = {
      page: 0
    }
  }

  // 1.2渲染二级列表
  creatSecondList(arr) {
    return arr.map(item => (
      <p key={item}>{item}</p>
    ))
  }

  // 1.渲染列表
  createList() {
    // 1.5 解构数据 限制page范围
    let { page } = this.state
    page = page % this.props.arr.length

    return this.props.arr.map((item, index) => (
      // 1.2遍历二级列表
      <li key={index}
        style={{ display: page == index ? '' : 'none' }}>{this.creatSecondList(item)}
      </li>
    ))
  }

  // 1.4 点击换一换
  changeList() {
    // 更新状态 每次加1
    this.setState({ page: this.state.page + 1 })
  }

  render() {
    return (
      <div>
        {/* 1.4 绑定事件 */}
        <span onClick={e => this.changeList()}>换一换</span>
        <ul>{this.createList()}</ul>
      </div>
    )
  }
}

render(<Demo arr={[
  ['2022年华语乐坛十件大事', '主持人李小萌否认被家暴', '权志龙世巡计划设置港澳场'],
  ['李纯马頔疑似恋情曝光', '吴磊现身写字楼被偶遇', '陈飞宇拍照万年不变剪刀手'],
  ['好友晒窦靖童谢顶发型为其庆生', '白鹿醉酒爬树路透', '内马尔晒与梅西同框合照'],
]}></Demo>, app);
```



### 1.9 组件通信

虚拟DOM中可以有子虚拟DOM，组件是对虚拟DOM的模拟，因此在组件中也可以定义子组件。

在Parent组件中定义的Child组件称之为Parent组件的子组件，Parent组件就是父组件。

​	 <Parent>

​			 <Child />

​	 </Parent>

组件间通信：组件之间的通信通常分成三类

​	 父组件向子组件通信

​	 子组件向父组件通信

​	 兄弟组件之间通信

### 1.10 父组件向子组件通信

父组件向子组件通信很简单：直接为子组件传递属性数据或者方法

传递的是属性：可以是变量，属性数据，状态数据等等。

​	 这些传递的数据改变，子组件接收的数据也将同步改变。

- 传递的是方法：有两种使用方式

  ​	 **作为子组件的事件回调函数**

  ​			默认参数是事件对象，this默认指向undefined，我们可以更改this指向：

  ​				可以让this指向子组件，

  ​				可以让this指向父组件。一旦指向父组件就永远指向父组件，

  ​			更改this指向的时候，还可以传递数据，顺序：父组件， 子组件， 事件对象

  ​	 **在子组件方法中执行**

  ​			默认没有参数，this指向子组件的属性对象。我们可以绑定父组件，此时将永远指向父组件。

  ​			更改this指向的时候，还可以传递数据，顺序：父组件， 子组件

注：工作中，为了简化绑定父组件过程，可以直接使用箭头函数，

```jsx
import React, { Component } from "react";
import { render } from 'react-dom'

class Demo extends Component {

  // 2.定义构造函数
  constructor(props) {
    super(props)
    // 初始化状态
    this.state = {
      msg: '父组件向子组件通信'
    }
  }

  // 3.定义父组件的方法
  parentMethod() {
    console.log('parentMethod', this, arguments);
  }

  render() {
    // 2.解构数据
    let { msg } = this.state

    return (
      <div>
        <h2>父组件向子组件传递数据</h2>
        <hr></hr>

        {/* 1.子组件 */}
        {/* 2.传递属性 直接给子组件传递即可 */}
        {/* <Child msg={msg}></Child> */}

        {/* 3.传递方法 */}
        {/* 在传递方法的时候 bind 直接绑定父组件
            this指向父组件。一旦指向父组件就永远指向父组件 */}
        {/* <Child metod={this.parentMethod.bind(this, 1, 2)}></Child> */}

        {/* 简写 */}
        <Child method={(...args) => this.parentMethod()}></Child>
      </div >
    )
  }
}

// 定义子组件
class Child extends Component {

  // 3.2 定义事件 在子组件中方法执行
  childMethod(e) {
    this.props.method(3, 4, e)
  }

  render() {
    return (
      <div>
        {/* 2.获取属性 */}
        <h2>子组件 {this.props.msg}</h2>

        {/* 3.接受父组件方法的方式*/}
        {/* 
          1.作为子组件的数据回调函数
          2.在子组件中方法执行
              默认参数是事件对象 this默认指向 undefined
              更改this指向
              让this指向子组件
              让this指向父组件 
                在传递方法的时候直接绑定父组件 一旦指向父组件就永远指向父组件
              更改this指向的时候还可以传递数据
                      顺序：父组件 子组件 事件对象
        */}
        {/* 3.1 作为子组件的数据回调函数 */}
        {/* this默认指向 undefined */}
        {/* <input type='text' placeholder="this指向undefined" onChange={this.props.metod}></input> */}
        {/* 更改this指向  让this指向子组件 */}
        {/* <input type='text' placeholder="this指向子组件" onChange={this.props.metod.bind(this, 3, 4)}></input> */}

        {/* 3.2 在子组件中方法执行
            默认没有参数 this指向子组件的属性对象
              可以绑定父组件 此时将永远指向父组件
              更改this指向的时候还可以传递数据
                      顺序：父组件 子组件 事件对象
        */}
        <input type='text' placeholder="在子组件中方法执行" onChange={e => this.childMethod(e)}></input>
      </div>
    )
  }
}
render(<Demo></Demo>, app)
```



### 1.11 子组件向父组件通信

> 父组件可以向子组件传递属性和方法，传递的方法绑定了父组件，就永远指向父组件了，此时在方法中，修改状态数据，修改的是谁的状态数据呢？
>
> ​	 修改的是父组件的状态数据，这就是子组件向父组件通信的原理。
>

分成三步

​	 第一步 在父组件中，为子组件传递方法，并绑定父组件

​	 第二步 在子组件中，执行方法，并传递子组件中的数据

​	 第三步 在父组件中，接收数据，并更新状态

```jsx
import React, { Component } from "react";
import { render } from 'react-dom'

class App extends Component {
  constructor(props) {
    super(props)
    this.state = {
      msg: ''
    }
  }

  // 1.定义父组件的方法
  // parentMethod(msg) {
  //   // console.log('parentMethod', this, arguments);

  //   // 3.修改父组件状态数据
  //   this.setState({ msg })
  // }

  render() {
    return (
      <div>
        <h2>父组件 {this.state.msg}</h2>
        <hr></hr>
        {/* 1.父组件先准备好一个方法 */}
        {/* <Child method={e => this.parentMethod(e)}></Child> */}

        {/* 在方法中只有一句话的时候 可以简写*/}
        <Child method={e => this.setState({ msg: e.target.value })}></Child>
      </div>
    )
  }
}

// 子组件
class Child extends Component {
  render() {
    return (
      <div>
        <h2>子组件</h2>
        {/* 2.字组件中执行方法 */}
        {/* <input type='text' onChange={e => this.props.method(e.target.value)}></input> */}

        {/* 简写 */}
        <input type='text' onChange={e => this.props.method(e)}></input>
      </div>
    )
  }
}

render(<App></App>, app)
```



### 1.12 兄弟组件之间的通信

父组件向子组件通信：为子组件传递属性数据或者方法

子组件向父组件通信：为子组件传递方法并绑定父组件,子组件执行方法并传递数据,父组件接收数据并更新状态

如果这两个子组件互为兄弟组件，就可以实现兄弟组件之间的通信了。

​	 兄弟组件之间通信：

​				==一个子组件将数据传递给父组件，再由父组件将数据传递给另一个组件。==

​	 流程分成四步：

​			 1 在父组件中，为一个子组件传递方法，并绑定父组件 

​			 2 在子组件中，执行方法，并传递子组件中的数据

​			 3 在父组件中，接收数据，并更新状态 

​			 4 在父组件中，将新的状态数据，传递给另一个子组件。

 兄弟组件之间的通信的核心是：有一个相同的父组件，相当于媒婆、

对于不相干的两个组件是无法通信的，后面会学习上下文数据对象（context）以及通信框架flux, reflux, redux等

```jsx
import React, { Component } from "react";
import { render } from 'react-dom'

class App extends Component {
  constructor(props) {
    super(props)
    this.state = {
      msg: ''
    }
  }

  render() {
    return (
      <div>
        <h1>父组件 {this.state.msg}</h1>
        <hr></hr>
        {/* 1.使用子组件 */}
        <Child method={msg => this.setState({ msg })}></Child>
        <hr></hr>
        {/* 3.直接传递给第二个子组件属性 */}
        <Other msg={this.state.msg}></Other>
      </div>
    )
  }
}

// 子组件1
class Child extends Component {
  render() {
    return (
      <div>
        {/* 2.在子组件中，执行父组件传过来的方法，并传递子组件中的数据给父组件 */}
        <input type='text' onChange={e => this.props.method(e.target.value)}></input>
      </div>
    )
  }
}

// 子组件2
class Other extends Component {
  render() {
    return (
      <div>
        {/* 4.接收 */}
        <h2>结果：{this.props.msg}</h2>
      </div>
    )
  }
}

render(<App></App>, app)
```



### 1.13 axios

发送请求使用axios，不需要安装，直接使用。

- axios的使用方式与jquery类似，提供了一些简便方法：

  - get(url, config) 发送get请求

     			url表示请求地址，

    ​			config表示配置项（我们可以定义params，headers等）

  - post(url, data, config)  发送post请求

    ​			 url表示请求地址，

    ​	 		data：表示携带的数据，

    ​			 config：表示配置项（我们可以定义params，headers等）
  
  ​	注意：不论是get请求还是post请求，都可以携带query数据，query数据可以在两个位置添加
  
  ​		 1 在url上添加query数据。
  
   		2 在config配置中的params属性中传递。

- axios实现了promise规范，因此，通过then方法监听结果

  第一个参数表示成功时候的回调函数:参数表示请求对象，其中data属性表示返回的数据

  ​	 当多次使用then方法的时候，前一个then方法的返回值，将作为后一个then方法的参数。

   第二个参数表示失败时候的回调函数，也可以通过catch方法监听失败

  

  axios提交的数据，默认使用的是json格式。

   我们可以通过修改headers中的Content-Type字段，来模拟表单提交。

   模拟表单： application/x-www-form-urlencoded
  
  ```js
  import React, { Component } from "react";
  import { render } from 'react-dom'
  // 引入axios
  import axios from 'axios'
  
  class App extends Component {
  
    async componentDidMount() {
      // 1.方式一 发送请求
      /*
        第一个参数 路径
        第二个参数 配置项
          get请求 url上可以携带query 数据 第二个参数配置项中params也可以携带query数据
      */
      // axios.get('/data/home?num=100', { params: { color: 'red' } })
      //   .then(res => console.log(111, res))
      //   .catch(err => console.log(222, err))
  
      // // 2.post请求
      // /*  需要三个参数 
      //         url表示请求地址，
      //         data：表示携带的数据，
      //         config：表示配置项（我们可以定义params，headers等） */
      // axios.post('/data/home?num=100', { title: '你好' }, {
      //   params: { color: 'red' },
      //   // 模拟表单数据
      //   // 修改请求头
      //   headers: {
      //     'Content-Type': 'application/x-www-form-urlencoded'
      //   }
      // })
      // .then(res => console.log(111, res))
      // .catch(err => console.log(222, err))
  
  
      // 3.方式二 异步 需要配置项
      let res = await axios.get('/data/home?num=100', { params: { color: 'red' } })
      console.log(111, res);
    }
  
    render() {
      return (
        <div>
          <h2>axios</h2>
        </div>
      )
    }
  
  }
  render(<App></App>, app)
  ```



### 1.14 换肤案例

![](./assets/16.png)



```js
import React, { Component } from 'react'
import { render } from 'react-dom'
// 引入 axios
import axios from 'axios'
// 引入样式文件
import './skin.less'

class App extends Component {
  // 2.定义构造函数
  constructor(props) {
    super(props)
    this.state = {
      arr: []
    }
  }

  async componentDidMount() {
    // 1.发送请求
    let { data } = await axios.get('/data/skin.json')
    // 2.1请求完毕后更新数据
    this.setState({ arr: data })
  }

  // 5.点击更改图片方法
  changeImage(e, id) {
    // 为body 设置背景图片
    document.body.style.backgroundImage = `url(img/skin/big_${id}.jpg)`
  }


  // 3.定义渲染列表的方法
  createList() {
    return this.state.arr.map(item => {
      return (
        // 4.li 点击事件
        <li key={item.id} onClick={e => this.changeImage(e, item.id)}>
          <img src={'img/skin/' + item.src}></img>
          <p>{item.title}</p>
        </li>
      )
    })
  }

  render() {
    return (
      <div>
        {/* 3.渲染列表 */}
        <ul className='list'>{this.createList()}</ul>
      </div>
    )
  }
}

render(<App></App>, app)
```

