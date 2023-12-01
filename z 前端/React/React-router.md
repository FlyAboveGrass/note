# 基础



## 路由配置



### 配置方式



**JSX 嵌套结构**

```
import { Redirect } from 'react-router'

React.render((
  <Router>
    <Route path="/" component={App}>
      <IndexRoute component={Dashboard} />
      <Route path="about" component={About} />
      <Route path="inbox" component={Inbox}>
        <Route path="/messages/:id" component={Message} />

        {/* 兼容旧的 url 跳转 /inbox/messages/:id 到 /messages/:id */}
        <Redirect from="messages/:id" to="/messages/:id" />
      </Route>
    </Route>
  </Router>
), document.body)
```



**route 数组对象**

```
const routeConfig = [
  { path: '/',
    component: App,
    indexRoute: { component: Dashboard },
    childRoutes: [
      { path: 'about', component: About },
      { path: 'inbox',
        component: Inbox,
        childRoutes: [
          { path: '/messages/:id', component: Message },
          { path: 'messages/:id',
            onEnter: function (nextState, replaceState) {
              replaceState(null, '/messages/' + nextState.params.id)
            }
          }
        ]
      }
    ]
  }
]

React.render(<Router routes={routeConfig} />, document.body)
```



### 进入和离开的 hook

[Route](https://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#route) 可以定义 [`onEnter`](https://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#enterhook) 和 [`onLeave`](https://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#leavehook) 两个 hook ，这些hook会在页面跳转[确认](https://react-guide.github.io/react-router-cn/docs/guides/basics/docs/guides/advanced/ConfirmingNavigation.md)时触发一次







## 路由匹配原理



[路由](https://react-guide.github.io/react-router-cn/docs/guides/basics/docs/Glossary.md#route)拥有三个属性来决定是否“匹配“一个 URL：

1. [嵌套关系](https://react-guide.github.io/react-router-cn/docs/guides/basics/RouteMatching.html#nesting) 
2. [`路径语法`](https://react-guide.github.io/react-router-cn/docs/guides/basics/RouteMatching.html#path-syntax)
3. [优先级](https://react-guide.github.io/react-router-cn/docs/guides/basics/RouteMatching.html#precedence)





## history



react 三种 history 形式

- [`browserHistory`](https://react-guide.github.io/react-router-cn/docs/guides/basics/Histories.html#browserHistory)
- [`hashHistory`](https://react-guide.github.io/react-router-cn/docs/guides/basics/Histories.html#hashHistory)
- [`createMemoryHistory`](https://react-guide.github.io/react-router-cn/docs/guides/basics/Histories.html#creatememoryhistory)



使用方式

```
// JavaScript 模块导入（译者注：ES6 形式）
import { browserHistory } from 'react-router'

render(
  <Router history={browserHistory} routes={routes} />,
  document.getElementById('app')
)
```





### browserHistory

推荐使用。它使用 浏览器的 history api 用于处理 url



#### 服务器配置





### hashHistory







### createMemoryHistory

Memory history 不会在地址栏被操作或读取。这就解释了我们是如何实现服务器渲染的。同时它也非常适合测试和其他的渲染环境（像 React Native ）





# 默认路由IndexRoute与 IndexLink



### 默认路由

```
<Router>
  <Route path="/" component={App}>
    <IndexRoute component={Home}/>
    <Route path="accounts" component={Accounts}/>
    <Route path="statements" component={Statements}/>
  </Route>
</Router>
```





### Index Links

如果需要在根路由被渲染后才激活的指向 `/` 的链接，请使用

 `<IndexLink to="/">Home</IndexLink>`



否则它会一直保持激活状态，因为所有路由的开头都是 `/`







# 高级



## 动态路由

React Router 里的[路径匹配](https://react-guide.github.io/react-router-cn/docs/guides/advanced/docs/guides/basics/RouteMatching.md)以及组件加载都是异步完成的，不仅允许你延迟加载组件，**并且可以延迟加载路由配置**。

Route 可以定义 [`getChildRoutes`](https://react-guide.github.io/react-router-cn/docs/guides/advanced/docs/API.md#getchildrouteslocation-callback)，[`getIndexRoute`](https://react-guide.github.io/react-router-cn/docs/guides/advanced/docs/API.md#getindexroutelocation-callback) 和 [`getComponents`](https://react-guide.github.io/react-router-cn/docs/guides/advanced/docs/API.md#getcomponentslocation-callback) 这几个函数。它们都是异步执行，并且只有在需要时才被调用。我们将这种方式称之为 “逐渐匹配”。



## 跳转确认



React Router 提供一个 [`routerWillLeave` 生命周期钩子](https://react-guide.github.io/react-router-cn/docs/guides/advanced/docs/Glossary.md#routehook)，这使得 React [组件](https://react-guide.github.io/react-router-cn/docs/guides/advanced/docs/Glossary.md#component)可以拦截正在发生的跳转，或在离开 [route](https://react-guide.github.io/react-router-cn/docs/guides/advanced/docs/Glossary.md#route) 前提示用户。

```
import { Lifecycle } from 'react-router'

const Home = React.createClass({

  // 假设 Home 是一个 route 组件，它可能会使用
  // Lifecycle mixin 去获得一个 routerWillLeave 方法。
  mixins: [ Lifecycle ],

  routerWillLeave(nextLocation) {
    if (!this.state.isSaved)
      return 'Your work is not saved! Are you sure you want to leave?'
  },

})
```





## 服务端渲染







## 组件声明周期

















# API

- [组件](https://react-guide.github.io/react-router-cn/docs/API.html#components)
    - [Router](https://react-guide.github.io/react-router-cn/docs/API.html#router)
    - [Link](https://react-guide.github.io/react-router-cn/docs/API.html#link)
    - [IndexLink](https://react-guide.github.io/react-router-cn/docs/API.html#indexlink)
    - [RoutingContext](https://react-guide.github.io/react-router-cn/docs/API.html#routingcontext)
- [组件的配置](https://react-guide.github.io/react-router-cn/docs/API.html#configuration-components)
    - [Route](https://react-guide.github.io/react-router-cn/docs/API.html#route)
    - [PlainRoute](https://react-guide.github.io/react-router-cn/docs/API.html#plainroute)
    - [Redirect](https://react-guide.github.io/react-router-cn/docs/API.html#redirect)
    - [IndexRoute](https://react-guide.github.io/react-router-cn/docs/API.html#indexroute)
    - [IndexRedirect](https://react-guide.github.io/react-router-cn/docs/API.html#indexredirect)
- [Route 组件](https://react-guide.github.io/react-router-cn/docs/API.html#route-components)
    - [已命名的组件](https://react-guide.github.io/react-router-cn/docs/API.html#named-components)
- [Mixins](https://react-guide.github.io/react-router-cn/docs/API.html#mixins)
    - [生命周期](https://react-guide.github.io/react-router-cn/docs/API.html#lifecycle-mixin)
    - [History](https://react-guide.github.io/react-router-cn/docs/API.html#history-mixin)
    - [RouteContext](https://react-guide.github.io/react-router-cn/docs/API.html#routecontext-mixin)
- [Utilities](https://react-guide.github.io/react-router-cn/docs/API.html#utilities)
    - [useRoutes](https://react-guide.github.io/react-router-cn/docs/API.html#useroutescreatehistory)
    - [match](https://react-guide.github.io/react-router-cn/docs/API.html#matchlocation-cb)
    - [createRoutes](https://react-guide.github.io/react-router-cn/docs/API.html#createroutesroutes)
    - [PropTypes](https://react-guide.github.io/react-router-cn/docs/API.html#proptypes)







# ——————————







# [react router dom](https://reactrouter.com/web/guides/philosophy)



> 参考链接： [react-router-dom@5.x官方文档翻译 - SegmentFault 思否](https://segmentfault.com/a/1190000020812860#comment-area)



## 重要组件



React Router中的组件主要分为三类：

- 路由器，例如<BrowserRouter>和<HashRouter>
- 路由匹配器，例如<Route>和<Switch>
- 导航，例如<Link>，<NavLink>和<Redirect>





### 路由器



路由器组件是每一个 react router 应用程序的核心



react-router-dom提供<BrowserRouter>和<HashRouter>路由器



**<BrowserRouter>**

使用常规路径。你的服务器需要在所有react router 客户端管理的url 上面提供相应的页面。

**<HashRouter**

将当前位置存储在url 的 hash 部分。不需要特殊的服务器配置。





使用路由器，只需确保将其渲染在元素层次结构的根目录下即可

```
import React from "react";
import ReactDOM from "react-dom";
import { BrowserRouter } from "react-router-dom";

function App() {
  return <h1>Hello React Router</h1>;
}

ReactDOM.render(
  <BrowserRouter>
    <App />
  </BrowserRouter>,
  document.getElementById("root")
);
```







### 路由匹配器

路由匹配器有两个



1. Switch

2. Route

    

搜索其子元素<Route>，以查找其路径与当前URL匹配的元素。当找到一个时，它将渲染该<Route>并忽略所有其他路由。这意味着您应该将<Route>包含更多特定路径（通常较长）的路径放在不那么特定路径之前。

如果没有匹配，switch 不显示任何内容





### 导航



**Link**

在程序中创建链接，渲染为 a 标签



**NavLink**

当其 prop 与当前位置匹配的时候，可以将自身设置为 “active”



> 任何时候要强制导航，都可以渲染<Redirect>。渲染<Redirect>时，它将会使用其props进行导航







## 服务端渲染







## 代码分割







## 滚动重置



当页面导航改变的时候，可以通过 **<ScrollTop>** 组件滚动回顶部



```
import { useEffect } from 'react';
import { useLocation } from 'react-router-dom';
export default function ScrollToTop() {
    const { pathname } = useLocation();
    console.log('pathname=>', pathname);
    useEffect(() => {
        window.scrollTo(0, 0);
    }, [ pathname ]);
    return null;
}
```

 



如果将标签页接口连接到路由器，当切换标签页的时候不希望滚动到顶部，而是滚动到特定位置。 可以使用 **<ScrollToTopOnMount>**



```
import { useEffect } from "react";

function ScrollToTopOnMount() {
  useEffect(() => {
    window.scrollTo(0, 0);
  }, []);

  return null;
}

// 使用以下代码将此内容渲染到某处：
// <Route path="..." children={<LongContent />} />
function LongContent() {
  return (
    <div>
      <ScrollToTopOnMount />

      <h1>Here is my long content page</h1>
      <p>...</p>
    </div>
  );
```





## 哲学









## 深层 Redux 迁移







## 静态路由







## API



除了 Hook 是函数之外，其他的提供都是标签功能

### [Hooks](https://reactrouter.com/web/api/Hooks)

访问路由器的状态并从组件内部执行导航



[useHistory](https://reactrouter.com/web/api/Hooks/usehistory)

[useLocation](https://reactrouter.com/web/api/Hooks/uselocation)

[useParams](https://reactrouter.com/web/api/Hooks/useparams)

[useRouteMatch](https://reactrouter.com/web/api/Hooks/useroutematch)



### [BrowserRouter](https://reactrouter.com/web/api/BrowserRouter)

它使用HTML5 history API (pushState、replaceState和popstate事件)来保持UI与URL同步。

[basename: string](https://reactrouter.com/web/api/BrowserRouter/basename-string)

[getUserConfirmation: func](https://reactrouter.com/web/api/BrowserRouter/getuserconfirmation-func)

[forceRefresh: bool](https://reactrouter.com/web/api/BrowserRouter/forcerefresh-bool)

[keyLength: number](https://reactrouter.com/web/api/BrowserRouter/keylength-number)

[children: node](https://reactrouter.com/web/api/BrowserRouter/children-node)



### [HashRouter](https://reactrouter.com/web/api/HashRouter)

[basename: string](https://reactrouter.com/web/api/HashRouter/basename-string)

[getUserConfirmation: func](https://reactrouter.com/web/api/HashRouter/getuserconfirmation-func)

[hashType: string](https://reactrouter.com/web/api/HashRouter/hashtype-string)

[children: node](https://reactrouter.com/web/api/HashRouter/children-node)



### [Link](https://reactrouter.com/web/api/Link)

提供围绕应用程序的声明式、可访问的导航，其实渲染出来的就是一个 `<a>` 标签，对标签的封装。

[to: string](https://reactrouter.com/web/api/Link/to-string)

[to: object](https://reactrouter.com/web/api/Link/to-object)

[to: function](https://reactrouter.com/web/api/Link/to-function)

[replace: bool](https://reactrouter.com/web/api/Link/replace-bool)

[innerRef: function](https://reactrouter.com/web/api/Link/innerref-function)

[innerRef: RefObject](https://reactrouter.com/web/api/Link/innerref-refobject)

[component: React.Component](https://reactrouter.com/web/api/Link/component-reactcomponent)

[others](https://reactrouter.com/web/api/Link/others)



### [Redirect](https://reactrouter.com/web/api/Redirect)

渲染<Redirect>将导航到新位置。新位置将覆盖历史记录堆栈中的当前位置

[to: string](https://reactrouter.com/web/api/Redirect/to-string)

[to: object](https://reactrouter.com/web/api/Redirect/to-object)

[push: bool](https://reactrouter.com/web/api/Redirect/push-bool)

[from: string](https://reactrouter.com/web/api/Redirect/from-string)

[exact: bool](https://reactrouter.com/web/api/Redirect/exact-bool)

[strict: bool](https://reactrouter.com/web/api/Redirect/strict-bool)

[sensitive: bool](https://reactrouter.com/web/api/Redirect/sensitive-bool)





### [Route](https://reactrouter.com/web/api/Route)

Router组件可能是React Router中了解和学习使用的最重要组件。它的最基本职责是在其路径与当前URL匹配时显示一些UI。



[Route render methods](https://reactrouter.com/web/api/Route/route-render-methods)

[Route props](https://reactrouter.com/web/api/Route/route-props)

[component](https://reactrouter.com/web/api/Route/component)

[render: func](https://reactrouter.com/web/api/Route/render-func)

[children: func](https://reactrouter.com/web/api/Route/children-func)

[path: string | string](https://reactrouter.com/web/api/Route/path-string-string)

[exact: bool](https://reactrouter.com/web/api/Route/exact-bool)

[strict: bool](https://reactrouter.com/web/api/Route/strict-bool)

[location: object](https://reactrouter.com/web/api/Route/location-object)

[sensitive: bool](https://reactrouter.com/web/api/Route/sensitive-bool)



### [Switch](https://reactrouter.com/web/api/Switch)

渲染与位置匹配的第一个子元素<Route>或<Redirect>。

<Switch>的独特之处在于它专门渲染一条路由。



[location: object](https://reactrouter.com/web/api/Switch/location-object)

[children: node](https://reactrouter.com/web/api/Switch/children-node)



### [Router](https://reactrouter.com/web/api/Router)

所有路由器组件的通用底层接口。通常，应用将使用高级路由器之一代替：



[history: object](https://reactrouter.com/web/api/Router/history-object)

[children: node](https://reactrouter.com/web/api/Router/children-node)



### ____________________

### 

### [NavLink](https://reactrouter.com/web/api/NavLink)

<Link>的特殊版本，当它与当前URL匹配时，它将为渲染的元素添加样式属性。

[activeClassName: string](https://reactrouter.com/web/api/NavLink/activeclassname-string)

[activeStyle: object](https://reactrouter.com/web/api/NavLink/activestyle-object)

[exact: bool](https://reactrouter.com/web/api/NavLink/exact-bool)

[strict: bool](https://reactrouter.com/web/api/NavLink/strict-bool)

[isActive: func](https://reactrouter.com/web/api/NavLink/isactive-func)

[location: object](https://reactrouter.com/web/api/NavLink/location-object)

[aria-current: string](https://reactrouter.com/web/api/NavLink/aria-current-string)



### [Prompt](https://reactrouter.com/web/api/Prompt)

离开页面之前提示用户



### [MemoryRouter](https://reactrouter.com/web/api/MemoryRouter)

[initialEntries: array](https://reactrouter.com/web/api/MemoryRouter/initialentries-array)

[initialIndex: number](https://reactrouter.com/web/api/MemoryRouter/initialindex-number)

[getUserConfirmation: func](https://reactrouter.com/web/api/MemoryRouter/getuserconfirmation-func)

[keyLength: number](https://reactrouter.com/web/api/MemoryRouter/keylength-number)

[children: node](https://reactrouter.com/web/api/MemoryRouter/children-node)





### [StaticRouter](https://reactrouter.com/web/api/StaticRouter)

[basename: string](https://reactrouter.com/web/api/StaticRouter/basename-string)[location: string](https://reactrouter.com/web/api/StaticRouter/location-string)[location: object](https://reactrouter.com/web/api/StaticRouter/location-object)[context: object](https://reactrouter.com/web/api/StaticRouter/context-object)[children: node](https://reactrouter.com/web/api/StaticRouter/children-node)







### [generatePath](https://reactrouter.com/web/api/generatePath)

[pattern: string](https://reactrouter.com/web/api/generatePath/pattern-string)[params: object](https://reactrouter.com/web/api/generatePath/params-object)



### [history](https://reactrouter.com/web/api/history)

[history is mutable](https://reactrouter.com/web/api/history/history-is-mutable)



### [location](https://reactrouter.com/web/api/location)



### [match](https://reactrouter.com/web/api/match)

[null matches](https://reactrouter.com/web/api/match/null-matches)



### [matchPath](https://reactrouter.com/web/api/matchPath)

[pathname](https://reactrouter.com/web/api/matchPath/pathname)[props](https://reactrouter.com/web/api/matchPath/props)[returns](https://reactrouter.com/web/api/matchPath/returns)



### [withRouter](https://reactrouter.com/web/api/withRouter)

[Component.WrappedComponent](https://reactrouter.com/web/api/withRouter/componentwrappedcomponent)[wrappedComponentRef: func](https://reactrouter.com/web/api/withRouter/wrappedcomponentref-func)

















































































