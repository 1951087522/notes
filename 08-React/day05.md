# React 第五天

## 一、React

### 1.1 Diffing 算法

>   **算法复杂度**

​	 在某一时间节点调用 React 的 render() 方法，会创建一棵由 React 元素组成的树。在下一次 state 或 props 更新时，相同的 render() 方法会返回一棵不同的树。React 需要基于这两棵树之间的差别来判断，如何有效率的更新 UI，以保证当前 UI 与最新的树保持同步。

​	 生成将一棵树转换成另一棵树的最小操作数的算法。 即使在最前沿的算法中，该算法的复杂程度为 O(n 3 )，其中 n 是树中元素的数量

​	 如果在 React 中使用了该算法，那么展示 1000 个元素所需要执行的计算量将在十亿的量级范围。这个开销实在是太过高昂

>   **React** **算法复杂度**

React 在以下两个假设的基础之上提出了一套 O(n) 的启发式算法：

​		 **两个不同类型的元素会产生出不同的树；**

​		 **开发者可以通过 key prop 来暗示哪些子元素在不同的渲染下能保持稳定；**

在实践中，我们发现以上假设在几乎所有实用的场景下都成立。

当对比两颗树时，React 首先比较两棵树的根节点。不同类型的根节点元素会有不同的形态。

>   **对比不同类型的元素**

当根节点为不同类型的元素时，React 会拆卸原有的树并且建立起新的树。举个例子，

​		 当一个元素从 <a> 变成 <img>，

​		 从 <Article> 变成 <Comment>，

​		 或从 <Button> 变成 <div> 都会触发一个完整的重建流程。

 当拆卸一颗树时，对应的 DOM 节点也会被销毁。组件实例将执行 componentWillUnmount() 方法。当建立一颗新的树时，对应的 DOM 节点会被创建以及插入

到 DOM 中。组件实例将执行 componentWillMount() 方法，紧接着 componentDidMount() 方法。所有跟之前的树所关联的 state 也会被销毁。

 在根节点以下的组件也会被卸载，它们的状态会被销毁。

>   **变更举例**

如，当比对以下更变时：

```html
	<div>
		<Counter />
	</div>
	<span>
		<Counter />
	</div>

```

React 会销毁 Counter 组件并且重新装载一个新的组件。

>   **对比同类型的元素**

当比对两个相同类型的 React 元素时，React 会保留 DOM 节点，仅比对及更新有改变的属性。比如：	<div className="before" title="stuff" />

```html
	<div className="after" title="stuff" />
```

通过比对这两个元素，React 知道只需要修改 DOM 元素上的 className 属性。
当更新 style 属性时，React 仅更新有所更变的属性。比如：

```html
	<div style={{color: 'red', fontWeight: 'bold'}} />
	<div style={{color: 'green', fontWeight: 'bold'}} />
```

通过比对这两个元素，React 知道只需要修改 DOM 元素上的 color 样式，无需修改 fontWeight。在处理完当前节点之后，React 继续对子节点进行递归

>   **对比同类型的组件元素**

当一个组件更新时，组件实例保持不变，这样 state 在跨越不同的渲染时保持一致。React 将更新该组件实例的 props 以跟最新的元素保持一致，并且调用该实例的 componentWillReceiveProps() 和 componentWillUpdate() 方法。

下一步，调用 render() 方法，diff 算法将在之前的结果以及新的结果中进行递归。

>   **对子节点进行递归**

在默认条件下，当递归 DOM 节点的子元素时，React 会同时遍历两个子元素的列表；当产生差异时，生成一个 mutation。在子元素列表末尾新增元素时，更变

开销比较小。比如：

```html
	<ul>									
  		<li>first</li>								
 		<li>second</li>							
	</ul>										
变更成：									
	<ul>
        <li>first</li>
        <li>second</li>
        <li>third</li>
    </ul>
```

React 会先匹配两个 <li>first < /li> 对应的树，然后匹配第二个元素 < li>second < /li> 对应的树，最后插入第三个元素的 < li>third < /li> 树。

>   **效率低下的变更**

在列表头部插入会很影响性能，并且更变开销会比较大。比如：

```html
	<ul>									
  		<li>first</li>								
 		<li>second</li>							
	</ul>										
变更成：									
	<ul>
        <li>third</li>
        <li>first</li>
        <li>second</li>
    </ul>
```

React 会针对每个子元素 mutate 而不是保持相同<li>first</li> 和 <li>second</li> 子树完成。这种情况下的变更低效并且可能会带来性能问题。

>   **keys**

为了解决以上问题，React 支持 key 属性。当子元素拥有 key 时，React 使用 key 来匹配原有树上的子元素以及最新树上的子元素。以下例子在新增 key 之后使得之前的低效转换变得高效：

```html
	<ul>									
  		<li key="2015">first</li>								
 		<li key="2016">second</li>							
	</ul>										
变更成：									
	<ul>
        <li key="2014">third</li>
        <li key="2015">first</li>
        <li key="2016">second</li>
    </ul>
```

现在 React 知道只有带着 '2014' key 的元素是新元素，带着 '2015' 以及 '2016' key 的元素仅仅移动了。

>   **设置****keys**

现实场景中，产生一个 key 并不困难。你要展现的元素可能已经有了一个唯一 ID，于是 key 可以直接从你的数据中提取：

```js
	 <li key={item.id}>{item.name}</li>
```

当以上情况不成立时，你可以新增一个 ID 字段到你的模型中，或者利用一部分内容作为哈希值来生成一个 key。这个 key 不需要全局唯一，但在列表中需要保持唯一。



==**使用keys注意事项**==

- 对于遍历列表的时候 要设置key值
- key值可以是下标  但是不要破坏原有列表的顺序  如果有破坏列表顺序的操作就不要使用下标作为key
- 尽量使用数据中的唯一值  例如id

>   **注意事项**

协调算法是一个实现细节。React 可以在每个 action 之后对整个应用进行重新渲染，得到的最终结果也会是一样的。在 context 下，重新渲染要在所有组件内调用 render 方法，这不代表 React 会卸载或装载它们。React 只会基于以上提到的规则来决定如何进行差异的合并。React团队定期探索优化算法，让常见用例更高效地执行。在当前的实现中，可以理解为一棵子树能在其兄弟之间移动，但不能移动到其他位置。在这种情况下，算法会重新渲染整棵子树。由于 React 依赖探索的算法，因此当以下假设没有得到满足，性能会有所损耗。

​		 该算法不会尝试匹配不同组件类型的子树。如果你发现你在两种不同类型的组件中切换，但输出非常相似的内容，建议把它们改成同一类型。在实践中，我们没有遇到这类问题。

​		 Key 应该具有稳定，可预测，以及列表内唯一的特质。不稳定的 key（比如通过 Math.random() 生成的）会导致许多组件实例和 DOM 节点被不必要地重新创建，这可能导致性能下降和子组件中的状态丢失。



==**Diffing 算法总结**==

前提：

​		 1 类型相同的两个元素（组件），我们认为是同一个。

​		 2 通过判断key，props来决定虚拟DOM或者组件是否应该更新

​		 根据以上两个前提，可以得到一个O（n）算法

比较顺序

​	 	1 比较根元素类型

​		 2 比较元素属性

​		 3 比较元素样式

​		 4 比较组件属性

​		 5 比较列表

​				 ①没有设置key，按照顺序比较，涉及排序，会有性能问题，

​				 ②设置了key，按照key字段比较，涉及排序，不会有性能问题

注意：

​		1 如果组件DOM树结构相似，建议成抽象成同一个组件。

​		2 key要稳定，可预测，列表唯一



### 1.2 **PureComponent**

> PureComponent	该组件主要是为了对类组件做性能优化的。	
>

​		 功能类似`shouldComponentUpdate`方法

使用方式与Component一样：

​		 class 组件类 extends PureComponent {}

​				 此时当组件的数据改变的时候，组件才会更新

注意：

​	 1 只对类组件生效，对函数组件无效

​	 2 只对数据（属性，状态）做浅层的比较，如果是引用类型的数据，不会深比较。

​			此时还得用 Component 的 shouldComponentUpdate 方法

```jsx
import React, { Component, PureComponent } from "react";
import { render } from 'react-dom'

// class App extends Component {
//   constructor(props) {
//     super(props);
//     this.state = {
//       isShow: false
//     }
//   }

//   // 监听页面滚动
//   componentDidMount() {
//     window.addEventListener('scroll', () => {
//       if (scrollY > 300) {
//         this.setState({ isShow: true })
//       } else {
//         this.setState({ isShow: false })
//       }
//     })
//   }

//   render() {
//     return (
//       <div className="app">
//         {/* 返回顶部 */}
//         <GoTop isShow={this.state.isShow}></GoTop>
//       </div>
//     )
//   }
// }

// // 1.继承 PureComponent 该组件主要是为了对类组件做性能优化的。
// class GoTop extends PureComponent {
//   // class GoTop extends Component {
//   render() {
//     console.log("GoTop");
//     return (
//       <div style={{
//         position: 'fixed',
//         right: 50,
//         bottom: 80,
//         fontSize: 20,
//         backgroundColor: 'pink',
//         // 通过属性控制显隐
//         display: this.props.isShow ? '' : 'none'
//       }}>
//         返回顶部
//       </div>
//     )
//   }
// }
// render(<App></App>, app)

// ------------------------------------------------

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      data: {
        isShow: false
      }
    }
  }

  // 监听页面滚动
  componentDidMount() {
    window.addEventListener('scroll', () => {
      if (scrollY > 300) {
        this.setState({ data: { isShow: true } })
      } else {
        this.setState({ data: { isShow: false } })
      }
    })
  }

  render() {
    return (
      <div className="app">
        {/* 返回顶部 */}
        <GoTop data={this.state.data}></GoTop>
      </div>
    )
  }
}

// 2.只对数据（属性，状态）做浅层的比较，如果是引用类型的数据，不会深比较。此时还得用 Component 的 shouldComponentUpdate 方法
// class GoTop extends PureComponent {
class GoTop extends Component {

  // 优化组件更新
  shouldComponentUpdate(props) {
    return props.data.isShow !== this.props.data.isShow
  }

  render() {
    console.log("GoTop");
    return (
      <div style={{
        position: 'fixed',
        right: 50,
        bottom: 80,
        fontSize: 20,
        backgroundColor: 'pink',
        // 通过属性控制显隐
        display: this.props.isShow ? '' : 'none'
      }}>
        返回顶部
      </div>
    )
  }
}
render(<App></App>, app)
// 总结:
    // PureComponent 只针对于类组件生效 并且 只能够处理浅层次的数据结构
    // 对于深层次的数据结构 使用shouldComponentUpdate实现优化更新
```



### 1.3 memo 方法

> react提供了memo方法，可以对函数组件做优化。
>

```js
用法：memo(Comp, (currentProps, nextProps) => {
		 currentProps：表示当前的属性数据
		 nextProps：表示新的属性数据
		 必须有返回值，表示是否不更新：true 表示不更新、 false 表示更新
 		与shouldComponentUpdate返回值正好相反，true表示更新 }
       ) 
```



用一个函数处理组件，并返回一个新组件，这个新组件就是高阶组件，这个方法（函数）叫高阶方法。

```jsx
import React, { Component, PureComponent, memo } from "react";
import { render } from 'react-dom'

// 定义组件类
class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      isShow: false
    }
  }

  // 组件构建完成之后
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

  render() {
    return (
      <div className="app">
        {/* 返回顶部组件 */}
        <DealMemo isShow={this.state.isShow}></DealMemo>
      </div>
    )
  }
}

// 对于函数组件无法继承PureComponent
let GoTop = props => {
  console.log('props');
  return (
    <div style={{
      position: 'fixed',
      right: 50,
      bottom: 80,
      fontSize: 20,
      backgroundColor: 'pink',
      // 通过属性控制显隐
      display: props.isShow ? '' : 'none'
    }}>
      返回顶部
    </div>
  )
}

// 2.还可以处理类组件 但是只能处理属性数据 无法处理状态数据
/* class GoTop extends Component {
  render() {
    console.log("GoTop");
    return (
      <div style={{
        position: 'fixed',
        right: 50,
        bottom: 80,
        fontSize: 20,
        backgroundColor: 'pink',
        // 通过属性控制显隐
        display: this.props.isShow ? '' : 'none'
      }}>
        返回顶部
      </div>
    )
  }
} */

// 使用memo
let DealMemo = memo(GoTop, (nowProps, newProps) => {
  /* 第一个参数 组件类
  第二个参数 回调函数
    第一个表示 当前属性数据
    第二个参数 新的属性数据 */
  // 必须有返回值 true 表示不更新 false更新
  return nowProps.isShow === newProps.isShow
})

render(<App></App>, app);

/* 
  memo 方法可以处理函数组件(主要应用) 类组件无法处理内部的状态 只能处理属性
  返回值是一个新组件 原来的组件不受影响 返回的新组件才能被优化
*/
```



### 1.4 cloneElement

有时候想复制一个虚拟DOM，可以使用`cloneElemnet`方法

用法与createElement方法类似



​	 cloneElement(vdom, props, ...children)

​			 第一个参数表示虚拟DOM

​			 第二个参数表示属性对象

​			 从第三个参数开始表示传递给新的虚拟DOM的子内容。



cloneElement与createElenent方法一样，也有jsx语法的等价语法。

​		 <vdom.type key={value} {...props} ></vdom.type>

```jsx
// 引入核心库
import React, { Component, cloneElement } from 'react';
// 渲染库
import { render } from 'react-dom';

// 定义虚拟DOM
let title = <h1 title="hello world" className="nihao" style={{ fontSize: 30 }}>hello world1</h1>;

// 定义数据对象
let data = {
  title: 'nihao123',
  className: 'box2',
  style: {
    fontSize: 40,
    color: 'pink'
  }
}

// 定义组件类
class App extends Component {
  render() {
    return (
      <div>
        <h1>app part</h1>
        <hr />
        {title}
        <hr />
        {/* 使用cloneElement方法 */}
        {/* 
            第一个参数表示虚拟DOM
            第二个参数表示属性对象
            从第三个参数开始表示传递给新的虚拟DOM的子内容。
        */}
        {cloneElement(title, null, '你好明天')}
        {cloneElement(title, { title: 'hello123', className: 'box', style: { color: 'red' } }, '你好明天')}
        <hr />

        {/* 有jsx语法的等价语法。 */}
        <title.type title="nihao222">hello 333</title.type>
        {/* 使用扩展运算符 添加数据 */}
        <title.type {...data} title="nihao"  >hello 333</title.type>
      </div>
    );
  }
}

// 渲染
render(<App></App>, app); 
```



### 1.5 Fragment

作用类似vue中的template元素。

> `Fragment` 是用来定义一个模板的组件，
>
> ​	 该组件可以作为根节点，但是不会渲染到页面中。
>

工作中:

​	 1 用来渲染多个兄弟根元素

​	 2 避免在特定元素之间，引入新元素。

```jsx
import React, { Component, Fragment } from "react";
import { render } from 'react-dom'

class Tds extends Component {
  // 2.避免在特定元素之间 引入新元素
  render() {
    return (
      <Fragment>
        <td>111</td>
        <td>222</td>
        <td>333</td>
      </Fragment>
    )
  }
}

class App extends Component {
  // 定义渲染方法
  createList() {
    // 1.用来渲染多个兄弟根元素
    return this.props.arr.map(item => (
      <Fragment key={item}>
        <li>{item}</li>
        <hr></hr>
      </Fragment>
    ))
  }

  render() {
    return (
      <div>
        <ul>{this.createList()}</ul>
        {/* 2.表格 */}
        <table>
          <tbody>
            <tr>
              <Tds></Tds>
            </tr>
            <tr>
              <Tds></Tds>
            </tr>
            <tr>
              <Tds></Tds>
            </tr>
          </tbody>
        </table>
      </div>
    )
  }
}

render(<App arr={['苹果', '西瓜', '香蕉', '橘子']}></App>, app);
```



### 1.6 错误边界组件

> 在一个组件中，如果某个子组件出现了错误，整个页面都无法渲染了。
>
> ​	 子组件中的错误冒泡到父组件中，影响了父组件的渲染。
>
> ​	 所以react提供了错误边界技术，让错误冒泡到错误边界组件中，这样就不会影响父组件的渲染了
>

错误边界组件

- 只需要定义`componentDidCatch`方法，就可以捕获错误，避免错误冒泡。

  ​			 第一个参数表示错误信息

  ​			 第二个参数表示错误描述信息

- 在错误边界组件中，可以定义静态方法`getDerivedStateFromError`。

  ​	 该方法的返回值表示错误产生的时候，修改的状态数据。

### 1.7 children

不论是虚拟DOM还是组件，其props属性中，都有一个children属性，代表子内容

​	 可以是文本，对象，数组。

我们可以直接通过`children`属性获取内容。

```jsx
// 引入核心库
import React, { Component } from 'react';
// 引入渲染库
import { render } from 'react-dom';


// 定义组件类
class App extends Component {

  render() {
    return (
      <div>

        <Error>
        {/* <h1>app part</h1> */}
        {/* 出现错误 */}
          <h1 style="color: red;">hello h1</h1>
        </Error>
        <hr></hr>

        <Error>
          <Child></Child>
        </Error>
      </div>
    )
  }
}

// 2.定义错误边界组件
class Error extends Component {
  // 定义状态
  constructor(props) {
    super(props);
    this.state = {
      hasError: false
    }
  }

  // 定义静态方法 getDerivedStateFromError 该方法返回值表示错误产生的时候 修改的状态数据
  static getDerivedStateFromError() {
    return {
      hasError: true
    }
  }

  render() {
    console.log(111, this.state.hasError);
    if (this.state.hasError) {
      return <h1>error</h1>
    } else {
      return this.props.children
    }
  }
}


// 子组件
class Child extends Component {
  // 1.只需要定义 componentDidCatch方法 就可以捕获 避免错误冒泡 但是父组件也出现错误就无法使用了
  // componentDidCatch() {
  //   console.log(arguments);
  // }

  render() {
    return (
      <div>
        {/* <h1>child part</h1> */}

        {/* 当出现语法错误 子组件中的错误冒泡到父组件中，影响了父组件的渲染。*/}
        <h2 style="color: red;">child part</h2>
        <hr></hr>
      </div>
    )
  }
}

render(<App></App>, app);
```



### 1.8 异步组件

> 将所有的组件都打包在一起，会导致文件很大，加载很慢，影响用户体验。
>
> ​	 所以我们要使用异步组件，让我们可以异步的加载组件。
>

 我们要使用`import`方法来异步加载，需要安装插件：

​		 babel-plugin-syntax-dynamic-import

 注：webpack 4.0的babel插件已经支持异步加载了。

### 1.9 lazy

 该方法可以异步加载组件

​	 参数回调函数

 回调函数的返回值表示加载的组件，有两种情况

​	 1 是promise对象，

​			 在promise对象中，做异步操作，再异步加载组件

​	 2 是import方法，

​			 在import中，直接异步加载组件

lazy方法加载的组件无法直接渲染，必须通过Suspense组件渲染。

### 1.10 Suspense

该组件就是用来负责渲染lazy方法加载的组件。

通过`fallback`属性，定义组件加载完成之前的提示内容。



==注意 当使用异步加载组件的时候 要在 webpack 中修改静态资源引入的路径 （publicPath）==

```js
        // 修改静态资源引入的相对路径
        publicPath: './dist/'
```



```jsx
import React, { Component, lazy, Suspense } from "react";
import { render } from "react-dom";

/* 
  lazy  该方法可以异步加载组件  参数回调函数
    回调函数的返回值表示加载的组件，有两种情况
      1 是promise对象，
        在promise对象中，做异步操作，再异步加载组件
      2 是import方法，
        在import中，直接异步加载组件
lazy方法加载的组件无法直接渲染，必须通过Suspense组件渲染。
*/
// 1.通过异步的方式引入
let Demo = lazy(() => new Promise(resolve => {

  // 点击的时候加载组件
  window.addEventListener('click', () => {
    resolve(import('./Demo'));
  })

}))

// 2.在import中，直接异步加载组件
// let Demo = lazy(() => import('./Demo'));

// 定义组件类
class App extends Component {
  render() {
    return (
      <div>
        <h1>app part</h1>
        <hr></hr>

        {/* Suspense 该组件负责渲染lazy方法加载的组件 */}
        {/* 
          该组件就是用来负责渲染lazy方法加载的组件。
            通过fallback属性，定义组件加载完成之前的提示内容。
        */}
        <Suspense fallback={<h1>loadding...</h1>}>
          {/* 使用组件 */}
          <Demo></Demo>
        </Suspense>
      </div>
    );
  }
}

render(<App></App>, app)
```



### 1.11 组件的约束性

用户可以与组件中的表单元素交互，这样就产生了数据，这些数据存储在哪里决定了组件的约束性。

> ​	 如果交互产生的数据存储在元素自身，该组件就是非约束性组件。
>
> ​	 如果交互产生的数据存储在组件状态中，该组件就是约束性组件。
>

### 1.12 ref

react提供了一种新的创建ref对象的方式。

​	 通过`createRef`可以创建一个ref对象

目的：代替ref字符串方式

优势：允许我们在组件外部获取组件内部的元素。



- 使用ref对象分成三步

  ​	 第一步 在构造函数中，创建ref对象，并存储在组件自身

  ​	 第二步 让ref对象指向元素。

  ​	 第三步 通过ref对象的current属性获取元素。

### 1.13 非约束性组件

交互产生的数据存储在==元素自身==，该组件就是一个非约束性组件。获取数据，存储数据都要通过该元素。

- 默认值

  ​	 	我们通过`defaultValue`或者`defaultChecked`属性定义默认值。都是非元素属性。

- 获取数据

  ​		 1 为元素设置ref属性

  ​		 2 通过this.refs获取元素，再通过元素获取数据

- 修改数据

  ​		 1 为元素设置ref属性

  ​		 2 通过this.refs获取元素，再修改元素的数据，

```jsx
import React, { Component, createRef } from "react";
import { render } from 'react-dom'

/* 
  使用ref对象分成三步
    1.在构造函数中或全局 创建ref对象 并存储在组件自身
    2.让ref对象指向元素
    3.通过ref对象的current属性获取元素
*/
// 全局ref对象
let password = createRef()

class App extends Component {

  // 获取数据/修改数据： 1 设置ref  2 获取对应的元素并获取内容
  // 获取数据
  getData() {
    console.log(this.refs.username.value);  // 这种方式即将淘汰
    console.log(password.current.checked);
  }

  // 修改数据
  setData() {
    this.refs.username.value = 'like'
    password.current.checked = false
  }

  render() {
    return (
      <div>
        <p>
          <label>用户名:</label>
          {/* 1.默认值 通过 defaultValue 或 defaultChecked 属性定义  都是非元素属性 */}
          <input type='text' defaultValue={'admin'} ref='username'></input>
        </p>
        <p>
          <label>密码:</label>
          <input type='checkbox' defaultChecked ref={password}></input>
        </p>
        <p>
          <button onClick={e => this.getData()}>获取数据</button>
          <button onClick={e => this.setData()}>修改数据</button>
        </p>
      </div>
    )
  }
}
render(<App></App>, app)
```



### 1.14 约束性组件

交互产生的数据存储在==组件的状态中==，该组件就是一个约束性组件。获取数据，修改数据都通过组件的状态。

- 默认值

  ​		 为value属性和checked属性绑定组件的状态数据。

  ​		 在`onChange`方法中，监听数据的改变，并更新状态

  ​				 如果是输入框，我们还可以做表单校验。判断内容是否合法：

  ​						合法则更新状态、

  ​						不合法则不更新状态

- 获取数据

  ​	 就是获取组件的状态数据。

- 修改数据

  ​	 就是修改组件的状态数据。

```js
import React, { Component } from "react";
import { render } from 'react-dom'

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      username: 'admin',
      password: true
    }
  }

  // 改变用户名的方法 在onChange 方法中 监听数据文档改变并更新状态
  changeUsername(e) {
    // 修改状态数据
    this.setState({ username: e.target.value })
  }

  // 获取数据
  getData() {
    console.log(this.state);
  }

  // 修改数据
  setData() {
    this.setState({
      username: 'like',
      password: false
    })
  }

  render() {
    return (
      <div>
        <p>
          <label>用户名:</label>
          {/* 1.通过value 设置默认值 */}
          {/* 在onChange 方法中 监听数据文档改变并更新状态 */}
          <input type='text' value={this.state.username} onChange={e => this.changeUsername(e)}></input>
        </p>
        <p>
          <label>密码:</label>
          {/* 2.通过value属性绑定组件的状态数据 */}
          <input type='checkbox' checked={this.state.password} onChange={e => this.setState({ password: e.target.checked })}></input>
        </p>
        <p>
          <button onClick={e => this.getData()}>获取数据</button>
          <button onClick={e => this.setData()}>修改数据</button>
        </p>
      </div >
    );
  }

}
render(<App></App>, app)

/* 
  react也可以看成是MVVM模式的框架。
    数据双向绑定有两个方向：
      数据由模型进入视图： 插值语法   // checked={this.state.password}
      数据由视图进入模型： 事件监听   // onChange={e => this.changeUsername(e)}
  react中数据双向绑定是手动实现的，vue中通过v-model指令实现。
*/
```



### 1.15 MVVM 模式

react也可以看成是MVVM模式的框架。

> 数据双向绑定有两个方向：
>
> ​		 数据由模型进入视图： 插值语法
>
> ​		 数据由视图进入模型： 事件监听
>

react中数据双向绑定是手动实现的，vue中通过v-model指令实现。



### 1.16 下拉框的约束性

通过`select`元素定义下拉框，通过`option`元素定义选项。

​	 选项的值默认是内容值，设置了value就是value值。

​	 下拉框分成单选下拉框和多选下拉框，通过`multiple`属性切换。

### 1.17 单选下拉框

- 非约束性

  ​		 默认值：通过`defaultValue`来定义。

  ​		 获取数据：获取元素再获取数据

  ​		 修改数据；获取元素再修改数据

- 约束性

  ​		 默认值：为select元素的`value`属性绑定状态，在`onChange`事件中，更新状态。

  ​		 获取数据：获取状态数据

  ​		 修改数据：修改状态数据。

  

```jsx
import React, { Component, createRef } from "react";
import { render } from 'react-dom'

// A.非约束性
// class App extends Component {

//   constructor(props) {
//     super(props);
//     this.color = createRef();
//   }

//   // 获取数据
//   getData() {
//     console.log(this.color.current.value);
//   }

//   // 修改数据
//   setData() {
//     this.color.current.value = 'isBlue'
//   }

//   render() {
//     return (
//       <div>
//         <p>
//           {/* 1.通过 defaultValue 定义默认值 */}
//           {/* <select defaultValue='green'> */}
//           {/* 选项的值默认是内容值 设置了value 就是value值 */}
//           <select defaultValue='isGreen' ref={this.color}>
//             <option value='isRed'>red</option>
//             <option value='isBlue'>blue</option>
//             <option value='isGreen'>green</option>
//           </select>
//         </p>
//         <p>
//           <button onClick={e => this.getData()}>获取数据</button>
//           <button onClick={e => this.setData()}>修改数据</button>
//         </p>
//       </div >
//     )
//   }
// }
// render(<App></App>, app)

// B.约束性
class App extends Component {

  constructor(props) {
    super(props);
    this.state = {
      color: 'isRed'
    }
  }

  // 修改内容值方法
  changeColor() {
    this.setState({ color: e.target.value })
  }

  // 获取数据
  getData() {
    console.log(this.state);
  }

  // 修改数据
  setData() {
    this.setState({
      color: 'isBlue'
    })
  }

  render() {
    return (
      <div>
        <p>
          {/*  默认值：为select元素的value属性绑定状态，在onChange事件中，更新状态。 */}
          <select value={this.state.color} onChange={e => this.changeColor(e)}>
            <option value='isRed'>red</option>
            <option value='isBlue'>blue</option>
            <option value='isGreen'>green</option>
          </select>
        </p>
        <p>
          <button onClick={e => this.getData()}>获取数据</button>
          <button onClick={e => this.setData()}>修改数据</button>
        </p>
      </div >
    )
  }
}
render(<App></App>, app)
```



### 1.18 多选下拉框

- 实现非约束性多选下拉框，以及约束性多选下拉框。
  	单选下拉框的值是字符串
    	多选下拉框的值是数组
  遍历下来的options属性，获取每一个option的值。

```js
import React, { Component, createRef } from 'react'
import { render } from 'react-dom'

// // A.非约束性
// class App extends Component {

//   constructor(props) {
//     super(props);
//     // 创建ref
//     this.colors = createRef()
//   }

//   // 1.获取数据
//   getData() {
//     // 获取options 选项
//     let options = this.colors.current.options

//     // 定义数组存储每一项的值
//     let arr = []

//     // 遍历
//     for (let i = 0; i < options.length; i++) {
//       // 判断当前项是否被选中
//       if (options[i].selected) {
//         // 存储数据
//         arr.push(options[i].value)
//       }
//     }

//     // 展示数据
//     console.log(arr);
//   }

//   // 2.修改数据
//   setData() {
//     // 2.1 定义修改的数组
//     let arr = ['isRed', 'isGreen', 'isBlue']

//     // 2.2 获取options项
//     let options = this.colors.current.options

//     // 2.3.a 遍历options 选项
//     /* for (let i = 0; i < options.length; i++) {
//       // 判断成员是否存在
//       if (arr.indexOf(options[i].value) >= 0) {
//         // 如果找到就勾选当前项
//         options[i].selected = true
//       } else {
//         options[i].selected = false
//       }
//     } */

//     // 2.3.b Es5
//     /* Array.from(options, item => {
//       if (arr.indexOf(item.value) >= 0) {
//         // 如果找到就勾选当前项
//         item.selected = true
//       } else {
//         item.selected = false
//       }
//     }) */

//     // 2.3.c 简化
//     // Array.from(options, item => item.selected = arr.indexOf(item.value) >= 0 ? true : false)

//   }

//   render() {
//     return (
//       <div>
//         <p>
//           {/* 下拉框分成单选下拉框和多选下拉框，通过multiple属性切换。 */}
//           {/* 默认值：通过defaultValue来定义。 */}
//           {/* 选项的值默认是内容值 设置了value 就是value值 */}
//           <select multiple defaultValue={['isGreen', 'isRed']} ref={this.colors}>
//             <option value='isRed'>red</option>
//             <option value='isGreen'>green</option>
//             <option value='isBlue'>blue</option>
//           </select>
//         </p>
//         <p>
//           <button onClick={e => this.getData()}>获取数据</button>
//           <button onClick={e => this.setData()}>修改数据</button>
//         </p>
//       </div>
//     );
//   }
// }


// B.约束性
class App extends Component {

  constructor(props) {
    super(props);
    this.state = {
      colors: ['isRed', 'isBlue']
    }
  }

  // 修改内容值得方法
  changeColor(e) {
    // 定义空数组
    let arr = []

    // 转为数组
    Array.from(e.target.options, item => {
      if (item.selected) {
        // 存储每一个值
        arr.push(item.value)
      }
    })

    // 修改
    this.setState(this.state)
  }

  // 1.获取数据
  getData() {
    console.log(this.state);
  }

  // 2.修改数据
  setData() {
    this.setState({
      colors: ['isRed', 'isGreen']
    })
  }

  render() {
    return (
      <div>
        <p>
          {/* 默认值：为select元素的value属性绑定状态，在onChange事件中，更新状态。 */}
          {/* 默认值：通过defaultValue来定义。 */}
          {/* 选项的值默认是内容值 设置了value 就是value值 */}
          <select multiple value={this.state.colors} onChange={e => this.changeColor(e)}>
            <option value='isRed'>red</option>
            <option value='isGreen'>green</option>
            <option value='isBlue'>blue</option>
          </select>
        </p>
        <p>
          <button onClick={e => this.getData()}>获取数据</button>
          <button onClick={e => this.setData()}>修改数据</button>
        </p>
      </div>
    );
  }
}
render(<App></App>, app)
```

