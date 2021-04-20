## 代码分割



### React.lazy

 React.lazy 可以让你像渲染常规组件一样渲染动态引入的组件。



React.lazy 的参数接受一个函数，函数动态调用 import 。 它必须要返回一个promise， 该promise 需要 resolve 一个 default export 的component 。



应当在 `Suspense` 组件中渲染 lazy 导入的组件，让我们能够在等待异步组件的时候做优雅降级。`Suspense` 组件置于懒加载组件之上的任何位置。你甚至可以用一个 `Suspense` 组件包裹多个懒加载组件。



```
import React, { Suspense } from 'react';

const OtherComponent = React.lazy(() => import('./OtherComponent'));

function MyComponent() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <OtherComponent />
      </Suspense>
    </div>
  );
}
```





### 基于路由的代码分割







### 命名导出

React.lazy 只支持默认导出，如果想要被引入的模块用命名导出（ Named exports），可以创建一个中间组件，来重新导出为默认模块。这能保证 tree shaking 不会出错，且不必要引入不需要的组件。





## Context



Context 主要应用场景在于*很多*不同层级的组件需要访问同样一些的数据。Context 提供了一种方法，让我们可以不用给每一层都提供一个 props， 就可以在组件树之间进行数据传递。



请谨慎使用，因为这会使得组件的复用性变差。组件中要书写 context 的引入，这对要引入这个组件的夫组件来说它们可能并不存在context，导致不能引入这个组件。



### 基本使用

1. 创建 context
2. 使用一个 provider 给以下的组件树
3. 子组件读取当前的 them context 

```
// Context 可以让我们无须明确地传遍每一个组件，就能将值深入传递进组件树。
// 为当前的 theme 创建一个 context（“light”为默认值）。
const ThemeContext = React.createContext('light');
class App extends React.Component {
  render() {
    // 使用一个 Provider 来将当前的 theme 传递给以下的组件树。
    // 无论多深，任何组件都能读取这个值。
    // 在这个例子中，我们将 “dark” 作为当前的值传递下去。
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// 中间的组件再也不必指明往下传递 theme 了。
function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // 指定 contextType 读取当前的 theme context。
  // React 会往上找到最近的 theme Provider，然后使用它的值。
  // 在这个例子中，当前的 theme 值为 “dark”。
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}

```



### 其它场景替代方案



#### 组件组合

组件组合也可以避免多层级传递数据





#### 将组件传递下去

这种方式可以让传递的时候中间的组件不需要知道上面的组件要传递给子组件什么内容。

但是这样子的方式只能够减少要传递的 props ，没有改变层层传递的本质。

```
function Page(props) {
  const user = props.user;
  const userLink = (
    <Link href={user.permalink}>
      <Avatar user={user} size={props.avatarSize} />
    </Link>
  );
  return <PageLayout userLink={userLink} />;
}

// 现在，我们有这样的组件：
<Page user={user} avatarSize={avatarSize} />
// ... 渲染出 ...
<PageLayout userLink={...} />
// ... 渲染出 ...
<NavigationBar userLink={...} />
// ... 渲染出 ...
{props.userLink}
```



### API



**React.createContext**



**Context.Provider**



**Class.contextType**



**Context.Consumer**



**Context.displayName**





### 在嵌套组件中更新context

你可以通过 context 传递一个函数，使得 consumers 组件更新 context



```
export const ThemeContext = React.createContext({
  theme: themes.dark,
  toggleTheme: () => {},
});
```



### 消费多个 Consumer

> 如果两个或者更多的 context 值经常被一起使用，那你可能要考虑一下另外创建你自己的渲染组件，以提供这些值



```
// Theme context，默认的 theme 是 “light” 值
const ThemeContext = React.createContext('light');

// 用户登录 context
const UserContext = React.createContext({
  name: 'Guest',
});

class App extends React.Component {
  render() {
    const {signedInUser, theme} = this.props;

    // 提供初始 context 值的 App 组件
    return (
      <ThemeContext.Provider value={theme}>        
      <UserContext.Provider value={signedInUser}>          
      	<Layout />
      </UserContext.Provider>      
      </ThemeContext.Provider>    
    );
  }
}
```







### 注意事项

context 会根据参考标识决定何时进行渲染。当 provider 进行重渲染的时候，以下的代码会重渲染下面的 consumers 组件， 因为value 总是会被赋值成新的对象。

```
class App extends React.Component {
  render() {
    return (
      <MyContext.Provider value={{something: 'something'}}>
        <Toolbar />
      </MyContext.Provider>
    );
  }
}
```



为了避免这种情况， 可以将 value 状态提升到父节点的 state 里面

```
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: {something: 'something'},
    };
  }

  render() {
    return (
      <MyContext.Provider value={this.state.value}>
        <Toolbar />
      </MyContext.Provider>
    );
  }
}
```









## 错误边界



组件内部的 JS 错误有可能会导致 React 的内部状态被破坏，并且在下一次的渲染中 产生无法追踪的错误。

部分UI 的错误不应该导致整个应用崩溃， React 引入了一个新的概念 —— 错误边界。



错误边界是一种 react 组件，他的作用是捕获并打印发生在其子组件树任何位置的 JS 错误， 并且会渲染出备用的UI，而不是那些崩溃的子组件。



> 注意： 错误边界无法捕获以下场景的错误
>
> 1. 事件处理
> 2. 异步代码
> 3. 服务端渲染
> 4. 自身抛出的错误







### 基本使用

如果一个 class 组件中定义了 [`static getDerivedStateFromError()`](https://zh-hans.reactjs.org/docs/react-component.html#static-getderivedstatefromerror) 或 [`componentDidCatch()`](https://zh-hans.reactjs.org/docs/react-component.html#componentdidcatch) 这两个生命周期方法中的任意一个（或两个）时，那么它就变成一个错误边界。

当抛出错误后，请使用 `static getDerivedStateFromError()` 渲染备用 UI ，使用 `componentDidCatch()` 打印错误信息。

```
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    // 更新 state 使下一次渲染能够显示降级后的 UI
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    // 你同样可以将错误日志上报给服务器
    logErrorToMyService(error, errorInfo);
  }

  render() {
    if (this.state.hasError) {
      // 你可以自定义降级后的 UI 并渲染
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children; 
  }
}


<ErrorBoundary>
  <MyWidget />
</ErrorBoundary>
```



> 错误边界只可以捕获子组件的错误，但是不可以捕获自身的错误
>
> 如果一个错误边界无法渲染错误信息，则错误会冒泡至最近的上层错误边界



### 组件栈跟踪





### 关于 try / catch



try / catch 是很好的错误处理方式，但是只能用于命令式的代码。

React 是声明式的，并且具体指出 `什么` 需要被渲染





错误边界**无法**捕获事件处理器内部的错误。事件处理器不会再渲染期间触发，因此事件处理的异常不会影响渲染。

在事件处理中需要捕获错误的话，可以直接使用 try / catch





## Refs 转发



**Ref 转发是一个可选特性，其允许某些组件接收 `ref`，并将其向下传递（换句话说，“转发”它）给子组件。**

### 

### 使用 refs 转发的思考

react 是声明式的， 组件对外隐藏其实现细节。这样子可以防止组件过度依赖于其它组件的DOM 结构。



使用 refs 转发适用于比较底层的组件，这些组件倾向于在整个应用中以一种类似常规 DOM `button` 和 `input` 的方式被使用，并且访问其 DOM 节点对管理焦点。



使用 forwardRef 时应当将它视为一个破坏性的更改，因为它可能会导致明显不同的行为（例如refs 被分配给了谁，导出什么类型）。所以当 React.forwardRef 存在时有条件的使用它时不推荐的。



### 转发 refs 到 DOM 组件

```
//FancyButton 使用 React.forwardRef 来获取传递给它的 ref，然后转发到它渲染的 DOM button
const FancyButton = React.forwardRef((props, ref) => (
  <button ref={ref} className="FancyButton">
    {props.children}
  </button>
));

// 你可以直接获取 DOM button 的 ref：
const ref = React.createRef();
<FancyButton ref={ref}>Click me!</FancyButton>;
```



### 

### 在高阶组件中转发 refs



### 在 devtools 中显示自定义名称





## Fragments



Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点



### 用法



```
class Columns extends React.Component {
  render() {
    return (
      <React.Fragment>
        <td>Hello</td>
        <td>World</td>
      </React.Fragment>
    );
  }
}
```



#### 短语法

```
render() {
    return (
      <>
        <td>Hello</td>
        <td>World</td>
      </>
    );
}
```





#### 带 key 的 Fragments

使用显式 `<React.Fragment>` 语法声明的片段可能具有 key。一个使用场景是将一个集合映射到一个 Fragments 数组



`key` 也是唯一可以传递给 `Fragment` 的属性

```
return (
    <dl>
      {props.items.map(item => (
        // 没有`key`，React 会发出一个关键警告
        <React.Fragment key={item.id}>
          <dt>{item.term}</dt>
          <dd>{item.description}</dd>
        </React.Fragment>
      ))}
    </dl>
  );
```





## 高阶组件（HOC）



**高阶组件是参数为组件，返回值为新组件的函数。**









## 深入 JSX



JSX 仅仅只是 `React.createElement(component, props, ...children)` 函数的语法糖



JSX 的编译结果会像如下的过程

```
<MyButton color="blue" shadowSize={2}>
  Click Me
</MyButton>

React.createElement(
  MyButton,
  {color: 'blue', shadowSize: 2},
  'Click Me'
)
```



### 指定 React 元素类型



#### 作用域



**大写字母开头的 JSX 标签意味着它们是 React 组件。**这些标签会被编译为对命名变量的直接引用，所以，当你使用 JSX `<Foo />` 表达式时，`Foo` 必须包含在作用域内



由于 JSX 会编译为 `React.createElement` 调用形式，所以 `React` 库也必须包含在 JSX 代码作用域内。

如果你不使用 JavaScript 打包工具而是直接通过 `<script>` 标签加载 React，则必须将 `React` 挂载到全局变量中



#### 在 JSX 类型中使用 点语法

在一个模块中导出许多的 React 组件的时候，可以使用点语法去使用模块中的组件。



#### 用户定义组件必须使用大写字母开头



### JSX 中的 props



#### js 表达式作为 props 



#### 字符串字面量



#### props 默认值为 true





#### 属性展开





### JSX 中的子元素



#### JSX 子元素

JSX 的子元素允许有多个 JSX 元素组成，这对嵌套组件非常有用。



#### JS 表达式作为子元素

JS 表达式可以被包裹在 `{}` 里面作为子元素



这对于展示列表和代替模版字符串很有用



#### 函数作为子元素



你可以将任何东西作为子元素传递给自定义组件，只要确保在该组件渲染之前能够被转换成 React 理解的对象。



#### Boolean、Null 、Undefined会被忽略





## Portals

> 类似于 vue3 里面的 teleport



Portals 提供了一种将子节点渲染到夫组件之外的Dom 节点的方案。



**createPortal** 第一个参数（`child`）是任何[可渲染的 React 子元素](https://zh-hans.reactjs.org/docs/react-component.html#render)，例如一个元素，字符串或 fragment。第二个参数（`container`）是一个 DOM 元素

```
ReactDOM.createPortal(child, container)
```



#### 通过 Portal 进行事件冒泡

尽管 portal 可以被放置在 DOM 树中的任何地方，但在任何其他方面，其行为和普通的 React 子节点行为一致。由于 portal 仍存在于 *React 树*， 且与 *DOM 树* 中的位置无关，那么无论其子节点是否是 portal，像 context 这样的功能特性都是不变的。

这包含事件冒泡。一个从 portal 内部触发的事件会一直冒泡至包含 *React 树*的祖先，即便这些元素并不是 *DOM 树* 中的祖先。假设存在如下 HTML 结构：





## [Profiler](https://zh-hans.reactjs.org/docs/profiler.html)



`Profiler` 测量渲染一个 React 应用多久渲染一次以及渲染一次的“代价”。 它的目的是识别出应用中渲染较慢的部分



> 在生产环境里面会被禁用
>
> 
>
> 尽管 `Profiler` 是一个轻量级组件，我们依然应该在需要时才去使用它。对一个应用来说，每添加一些都会给 CPU 和内存带来一些负担。







## 协调

































