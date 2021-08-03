## React

### 引入文件

```html
<script src="./node_modules/react/umd/react.development.js"></script>
<script src="./node_modules/react-dom/umd/react-dom.development.js"></script>
```

### 创建元素

```react
const title = React.createElement('h1', null, 'Hello World')
```

参数1：元素名称，参数2：元素属性（没有填null），参数3、4、5、……：元素的子节点。

如果元素属性有多个则用`{title: '标题', id: 'p1'}`的格式框起来。

### 渲染元素

```react
ReactDOM.render(title, document.getElementById('root'))
```

参数1：要渲染的react元素，参数2：挂载点。

### 脚手架初始化项目

`npx create-react-app my-app`

启动项目：`npm start`

### 脚手架使用

导入两个包

```js
import React from 'react'
import ReactDOM from 'react-dom'
```

创建元素：`React.createElement()`

渲染元素：`ReactDOM.render()`

### JSX

#### createElement的问题

1. 过于繁琐
2. 不直观

#### 简介

JSX = JavaScript XML

声明式语法直观，与html的结构相同。

#### 使用步骤

1. 创建元素

   ```jsx
   const title = <h1>Hello World in JSX</h1>
   ```

2. 渲染元素

   ```jsx
   ReactDOM.render(title, document.getElementById('root'))
   ```

#### 注意点

1. 采用驼峰命名法。

2. 特殊的属性名：class->className for->htmlFor, tabindex->tabIndex

3. 使用小括号包裹JSX，避免js自动插入分号。

   ```jsx
   const a = (
   	<div>Hello World</div>
   )
   ```

#### 嵌入js表达式

语法：`{js表达式}`

```jsx
const name = 'Jack'
const a = (
	<div>My name is: {name}</div>
)
```

#### 条件渲染

```jsx
const ifLoading = true
const loadData = () => {
    if (ifLoading) {
        return (<div>数据加载中</div>)
    }
    return (
    	<div>数据加载完成</div>
    )
}
// loadData内return的是加载与否的html格式
const title = (
	<h1>
    	{loadData()}
    </h1>
)
// 也可以用三元表达式等等
```

#### 列表渲染

遍历一组数据使用map()，需要加上唯一的key，遍历谁给谁加key。（尽量避免用索引号作为key）

```jsx
const data = [
    {id: 1, name: 'a'},
    {id: 2, name: 'b'},
    {id: 3, name: 'c'}
]
const title = (
  <ul>{data.map(item => <li key={item.id}>{item.name}</li>)}</ul>
)
```

#### 样式处理

##### 行内样式style

```jsx
<h1 style={{color: 'red', backgroundColor: 'blue'}}>
	样式处理
</h1>
```

外层括号表示要引入js表达式，内层为表达式的内容。

##### 类名className

```jsx
<h1 className="title">
	样式处理
</h1>
```

直接写即可，注意引入样式文件。

### 组件

#### 创建组件

##### 使用函数创建

使用js的函数创建的组件

注意：1. 函数名称由大写字母开头。

2. 必须有返回值，表示组件的结构。
3. 返回值为null表示不渲染任何内容。
4. 用函数名作为组件名。

```jsx
function Hello () {
    return (
        <div>函数组件</div>
    )
}
ReactDOM.render(<Hello />, document.getElementById('root'))
```

使用箭头函数创建

```jsx
const Hello = () => <div>箭头函数</div>
ReactDOM.render(<Hello />, document.getElementById('root'))
```

##### 使用类创建组件

使用ES6的class创建的组件

注意：1. 类名称必须以大写字母开头。

2. 类组件应该继承React.Component父类，从而调用父类的方法或者属性。
3. 类组件必须有render()方法。
4. render()方法必须有表示组件结构的返回值。

```jsx
class Hello extends React.Component {
    render() {
        return <div>Hello World</div>
    }
}
ReactDOM.render(<Hello />, document.getElementById('root'))
```

#### 把组件抽离为独立的js文件

1. 创建`组件名.js`
2. 在`组件名.js`中导入React
3. 创建组件（函数或者类）
4. 导出组件
5. 导入组件
6. 渲染组件

```jsx
// 组件名.js
import React from 'react'
class Hello extends React.Component {
    render() {
        return <div>Hello World</div>
    }
}
// 导出组件
export default Hello
```

```jsx
// index.js
import Hello from '...'
// 渲染组件
ReactDOM.render(<Hello />, document.getElementById('root'))
```

#### 事件处理

##### 事件绑定

与DOM相似

on + 事件名称 = {事件处理程序}

`onClick = {() => {}}`

注意：React事件采用驼峰命名法。

在类中绑定：

```jsx
class Hello extends React.Component {
    handleOnClick () {
        alert('onclick!')
    }
    render() {
        return <button onClick={this.handleOnClick}>Hello World</button>
    }
}
```

在函数中绑定：

```jsx
function App () {
    function handleOnClick () {
        alert('onclick!')
    }
    render() {
        return <button onClick={handleOnClick}>Hello World</button>
    }
}
```

##### 事件对象

可以通过事件处理程序的参数获取到事件对象

React中的事件对象叫做合成事件（对象），兼容所有浏览器

```jsx
// 阻止浏览器的默认行为
e.preventDefault()
```

#### 有状态组件

即为类组件。

状态（state）即数据。

有自己的状态，负责更新UI（动）。

#### 无状态组件

即为函数组件。

状态（state）即数据。

没有自己的状态，只负责数据展示（静）。

#### state和setState()

##### state的基本使用

即数据，是组件内部的私有数据，只能在组件内部使用。

state的值是对象，表示一个组件内可有多个数据。

复杂的定义：

```jsx
constructor () {
   super()

   this.state = {
       count: 0
   }
}
```

简单的定义：

```jsx
state = {
    count: 0
}
```

使用：`this.state.count`

##### setState()修改状态

`this.setState({要修改的数据})`

注意：不能直接修改state中的值，一定要用setState。

```jsx
render() {
  return (
      <div>
          <div> {this.state.count} </div>
          <button onClick={() => {
              this.setState({
                  count: this.state.count + 1
              })
          }}>+1</button>
      </div>
  )
}
```

作用：1. 修改state 2. 更新UI 

**数据驱动视图思想**

##### 抽离事件处理程序

事件处理程序中的this为undefined，因而希望this指向组件实例

#### 事件绑定this指向

##### 箭头函数

箭头函数自身不绑定this

因而可以直接在jsx内部用箭头函数调用setState修改state。

##### Function.prototype.bind()

使用ES5的bind方法把this与组件实例绑定

```jsx
constructor () {
    super()
    this.increase = this.increase.bind(this)
}
increase () {
    this.setState({
        count: this.state.count += 1
    })
}
state = {
    count: 0
}
```

一旦绑定好了之后不管怎么使用this都不会变。

##### class的实例方法

利用箭头函数形式的class实例方法

```jsx
class Hello extends React.Component {
    increase = () => {
        this.setState({
            count: this.state.count += 1
        })
    }
    state = {
        count: 0
    }
    render() {
        return (
            <div>
                <div> {this.state.count} </div>
                <button onClick={this.increase}>+1</button>
            </div>
        )
    }
}
```

#### 表单处理

##### 受控组件

###### 定义

html的表单元素可输入，有自己的可变状态，但react一般通过state统一管理数据并用setState来改变。

因此react会把state和value绑定，由state的值控制表单元素的值value。

受控组件就是其值受到react控制的表单元素。

`<input type="text' value={this.state.txt} />`

###### 使用步骤

1. 在state中添加一个状态作为表单元素的value值（控制表单元素值的来源）
2. 给表单元素绑定事件将其值设置为state的值（控制表单元素值的变化）

```jsx
state = {txt: ''}
<input type="text" value={this.state.txt}
    onChange={e => this.setState({ txt: e.target.value })}
/>
```

###### 多表单元素优化

使用一个事件处理程序处理多个元素的值

1. 给表单元素添加name属性，名称与state相同
2. 根据表单元素类型获取对应的值
3. 在change事件处理过程中通过[name]修改对应的state

```jsx
<input type="text" name="txt" value={this.state.txt} onChange={this.handleForm} />

// 根据表单元素的类型获取值
const value = target.type === 'checkbox' ? target.checked : target.value

// 根据name设置对应的state
this.setState({
    [name]: value
})
```

##### 非受控组件（DOM）

借助于ref，使用原生DOM获取表单元素。

1. 调用React.createRef()创建一个ref对象。

   ```jsx
   constructor () {
       super()
       this.txtRef = React.createRef()
   }
   ```

2. 把创建好的ref对象添加到文本框中

   ```jsx
   <input type="text" ref={this.txtRef} />
   ```

3. 通过ref对象获取文本框的值

   ```jsx
   this.txtRef.current.value
   ```

#### 组件通讯

#### 组件的props

用于接收传递给组件的数据。

1. 给组件标签添加属性以传递数据

2. 接收数据：函数组件通过参数props，类组件通过this.props。

   ```jsx
   <Hello name="jack" age={19} />
   
   function Hello(props) {
       console.log(props)
       return (
       	<div>接收数据：{props.name}</div>
       )
   }
   ```


特点：1. 可以传任意类型的数据，{}代表具体的数字，''代表字符串。

2. props只读，不能从内部修改外部。

3. 类组件若写了构造函数则应将props传递给super()（只针对constructor内部）。

   ```jsx
   class Hello extends React.Component {
       constructor(props) {
           super(props)
       }
       render() {
           return <div>{this.props.name}</div>
       }
   }
   ```

#### 父组件->子组件

1. 父组件提供要传递的state。

2. 给子组件的标签添加属性，值为state中的数据。

   ```jsx
   class Parent extends React.Component {
       state = {lastName:'一'}
       render() {
           return (<div><Child name={this.state.lastName}></Child></div>)
       }
   }
   ```

   ```jsx
   function Child(props) {
       return <div>{props.name}</div>
   }
   ```

3. 子组件通过props接收数据。

#### 子组件->父组件

1. 父组件提供回调函数用于接收数据。

2. 子组件通过props调用回调函数，并作为参数传递给回调函数。

   ```jsx
   class Parent extends React.Component {
       getChildMsg = {msg} => {
           console.log(msg)
       }
       render() {
           return (<div><Child getMsg={this.getChildMsg}></Child></div>)
       }
   }
   ```

   ```jsx
   class Child extends React.Component {
       state = {msg:'test'}
   
   	handleClick = () => {
           this.props.getMsg()
       }
       
       render() {
           return (<div className='child'><button onClick={this.handleClick}></button></div>)
       }
   }
   ```

#### 兄弟组件

将共享状态提升到最近的公共父组件中由公共父组件的state来管理这个状态。

要通讯的子组件只需要通过props接收状态或者操作状态的方法。

注意父组件要提供操作状态的方法。

#### Context

1. 调用React.createContext()创建Provider（提供数据）和Consumer（消费数据）两个组件。

   `const {Provider, Cousumer} = React.createContext()`

2. 使用Provider组件作为父结点。(只需要在最外层的组件加)

   ```jsx
   <Provider>
       <div className="App">
       	<Child1></Child1>
       </div>
   </Provider>
   ```

3. 设置value属性表示要传递的数据。

   `<Provider value="test">`

4. 调用Consumer组件接收数据。（只需要在最内层的组件加）

   ```jsx
   <Consumer>
   	{data => <span>{data}</span>}
   </Consumer>
   ```

#### props深入

##### children

组件标签的子节点。

值可以是任意值。

##### 校验

组件无法保证传入的是什么格式的数据，因而在创建组件的时候指定props的类型、格式。


校验可以在报错内提供具体的错误原因。

使用步骤：1. `npm i prop-types`

2. 导入包：`import PropTypes from 'prop-types'`

3. 添加校验规则：`组件名.propTypes = {}`

4. 具体的规则由对象指定

   ```jsx
   App.propTypes = {
       colors:PropTypes.array
   }
   ```


约束规则：

- 常见类型：array、bool、func、number、object、string
- React元素类型：element
- 必填项：isRequired
- 特定结构的对象：shape({ })

##### 默认值

```jsx
//设置默认值
App.defaultProps = {
    pageSize: 10
}
//使用组件不传入值时使用pageSize的默认值
<App />
```

```jsx
//组件内部使用默认值
...
{props.pageSize}
```

#### 生命周期

##### 创建时

constructor() -> render() -> componentDidMount()

constructor() 创建组件时执行，可用于初始化state，解决this的指向问题。

render() 每次组件渲染都会触发，用于渲染UI，但不能调用setState()。

componentDidMount()组件完成DOM渲染后，可进行网络请求以及DOM操作。

##### 更新时

render()->componentDidUpdate()

触发更新的条件：1. 组件接收到新的props 2. setState() 3. forceUpdate()

render()与创建时相同

componentDidUpdate()在组件更新（完成DOM渲染）后触发，可用于发送网络请求，进行DOM操作。注：setState()，发送请求必须放在if中，否则会导致递归更新。

```jsx
if(prevProps.count !== this.props.count) {
	this.setState({})   
    //发送请求也放这里
}
//count是props中的一个值
```

##### 卸载时

componentWillUnmount()执行清理工作（比如清理定时器）

#### render-props

复用相似的功能（state以及操作state的方法）

使用步骤：1. 创建组件，在组件中提供复用的状态以及操作状态的方法。

2. 把要复用的状态作为props.render(state)方法的参数，暴露到组件外部。
3. 使用props.render()的返回值作为要渲染的内容。

**使用children代替render属性（更好）**

在组件内部用大括号包裹需要return的箭头函数部分。

##### 代码优化

1. 给props添加props校验。
2. 在组件卸载时把相关事件绑定解除。

#### 高阶组件

目的：实现状态逻辑复用。

HOC是一个函数，接收要包装的组件，返回增强后的组件。

HOC内部创建一个类组件，在组件内部提供复用的状态逻辑代码，通过prop将复用的状态传递给被包装组件WrappedComponent。

使用步骤：1. 创建函数，名称以with开头。

2. 指定函数的参数，参数以大写字母开头（作为要渲染的组件）。

3. 在函数内部创建一个类组件，提供复用的状态逻辑代码，并返回。

4. 在该组件中渲染参数组件，同时将状态通过prop传递给参数组件。

5. 调用HOC，传入要增强的组件，通过返回值拿到增强后的组件并渲染到页面中。

##### 设置displayName

用于调试时区分不同的组件。

```jsx
Mouse.displayName = 'WithMouse${getDisplayName(WrappedComponent)}'

function getDisplayName(WrappedComponent) {
    return WrappedComponent.displayName || WrappedComponent.name || 'Component'
}
```

##### 传递props

高阶组件没有继续往下传递props，只需渲染WrappedComponent时将state和this.props一起传递给组件。

```jsx
<WrappedComponent {...this.state} {...this.props} />
```

组件模型：(state,props) => UI

### React原理

#### setState()

##### 注意点

此方法是异步更新数据的。

注意：后面的setState()不要依赖于前面的setState()。

可以多次调用setState()，但只会触发一次重新渲染。

推荐语法：使用`setState((state,props) => {})`语法，其中参数代表最新的state和props。

```jsx
this.setState((state,props) => {
    return {
        count: state.count + 1
    }
})
```

##### 第二个参数

在状态更新后立即执行某个操作

setState(updater[,callback])

```jsx
this.setState(
	(state,props) => {},
    {} => {console.log('立即执行')}
)
```

#### JSX语法原理

JSX本质上是createElement()的语法糖，会被编译为createElement()方法。

createElement()方法会转换为React元素，是一个js的对象。

#### 组件更新机制

注：setState()的作用：1. 修改state 2. 更新组件

父组件更新时会重新渲染子组件，对兄弟组件无影响。

#### 组件性能优化

##### 减轻state

state中只存储跟组件渲染相关的数据。

不用做渲染的数据以及在多个方法中用到的数据应该放在this中。

##### 避免不必要的重新渲染

原因：父组件更新时会引起子组件的更新。

解决方式：使用钩子函数shouldComponentUpdate(nextProps,nextState)，返回true表示重新渲染，false表示不重新渲染。钩子函数放在类组件内部，直接根据条件return。

注：这个钩子函数在更新阶段，在组件重新渲染前执行。（shouldComponentUpdate->render）

*两个参数表示最新的内容。*

##### 纯组件

纯组件内部的对比是浅层对比。

值：直接比较值是否相等。

引用类型：只比较对象的引用（地址）是否相同。

注意：state或者props中属性值为引用类型时应该创建新数据，不要直接修改原数据。不要用数组的push/unshift等直接修改当前数组的方法，而应该用concat/slice等这些返回新数组的方法。或者如下：

```jsx
this.setState({
    list: [...this.state.list,{新数据}]
})
```

#### 虚拟DOM和Diff算法

目的：部分更新，只更新变化的地方。

虚拟DOM：本质上是一个JS对象，用于描述UI。

执行过程：1. 初次渲染(render())，根据初始state和JSX结构创建一个虚拟DOM对象（树）。

2. 根据虚拟DOM生成真正的DOM，渲染到页面中。
3. 数据变化后重新根据新的数据创建新的虚拟DOM对象（树）。
4. 与上一次得到的虚拟DOM对象使用Diff算法对比得到需要更新的内容。
5. 将变化的内容更新到DOM中重新渲染到页面。

虚拟DOM为react多平台的应用提供了保障。

### React路由基础

#### 基本使用

1. 安装包：`yarn add react-router-dom`

2. 导入路由核心组件：Router/Route/Link

   `import { BrowserRouter as Router, Route, Link } from 'react-router-dom'`

3. 使用Router组件包裹整个应用。

4. 使用Link组件作为导航菜单（路由入口）

   `<Link to="/first">页面一</Link>`

5. 使用Route组件配置路由规则和要展示的组件（路由出口）。

   `<Route path="/first" component={First}></Route>`

**常用组件说明：**

1. Router组件：包裹整个应用，一个React应用只使用一次。

2. 两种常用的Router：HashRouter和BrowserRouter。

3. HashRouter组件：使用URL的哈希值实现。（#）

4. BrowserRouter组件：使用h5的history API实现。

5. Link组件：用于指定导航链接（a标签）。

   to属性：浏览器地址栏的pathname

6. Route组件：指定路由展示组件相关信息。

   path属性：路由规则

   component属性：展示的组件

#### 路由执行过程

1. 点击Link组件（a标签），修改浏览器地址栏中的url。
2. React路由监听到地址栏url的变化。
3. React路由内部遍历所有Route组件，使用路由规则（path）与pathname进行匹配。
4. 当路由规则（path）能够匹配地址栏中的pathname时展示Route组件的内容。

#### 编程式导航

通过js代码实现页面跳转。

`this.props.history.push('/home')`

注：1. history是React路由提供的，用于获取浏览器历史记录的相关信息。

2. push(path)：跳转到某个页面，参数path表示要跳转的路径。
3. go(n)：前进或者后退到某个页面，参数n表示前进或者后退页面的数量。（-1表示上一个页面）

#### 默认路由

表示进入页面时就会匹配的路由。

默认路由path为：/

`<Route path="/" component={Home} >`

#### 匹配模式

##### 模糊匹配模式

默认情况下React路由是模糊匹配模式。

规则：只要pathname（Link组件中to属性的值）以path（Route组件中path属性的值）开头就会匹配成功。

##### 精确匹配

给Route组件添加exact属性就可以变为精确匹配模式。

`<Route exact path="/" component=... />`

规则：只有当path和pathname完全匹配时才会展示该路由。

# React基础部分结束！









