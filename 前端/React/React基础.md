### React特点

1. 声明式
2. 高效。虚拟dom实现dom渲染，最大限度减少dom操作
3. 灵活。 可以跟其它库灵活运用
4. JSX。在js里面书写html
5. 组件化、模块化。React最大的优点，能够很好的进行代码的复用
6. 单向数据流。
7. 函数式编程
8. 视图层框架



### JSX

React 认为渲染逻辑本质上与其他 UI 逻辑内在耦合，React 并没有采用将*标记与逻辑进行分离到不同文件*这种人为地分离方式，而是通过将二者共同存放在称之为“组件”的松散耦合单元之中，来实现[*关注点分离*](https://en.wikipedia.org/wiki/Separation_of_concerns)。

#### 优缺点：

**优点**

1. 执行更快。 编译为js代码时进行了优化
2. 类型更安全。 如果有错误，编译的时候就会出错，及时发现，
3. 书写模板简单快速

**缺点**

1. 必须保证有且仅有一个根元素
2. 标签名小写，大写的默认为组件



#### JSX表达式

 JSX 语法中，你可以在大括号内放置任何有效的 [JavaScript 表达式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators#Expressions)

**JSX本身也是一个表达式**。 在编译之后，JSX 表达式会被转为普通 JavaScript 函数调用，并且对其取值后得到 JavaScript 对象。



#### JSX防止注入攻击

React DOM 在渲染所有输入内容之前，默认会进行[转义](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html)。所有的内容在渲染之前都被转换成了字符串，能够有效的防止XSS攻击。



#### 注意事项

1. 混入js表达式的时候用 {} ， 指定内联样式用 {{}}
2. 必须保证有且仅有一个根元素。标签名小写，大写的默认为组件
3. 指定样式的类名使用 className 而不是 class
4. 使用 Fragment 可以让 jsx 外层div不显示出来 
5. 书写注释使用大括号，页面不会显示。   { // }



### 元素渲染

元素是React应用的最小砖块，组件都由元素构成

#### 元素渲染为dom

#### 更新已渲染的元素

React 元素是[不可变对象](https://en.wikipedia.org/wiki/Immutable_object)。一旦创建就无法修改，更新UI的唯一方式是创建一个全新的元素， 并且传入ReactDOM.render()

> 在实践中，大多数 React 应用只会调用一次 [`ReactDOM.render()`](https://zh-hans.reactjs.org/docs/react-dom.html#render)。

##### react只会更新需要更新的部分



### 组件&Props

react中的组件类似于函数，接受任意入参（props），返回描述页面展示类内容的react元素



#### 组件 & 模块

##### 模块

含义： 向外提供特定功能的js 程序

作用： 简化js的编写，复用部分的功能， 提高js运行效率

##### 组件

含义： 能够实现局部功能效果的代码和资源的集合

作用： 复用编码，简化编码， 提高运行时效率



#### 函数组件 & class组件

```
// 函数组件
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

```
// class组件
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}

```

#### Props的只读性

组件无论是使用[函数声明还是通过 class 声明](https://zh-hans.reactjs.org/docs/components-and-props.html#function-and-class-components)，都决不能修改自身的 props



### State & 生命周期



#### 生命周期

 

```
// 复杂的元素只能用class组件
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    // 使用setState改变元素状态
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

#### State的使用

1. 不要直接修改state ， 要使用setState方法
2. state的更新可能是异步的。不要依赖他的值来更新下一个状态。
   1. 要解决这个问题，可以让 `setState()` 接收一个函数而不是一个对象
3. state 的更新会被合并
   1. 调用 `setState()` 的时候，React 会把你提供的对象合并到当前的 state。



#### 数据是向下流动的

父级无法访问的子组件的数据，但是组件可以选择将数据传入子组件





### 事件处理

react的事件处理和html很相似， 语法上有所不同

1. 使用 JSX必须传入一个函数，而不是字符串

#### 阻止默认行为

不可以通过`return false` 阻止默认事件， 必须显式的 `e.preventDefault` 

react中的e 是一个合成事件， 是react 根据 w3c 规范定义的，没有兼容问题



#### JSX 回调函数中的 this 

> JavaScript 中，class 的方法默认不会[绑定](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) `this`。



1. render方法中的this为组件的实例对象

2. 组件自定义的方法，this为undefined。 解决办法

   1. 使用bind绑定this

      ```
        constructor(props) {
          super(props);
          this.state = {isToggleOn: true};
      
          // 为了在回调中使用 `this`，这个绑定是必不可少的
          this.handleClick = this.handleClick.bind(this);
        }
      
        handleClick() {
          this.setState(state => ({
            isToggleOn: !state.isToggleOn
          }));
        }
      
        render() {
          return (
            <button onClick={this.handleClick}>
              {this.state.isToggleOn ? 'ON' : 'OFF'}
            </button>
          );
        }
      ```

      

   2. 箭头函数

      ```
      class LoggingButton extends React.Component {
        handleClick() {
          console.log('this is:', this);
        }
      
        render() {
          // 此语法确保 `handleClick` 内的 `this` 已被绑定。
          return (
            <button onClick={() => this.handleClick()}>
              Click me
            </button>
          );
        }
      }
      ```

      

   3. 向事件处理程序中传递参数

      ```
      <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
      <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
      ```



### 条件渲染

#### 与运算符 &&

需要注意的是， 左边的表达式必须是一个boolean值，因为react不会自动进行隐式转换。 

```
function Mailbox(props) {
  const unreadMessages = props.unreadMessages;
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 &&
        <h2>
          You have {unreadMessages.length} unread messages.
        </h2>
      }
    </div>
  );
}

// 这个会返回 <div>0</div>
render() {
  const count = 0;
  return (
    <div>
      { count && <h1>Messages: {count}</h1>}
    </div>
  );
}
```



#### 三目运算符

```
render() {
  const isLoggedIn = this.state.isLoggedIn;
  return (
    <div>
      {isLoggedIn
        ? <LogoutButton onClick={this.handleLogoutClick} />
        : <LoginButton onClick={this.handleLoginClick} />
      }
    </div>
  );
}
```

#### 阻止组件渲染

在极少数情况下，你可能希望能隐藏组件，即使它已经被其他组件渲染。若要完成此操作，你可以让 `render` 方法直接返回 `null`，而不进行任何渲染。





### 列表渲染



```
function NumberList(props) {
  const numbers = props.numbers;
  return (
    <ul>
      {numbers.map((number) =>
        <ListItem key={number.toString()}
                  value={number} />
      )}
    </ul>
  );
}
```



#### 用 key 提取组件

元素的 key 只有放在就近的数组上下文中才有意义。

比方说，如果你[提取](https://zh-hans.reactjs.org/docs/components-and-props.html#extracting-components)出一个 `ListItem` 组件，你应该把 key 保留在数组中的这个 `<ListItem />` 元素上，而不是放在 `ListItem` 组件中的 `<li>` 元素上。





key 只是在兄弟节点直接拿必须唯一， 但是全局则不必要

key 会传递信息给react ， 但是不会传递给组件。 如果组件中需要使用 key 属性的值， 那么必须使用其它属性名传递这个值。









### [状态提升](https://zh-hans.reactjs.org/docs/lifting-state-up.html)

多个组件需要反映相同的变化数据，这时我们建议将共享状态提升到最近的共同父组件中去。

```
class Calculator extends React.Component {
  constructor(props) {
    super(props);
    this.handleCelsiusChange = this.handleCelsiusChange.bind(this);
    this.handleFahrenheitChange = this.handleFahrenheitChange.bind(this);
    this.state = {temperature: '', scale: 'c'};
  }

  handleCelsiusChange(temperature) {
    this.setState({scale: 'c', temperature});
  }

  handleFahrenheitChange(temperature) {
    this.setState({scale: 'f', temperature});
  }

  render() {
    const scale = this.state.scale;
    const temperature = this.state.temperature;
    const celsius = scale === 'f' ? tryConvert(temperature, toCelsius) : temperature;
    const fahrenheit = scale === 'c' ? tryConvert(temperature, toFahrenheit) : temperature;

    return (
      <div>
        <TemperatureInput
          scale="c"
          temperature={celsius}
          onTemperatureChange={this.handleCelsiusChange} />
        <TemperatureInput
          scale="f"
          temperature={fahrenheit}
          onTemperatureChange={this.handleFahrenheitChange} />
        <BoilingVerdict
          celsius={parseFloat(celsius)} />
      </div>
    );
  }
}
```



### 组合 & 继承

> 推荐使用组合而非继承来实现组件间的代码重用



#### 包含关系

当无法提前知晓子组件的具体内容时， 使用一个特殊的 `children` prop 来将他们的子组件传递到渲染结果，这使得别的组件可以通过 JSX 嵌套，将任意组件作为子组件传递给它们

```
function FancyBorder(props) {
  return (
    <div className={'FancyBorder FancyBorder-' + props.color}>
      {props.children}
    </div>
  );
}

function WelcomeDialog() {
  return (
    <FancyBorder color="blue">
      <h1 className="Dialog-title">
        Welcome
      </h1>
      <p className="Dialog-message">
        Thank you for visiting our spacecraft!
      </p>
    </FancyBorder>
  );
}
```





有时我们需要将所需内容传入 props ，然后指定 元素的位置。

```
function SplitPane(props) {
  return (
    <div className="SplitPane">
      <div className="SplitPane-left">
        {props.left}
      </div>
      <div className="SplitPane-right">
        {props.right}
      </div>
    </div>
  );
}

function App() {
  return (
    <SplitPane
      left={
        <Contacts />
      }
      right={
        <Chat />
      } />
  );
}
```



有点类似于Vue 里面插槽的概念，但在 **React 中没有“插槽**” 这一概念的限制，你可以将任何东西作为 props 进行传递



























// react中的全局状态管理

// 有没有数据驱动和双向绑定

