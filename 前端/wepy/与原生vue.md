原生小程序的没有双向绑定，并且绑定方式只支持 {{}} 的双大括号形式，不支持v-bind / ： 的绑定方式。



# 与原生小程序对比



### 原生小程序

1. 组件文件，一个自定义组件由 `json` `wxml` `wxss` `js` 4个文件组成，四个文件必须同名。
2. 只能简单实现双向绑定，对象不支持。数据的跟踪依赖于[数据监听器](https://developers.weixin.qq.com/miniprogram/dev/framework/custom-component/observer.html)
3. 事件绑定使用 bind：tap 的形式，传递参数依赖于设置 data-数据 属性的形式 
4. 代码复用的形式是定义一个 behavior
5. 组件内容分发使用抽象节点
6. 发请求通过 wx.request 的 回调函数



### wepy

1. 单文件组件， 组件文件主要由 `<script>`，`<template>`，`<style>`，`<config>` 四部分组成， 并且用 <config> 标签代替原生小程序的 json 文件
2. wepy更贴近于 vue， 有数据双向绑定的特性。（1.x版本用 .async / twoWay ； 2.x 版本直接使用 v-model ）
3. 事件绑定融合了vue和原生的特性， 可以使用 @tap 和 v-on：tap 的形式， 并且可以使用事件修饰符。
4. 代码复用沿用 vue 中的 mixin 混入
5. 组件内容分发使用 slot 插槽
6. 支持 promise 和 async/ await 异步编程
7. WePY中的组件都是静态组件，是以组件ID作为唯一标识的，每一个ID都对应一个组件实例，当页面引入两个相同ID的组件时，这两个组件共用同一个实例与数据，当其中一个组件数据变化时，另外一个也会一起变化。



# [wepy 1.0 vs 2.0](https://github.com/Bulandent/blog/issues/4)



### 代码结构

#### config

2.0 中将1.0里面放在script 里面的 config 提出来专门作为一个标签 <config> 



#### script

由类结构变成了对象结构

**1.0**

```
export default class HOME extends wepy.page {
    data = {}
    methods = {}
    onLoad() {}
    onShow() {}
}
```

**2.0**

```
export default class HOME extends wepy.page {
    data = {}
    methods = {}
    onLoad() {}
    onShow() {}
}
```

#### 函数定义

**自定义方法和组件事件处理函数需要移到 methods 里， 生命周期函数依然放在外面与 methods 同级**

- 生命周期函数，比如 `onLoad`、`onShow` 等；
- `wxml` 事件处理函数，即在 `wxml` 中绑定的事件，这类函数需要定义在 `methods`，比如：`bindtap`、`bindchange` 等；
- 组件间事件处理函数，响应组件之间通过 `$broadcast`、`$emit`、`$invoke` 所传递的事件函数，这类函数需要定义在 `events` 对象里；
- 自定义函数，即用于被其他函数直接调用的函数，需要定义在和 `methods` 同级的位置。



### 组件注册方式调整

**1.0**

```
export default class APP extends wepy.app {}  // 注册程序
export default class HOME extends wepy.page {}  // 注册页面
export default class LIST extends wepy.component {}  // 注册组件
```



**2.0**

```
wepy.app({})  // 注册程序
wepy.page({})  // 注册页面
wepy.component({})  // 注册组件
```



### 组件引入方式

`2.x` 版本中组件引入不再通过 `import` 进行导入，而是直接定义在页面的配置 `<config>` 中。



### 生命周期

生命周期基本和1.0 保持一致

组件中的生命周期 onLoad 改为 ready

| 级别      | 1.7.2      | 2.x       |
| --------- | ---------- | --------- |
| app       | onLaunch   | onLaunch  |
| app       | onShow     | onShow    |
| page      | onLoad     | onLoad    |
| page      | onShow     | onShow    |
| page      | onReady    | onReady   |
| component | -          | created   |
| component | -          | attached  |
| component | **onLoad** | **ready** |



### 不再支持拦截器interceptor





### 模版语法修改

```
<template>
    <view>
        <!-- 属性绑定 -->
        <view id="{{ id }}"></view>
        
        <!-- 数据绑定 -->
        <view>{{ name }}</view>
        
        <!-- 事件绑定 -->
        <view bindtap="change({{ index }})"></view>
        
        <!-- class绑定 -->
        <view class="change {{hasData ? 'has-data' : '' }}"></view>
        
        <!-- style绑定 -->
        <view style="{{ 'color:' + color + ';' + 'font-size:' + fontSize + ';' }}"></view>
        
        <!-- 条件判断 -->
        <view wx:if="{{ flag1 }}"></view>
        <view wx:elif="{{ flag2 }}"></view>
        <view wx:else></view>
        
        <!-- 显示判断 -->
        <view hidden="{{ !isShow }}"></view>

        <!-- 列表渲染，默认是：item、index -->
        <view wx:for="{{ array }}" wx:for-index="idx" wx:for-item="itemName"></view>
    </view>
</template>
```

**2.0**

```
<template>
    <view>
        <!-- 属性绑定 -->
        <view :id="id"></view>
        
        <!-- 数据绑定 -->
        <view>{{ name }}</view>
        
        <!-- 事件绑定 -->
        <view @tap="change( index )"></view>
        
        <!-- class绑定 -->
        <view class="change" :class="{ 'has-data': hasData }"></view>
        
        <!-- style绑定 -->
        <view :style="{'color': color, 'font-size': fontSize }"></view>
        
        <!-- 条件判断 -->
        <view v-if="flag1"></view>
        <view v-else-if="flag2"></view>
        <view v-else></view>
        
        <!-- 显示判断 -->
        <view v-show="isShow"></view>

        <!-- 列表渲染，默认是：item、index -->
        <view v-for="(item, index) in array" :key="index"></view>
    </view>
</template>
```





### 双向绑定方式与vue一致



### 组件通信不再支持 $broadcast

在 `2.x` 中不再支持父级给子组件进行事件广播了，而是可以通过给子组件加上 `ref` 属性后，通过 `this.$refs` 来直接操作子组件函数来达成通信的目的















