# react高级



## [React-Fiber](https://juejin.cn/post/6943896410987659277#heading-12)



### 概念明确

#### **浏览器刷新率（fps：帧/秒）**

[帧](https://zh.m.wikipedia.org/zh-hans/帧率)：指显示器显示的每一张画面。

 fps 全称为 `Frames Per Second`，即 `每一秒的更新帧数`。fps 越大，每秒的更新画面就越多，连续画面的显示就越流畅。浏览器的 fps 一般等于屏幕的刷新率，不会超过屏幕的刷新率。目前浏览器大多是 60Hz（60帧/s），**即 1s 刷新 60 次**，1000ms / 60hz = 16.6 ，大概每过16.6ms 浏览器会渲染一帧画面。

​

**浏览器一帧会经过下面这几个过程**：

1. Tasks: 宏任务，如点击事件、键盘事件、滚动事件、 setTimeout、setInterval等
2. Microtasks: 微任务，如 `Promise`
3. RAF: `requestAnimationFrame`
4. Layout: CSS 计算，页面布局
5. Paint: 页面绘制
6. RIC: `requestIdleCallback`



**虚拟Dom的遍历**

React Fiber 出现之前，React 通过原生执行栈**递归遍历** VDOM。而递归的过程是无法被中断的。浏览器引擎会从执行栈的顶端开始执行，执行完毕就弹出当前执行上下文，开始执行下一个函数，直到执行栈被清空才会停止。然后将执行权交还给浏览器。



React Fiber 中用**链表遍历**的方式替代了 React 16 之前的栈递归方案。

链表相比顺序结构数据格式的好处就是：

1. 操作更高效，比如顺序调整、删除，只需要改变节点的指针指向就好了。
2. 不仅可以根据当前节点找到下一个节点，在多向链表中，还可以找到他的父节点或者兄弟节点。

但链表也不是完美的，缺点就是：

1. 比顺序结构数据更占用空间，因为每个节点对象还保存有指向下一个对象的指针。
2. 不能自由读取，必须找到他的上一个节点。



### 为什么需要 React-Fiber

React V15 在渲染时，会递归比对 VirtualDOM 树，找出需要变动的节点，然后同步更新它们， 一气呵成。这个过程期间， React 会占据浏览器资源，这会导致用户触发的事件得不到响应，并且会导致掉帧，**导致用户感觉到卡顿**。



React-Fiber 让这个执行过程变成可被中断。“适时”地让出 CPU 执行权，除了可以让浏览器及时地响应用户的交互，还有其他好处:

- 分批延时对DOM进行操作，避免一次性操作大量 DOM 节点，可以得到更好的用户体验；
- 给浏览器一点喘息的机会，它会对代码进行编译优化（JIT）及进行热代码优化，或者对 reflow 进行修正。



### React-Fiber 原理

首先先理解两个概念：

- current tree

    current tree 表示的是当前用户界面上所渲染的内容。

- working progress tree

    当节点更新的时候，fiber 会创建一棵 working progress tree ，react 的操作都会反应到这棵树上，并且在下次渲染发生的时候使用这个树去替换current tree 渲染到用户界面上



每个 *fiber* 都是一个链表的节点，它们通过子、兄弟节点和返回引用连接起来。第一次渲染的时候，react 会浏览每一个react 元素，然后组合成一棵fibers 树。

**虽然我们称之为树，但 React Fiber 创建了一个节点的链表，其中每个节点都是一个 fiber。**



Fiber 树的遍历采用的是深度优先遍历的方式

1. 开始：Fiber 从最上面的 React 元素开始遍历，并为其创建一个 fiber 节点。
2. 子节点：然后，它转到子元素，为这个元素创建一个 fiber 节点。这样继续下去，直到到达叶子元素。
3. 兄弟节点： 现在，它检查是否有兄弟节点元素。如果有，它就遍历兄弟节点元素，然后再到兄弟姐妹的叶子元素。
4. 返回：如果没有兄弟节点，那么它就返回到父节点。



### React-Fiber 工作流程

>React 的更新分为两个阶段
>
>- `Reconciliation`：协调阶段，新旧两棵树进行 diff 比较，找到所有需要更新的节点；
>- `Commit`：提交阶段，将上一步找到的需要更新的 dom 节点更新到页面上，这个阶段是同步执行不可被打断的。

#### 初始化渲染

在第一次渲染之前并不存在 fiber 树。react fiber 遍历每一个组件的渲染函数的输出，并为每一个 react 元素在树上创建一个 fiber 节点。react 元素可以是类组件，也可以是宿主组件；对于类组件，react 会创建一个它的实例；对于宿主组件，react 会从react 元素中获得数据和props。



每一个fibers 树都会有一个根 fiber（如果在dom 中导入多个 react 应用，可以有多个）。App 组件渲染在 id 为 root 的 div 中，创建一个 fiber ，然后直接往下深度遍历，形成的结构如下图：

![React Fiber relationship](fiber初始化.png)



#### 协调阶段

**工作单元**

React fiber 将更新划分为工作单元，可以为每一个工作单元分配优先级，并对其进行**暂停、重用和终止**的操作。



协调阶段（Reconciler）递归遍历 VDOM 这个大任务分成若干小任务，每个任务只负责一个节点的处理。再通过时间分片，在一个时间片中执行一个或者多个任务。这里提一下，所有的小任务并不是一次性被切分完成，而是处理当前任务的时候生成下一个任务，如果没有下一个任务生成了，就代表本次渲染的 Diff 操作完成。



#### 挂起

首先 React 向浏览器请求调度，浏览器在一帧中如果还有空闲时间，会去判断是否存在待执行任务，不存在就直接将控制权交给浏览器，如果存在就会执行对应的任务，当第一个小任务完成后，先判断这一帧是否还有空闲时间，没有就挂起下一个任务的执行，记住当前挂起的节点，让出控制权给浏览器执行更高优先级的任务。

#### 恢复

在浏览器渲染完一帧后，判断当前帧是否有剩余时间，如果有就恢复执行之前挂起的任务。如果没有任务需要处理，代表协调阶段完成，可以开始进入渲染阶段。这样完美的解决了调和过程一直占用主线程的问题。

#### 终止

其实并不是每次更新都会走到提交阶段。当在调和过程中触发了新的更新，在执行下一个任务的时候，判断是否有优先级更高的执行任务，如果有就终止原来将要执行的任务，开始新的 workInProgressFiber 树构建过程，开始新的更新流程。





![undefined](fiber工作单元.png)

> 在 React 15 中，堆栈协调器是同步的。所以，一个更新会递归地遍历整个树，并制作一个树的副本。假设在这之间，如果有其他的更新比它的优先级更高，那么就没有机会中止或暂停第一个更新并执行第二个更新。

**遍历**（待补充）

这个阶段， current tree 已经存在了，在发生更新的时候，都会建立一个 working progress 树.

从根 fiber 开始，深度遍历所有节点，如果发现与 current tree 不一样，它不会为每一个 react 元素创建一个新的 fiber，而是会为该 react 元素使用预先存在的 fiber，并且在更新阶段合并来自更新元素的数据和 props。



**收集副作用**

收集`effect list`的具体步骤为：

1. 如果当前节点需要更新，则打`tag`更新当前节点状态（props, state, context等）
2. 为每个子节点创建fiber。如果没有产生`child fiber`，则结束该节点，把`effect list`归并到`return`，把此节点的`sibling`节点作为下一个遍历节点；否则把`child`节点作为下一个遍历节点
3. 如果有剩余时间，则开始下一个节点，否则等下一次主线程空闲再开始下一个节点
4. 如果没有下一个节点了，进入`pendingCommit`状态，此时`effect list`收集完毕，结束。





#### 提交阶段

完成的工作将被用来在用户界面上渲染它。由于这一阶段的结果对用户来说是可见的，所以不能被分成部分渲染。这个阶段是一个同步的阶段。在这里，*workInProgress* 树成为 *current* 树，因为它被用来渲染 UI。







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



```
const Home = lazy(() => import('./routes/Home'));
const About = lazy(() => import('./routes/About'));

const App = () => (
  <Router>
    <Suspense fallback={<div>Loading...</div>}>
      <Switch>
        <Route exact path="/" component={Home}/>
        <Route path="/about" component={About}/>
      </Switch>
    </Suspense>
  </Router>
);
```



### 命名导出

React.lazy 只支持默认导出，如果想要被引入的模块用命名导出（ Named exports），可以创建一个中间组件，来重新导出为默认模块。这能保证 tree shaking 不会出错，且不必要引入不需要的组件。









## Context



Context 主要应用场景在于*很多*不同层级的组件需要访问同样一些的数据。Context 提供了一种方法，让我们可以不用给每一层都提供一个 props， 就可以在组件树之间进行数据传递。



请谨慎使用，因为这会使得组件的复用性变差。组件中要书写 context 的引入，这对要引入这个组件的父组件来说它们可能并不存在context，导致不能引入这个组件。



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


### JSX.Element   vs  ReactNode

**JSX.Element**
```
const element: JSX.Element = <div>Hello, world!</div>;
```

定义：在 TypeScript 中，`JSX.Element` 是一个类型，它表示一个 <u>JSX 表达式被编译后的 JavaScript 对象</u>。这个对象代表了 React 组件或 HTML 元素的结构。

### ReactNode


```
function MyComponent ({ children }: { children: ReactNode }) {
  return <div>{children}</div>;
}
```


`ReactNode` 是一个通用的类型，它可以表示任何可以被渲染到 DOM 中的 React 元素，这包括组件、字符串、数字、数组等，是多种 react 元素的联合

```ts
type ReactText = string | number;
type ReactChild = ReactElement | ReactText;

interface ReactNodeArray extends Array<ReactNode> {}
type ReactFragment = {} | ReactNodeArray;

type ReactNode = ReactChild | ReactFragment | ReactPortal | boolean | null | undefined;
```




### 什么是 JSX

JSX 仅仅只是 `React.createElement(component, props, …children)` 函数的语法糖



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

Portals 提供了一种将子节点渲染到父组件之外的Dom 节点的方案。



**createPortal** 第一个参数（`child`）是任何[可渲染的 React 子元素](https://zh-hans.reactjs.org/docs/react-component.html#render)，例如一个元素，字符串或 fragment。第二个参数（`container`）是一个 DOM 元素

```
ReactDOM.createPortal(child, container)
```



### 通过 Portal 进行事件冒泡

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









## Refs and the DOM



在典型的 react 数据流中， props 是父组件和子组件交互的唯一方式；如果想要修改一个子组件，那么我们需要使用新的props 去重新渲染它。



refs 提供了一种新的方式，**让我们可以强制的去修改子组件**。



### 使用场景



下面是几个适合使用 refs 的情况：

- 管理焦点，文本选择或媒体播放。
- 触发强制动画。
- 集成第三方 DOM 库。





我们应当避免使用 refs 去做 props 就可以做到的事情。 例如状态提升让子组件反应相同的变化数据。





### 使用方法

ref 的值根据节点的类型而有所不同：

- 当 `ref` 属性用于 HTML 元素时，构造函数中使用 `React.createRef()` 创建的 `ref` 接收**底层 DOM 元素**作为其 `current` 属性。
- 当 `ref` 属性用于自定义 class 组件时，`ref` 对象接收组件的**挂载实例**作为其 `current` 属性。
- **你不能在函数组件上使用 `ref` 属性**，因为他们没有实例。



```
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}


const node = this.myRef.current;
```



### 将 DOM refs 暴露给父组件



有时候需要父组件引用子组件中的 DOM 节点，（不建议这样，会打破组件封装）。

- 可以在父组件向子组件添加 ref ， 但是这只能获取组件实例获取不到组件的 dom
- **Ref 转发**使组件可以像暴露自己的 ref 一样暴露子组件的 ref





### 回调 Refs



回调 refs 中，你会传递一个函数。这个函数中接受 React 组件实例或 HTML DOM 元素作为参数，以使它们能在其他地方被存储和访问。



React 将在组件挂载时，会调用 `ref` 回调函数并传入 DOM 元素，当卸载时调用它并传入 `null`。在 `componentDidMount` 或 `componentDidUpdate` 触发前，React 会保证 refs 一定是最新的。



## Rander Props



**render prop 是一个用于告知组件需要渲染什么内容的函数 prop。**





## PropTypes 类型检查



```
// 普通用法
MyComponent.PropTypes = {
	title: PropTypes.string.required,
	id: PropTypes.oneOfType(PropTypes.string, PropTypes.number)
}

// 只允许一个 prop
MyComponent.propTypes = {
  children: PropTypes.element.isRequired
};

// 指定 props 的默认值：
Greeting.defaultProps = {
  name: 'Stranger'
};
```







## 静态类型检查



我们建议在大型代码库中使用 Flow 或 TypeScript 来代替 `PropTypes`进行静态类型检查











# Hook



hook 可以让你在不写 class 组件的时候就可以使用state 和其他 react 特性



## hook 基础



### **hook 的特点**

- 完全可选
-  100 % 向后兼容
- 现在可用
- 替换 state 而不是合并



### 为什么需要 hook



**状态的复用**

react 没有提供将可复用性行为 “附加” 到组件的途径（例如连接到 store）。 **hook 使你能够在无需修改组件结构的情况下复用状态逻辑**



**复杂组件的简化**

hook 可以将组件中相互关联的部分拆分成更小的函数，不用强制按照生命周期划分，你还可以使用 reducer 去管理组件内部的状态。



**class 难以理解**

Hook 使你在非 class 的情况下可以使用更多的 React 特性



### 使用 hooks 的注意事项

- 只能在函数内部的最外层调用 Hook，不要在循环、条件判断或者子函数中调用
    - 遵守这条规则，你就能确保 Hook 在每一次渲染中都按照同样的顺序被调用。（React 靠的是 Hook 调用的顺序识别 state 对应哪一个 useState ）
- 只能在 React 的函数组件中调用 Hook，不要在其他 JavaScript 函数中调用





## state hook

在没有 hook 的时候，我们如果要给一个组件设定一个状态，那么我们必须把它写成 class 组件，因为函数组件没有状态，只能够编写静态内容。

有hook 之后，我们就可以向现有的函数组件里面添加状态了。



```
import React, { useState } from 'react';

function Example() {
  // 声明一个叫 "count" 的 state 变量
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```



### **useState**



useState 定义一个变量，他是一个新的方法，作用和 class 组件里面的 `this.state ` 完全相同。一般的变量会在函数退出的时候销毁变量，useState 定义的变量会被 react 保留。



**参数**



**返回值**

当前 state 以及更新 state 的函数



**读取**









## Effect Hook

数据获取，设置订阅以及手动更改 React 组件中的 DOM 都属于副作用。



使用 Hook 其中一个[目的](https://zh-hans.reactjs.org/docs/hooks-intro.html#complex-components-become-hard-to-understand)就是要解决 class 中生命周期函数经常包含不相关的逻辑，但又把相关逻辑分离到了几个不同方法中的问题



可以把 `useEffect` Hook 看做 `componentDidMount`，`componentDidUpdate` 和 `componentWillUnmount` 这三个函数的组合。



### 无需清除的effect



react 更新 dom 之后额外运行一些代码（例如 发送网络请求， 手动变更 dom， 记录日志），这些属于常见的不需要清除的effect





#### **class中的副作用**

class 组件中，我们不希望在 `render` 函数中有任何的副作用，我们希望在 react 更新 dom 之后才执行我们的操作。

这就是为什么我们把副作用放到了 `componentDidMount` 和 `componentDidUpdate` 函数中





### useEffect

由于添加和删除订阅的代码的紧密性，所以 `useEffect` 的设计是在同一个地方执行。如果你的 effect 返回一个函数，React 将会在执行清除操作时调用它：



```
function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);

		return function cleanup() {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```



**为什么返回函数**

这是 effect 可选的清除机制，每个 effect 可以返回一个清除函数，可以将副作用的添加和清除放在一起。



**什么时候清除**

react 会在组件卸载的时候进行清除。



## effect 使用提示



### 使用多个 effect 实现关注点分离







### 为什么每次更新都要运行 effect





### 通过跳过 effect 进行性能优化



每次渲染后都执行清理或者执行 effect 可能会导致性能问题。



可以通知 React **跳过**对 effect 的调用，只要传递数组作为 `useEffect` 的第二个可选参数即可



```
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // 仅在 count 更改时更新
```





## 自定义 Hook



自定义 hook 可以让我们把组件逻辑提取到可重用的函数中





### 提取自定义 hook



hook 是一个函数，名称总是以 `use` 开头，函数内部可以调用其他的hook





```
// 定义自定义的 hook
function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }

    ChatAPI.subscribeToFriendStatus(friendID, handleStatusChange);
    return () => {
      ChatAPI.unsubscribeFromFriendStatus(friendID, handleStatusChange);
    };
  });

  return isOnline;
}

// 使用自定义 hook
function FriendListItem(props) {
  const isOnline = useFriendStatus(props.friend.id);

  return (
    <li style={{ color: isOnline ? 'green' : 'black' }}>
      {props.friend.name}
    </li>
  );
}
```





### 多个 hook 之间信息传递













## hook API

> [React Hooks 详解 【近 1W 字】+ 项目实战 - 掘金 (juejin.cn)](https://juejin.cn/post/6844903985338400782)

### [基础 Hook](https://zh-hans.reactjs.org/docs/hooks-reference.html#basic-hooks)



#### [`useState`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usestate)

返回一个 state，以及更新 state 的函数。



若新的 state 需要通过使用先前的 state 计算得出，那么可以将函数传递给 `setState`。该函数将接收先前的 state，并返回一个更新后的值



#### [`useEffect`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useeffect)

执行带有副作用的函数



#### [`useContext`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecontext)

接收一个 context 对象（`React.createContext` 的返回值）并返回该 context 的当前值。

调用了 `useContext` 的组件总会在 context 值变化时重新渲染。如果重渲染组件的开销较大，你可以 [通过使用 memoization 来优化](https://github.com/facebook/react/issues/15156#issuecomment-474590693)。



### [额外的 Hook](https://zh-hans.reactjs.org/docs/hooks-reference.html#additional-hooks)



#### [`useReducer`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usereducer)

[`useState`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usestate) 的替代方案。它接收一个形如 `(state, action) => newState` 的 reducer，并返回当前的 state 以及与其配套的 `dispatch` 方法

适用于state 逻辑较复杂且包含多个子值，或者下一个 state 依赖于之前的 state 等



#### [`useCallback`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usecallback)

```
const memoizedCallback = useCallback(
  () => {
    doSomething(a, b);
  },
  [a, b],
);
```

返回一个 [memoized](https://en.wikipedia.org/wiki/Memoization) 回调函数。

把内联回调函数及依赖项数组作为参数传入 `useCallback`，它将返回该回调函数的 memoized 版本，该回调函数仅在某个依赖项改变时才会更新。



#### [`useMemo`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usememo)

返回一个 [memoized](https://en.wikipedia.org/wiki/Memoization) 值。

把“创建”函数和依赖项数组作为参数传入 `useMemo`，它仅会在某个依赖项改变时才重新计算 memoized 值。这种优化有助于避免在每次渲染时都进行高开销的计算。

传入 `useMemo` 的函数会在渲染期间执行。请不要在这个函数内部执行与渲染无关的操作



#### [`useRef`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useref)

一般用于命令式的访问子组件

`useRef` 返回一个可变的 ref 对象，其 `.current` 属性被初始化为传入的参数（`initialValue`）。返回的 ref 对象在组件的整个生命周期内保持不变。

如果你将 ref 对象以 `<div ref={myRef} />` 形式传入组件，则无论该节点如何改变，React 都会将 ref 对象的 `.current` 属性设置为相应的 DOM 节点。

> 当 ref 对象内容发生变化时，`useRef` 并*不会*通知你。变更 `.current` 属性不会引发组件重新渲染。如果想要在 React 绑定或解绑 DOM 节点的 ref 时运行某些代码，则需要使用[回调 ref](https://zh-hans.reactjs.org/docs/hooks-faq.html#how-can-i-measure-a-dom-node) 来实现。

#### [`useImperativeHandle`](https://zh-hans.reactjs.org/docs/hooks-reference.html#useimperativehandle)

```
function FancyInput(props, ref) {
  const inputRef = useRef();
    useImperativeHandle(ref, () => ({
      focus: () => {
        inputRef.current.focus();
      }
    }));
  return <input ref={inputRef} ... />;
}
FancyInput = forwardRef(FancyInput);
```

让你在使用 `ref` 时自定义暴露给父组件的实例值。在大多数情况下，应当避免使用 ref 这样的命令式代码。

`useImperativeHandle` 应当与 [`forwardRef`](https://zh-hans.reactjs.org/docs/react-api.html#reactforwardref) 一起使用



#### [`useLayoutEffect`](https://zh-hans.reactjs.org/docs/hooks-reference.html#uselayouteffect)

其函数签名与 `useEffect` 相同，但它会在所有的 DOM 变更之后同步调用 effect。可以使用它来读取 DOM 布局并同步触发重渲染。在浏览器执行绘制之前，`useLayoutEffect` 内部的更新计划将被同步刷新。

#### [`useDebugValue`](https://zh-hans.reactjs.org/docs/hooks-reference.html#usedebugvalue)

用于在 React 开发者工具中显示自定义 hook 的标签。
