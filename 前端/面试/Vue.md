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









# Vuex



# Vue-Router