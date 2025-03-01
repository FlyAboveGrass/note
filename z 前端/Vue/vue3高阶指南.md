# Vue3的优势

**diff算法优化**

Vue2 中对于虚拟DOM会进行全量的对比。

Vue3 对于不同的元素，会添加不同的静态标记。对于静态的不会变化的元素不添加标记。在发生更新的时候会只更新有静态标记的节点，并且通过标记得知节点需要对比的具体内容。

**静态提升**

1. Vue2中无论元素是否参加更新，每次都会重新创建，然后再渲染。
2. Vue3 中对于不参与更新的元素会做静态提升，以全局变量的形式只创建一次，再渲染的时候可以直接复用。（空间换时间）

**事件侦听器缓存**

方法被存入 cache。在使用的时候，如果能在缓存中找到这个方法，那么它将直接被使用。如果找不到，那么将这个方法注入缓存。

**ssr渲染**

# 响应性

## Vue如何追踪变化

​    把一个普通的 JavaScript 对象作为 data 选项传给应用或组件实例的时候，Vue 会使用带有 getter 和 setter 的处理程序遍历其所有 property 并将其转换为 [**Proxy**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)**，** property 的值被访问或修改的情况下进行依赖跟踪和变更通知。组件就成为了其每个 property 的订阅者。当 Proxy 拦截到 set 操作时，该 property 将通知其所有订阅的组件重新渲染。

当更改发生的时候：

1. ~~当某个值发生变化时进行检测~~：我们不再需要这样做，因为 Proxy 允许我们拦截它
2. **跟踪更改它的函数**：我们在 Proxy 中的 getter 中执行此操作，称为 effect
3. **触发函数以便它可以更新最终值**：我们在 Proxy 中的 setter 中进行该操作，名为 trigger

# 响应性基础

## Composition API（组合API）

setup是一个接受props和context的函数。从setup中返回的所有内容都将暴露给组件的其他部分（数据，计算属性，方法，生命周期钩子等）以及组件的模板，都允许响应式的使用

setup的作用在于将功能分割成一块一块的暴露出来使用，同一功能的内容可以聚集在一起。Vue2中新增逻辑则是通过在所有的地方都要添加功能（data， methods， computed等），维护的时候同一功能的实现很分散。

setup中不存在this（this的值为undefind）。setup是在解析其他组件选项的之前被调用的，执行的时候尚未创建组件实例，所以我们在这时候不能访问到组件中的任何属性（本地状态，计算属性和方法等）。

setup不允许存在异步。因为setup的内容会立即执行，并且return给组件使用。

## ref

它有个key为symbol的属性做类型标识，有个属性value用来存储数据。这个数据可以是任意的类型，唯独不能是被嵌套了Ref类型的类型

Ref的类型是响应式的类型，但是只能监听简单数据类型。

也可以用Ref来封装对象，只不过在访问上多一层 .value，写法麻烦一点。

修改ref定义的数据时，实际上要操作ref数据的 value属性。但是在页面里面则不需要用 .value属性读取，Vue3会自动给我们添加.value 。

<u>ref 的本质： ref（val） => reactive（{ value ： val }）</u>

## reactive

reactive是Vue3 中提供的实现响应式数据的方法。与Vue2中使用object.definedProperty不同，Vue3中采用Proxy实现的。

reacitive可以用来监听对象或者数组等复杂类型的。(可以用reactive去封装简单数据类型，但是要把参数从基本类型转换成对象。)

reactive包装对象的源数据obj 改变时，会改变包装对象里面的值，因为包装对象里的值实际上是对obj的引用。但是改变元对象不可以触发视图的更新，因此当我们想要改变包装对象但是又不想触发更新的时候可以改变源对象的值。



如果给reactive传递了其他对象,

```
reactive（{ date： new Date() }
```

- 默认情况下，修改对象无法实现数据绑定更新
- 如果需要更新，需要重新赋值。（即不能直接操作数据，要重新提供一个新数据去替代原数据，这样才可以更改掉存储数据的引用）。（如 xx.date = new Date() , 而不是 xx.date.SetDate() ）

## ref和reactive的联系/区别

1、 reactive用于对象。ref用于简单数据类型，ref实际上是reactive多包了一层。

2、响应式状态解构： 将reactive的对象变成ref。

```
const book = reactive({ 
    author: 'Vue Team', 
    title: 'Vue 3 Guide'
})

// 解构的两个 property 的响应性都会丢失
let { author, title } = book
// ref 将保留与源对象的响应式关联
let { author, title } = toRefs(book)
```

3、使用readonly防止更改响应式对象

```
const original = reactive({ count: 0 })
const copy = readonly(original) 

original.count++ // 在copy上转换original 会触发侦听器依赖 ，正常修改
copy.count++ // 转换copy 将导失败并导致警告 ， 无法修改，会报错
```

# shallow（浅）

 **Vue提供的一种非递归监听的方案**

递归监听： 对于ref 和 reactive 包装的对象，都是改变一层的属性就会递归的检查每一层的所有属性进行更新检查的。这样虽然保证了每一层的值的改变都可以监听到，但是也带来了性能的消耗。

## shallowRef

ref在对对象转换reactive化的过程中，会判断是否是shallow 的，如果是，那么只会对最外层的数据执行数据监听；如果不是，那么会对整个数据都进行监听。

## triggerRef

对于shallow过的ref对象，我们修改了内层的数据是不会更新视图的，如果想进行更新的话可以使用reiggerRef 手动触发视图的更新。

## shallowReactive

类似于shallowRef, 改变对象的值的时候直接给属性赋值。

## 注意

Vue没有给reactive提供手动触发视图更新的方法，没有triggerReactive。shallowRef是特殊的

shallowReactive， 就像ref是特殊的reactive一样

# Raw

当我们想改变数据但是不影响视图的时候，可以通过修改源数据的方式来进行。如果源数据的引用没有保存或者没有暴露出来无法获取到的情况下，可以用toRaw来获得源数据的引用。

reactive的toRaw： 是一个用来优化资源加载的方案，可以重新获得源数据的引用。

ref 的 toRaw： 如果要获取ref的源数据，要明确的告诉toRaw要获取的是 .value 的值 toRaw（xx.value）。直接获取只会返回自身（ref 的本质是 reative（{ value ： obj}））。ref中的  .value 保存的才是当初传入的原始数据。

markRaw : 标识一个**对象**永远不被跟踪。再对它进行reactive的时候即使数据更新，也不会再更新视图了。

# 响应式计算和侦听计算值

​    computed方法接受 getter 函数并为 getter 返回的值返回一个**不可变**的响应式 [ref](https://vue3js.cn/docs/zh/guide/reactivity-fundamentals.html#创建独立的响应式值作为-refs) 对象。（不可变的意思是不能直接对计算出来的值修改，但是修改源属性可以改变他）。

​    如果需要改变computed属性的不可变性，可以给计算属性添加一个getter，创建一个可写的ref对象

```
const count = ref(1)
const plusOne = computed({  
    get: () => count.value + 1,  
    set: val => {   
    	count.value = val - 1
    }
})
```

## [watchEffect](https://vue3js.cn/docs/zh/guide/reactivity-computed-watchers.html#watcheffect)

传入一个函数响应式的追踪依赖项，依赖变更的时候会再次运行该函数。（类似watch）

### 停止侦听

watchEffect在setup或者生命周期钩子中调用，侦听器会连接到组件的生命周期，组件销毁自动停止。也可以显式调用返回值以停止侦听。

```
const stop = watchEffect(() => {}) 
stop() // 停止监听
```

### **清除副作用**

### 副作用刷新时机

### 侦听器调试

## [watch](https://vue3js.cn/docs/zh/guide/reactivity-computed-watchers.html#watch)

### 侦听单个数据源

### 侦听多个数据源

# **可复用&组合**

## **mixin混入**

混入 (mixin) 提供了一种非常灵活的方式，来分发 Vue 组件中的可复用功能。一个混入对象可以包含任意组件选项。

### 使用场景

1. 公共方法（如打开弹窗）
2. 常量集合（如权限码）

## [指令](https://cn.vuejs.org/v2/guide/custom-directive.html#%E7%AE%80%E4%BB%8B)

### [钩子函数](https://cn.vuejs.org/v2/guide/custom-directive.html#%E9%92%A9%E5%AD%90%E5%87%BD%E6%95%B0)

- `bind`：只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置。
- `inserted`：被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。
- `update`：所在组件的 VNode 更新时调用，**但是可能发生在其子 VNode 更新之前**。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新 (详细的钩子函数参数见下)。

### [钩子函数的参数](https://cn.vuejs.org/v2/guide/custom-directive.html#%E9%92%A9%E5%AD%90%E5%87%BD%E6%95%B0%E5%8F%82%E6%95%B0)

- `el`：指令所绑定的元素，可以用来直接操作 DOM。
- `binding`：一个对象，包含以下 property：
  - `name`：指令名，不包括 `v-` 前缀。
  - `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
  - `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
  - `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
  - `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
  - `modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }`。
- `vnode`：Vue 编译生成的虚拟节点。移步 [VNode API](https://cn.vuejs.org/v2/api/#VNode-接口) 来了解更多详情。
- `oldVnode`：上一个虚拟节点，仅在 `update` 和 `componentUpdated` 钩子中可用。

### 使用实例

```
// 注册一个全局自定义指令 `v-focus`
Vue.directive('focus', {
  // 当被绑定的元素插入到 DOM 中时……
  inserted: function (el) {
    // 聚焦元素
    el.focus()
  }
})

// 局部指令。组件中也接受一个 directives 的选项
directives: {
  focus: {
    // 指令的定义
    inserted: function (el) {
      el.focus()
    }
  }
}
```

### 动态指令参数

```
app.directive('pin', {  
    // 指令名
    mounted(el, binding) {  
    // el实例和 传入参数
    el.style.position = 'fixed'      
    // binding.arg 是参数名   
    const s = binding.arg || 'top'  
    // binding.value 是传入值
    el.style[s] = binding.value + 'px' 
    }
})
```



<p v-pin:[direction]="pinPadding"></p>

### **函数简写**

在mounted和updated的时候触发相同的行为，而不关心其他的钩子，可以采用函数简写

## Teleport

Teleport 提供了一种干净的方法，允许控制在哪一个父节点下呈现html，而不用求助于全局状态或者拆分两个组件。

```
<teleport to="body">
......
</teleport>
```

如果teleport包含Vue组件，那个Vue组件是 `<teleport> `父组件的逻辑子组件。

如果在同一个目标上使用多个teleport，那么多个teleport的内容会按顺序挂载到同一个目标元素中。

## [Suspense](https://juejin.cn/post/6854573214547312654)

vue3中自带的内置标签，类似keep-alive的用法

当要加载的组件不满足状态时,`Suspense` 将回退到 `fallback`状态一直到加载的组件满足条件，才会进行渲染。（<AsyncComponent/>组件需要返回一个Promise）

```
// #default插槽里面的内容就是你需要渲染的异步组件;#fallback就是你指定的加载中的静态组件。
  <Suspense>
      <template #default>
      	<AsyncComponent/>
      </template>
      <template #fallback>
      	<h1>Loading</h1>
      </template>
  </Suspense>
```

​
