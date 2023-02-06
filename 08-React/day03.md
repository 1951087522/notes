# React 第三天

## 一、React

### 1.1上下文数据-context

组件之间可以通信，但是如果组件之间的嵌套层级很深，此时通信成本很高。所以react就提供了一个上下文数据对象：Context。

​	 工作中一些全局的配置数据可以放在上下文数据对象中，例如：网站的主题配置，语言包配置

使用数据：使用上下文数据分成两步

​	 第一步 创建上下文数据对象，核心库react提供了`createContext`方法，可以创建上下文数据对象

​			 参数对象就是默认的数据，创建的结果：提供了两个组件：`Provider`和`Consumer`

​	 第二步 让组件获取上下文数据对象中的数据。

​			 类组件：通过静态属性`contextType`来接收，属性值就是上下文数据对象。

​					 组件中，通过`this.context`属性获取上下文数据。

​			 函数组件：通过`Consumer`传递，内容是一个方法，参数是上下文数据，返回值就是创建的虚拟DOM

### 1.2 修改上下文数据

上下文数据对象提供了`Provider`组件，可以修改上下文中的数据，

​	 通过`value`属性修改。

注意：Provider组件修改数据后：

> ​	 Provider组件的子组件将接收新的数据
>
> ​	 Provider组件的父组件以及兄弟组件不受影响。
>

当多次使用Provider组件的时候，子组件会根据就近原则，从最近的Provider组件中获取数据。

```jsx
import React, { Component, createContext } from 'react'
import { render } from 'react-dom'

// 1.创建上下文数据
let contextData = createContext({
  // 主题
  theme: 'gold',
  // 字体
  lang: "中文"
})

// 3.解构数据
let { Provider, Consumer } = contextData

// 定义类组件
class App extends Component {
  render() {
    return (
      <div>
        <h1 style={{ color: this.context.theme }}>app part {this.context.lang} </h1>
        <hr></hr>
        {/* 
          3.修改上下文的数据 provider
            当内部的子组件修改了上下文数据 只能影响后代组件
            不会影响父组件和兄弟组件
        */}
        <Provider value={{ theme: 'pink', lang: 'English' }}>
          <Child></Child>
        </Provider>
        <hr></hr>
        <Other></Other>
      </div>
    )
  }
}

// 2.类组件接收数据 通过静态属性 contextType 属性值就是上下文数据对象
App.contextType = contextData

// 儿子组件
class Child extends Component {
  render() {
    return (
      <div>
        <h2 style={{ color: this.context.theme }}>Child part {this.context.lang} </h2>
        <hr></hr>
        {/* 2.1 接收数据也可以采取父向子传递数据 */}
        {/* <Demo lang={this.context.lang} theme={this.context.theme}></Demo> */}

        {/* 2.2 可以使用扩展远赴一次性传递数据 */}
        <Demo {...this.context}></Demo>
      </div>
    )
  }
}
// 使用上下文数据
Child.contextType = contextData

// 儿子函数组件
// 函数组件使用 Consumenr接收数据
let Other = () => {
  return <Consumer>{({ theme, lang }) => {
    return <h2 style={{ color: theme }}>Other part {lang} </h2>
  }}
  </Consumer>
}

// 孙子组件
class Demo extends Component {
  render() {
    return (
      <div>
        {/* 孙子组件通过父组件 Child接收数据*/}
        <h3 style={{ color: this.props.theme }}>Demo part {this.props.lang} </h3>
      </div>
    )
  }
}

render(<App></App>, app)
```



### 1.3 生命周期

为了描述组件创建，存在，销毁的过程，react也为组件提供了生命周期技术。

​	 共分三大周期：

​			 创建期，

​			 存在期，

​			 销毁期。

### 1.4 创建期

创建期共分五个阶段，描述组件创建的过程。

​	 在ES5开发中：getDefaultProps, getInitialState, componentWillMount, render, componentDidMount

​	 在ES6开发中，创建期的前两个阶段方法有所改变。

（ 1 ）定义组件默认属性数据： `defaultProps`静态属性

​		此时组件尚未创建。属性值就是默认的属性数据。

​		 没有为组件传递属性数据，组件就会使用该默认属性数据

（ 2 ）构造函数（初始化状态数据）：`constructor`

​		 构造函数有两个参数：属性数据，上下文数据。

​		 构造函数中，要使用super方法实现构造函数式继承。我们要向super方法传递属性数据和上下文数据

​				如果没有传递属性数据和上下文数据：此时参数props与this.props不同，并且参数context与this.context不同

​				如果传递了属性数据和上下文数据：此时参数props与this.props全等，并且参数context与this.context全等。

​		 				所以工作中建议我们传递属性数据和上下文数据。

​				但是由于工作中，上下文数据不会经常使用。所以通常我们只传递属性数据就可以了。在构造函数中，我们为this.state赋值，实现状态数据的初始化。

​		 我们可以用属性数据和上下文数据为状态数据赋值，实现数据由外部流入内部。

​				 这么做的原因是：可以在组件内部维护（修改）属性数据和上下文数据（不会影响其它组件）。

（ 3 ）组件即将构建： `componentWillMount`

​		此时已经有了属性数据，状态数据和上下文数据，但是还不能获取虚拟DOM。

​		工作中，我们可以在该阶段发送请求，初始化插件等

（ 4 ）渲染输出虚拟DOM： `render`

​		这个方法主要用来渲染虚拟DOM。返回值就是渲染的虚拟DOM，有且只有一个根元素。

​		注意：render方法主要负责渲染，所以不要在该方法中实现业务逻辑（如发送请求等）。

​	   不要在该方法中，尝试修改数据（如状态数据）。虚拟DOM是在返回值中创建的，因此在render方法中，无法获取虚拟DOM对应的真实DOM。

 （ 5 ）组件构建完毕：`compomentDidMount`

​		此时组件就创建出来了，已经有了属性数据，状态数据，上下文数据以及虚拟DOM。

​		该方法执行完毕，标志着创建期的结束，存在期的开始。我们可以在该方法中，绑定事件（全局），发送请求，使用插件等等。

注：创建期的方法在组件一生中，只能执行一次。后面四个方法中this都指向组件，后三个方法没有参数。

### 1.5 获取元素

渲染库react-dom提供了`findDOMNode`方法，可以获取虚拟DOM对应的真实DOM元素。

​		获取的是源生DOM，因此要用源生的DOM API去操作。

注意：工作中尽量不要这样去操作虚拟DOM，因为对源生DOM的操作是会被React忽视掉的。

```jsx
// 导入核心库
import React, { Component, createContext } from 'react';
// 导入渲染库
import { render, findDOMNode } from 'react-dom';

// 创建上下文数据
let ColorContext = createContext({
    color: 'green'
})

// 定义组件类
class App extends Component {
    // 2 定义构造函数
    constructor(props, context) {
        // 但是由于工作中，上下文数据不会经常使用。所以通常我们只传递属性数据就可以了
        super(props, context);
        console.log(222, 'constructor', 'props:', props, this.props, props === this.props,  'context:', context, this.context);
    }

    // 3 组件即将构建
    componentWillMount() {
        console.log(333, 'componentWillMount', this.props, this.context, findDOMNode(this));
    } 
    
    // 5 构建完成
    componentDidMount() {
        console.log(555, 'componentDidMount',  this.props, this.context, findDOMNode(this));
    } 

    // 4
    render() {
        // 在return之后 才可以访问虚拟DOM 在此之前无法访问
        // console.log(444, 'render', this.props, this.context, findDOMNode(this));
        console.log(444, 'render', this.props, this.context);
        return (
            <div>
                <h1>hello app</h1>
            </div>
        );
    }
}

// 定义上下文数据
App.contextType = ColorContext;

// 1 
// 定义默认数据
App.defaultProps = {
    msg: 'hello msg'
}

console.log(111, 'defaultProps');

// 除了第一个阶段  后面四个方法中this都指向组件，后三个方法没有参数。


// 渲染
render(<App title="nihao"></App>, app);
```



### 1.6 存在期

存在期跟创建期一样，也分五个阶段，来描述组件数据更新的过程。

​	一旦组件的 ==属性数据 、状态数据、上下文数据==改变，组件就会进入存在期。五个阶段：

（ 1 ）组件即将接收新的属性数据： `componentWillReceiveProps`

​		只有属性数据改变，才会执行该阶段方法，状态数据改变不会执行。

​		第一个参数表示新的属性数据。this上的还是旧的属性数据和旧的状态数据。

​		我们可以用属性数据更新状态，实现外部的数据存储到内部。将外部数据存储到组件内部是该阶段的唯一作用。

​		此时新的属性数据和新的状态数据一同进入第二个阶段方法。

（ 2 ）组件是否应该更新: `shouldComponentUpdate`

​		第一个参数表示新的属性数据。第二个参数表示新的状态数据。

​		this上的还是旧的属性数据和旧的状态数据。

​		必须有返回值，表示是否更新：

​				true，表示更新、

​				false，表示不更新。

​	  工作中，通常比较属性数据是否改变或者状态数据是否改变来决定是否更新

​	  作用：起到更新优化的作用（如：在高频事件中应用）。

（ 3 ）组件即将更新: `componentWillUpdate`

​		第一个参数表示新的属性数据。第二个参数表示新的状态数据。

​		this上的还是旧的属性数据和旧的状态数据。数据在该阶段还没有更新，该阶段执行完毕，数据才会更新，我们可以在该阶段调整插件等等。

（ 4 ）组件渲染虚拟DOM: `render`

​		没有参数，数据已经更新了，与创假期render是同一个方法。

​		this上的已经是新的属性数据和新的状态数据了。

​		所以渲染虚拟DOM的时候，就可以使用这些新的数据了，

（ 5 ）组件更新完毕: `componentDidUpdate`

​		第一个参数表示旧的属性数据。

​		第二个参数表示旧的状态数据。

​		this上的已经是新的属性数据和新的状态数据了。

​		虚拟DOM也可以更新了，所以在该阶段新数据，旧的数据都可以访问到，

​		我们可以在该阶段，继续请求数据，处理插件，调整事件等等。

​		组件已经更新完毕，只是一次更新结束，存在期仍然继续。

​		注：这些方法中的this都指向组件



- 属性数据：

```jsx
import React, { Component } from 'react'
import { render } from 'react-dom'

class App extends Component {
  // 定义构造函数
  constructor(props) {
    super(props)
    this.state = {
      isShow: false
    }
  }

  // 监听页面滚动 组件创建之后
  componentDidMount() {
    window.addEventListener('scroll', () => {
      if (scrollY > 300) {
        this.setState({ isShow: true })
      } else {
        this.setState({ isShow: false })
      }
    })
  }


  // 4.渲染虚拟DOM 
  render() {
    return (
      <div>
        <h2>app part</h2>
        <hr></hr>
        {/* 使用组件 */}
        <GoTop isShow={this.state.isShow}></GoTop>
      </div>
    )
  }
}

class GoTop extends Component {

  // 1.组件即将接受属性数据
  componentWillReceiveProps(props) {
    console.log(111, 'componentWillReciveProps', '参数:', props, '自身的:', this.props);
  }
  // 2.组件是否更新 常在此阶段优化
  shouldComponentUpdate(props) {
    console.log(222, 'shouldcomponentUpdate', '参数:', props, '自身的:', this.props);
    // return true

    // 优化 判断数据是否不相等 不相等是才更新
    return props.isShow !== this.props.isShow
  }
  // 3.组件即将更新
  componentWillUpdate(props) {
    console.log(333, 'componentWillUpdate', '参数:', props, '自身的:', this.props);
  }
  // 5.组件更新完毕
  componentDidUpdate(props) {
    console.log(555, 'componentDidUpdate', '参数:', props, '自身的:', this.props);
  }

  // 4.渲染虚拟DOM
  render() {
    console.log(444, 'render', this.props);
    return (
      <div style={{
        position: 'fixed',
        right: 50,
        bottom: 50,
        width: 100,
        height: 100,
        background: 'gold',
        textAlign: 'center',
        lineHeight: '100px',
        color: 'fff',
        // 通过属性数据控制元素显影
        display: this.props.isShow ? '' : 'none'
      }}>返回顶部
      </div>
    )
  }
}

render(<App></App>, app)

/* 
  存在期中修改属性数据
    从第一个阶段方法开始执行
    前三个阶段生命周期中的参数都是新的 this身上的数据还是旧的 说明此时数据还未更新
    直到第四个阶段 render 方法 this身上的数据已经是新的 说明从三阶段之后 四阶段之前 数据才开始更新
    到组件更新完毕 参数表示旧的数据 this身上的数据是新的 因为此阶段数据已经更新完了
*/
```

- 状态数据

```js
import React, { Component } from 'react'
import { render, findDOMNode } from 'react-dom'

class App extends Component {
  render() {
    return (
      <div>
        <h2>app part</h2>
        <hr></hr>
        {/*使用组件 */}
        <GoTop></GoTop>
      </div>
    )
  }
}

class GoTop extends Component {
  // 定义构造函数
  constructor(props) {
    super(props)
    this.state = {
      isShow: false
    }
  }

  // 组件创建之后  
  componentDidMount() {
    // 监听页面滚动
    window.addEventListener('scroll', () => {
      if (scrollY > 300) {
        this.setState({ isShow: true })
      } else {
        this.setState({ isShow: false })
      }
    })
  }

  // 1.组件即将接受属性数据
  componentWillReceiveProps(props) {
    console.log(111, 'compoentWillReceiveProps');
  }

  // 2.组件是否更新
  shouldComponentUpdate(props, state) {
    // console.log(222, 'shouldCompoentUpdate', '参数:', state, '自身:', this.state);

    // 必须有返回值
    return props.isShow !== this.props.isShow || state.isShow !== this.state.isShow;
  }

  // 3 组件即将更新
  componentWillUpdate(props, state) {
    console.log(333, 'componentWillUpdate', '参数:', state, '自身的:', this.state);
  }

  // 5 组件更新完毕
  componentDidUpdate(props, state) {
    console.log(555, 'componentDidUpdate', '参数:', state, '自身的:', this.state);
  }

  // 4.渲染虚拟DOM 
  render() {
    console.log(444, 'render', '自身的:', this.state);
    return (
      <div style={{
        position: 'fixed',
        right: 50,
        bottom: 50,
        width: 100,
        height: 100,
        background: 'green',
        textAlign: 'center',
        lineHeight: '100px',
        color: '#fff',
        // 通过状态数据控制该元素的显示
        display: this.state.isShow ? '' : 'none'
      }}>
        返回顶部
      </div>
    )
  }
}
// 渲染组件
render(<App></App>, app)

/* 
  存在期中修改属性数据
    从第一个阶段方法开始执行
    前三个阶段生命周期中的参数都是新的 this身上的数据还是旧的 说明此时数据还未更新
    直到第四个阶段 render 方法 this身上的数据已经是新的 说明从三阶段之后 四阶段之前 数据才开始更新
    到组件更新完毕 参数表示旧的数据 this身上的数据是新的 因为此阶段数据已经更新完了

  存在期中修改状态数据
    从第二个生命周期开始执行
    其他什么生命周期方法的行为 跟修改属性数据是一致的
    同时属性和状态数据的改变 不会让生命周期执行两次 而是合并执行 
*/
```



### 1.7 存在期与上下文数据

上下文数据改变组件也会执行存在期，但是有一些不同

 	1 ==上下文数据不会执行shouldComponentUpdate方法。 上下文数据改变，组件更新不能被优化。==

​	 2 上下文数据与属性数据一样，都是从外部传递的， 

​			 因此也会执行第一个周期方法： compoenntWillReceiveProps。并且追加一个参数：新的上下文数据

​			 与之类似的方法是第三个周期方法：componentWillUpdate，也会追加一个参数：新的上下文数据

​			 componentDidUpdate方法没有影响。

​	 3 上下文数据与属性数据，状态数据一样，

 			也是在componentWillUpdate方法执行完毕之后，render方法执行之前更新的。



```jsx
// 引入核心库
import React, { Component, createContext } from 'react';
// 引入渲染库
import { render, findDOMNode } from 'react-dom';

// 创建上下文数据
let ColorContext = createContext({
    color: 'green'
})

// 解构数据
let { Provider } = ColorContext;

// 定义组件
class App extends Component {
    constructor(props, context) {
        super(props);
        this.state = {
            // 将上下文数据 转为状态数据
            color: context.color
        }
    }

    // 监听页面滚动
    componentDidMount() {
        window.addEventListener('scroll', () => {
            if (scrollY > 300) {
                this.setState({ color: 'red' })
            } else {
                this.setState({ color: 'green' })
            }
        })
    }


    render() {
        return (
            <div>
                <h1>app part</h1>
                <hr />
                {/* <GoTop isShow={this.state.isShow}></GoTop> */}

                {/* 更改上下文数据 */}
                <Provider value={{ color: this.state.color }}>
                    <GoTop></GoTop>
                </Provider>
            </div>
        );
    }
}


// 定义上下文数据
App.contextType = ColorContext;


class GoTop extends Component {

    constructor(props) {
        super(props);
        this.state = {
            // 将属性数据转为状态数据
            isShow: props.isShow
        }
    }


    // 1 组件即将接受属性数据
    componentWillReceiveProps(props, context) {
        console.log(111, 'componentWillReceiveProps', 'context:', context, 'this.context:', this.context);

        // 将外部数据存储到组件内部是该阶段的唯一作用。
        this.setState({  isShow: props.isShow });
    }

    // 2 组件是否应该更新
    shouldComponentUpdate(props, state) {
        console.log(222, 'shouldComponentUpdate', 'state:', state, 'this.state:', this.state, );

        // return true;

        // 判断数据是否相同 
        // return props.isShow !== this.props.isShow;

        // 兼容属性数据和状态数据
        return state.isShow !== this.state.isShow || props.isShow !== this.props.isShow;
    }

    // 3 组件即将更新
    componentWillUpdate(props, state, context) {
        console.log(333, 'componentWillUpdate', 'context:', context, 'this.context:', this.context, findDOMNode(this).style.display);
    }


    // 5 组件更新完毕了
    componentDidUpdate(props, state) {
        console.log(555, 'componentDidUpdate', 'state:', state, 'this.state:', this.state, findDOMNode(this).style.display);
    }
    
    
    // 4
    render() {
        console.log(444, 'render', 'this.context:', this.context);
        return <div style={{
            position: 'fixed',
            bottom: 50,
            right: 50,
            width: 100,
            height: 100,
            backgroundColor: 'pink',
            color: '#fff',
            fontSize: 20,
            textAlign: 'center',
            lineHeight: '100px',

            // 上下文数据改变
            color: this.context.color
        }}>返回顶部</div>
    }
}

GoTop.contextType = ColorContext;

/**
 *  属性数据:
 *      1 从第一个阶段方法开始执行的
 *      2 前三个阶段方法中的props参数 都是新的值  this.props中的值是旧的值  说明此时组件数据还没有更新
 *      3 在render阶段 this.props的值已经是最新的了  说明从该阶段开始 组件数据已经更新了
 *      4 componentDidUpdate阶段方法中 参数表示旧的值了 this.props是新的值
 * 
 * 
 *  状态数据:
 *      1 从第二个阶段方法开始执行
 *      2 对于其它的方法同 属性数据的改变都是一致的
 * 
 * 
 *   上下文数据的改变:
 *      1 上下文数据的参与 不会让组件得到更新优化的
 *      2 第一个阶段方法也会执行 并且追加了一个context数据 
 *      3 第三个阶段方法 也追加了context数据
 *      4 第五个阶段方法 没有改变
 *      由于上下文数据的参与 不能让组件得到更新优化 所以在工作中 没有必要的数据 不要定义在上下文对象中
 **/


// 渲染
render(<App></App>, app);

```



### 1.8 销毁期

组件从页面中删除，组件将进入销毁期，执行销毁期的方法：`componentWillUnmount`

​	 没有参数，this指向组件

这是我们最后一次访问组件了，该方法执行完毕，就再也无法访问组件了

```jsx
// 引入核心库
import React, { Component } from 'react';
// 引入渲染库
import { render, unmountComponentAtNode } from 'react-dom';

// 定义组件
class App extends Component {
    // 销毁期的方法
    componentWillUnmount() {
        console.log('组件销毁了');
    }

    render() {
        return (
            <div>
                <h1>app part</h1>
            </div>
        );
    }
}



// 渲染
render(<App></App>, app);

// 3s之后移除节点
setTimeout(() => {
    unmountComponentAtNode(app);
}, 3000);


// 总结:
    // 创建期: defaultProps  constructor  componentWillMount  render  componentDidMount
    // 存在期: componentWillReceiveComponent   shouldComponentUpdate   componentWillUpdate  render  componentDidUpdate
    // 销毁期: componentWillUnmount

```

### 1.9 存在期与非更新数据

属性数据，状态数据以及上下文数据改变，组件会更新，

==如果希望在组件中，维护的数据改变的时候，组件不更新，定义这类数据，有两种方式：==

​	 1 将数据存储在组件的实例上（构造函数定义）

​	 2 将数据存储在组件的原型上（特性方法定义）

​	 这两类数据改变，组件不会更新。

工作中，我们可以将定时器句柄，节流开关等数据放在这里。这样，我们改变数据，组件就不会更新了。

```js
import React, { Component } from "react";
import { render } from 'react-dom'

class App extends Component {
  render() {
    return (
      <div>
        <h1>app part</h1>
        <GoTop></GoTop>
      </div>
    )
  }
}

class GoTop extends Component {
  constructor(props) {
    super(props)
    // 1.实例数据
    this.num = 10
    // 赋值number
    this.number = 20
  }

  // 2.定义原型属性数据
  set number(val) {
    this._num = val
  }
  get number() {
    return this._num
  }

  // 监听页面滚动
  componentDidMount() {
    window.addEventListener('scroll', () => {
      this.num++
      this.number++
      console.log(this.num, this.number);
    })
  }

  // 1.组件即将接受属性数据
  componentWillReceiveProps(props, context) {
    console.log(111, 'componentWillReceiveProps');
  }

  // 2.组件是否更新
  shouldComponentUpdate(props, state) {
    console.log(222, 'componentUpdate');

    // 判断数据是否不相同
    return props.isShow !== this.props.isShow !== state.isShow !== this.state.isShow
  }

  // 3.组件即将更新
  componentWillUpdate(props, state, context) {
    console.log(333, 'componentWillUpdate');
  }

  // 5.组件更新完毕
  componentDidUpdate(props, state) {
    console.log(555, 'componentDidUpdate');
  }

  // 渲染虚拟DOM
  render() {
    console.log(444, 'render');
    return (
      <div style={{
        position: 'fixed',
        right: 50,
        bottom: 50,
        width: 100,
        height: 100,
        background: 'green',
        textAlign: 'center',
        lineHeight: '100px',
        color: '#fff'
      }}>
        返回顶部
      </div>
    )
  }
}
render(<App></App>, app);

```





### 1.10 非 React 类库

我们在react中，使用一些非react类库（第三方插件等，不是react家族的插件）的时候，一定要注意组件生命周期。

```jsx
// 引入核心库
import React, { Component } from 'react';
// 引入渲染库
import { render } from 'react-dom';
// 引入库文件
import ImageZoom from '../image-zoom.jsx';

// 使用方法
// new ImageZoom(app2, 'demo.jpg', 200)


// 定义组件
class App extends Component {
    // 在即将构建阶段无法获取真实的元素
    // componentWillMount() {
    // 组件构建完成
    componentDidMount() {
        new ImageZoom(this.refs.contain, 'demo.jpg', 200);
    }

    render() {
        return (
            <div>
                <h1>hello app</h1>
                <hr />
                <div className="container" ref="contain"></div>
            </div>
        );
    }
}

// 渲染
render(<App></App>, app);
```



### 1.11 使用侵入式类库

当我们使用非react类库的时候，我们可能会操作虚拟DOM，根据是否操作了虚拟DOM对应的真实DOM元素，可以将非react类库分成两类：

​		 非侵入式类库：没有操作虚拟DOM对应的真实DOM元素

​		 侵入式类库：直接操作了虚拟DOM对应的真实DOM元素

​				一个类库是否是侵入式的是相对的，

​		例如 使用jQuery：

​				如果使用jQuery只是发送请求等等，没有操作虚拟DOM对应的真实DOM元素，此时jQuery就是非侵入式的。

​				如果使用jQuery修改了元素样式，内容，属性等，操作了虚拟DOM对应的真实DOM元素，此时jQuery就是侵入式类库。

​		使用侵入式类库的时候，对真实DOM的操作，不会被react获知，==因此一定要注意组件的生命周期。==

​		在ES6中，使用jQuery，要安装jQuery：npm install jquery

工作中，能用react实现的效果，尽量使用react，不要使用侵入式类库实现。

```jsx
import React, { Component } from "react";
import { render } from 'react-dom'

// 1.引入jQuery 侵入式类库：直接操作了虚拟DOM对应的真实DOM元素
// import $ from 'jquery'

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      // 将属性数据作为状态数据
      arr: props.arr
    }
  }

  /*   // 1.使用jQuery 改变 需要两步 注意生命周期
    // 组件构建完毕
    componentDidMount() {
      // 设置样式
      $('li').css('color', 'gold')
    }
  
    // 组件更新完毕之后
    componentDidUpdate() {
      $('li').css('color', 'gold')
    } */

  // 定义渲染列表的方法
  createList() {
    return this.state.arr.map((item, index) => (
      // 工作中能用react实现的尽量使用react 不要使用侵入式类库
      <li li key={index} style={{ color: 'gold' }}> {item}</li>
    ))
  }

  render() {
    return (
      <div>
        <button onClick={e => this.setState({ arr: ['玄彬公开谈儿子长相难掩幸福', '曝N号房主犯曾跟踪调查金智秀', '陈乔恩自曝曾患睡眠呼吸中止症', '张雨绮疑遇航班突发事件受惊吓', '陈飞宇同妈妈哥哥散步'] })}>切换视图</button>
        <ul>{this.createList()}</ul>
      </div>
    );
  }
}

render(<App arr={['杜海涛与鹿晗同框面部圆润', '熊黛林郭可颂挽手逛街', '李小璐带女儿甜馨看电影']}></App>, app);
```

