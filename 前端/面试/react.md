### 虚拟 dom

虚拟DOM是对DOM的抽象，这个对象是更加轻量级的对DOM的描述。它设计的最初目的，就是更好的跨平台。

本质上来说，Virtual Dom是一个JavaScript对象，通过对象的方式来表示DOM结构。将页面的状态抽象为JS对象的形式，使跨平台渲染成为可能。通过事务处理机制，将多次DOM修改的结果一次性的更新到页面上，从而有效的减少页面渲染的次数，减少修改DOM的重绘重排次数，提高渲染性能。

**优点：**

1. 保证性能下限，在不进行手动优化的情况下，提供过得去的性能
2. 跨平台



### class 组件和函数组件

|          | class组件                            | 函数组件                      |
| -------- | ------------------------------------ | ----------------------------- |
| 模型     | 面向对象编程                         | 函数式编程                    |
| 组件实例 | 有                                   | 无                            |
| 生命周期 | 有                                   | 无，有useEffect               |
| 状态     | state/setState                       | props                         |
| 性能     | shouldComponentUpdate 阻断渲染。较差 | React.memo 缓存渲染结果。较好 |
| 复用性   | 弱                                   | 强                            |
| 灵活性   | 差                                   | 好                            |



组合优于继承



### Hooks

类组件相对而言太繁杂了，对于解决许多问题来说，**编写一个类组件实在是一个过于复杂的姿势**，复杂的姿势必然带来高昂的理解成本。由于开发者编写的逻辑在封装后是和组件粘在一起的，这就使得**类组件内部的逻辑难以实现拆分和复用。**

函数组件肉眼可见的特质自然包括轻量、灵活、易于组织和维护、较低的学习成本等。函数组件比起类组件少了很多东西，比如生命周期、对 state 的管理等。而React-Hooks 的出现，就是为了帮助函数组件补齐这些（相对于类组件来说）缺失的能力，让函数组件也能够实现全功能的组件。



#### Hooks 解决了什么问题

**在组件之间复用状态**

 可以使用 Hook 从组件中提取状态逻辑，使得这些逻辑可以单独测试并复用。Hook 使我们在无需修改组件结构的情况下复用状态逻辑

 **复杂组件变得难以理解**

在组件中，每个生命周期常常包含一些不相关的逻辑。Hook 将组件中相互关联的部分拆分成更小的函数（比如设置订阅或请求数据），而并非强制按照生命周期划分。

**难以理解的 class**

使用 class 组件，我们必须去理解 JavaScript 中 this 的工作方式，这与其他语言存在巨大差异。





#### **Hooks 的限制**

- 不要在循环、条件或嵌套函数中调用 Hook；
- 只能在函数组件中调用 Hook。



#### React hooks 和 生命周期

| **class 组件**           | **Hooks 组件**            |
| ------------------------ | ------------------------- |
| constructor              | useState                  |
| getDerivedStateFromProps | useState 里面 update 函数 |
| shouldComponentUpdate    | useMemo                   |
| render                   | 函数本身                  |
| componentDidMount        | useEffect                 |
| componentDidUpdate       | useEffect                 |
| componentWillUnmount     | useEffect 里面返回的函数  |
| componentDidCatch        | 无                        |
| getDerivedStateFromError | 无                        |



useEffect 和useLayoutEffect区别

[React Hooks 详解 【近 1W 字】+ 项目实战 - 掘金 (juejin.cn)](https://juejin.cn/post/6844903985338400782#heading-23)













### 高阶组件 HOC

HOC 是自定义组件，在它之内包含另一个组件。它们可以接受子组件提供的任何动态，但不会修改或复制其输入组件中的任何行为。你可以认为 HOC 是“纯（Pure）”组件。



#### 用途

- 代码重用，逻辑和引导抽象
- 渲染劫持
- 状态抽象和控制
- Props 控制





### setState是同步的还是异步

**1. setState 是同步还是异步？**

我的回答是执行过程代码同步的，只是合成事件和钩子函数的调用顺序在更新之前，导致在合成事件和钩子函数中没法立马拿到更新后的值，形式了所谓的“异步”，所以表现出来有时是同步，有时是“异步”。

**2. 何时是同步，何时是异步呢？**

只在合成事件和钩子函数中是“异步”的，在原生事件和 setTimeout/setInterval等原生 API 中都是同步的。简单的可以理解为被 React 控制的函数里面就会表现出“异步”，反之表现为同步。

3. **那为什么会出现异步的情况呢**？

为了做性能优化，将 state 的更新延缓到最后批量合并再去渲染对于应用的性能优化是有极大好处的，如果每次的状态改变都去重新渲染真实 dom，那么它将带来巨大的性能消耗。

4. **那如何在表现出异步的函数里可以准确拿到更新后的** state 呢？

通过第二个参数 setState(partialState, callback) 中的 callback 拿到更新后的结果。或者可以通过给 setState 传递函数来表现出同步的情况：

```kotlin
this.setState((state) => {
    return { val: newVal }
})
```



### React 事件

React并不是将click事件绑定到了div的真实DOM上，而是在document处监听了所有的事件。这样的方式不仅仅减少了内存的消耗，还能在组件挂在销毁时统一订阅和移除事件。

React基于Virtual DOM实现了一个SyntheticEvent层（合成事件层），定义的事件处理器会接收到一个合成事件对象的实例，它符合W3C标准，且与原生的浏览器事件拥有同样的接口，支持冒泡机制，所有的事件都自动绑定在最外层上。因此如果不想要是事件冒泡的话应该调用event.preventDefault()方法，而不是调用event.stopProppagation()方法。



在React底层，主要对合成事件做了两件事：

- **事件委派：** React会把所有的事件绑定到结构的最外层，使用统一的事件监听器，这个事件监听器上维持了一个映射来保存所有组件内部事件监听和处理函数。
- **自动绑定：** React组件中，每个方法的上下文都会指向该组件的实例，即自动绑定this为当前组件。



实现合成事件的目的如下：

- 合成事件首先抹平了浏览器之间的兼容问题，另外这是一个跨浏览器原生事件包装器，赋予了跨浏览器开发的能力；
- 对于原生浏览器事件来说，浏览器会给监听器创建一个事件对象。如果你有很多的事件监听，那么就需要分配很多的事件对象，造成高额的内存分配问题。但是对于合成事件来说，有一个事件池专门来管理它们的创建和销毁，当事件需要被使用时，就会从池子中复用对象，事件回调结束后，就会销毁事件对象上的属性，从而便于下次复用事件对象。



**react 事件执行顺序** 

- `react`的所有事件都挂载在`document`中
- 当真实dom触发后冒泡到`document`后才会对`react`事件进行处理
- 所以原生的事件会先执行
- 然后执行`react`合成事件
- 最后执行真正在`document`上挂载的事件



### React 重渲染



#### 什么时候会重渲染

- **setState 方法**
    - 当 setState 传入 null 时，并不会触发 render
- **父组件重新渲染**
    - 只要父组件重新渲染了，即使传入子组件的 props 未发生变化，那么子组件也会重新渲染，进而触发 render



##### **shouldComponentUpdate**

当React将要渲染组件时会执行`shouldComponentUpdate`方法来看它是否返回`true`（组件应该更新，也就是重新渲染）。所以需要重写`shouldComponentUpdate`方法让它根据情况返回`true`或者`false`来告诉React什么时候重新渲染什么时候跳过重新渲染。



#### 重渲染发生了什么

- 会对新旧 VNode 进行对比，也就是我们所说的Diff算法。
- 对新旧两棵树进行一个深度优先遍历，这样每一个节点都会一个标记，在到深度遍历的时候，每遍历到一和个节点，就把该节点和新的节点树进行对比，如果有差异就放到一个对象里面
- 遍历差异对象，根据差异的类型，根据对应对规则更新VNode



#### 如何避免不必要的重渲染

**shouldComponentUpdate 和 PureComponent**

在 React 类组件中，可以利用 shouldComponentUpdate 或者 PureComponent 来减少因父组件更新而触发子组件的 render，从而达到目的。shouldComponentUpdate 来决定是否组件是否重新渲染，如果不希望组件重新渲染，返回 false 即可。

**利用高阶组件**

在函数组件中，并没有 shouldComponentUpdate 这个生命周期，可以利用高阶组件，封装一个类似 PureComponet 的功能

**使用 React.memo**

React.memo()可接受2个参数，第一个参数为纯函数的组件，第二个参数用于对比props控制是否刷新，与`shouldComponentUpdate()`功能类似。

```text
import React from "react";

function Child({seconds}){
    console.log('I am rendering');
    return (
        <div>I am update every {seconds} seconds</div>
    )
};

function areEqual(prevProps, nextProps) {
    if(prevProps.seconds===nextProps.seconds){
        return true
    }else {
        return false
    }

}
export default React.memo(Child,areEqual)
```



### 非受控组件

#### **受控组件** 

在使用表单来收集用户输入时，例如 `<input><select><textearea>` 等元素都要绑定一个change事件，当表单的状态发生变化，就会触发onChange事件，更新组件的state，触发视图的重新渲染，完成表单组件的更新。

这种组件在React中被称为**受控组件**，在受控组件中，组件渲染出的状态与它的 value 或 checked 属性相对应，react 通过这种方式消除了组件的局部状态，使整个状态可控。react官方推荐使用受控表单组件。



**受控组件缺陷：** 表单元素的值都是由React组件进行管理，当有多个输入框，或者多个这种组件时，如果想同时获取到全部的值就必须每个都要编写事件处理函数



#### **非受控组件** 

如果一个表单组件没有value props（单选和复选按钮对应的是checked props）时，就可以称为非受控组件。在非受控组件中，可以使用一个ref来从DOM获得表单值。而不是为每个状态更新编写一个事件处理程序。



### React.forwardRef

React.forwardRef 会创建一个React组件，这个组件能够将其接受的 ref 属性转发到其组件树下的另一个组件中。这种技术并不常见，但在以下两种场景中特别有用：

- 转发 refs 到 DOM 组件
- 在高阶组件中转发 refs





### 组件通信



#### 父子组件

- 父传子： props
- 子传父： 父将方法通过 props 传给子，子调用



#### 爷孙组件

- props 层层传递
    - 麻烦，复杂度高
- context
    - 破坏了组件的低耦合



#### 通用通信方式

- Redux
- 发布订阅模式
- 兄弟组件，可以通过共同的父节点进行通信。







### React 和 Vue 

####  相似

- 都将注意力集中保持在核心库，而将其他功能如路由和全局状态管理交给相关的库
- Virtual DOM
- 都有props的概念，允许组件间的数据传递
- 组件化



#### 不同之处

- 数据流
    - vue 双向绑定，react 单向数据流
- 虚拟 dom
    - Vue 在渲染过程中，会跟踪每一个组件的依赖关系，不需要重新渲染整个组件树。
    - React 每当应用的状态被改变时，全部子组件都会重新渲染。
- 模版
    - vue 接近原生
    - react 推荐 jsx
- 数据监听
    - vue 对数据进行劫持
    - react 通过引用比较
- 组件拓展
    - vue 通过 mixins
    - react 通过高阶组件













