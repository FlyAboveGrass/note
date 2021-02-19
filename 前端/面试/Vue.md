# Vue

### [Vue组件通信](https://juejin.cn/post/6844904048118726663#heading-18)

1. 父子组件通信。 
   1. props 和 $emit
   2. $refs
2. 祖先后代
   1. [provider和inject(依赖注入)](https://vue3js.cn/docs/zh/guide/component-provide-inject.html)
3. 通用
   1. [Vuex](https://vuex.vuejs.org/zh/)
   2. eventBus事件总线



### [v-for为什么需要key](https://vue3js.cn/docs/zh/guide/list.html#%E7%BB%B4%E6%8A%A4%E7%8A%B6%E6%80%81)

[讨论](https://github.com/Advanced-Frontend/Daily-Interview-Question/issues/1)

使用 v-for渲染的元素列表时，它默认使用“就地更新”的策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序，而是**就地更新每个元素**，并且确保它们在每个索引位置正确渲染。

> 建议尽可能在使用 `v-for` 时提供 `key` attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。

[key](https://vue3js.cn/docs/zh/api/special-attributes.html#key)是给每一个vnode的唯一id,可以依靠key,更准确, 更快的拿到oldVnode中对应的vnode节点。

1. 在diff算法执行是更快的找到对应的节点，提高diff速度
2. 避免“原地复用”所带来的副作用。

> 不建议使用index作为key。 因为当item和index的对应关系发生改变，，某个item前后两次并不是同一个index也就是不是同一个key，是不能发生复用的。



### Vue中的指令

#### 指令修饰符

+ stop： 阻止事件冒泡
+ prevent： 阻止默认事件
+ once： 只触发一次
+ capture（捕获）： 子元素触发的事件先在这里进行处理，然后再交给子元素处理
+ self： 只响应监听器绑定的该元素的本身触发的事件
+ passive： 主要用于优化移动端的滚动。每次滚动都要去查询一下是否有使用preventDefault阻止默认事件，使用passive就可以直接告诉浏览器我们没有preventDefault， 减少查询的时间获得更好的滑动体验。

> 浏览器只有等内核线程执行到事件监听器对应的JavaScript代码时，才能知道内部是否会调用preventDefault函数来阻止事件的默认行为，所以浏览器本身是没有办法对这种场景进行优化的。这种场景下，用户的手势事件无法快速产生，会导致页面无法快速执行滑动逻辑，从而让用户感觉到页面卡顿。



### $nextTick

在$nextTick里面的回调函数，会推迟到DOM更新完成，下一个DOM更新周期去执行

在Vue中，数据变化并不会马上更新DOM，而是会开启一个队列，并且缓冲在同一个事件循环中的数据改变，对于同一数据的多次改变将去除重复的，减少更新的次数，避免不必要的DOM操作。



```
getText:function(){
    this.showDiv = true;
    this.$nextTick(function(){
    	var text = document.getElementById('div').innnerHTML;
    	console.log(text);  
    });
}
```

### 为什么Vue中的data是一个函数

如果data是一个对象，那么这个组件被重复使用的时候，他们的data是共享的，一个改变另一个也会跟着改变。要保证data是私有的、独立的，不会被其他的组件影响。

#### 如果data是对象

```
var MyComponent = function() {}
MyComponent.prototype.data = {
  a: 1,
  b: 2,
}
```

#### 如果data是函数

```
var MyComponent = function() {
  this.data = this.data()
}
MyComponent.prototype.data = function() {
  return {
    a: 1,
    b: 2,
  }
}
```

### keep-alive

定义： 包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们

作用： 保留组件的状态，避免重新渲染

> 不会在函数式组件中正常工作，因为它们没有缓存实例。
>
> `activated` 和 `deactivated`[生命周期](http://www.html5iq.com/content?aid=5b3b169bcb794e5b86cd4be9) 将会在 树内的所有嵌套组件中触发。

#### 属性

- `include:`字符串或正则表达式。只有匹配的组件会被缓存。
- `exclude：`字符串或正则表达式。任何匹配的组件都不会被缓存。

#### 用法

```
<!-- 逗号分隔字符串 -->
<keep-alive include="a,b">
  <component :is="view"></component>
</keep-alive>

<!-- 正则表达式 (使用 `v-bind`) -->
<keep-alive :include="/a|b/">
  <component :is="view"></component>
</keep-alive>

<!-- 数组 (使用 `v-bind`) -->
<keep-alive :include="['a', 'b']">
  <component :is="view"></component>
</keep-alive>
```

### [生命周期](https://zhuanlan.zhihu.com/p/71958016)

![](E:\Code\笔记\笔记图片\vue生命周期.jpg)

| 生命周期钩子函数（11个）     | 详细                                                         |
| :--------------------------- | :----------------------------------------------------------- |
| beforeCreate                 | 在`实例初始化之后`，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。 |
| created                      | 在`实例创建完成后`被立即调用。实例已完成：`数据观测 (data observer)`， `属性和方法的运算`，`watch/event 事件回调`。实例还没挂载，$el 属性目前不可见。 |
| beforeMount                  | 在`挂载开始之前`被调用：相关的 render 函数首次被调用。虚拟Dom已经创建完成，可以对数据进行更改，不会触发updated。 |
| mounted                      | `el` 被新创建的 `vm.$el` 替换，并`挂载到实例上去之后`调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。 |
| beforeUpdate                 | `响应式数据发生更新，虚拟dom重新渲染之前被触发`。这里适合在更新之前访问现有的 DOM，比如手动移除已添加的事件监听器，当前阶段进行更改数据，不会造成重渲染。**该钩子在服务器端渲染期间不被调用，因为只有初次渲染会在服务端进行。** |
| updated                      | 由于数据更改导致的`虚拟 DOM 重新渲染和打补丁`，在这`之后`会`调用`该钩子。 |
| activated                    | `keep-alive 组件激活时调用`。**该钩子在服务器端渲染期间不被调用。** |
| deactivated                  | `keep-alive 组件停用时调用`。**该钩子在服务器端渲染期间不被调用。** |
| beforeDestroy                | 实例销毁之前调用。在这一步，实例仍然完全可用。**该钩子在服务器端渲染期间不被调用。** |
| destroyed                    | Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。**该钩子在服务器端渲染期间不被调用。** |
| errorCaptured（2.5.0+ 新增） | 当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 false 以阻止该错误继续向上传播。 |

# Vuex



# Vue-Router