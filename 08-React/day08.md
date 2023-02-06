# React 第八天

## 一、UI

### 1.1 富文本编辑器

react中富文本编辑器：`react-quill`，依赖quill。

​		 安装 npm i quill react-quill

​		安装完成，直接使用，

- 依赖quill模块中的三个样式文件

  ​		 quill.core.css，

  ​		 quill.snow.css，

  ​		 quill.bubble.css

- 可以实现数据双向绑定

  ​		 通过value绑定状态数据

  ​		 在onChnage事件中更新状态

   				参数就是内容。

  ```js
  import React, { Component } from 'react'
  import { render } from 'react-dom'
  // 引入 react-quill 模块
  import ReactQuill from 'react-quill'
  // 引入样式文件
  import 'react-quill/dist/quill.core.css'
  import 'react-quill/dist/quill.snow.css'
  import 'react-quill/dist/quill.bubble.css'
  
  class App extends Component {
      // 定义状态
      constructor(props) {
          super(props);
          this.state = {
              msg: ''
          }
      }
  
      render() {
          return (
              <div>
                  <h1>富文本编辑器</h1>
                  <hr></hr>
                  {/* 支持双向绑定 */}
                  <ReactQuill value={this.state.msg} onChange={msg => this.setState({ msg })}></ReactQuill>
                  {/* 渲染标签 */}
                  <h1 dangerouslySetInnerHTML={{ __html: this.state.msg }}></h1>
              </div>
          );
      }
  }
  
  render(<App></App>, app)
  ```

  

### 1.2 antd-mobile UI 移动框架

ant-design UI框架是由阿里金服团队维护的。	

​		 官网：https://ant.design/index-cn

​		 包含：pc端，移动端，native端等UI框架。

antd-mobile是其在移动端的一个ui框架	***

​		 官网：https://antd-mobile-v2.surge.sh/docs/react/introduce-cn

​		 我们学习的就是移动端的UI框架，移动端开发要设置mete标签

组件默认没有样式，我们要手动的webpack中引入样式 

​		 antd-mobile/dist/antd-mobile.css|less

组件命名规范：首字母大写



### 1.3 按需打包

在vue中，使用组件之前，建议全部安装，这样使用更方便。

在react中，为了优化，建议我们使用什么组件，引入什么组件。

所以react提供了一个按需打包的技术，与按需加载类似，都是为了性能优化。

**按需加载**

​		 在浏览器端，需要什么资源，加载什么资源（如：require.js, sea.js）.

**按需打包**

​		 在编译阶段，需要什么资源，打包什么资源

react提供了一个按需打包插件：babel-plugin-import插件

​		 在babel的配置options.plugins中配置该插件。是一个数组，每一个成员代表一个插件。

​				 成员可以是字符串，直接引入

​				 成员还可以是数组：

​						 第一个成员代表插件名称，

​						 第二个成员代表插件配置

​								 libraryName  按需打包的模块

​								 style 按需打包的样式文件类型

​		 注意：一旦使用按需打包的技术，样式就不需要手动引入了，会自动打包进来。

```jsx
import React, { Component } from "react";
import { render } from 'react-dom'
// 引入UI库
import { Button, WingBlank, WhiteSpace } from 'antd-mobile'
// 按需打包，不需要引入样式
// import 'antd-mobile/dist/antd-mobile.less';

class App extends Component {
    constructor(props) {
        super(props);
        this.state = {
            msg: ''
        }
    }

    render() {
        return (
            <div>
                {/* 设置样式 */}
                <WhiteSpace style={{ height: 300 }}></WhiteSpace>
                <Button type="primary">登录</Button>
                <WhiteSpace></WhiteSpace>

                {/* 两翼留白 */}
                <WingBlank>
                    <Button type="ghost">注册</Button>
                </WingBlank>

                <Button type='warning'>提交</Button>
                
            </div>
        );
    }
}

render(<App></App>, app)
```



### 1.4 element-react

与element-ui一样，是由同一个团队开发并维护的。

​		 官网：https://elemefe.github.io/element-react/#/zh-CN/quick-start

该模块默认没有样式，使用样式要安装。

​		 npm i element-react element-theme-default

样式文件中，默认引入了字体图标，我们要webpack使用`url-loader`加载机处理这些文件。

```js
// 配置icon
{
    test: /\.(woff|ttf)$/,
        loader: 'url-loader'
}
```

命名规范：组件首字母大写。

### 1.5 表单校验

- 对用户在表单中输入的内容做校验。


- 三个组件 vue中 react中

  ​		 el-form Form

  ​		 el-form-item From.Item (通常解构后，再使用Item)

  ​		 el-input Input

- 设置内容分成三步

  ​		 1 设置 Input 组件的 `placeholder` 属性

  ​		 2 设置 Form.Item 组件的 `label` 属性，代表该控件的字段名称。

  ​		 3 设置 Form组件 的 `labelWidth` 属性，设置label显示的宽度。

- 对输入的内容做校验共分四步：

  ​		 1 Input组件，实现数据双向绑定

  ​				 注意：如果字段很多，修改数据的时候，不要相互覆盖。

  ​		 2 Form组件，设置 `model` 属性,*校验的数据字段*

  ​		 3 Item组件，设置 `prop` 属性,*校验的具体数据*

  ​		 4 Form组件，设置 `rules` 属性。校验规则是一个对象，

  ​				 key表示校验字段的名称。 

  ​				 value是一个数组，表示多条规则，每一个成员是对象，代表一条规则。

  ​						 `required`  是否是必填的

  ​						 `message`  出现错误的时候，提示的内容

  ​						 `validator` 校验方法

  ​						 `trigger`  什么时候触发校验（默认是一边输入，一边校验）

- 表单提交的校验共分三步：

  ​		 1 绑定提交事件

  ​		 2 为Form组件设置ref。

  ​		 3 通过ref获取组件，调用validate方法校验表单。

```jsx
import React, { Component } from 'react'
import { render } from 'react-dom'
// 引入UI库
import { Form, Input, Button, } from 'element-react'
// 引入主题包
import 'element-theme-default'

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      form: {
        username: '',
        password: ''
      },
      // 校验对象
      rules: {
        username: [
          /* 
            required:必填项
            message:提示内容
            trigger:失去焦点触发
          */
          { required: true, message: '请输入用户名', trigger: 'blur' },
          // 自定义校验规则
          {
            validator: function (rule, value, cb) {
              if (!/^\w{4,8}$/.test(value)) {
                cb(new Error('用户名是4-8位数字字母下划线'))
              } else {
                // 验证通过也要执行 否则输入成功不校验
                cb()
              }
            }
          }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          {
            validator: function (rule, value, cb) {
              if (!/^\d.*[a-zA-Z]|[a-zA-Z].*\d$/.test(value)) {
                cb(new Error('密码是字母或者数字开头'))
              } else {
                cb()
              }
            }
          }
        ]
      }
    }
  }

  // 提交方法
  submit() {
    this.refs.form.validate(valid => {
      if (valid) {
        console.log('提交成功');
      }
    })
  }

  // 解构数据
  render() {
    let { form, rules } = this.state
    return (
      <div>
        {/* 
            设置内容分成三步
              1 设置Input组件的 placeholder 属性
              2 设置Form.Item组件的 label 属性，代表该控件的字段名称。
              3 设置Form组件的 labelWidth 属性，设置label显示的宽度。  
              
            设置校验
              1.Input 组件 实现数据双向绑定
              2.Form 组件 设置model属性 校验的数据字段
              3.Item 组件 设置prop 属性 校验的具体数据
              4.Form 组件 设置rules 属性 校验规则是一个对象
              
            提交校验
              1.绑定提交事件
              2.为 Form 组件设置 ref
              3.通过ref获取组件 调用 validate方法检验表单
        */}

        <Form labelWidth='200' style={{ width: 700, margin: '50px auto' }} model={form} rules={rules} ref='form'>
          <Form.Item label='用户名' prop='username'>
            <Input placeholder='请输入用户名' value={form.username} onChange={username => this.setState({ form: { ...form, username } })}></Input>
          </Form.Item>
          <Form.Item label='密码' prop='password'>
            <Input placeholder='请输入密码' value={form.password} onChange={password => this.setState({ form: { ...form, password } })}></Input>
          </Form.Item>
          <Form.Item>
            <Button type='primary' onClick={e => this.submit()}>登录</Button>
          </Form.Item>
        </Form >
      </div >
    )
  }
}
render(<App></App>, app)
```





## 二、create-react-app 脚手架

### 2.1 create-react-app

与vue-cli一样，create-react-app是react的一款脚手架。

 		内置很多功能，让我们可以直接进入开发。

安装指令

​		 npm i -g creact-react-app

​		 安装完毕，提供了create-react-app指令

创建项目

​		 create-react-app 项目名称

​		 此时链接网络就可以下载项目了。

create-react-app创建的项目也是通过yarn来管理和维护的，安装模块用yarn安装。

​		 例如：yarn add react-router react-router-dom redux react-redux

 yarn add指令功能与npm install指令是一样的。

### 2.2 目录部署

node_modules 依赖模块

public 静态目录

​			 favicon.ico：logo

​			 index.html：模板入口文件 

​			 manifest.json：离线缓存配置

src  开发目录

​		 App.css：应用程序样式

​		 App.js：应用程序脚本 

​		 App.test.js：应用程序单元测试

​		 index.css：全局样式 

​		 logo.svg：eact的logo

​		 registreServiceWorker.js：web workers

.gitignore  git 配置

package.json npm 包配置

README.md 介绍文件

yarn.lock yarn锁文件



### 2.3 指令

- create-react-app创建的项目提供了一些指令：

  ​		 start 启动项目，默认端口号是3000

  ​		 build  发布项目，默认发布到build目录下

  ​		 test  单元测试

  ​		 `eject`  输入webpack配置

任务：

​		 将静态资源发布到外面的`dist`目录下

​		 将模板资源发布到外面的`views`目录下

 这些指令既可以使用yarn来启动，也可以使用npm来启动。

### 2.4 PWA 应用

PWA应用是一个渐进式的web应用，介于源生app与网站页面之间的一个应用。

其中

​		 manifest.json就是离线缓存的文件

​		 registreServiceWorker.js也是为pwa应用提供的web worker文件。

在浏览器中，点击“更多工具-创建快捷方式”就可以在桌面上，创建一个离线应用。

## 三、单元测试

### 3.1 单元测试

为了确保代码质量，我们要对代码做测试，基于文件或者模块的测试我们称之为单元测试。

react也是使用ject框架进行测试的。

==默认的测试文件：.test. 为后缀的==

`yarn test`  单元测试

测试结果

​		 测试通过： 所有的测试单元都通过

​		 测试失败： 有一个测试单元失败了

### 3.2 测试方法

`describe` 整体描述

​		 第一个参数表示整体描述

​		 第二个参数表示测试内容

`it`  定义每一次测试

​		 第一个参数表示本次测试的描述

​		 第二个参数表示本次测试的实现

expect  测试的断言方法

​		 参数是运行的语句

​		 我们可以对返回值执行断言方法，判断结果

### 3.3 断言方法

- toBe    

   		 类似于`===`。例如：expect(true).toBe(true);

- toEqual

  ​		  比较变量字面量的值。例如： expect({ foo: 'foo'}).toEqual( {foo: 'foo'} );

- toMatch

  ​		是否匹配正则表达式。例如： expect('foo').toMatch(/foo/);

- toBeDefined

  ​	    检验变量是否定义。例如： var foo = { bar: 'foo'}; expect(foo.bar).toBeDefined();

- toBeNull

  ​	    检验变量是否为`null` 。例如： var foo = null; expect(foo).toBeNull(); 

- toBeTruthy

  ​	    检查变量值是否能转换成布尔型`真`值。例如： expect({}).toBeTruthy();

- toBeFalsy

  ​	    检查变量值是否能转换成布尔型`假`值。例如： expect('').toBeFalsy();

- toContain

  ​	    检查在数组中是否包含某个元素。例如： expect([1,2,4]).toContain(1);

- toBeLessThan

  ​	    检查变量是否小于某个数字。例如： expect(2).toBeLessThan(10);

- toBeGreaterThan

  ​	    检查变量是否大于某个数字或者变量。例如： expect(2).toBeGreaterThan(1);

- toBeCloseTo

  ​		 比较两个数在保留几位小数位之后，是否相等,用于数字的精确比较。

  ​		 例如： expect(3.1).toBeCloseTo(3, 0);

- toThrow

  ​		 检查一个函数是否会throw异常。例如： expect(function(){ return a + 1;}).toThrow(); // true

  ​		 expect(function(){ return a + 1;}).not.toThrow(); // false

- toHaveBeenCalled

  ​		 检查一个监听函数是否被调用过

- toHaveBeenCalledWith

  ​		 检查监听函数调用时的参数匹配信息

### 3.4 周期方法

beforeEach 每一个it语句执行前

afterEach · 每一个it语句执行后

beforeAll 所有的it语句执行前

afterAll 所有的it语句执行后

```js
// 引入测试文件
import addNum, { msg, obj } from './demo'

// 整体测试
describe('测试文件', () => {

  // 进行单次测试
  it('测试msg', () => {
    // 执行断言方法
    expect(msg)
      // 调用断言方法
      .toMatch(/msg/)
  })

  it('测试obj数据', () => {
    expect(obj)
      // 比较变量字面量的值
      .toEqual({ width: 100, height: 200 })
  })

  it('测试默认数据方法', () => {
    expect(addNum(3, 5))
      // 类似于 ===
      .toBe(8)
  })

  // 周期方法 不会跨文件
  // beforeEach 每一个it语句执行之前
  beforeEach(() => {
    console.log('beforeEach--it语句执行之前');
  })

  // afterEach 执行之后
  afterEach(() => {
    console.log('afterEach--执行之后');
  })

  // beforeAll 所有的it语句执行之前
  beforeAll(() => {
    console.log('befroeAll--所有的it语句执行之前');
  })
  // afterAll 所有的it语句执行之后
  afterAll(() => {
    console.log('afterAll--所有的it语句执行之后');
  })
})
```

