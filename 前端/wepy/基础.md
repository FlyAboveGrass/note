# 基础



## wepy实例

### 程序和页面

每一个wepy页面有独立的wepypage实例，小程序本身也有 wepyApp 实例。而且他们并未继承自原生的page和 app。

WePY 提供 `wepy.app`，`wepy.page`，`wepy.component` 等入口 方法注册程序、页面、以及组件。注册后在组件的生命周期事件(onLaunch/onLoad/created)里，会自动创建相对应的 WePY 实例





### 入口

> WePY 2.0 中，Page 同样是使用小程序原生的 Component 方法进行注册的。

``` 
// 定义组件
<script>
import wepy from '@wepy/core'

wepy.component({
  // 选项
})
</script>

//另一个文件引入组件
<config>
{
  "usingComponents": {
    "comA": "components/comA"
  }
}
</config>
```

 

> 与 WePY 1 或者 Vue 不同的是，组件的引用方式保留了原生的 `usingComponents` 方式。不可以使用 `import` 的方式导入。



### 生命周期

WePY 单文件组件主要由 `<script>`，`<template>`，`<style>`，`<config>` 四部分组成（也包括小程序 `<wxs>` 标签）

![wepy生命周期](../../笔记图片/wepy生命周期.png)



#### app生命周期

[WePY 单文件组件主要由 `<script>`，`<template>`，`<style>`，`<config>` 四部分组成（也包括小程序 `<wxs>` 标签）](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html)



| 属性                                                         | 类型     | 默认值 | 必填 | 说明                                                         | 最低版本                                                     |
| :----------------------------------------------------------- | :------- | :----- | :--- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| [onLaunch](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onLaunch-Object-object) | function |        | 否   | 生命周期回调——监听小程序初始化。                             |                                                              |
| [onShow](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onShow-Object-object) | function |        | 否   | 生命周期回调——监听小程序启动或切前台。                       |                                                              |
| [onHide](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onHide) | function |        | 否   | 生命周期回调——监听小程序切后台。                             |                                                              |
| [onError](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onError-String-error) | function |        | 否   | 错误监听函数。                                               |                                                              |
| [onPageNotFound](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onPageNotFound-Object-object) | function |        | 否   | 页面不存在监听函数。                                         | [1.9.90](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [onUnhandledRejection](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onUnhandledRejection-Object-object) | function |        | 否   | 未处理的 Promise 拒绝事件监听函数。                          | [2.10.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| [onThemeChange](https://developers.weixin.qq.com/miniprogram/dev/reference/api/App.html#onThemeChange-Object-object) | function |        | 否   | 监听系统主题变化                                             | [2.11.0](https://developers.weixin.qq.com/miniprogram/dev/framework/compatibility.html) |
| 其他                                                         | any      |        | 否   | 开发者可以添加任意的函数或数据变量到 `Object` 参数中，用 `this` 可以访问 |                                                              |



#### 页面生命周期

`wepy.page` 本质上也是调用原生方法 `Component` 注册页面，因此它包含了 `Component` 的完整生命周期。同时，为了兼容对原有 `Page` 的使用习惯，也保留了所有 `Page` 特有的生命周期事件。

| 通过 Page({}) 注册页面 | 通过 wepy.page({}) 注册页面 | 说明                                                         |
| ---------------------- | --------------------------- | ------------------------------------------------------------ |
| onLoad                 | onLoad                      | 参看[官方文档 Page](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Page.html) |
| onShow                 | onShow                      | 同上                                                         |
| onReady                | onReady                     | 同上                                                         |
| onHide                 | onHide                      | 同上                                                         |
| onUnload               | onUnload                    | 同上                                                         |
| onPullDownRefresh      | onPullDownRefresh           | 同上                                                         |
| onReachBottom          | onReachBottom               | 同上                                                         |
| onShareAppMessage      | onShareAppMessage           | 同上                                                         |
| onPageScroll           | onPageScroll                | 同上                                                         |
| onResize               | onResize                    | 同上                                                         |
| onTabItemTap           | onTabItemTap                | 同上                                                         |
| onAddToFavorites       | onAddToFavorites            | 同上                                                         |
| -                      | created                     | 参看[官方文档 Component](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Component.html) |
| -                      | attached                    | 同上                                                         |
| -                      | ready                       | 同上                                                         |
| -                      | moved                       | 同上                                                         |
| -                      | detached                    | 同上                                                         |
| -                      | error                       | 同上                                                         |



#### 组件生命周期

| 通过 Component({}) 注册页面 | 通过 wepy.component({}) 注册页面 | 说明                                                         |
| --------------------------- | -------------------------------- | ------------------------------------------------------------ |
| -                           | beforeCreate                     | 本质上与wepy.created 一样都是在 Component.created 阶段触发，但 wepy.beforeCreate 在 Component.created 刚进入时触发，然后进行 WePY 的 data, props, methods 等等初始化，完成后再触发 wepy.created |
| created                     | created                          | 参看[官方文档 Component](https://developers.weixin.qq.com/miniprogram/dev/reference/api/Component.html) |
| attached                    | attached                         | 同上                                                         |
| ready                       | ready                            | 同上                                                         |
| moved                       | moved                            | 同上                                                         |
| detached                    | detached                         | 同上                                                         |
| error                       | error                            | 同上                                                         |





## [模板语法](https://wepyjs.gitee.io/wepy-docs/2.x/#/base/template)





## 计算属性和监听器

### [计算属性的 setter](https://wepyjs.gitee.io/wepy-docs/2.x/#/base/observe?id=计算属性的-setter)

计算属性默认只有 getter，不过在需要时你也可以提供一个 setter：

```vue
// ...
computed: {
  fullName: {
    // getter
    get: function () {
      return this.firstName + ' ' + this.lastName
    },
    // setter
    set: function (newValue) {
      var names = newValue.split(' ')
      this.firstName = names[0]
      this.lastName = names[names.length - 1]
    }
  }
}
```





## 条件渲染和列表渲染

### 条件渲染

> wepy推荐使用 v-if 和 v-show 控制条件渲染



**wepy支持的条件渲染**

```
<div wx:if="{{ condition3 }}"></div>
<div wx:elif="{{ condition4 }}"></div>
<div wx:else></div>

<div hidden="{{ condition }}"></div>
```





### 列表渲染

> wepy 推荐使用 v-for 进行列表渲染



```
<div v-for="(item, idx) in array">
  {{idx}}: {{itemName.message}}
</div>
```



## 事件处理

#### 原理

小程序原生的事件系统使用的是bind、catch 进行监听， wepy 采用的是 vue 的风格，用 v-on 和 @ 进行监听。



WePY 在编译过程中，会找到所有监听事件，并为其分配事件 ID。同时将事件代码（可以是一个方法，也可以是一个简单的代码片段）注入至 JS 代码中。 然后当事件分发器接收到原生事件时，会通过事件 ID，分发到相应的事件逻辑当中。

这样做的好处主要是：

1. 可控性更强。用户可利用相关钩子函数从而处理全局的用户事件。（典型场景：为页面全部按钮统一增加一个点击上报功能）
2. 灵活度更高。相对于原生只能使用函数名的方式来说，还可使用简单代码片段。（典型场景：@tap="num++"



![](../../笔记图片/wepy事件处理.png)





```
<template>
  <!-- 使用代码片段响应事件 -->
  <button @tap="num =+ 1">Counter - {{num}}</button> 

  <!-- 类原生方式，使用事件函数响应事件 -->
  <button @tap="handler">Handle my event</button>

  <!-- 类Vue方式，原生不支持携带参数 -->
  <button @tap="handlerWithArgs(1, 2)">Handle my event with arguments</button>
</template>
```



#### 特殊参数

小程序原生事件默认传递一个event 参数。

Wepy 的程序事件分发器在处理事件的时候与vue 类似， 它有一个 $event 属性， $event 属性是对event 进行了一层封装， 目的是毫无侵入的对齐 web event 标准。如果要使用原生事件参数，可以使用 $event.$wx .





#### 事件修饰符



小程序的事件系统中，可以使用 `bind`, `catch`, `capture-bind`, `capture-catch` 等来处理事件的冒泡与捕获。 

wepy 中和vue一样采用事件修饰符完成这一动作。

1. stop

2. capture

3. wxs

    标识wxs函数响应事件

    下边的`change:prop`（属性前面带 `change:` 前缀）是在 prop 属性被设置的时候触发 WXS 函数，值必须用{{}}括起来。在 `propValue` 值发生变化之后会触发。

    ```
    <wxs module="test" src="./test.wxs"></wxs>
    <div 
    change:prop="{{test.propObserver}}" 
    :prop="propValue" 
    @touchmove.wxs="test.touchmove" 
    class="movable"
    ></div>
    
    // 在test.wxs 中定义
    module.exports = {
      touchmove: function(event, instance) {
        console.log('log event', JSON.stringify(event))
      },
      propObserver: function(newValue, oldValue, ownerInstance, instance) {
        console.log('prop observer', newValue, oldValue)
      }
    }
    ```

    

4. mut

    标识互斥事件



## 表单

- input 和 textarea 元素使用 value 作为 prop ，并通过 input 事件触发 v-model 属性值的更新。
- picker，checkbox-group，radio-group 元素将使用 value 作为 prop ，并通过 change 事件触发 v-model 属性值的更新。
- switch 元素将使用 checked 作为 prop ，并通过 change 事件触发 v-model 属性值的更新。



> 小程序的checkbox目前不支持 `change` 事件, 请使用`checkbox-group`标签





# 组件



## 介绍

#### [路径规则](https://wepyjs.gitee.io/wepy-docs/2.x/#/component/intro?id=路径规则)

- 如果路径是绝对路径 (例如 `/components/button-counter`)，会原样保留
- 如果路径以`.`开头，将会被看作相对的模块依赖，并按照你的本地文件系统上的目录结构进行解析。
- 如果路径以`~`开头。如果你在[编译配置文件](https://wepyjs.gitee.io/wepy-docs/2.x/#/cli/config?id=resolve)中配置了 `alias`，这将变得很有用。所有 `@wepy/cli` 创建的项目都默认配置了将 `@` 指向 `/src`。

```vue
<config>
{
  "usingComponents": {
    "button-counter": "~@/components/button-counter"
  }
}
</config>
```

- 如果路径以 `module:` 开头。将会被看作模块依赖。这意味着你可以用该特性来引入第三方 NPM 组件：

```vue
<config>
{
  "usingComponents": {
    "some-3rd-party-component": "module:some-3rd-party-component"
  }
}
</config>
```

- 如果路径以 `plugins:` 开头。将会被看作小程序插件提供的自定义组件模块依赖。这意味着你可以使用该特性来引入小程序插件中的自定义组件：

```vue
<config>
{
  "usingComponents": {
    "3rd-plugin-component": "plugins://appid/3rd-plugin-component"
  }
}
</config>
```



## Prop



**在wepy中，当prop和properties同时设置时，prop将被忽略**



> 在 properties 定义段中，属性名采用驼峰写法（propertyName）；在 wxml 中，指定属性值时则对应使用连字符写法（component-tag-name property-name="attr value"），应用于数据绑定时采用驼峰写法（attr=""）



## 自定义事件



小程序原生中，使用 `bind:customevent` 进行事件绑定，使用 `wx.triggerEvent` 触发组件的自定义事件。 而在 WePY 中，还原了 Vue 的实用场景，使用 `v-on` 或 `@` 进行事件绑定，使用 `$emit` 触发组件自定义事件。





### 特殊参数

### [arguments](https://wepyjs.gitee.io/wepy-docs/2.x/#/component/event?id=arguments)

此参数用于获取子组件 $emit 时，所传递的参数。参数实际绑定在 `$event.$wx.$detail.arguments` 中，但你不需要关心。只需要通过函数参数获取即可。

### [$event](https://wepyjs.gitee.io/wepy-docs/2.x/#/component/event?id=event)

此参数用于获取子组件 $emit 所传递的第一个参数。此处是为了与 Vue 的形为保持一致，因 Vue 的自定义组件属于纯代码封装事件，不存在原生 Web 事件。因此，此时 **`$event 获取 $emit 第一个参数`。**

### [$wx](https://wepyjs.gitee.io/wepy-docs/2.x/#/component/event?id=wx)

此参数用于获取小程序原生 event。原生小程序在触发自定义事件时。会产生一个 event。



## 插槽

用法参考 [vue官方文档](https://cn.vuejs.org/v2/guide/components-slots.html)



支持的插槽

- 插槽内容
- 编译作用域
- 具名插槽



不支持的插槽

- 后备内容
- 作用域插槽
- 动态插槽名





# 可复用

## 混入 mixin





## Hook

























