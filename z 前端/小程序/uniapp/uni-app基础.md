## uni-app 基础

easyCom

条件编译

自定义组件

### 生命周期

#### [应用生命周期](https://uniapp.dcloud.io/collocation/frame/lifecycle?id=%e5%ba%94%e7%94%a8%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f)

- 应用生命周期仅可在`App.vue`中监听，在其它页面监听无效。

| 函数名               | 说明                                                         |
| :------------------- | :----------------------------------------------------------- |
| onLaunch             | 当`uni-app` 初始化完成时触发（全局只触发一次）               |
| onShow               | 当 `uni-app` 启动，或从后台进入前台显示                      |
| onHide               | 当 `uni-app` 从前台进入后台                                  |
| onError              | 当 `uni-app` 报错时触发                                      |
| onUniNViewMessage    | 对 `nvue` 页面发送的数据进行监听，可参考 [nvue 向 vue 通讯](https://uniapp.dcloud.io/nvue-api?id=communication) |
| onUnhandledRejection | 对未处理的 Promise 拒绝事件监听函数（2.8.1+）                |
| onPageNotFound       | 页面不存在监听函数                                           |
| onThemeChange        | 监听系统主题变化                                             |

#### [页面生命周期](https://uniapp.dcloud.io/collocation/frame/lifecycle?id=%e9%a1%b5%e9%9d%a2%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f)

| 函数名            | 说明                                                         | 平台差异说明                                                 | 最低版本 |
| :---------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :------- |
| onInit            | 监听页面初始化，其参数同 onLoad 参数，为上个页面传递的数据，参数类型为 Object（用于页面传参），触发时机早于 onLoad | 百度小程序                                                   | 3.1.0+   |
| onLoad            | 监听页面加载，其参数为上个页面传递的数据，参数类型为 Object（用于页面传参），参考[示例](https://uniapp.dcloud.io/api/router?id=navigateto) |                                                              |          |
| onShow            | 监听页面显示。页面每次出现在屏幕上都触发，包括从下级页面点返回露出当前页面 |                                                              |          |
| onReady           | 监听页面初次渲染完成。注意如果渲染速度快，会在页面进入动画完成前触发 |                                                              |          |
| onHide            | 监听页面隐藏                                                 |                                                              |          |
| onUnload          | 监听页面卸载                                                 |                                                              |          |
| onResize          | 监听窗口尺寸变化                                             | App、微信小程序                                              |          |
| onPullDownRefresh | 监听用户下拉动作，一般用于下拉刷新，参考[示例](https://uniapp.dcloud.io/api/ui/pulldown) |                                                              |          |
| onReachBottom     | 页面滚动到底部的事件（不是scroll-view滚到底），常用于下拉下一页数据。具体见下方注意事项 |                                                              |          |
| onTabItemTap      | 点击 tab 时触发，参数为Object，具体见下方注意事项            | 微信小程序、支付宝小程序、百度小程序、H5、App（自定义组件模式） |          |
| onShareAppMessage | 用户点击右上角分享                                           |                                                              |          |
|                   |                                                              |                                                              |          |

#### [组件生命周期](https://uniapp.dcloud.io/collocation/frame/lifecycle?id=%e7%bb%84%e4%bb%b6%e7%94%9f%e5%91%bd%e5%91%a8%e6%9c%9f)

| 函数名        | 说明                                                         | 平台差异说明 | 最低版本 |
| :------------ | :----------------------------------------------------------- | :----------- | :------- |
| beforeCreate  | 在实例初始化之后被调用。[详见](https://cn.vuejs.org/v2/api/#beforeCreate) |              |          |
| created       | 在实例创建完成后被立即调用。[详见](https://cn.vuejs.org/v2/api/#created) |              |          |
| beforeMount   | 在挂载开始之前被调用。[详见](https://cn.vuejs.org/v2/api/#beforeMount) |              |          |
| mounted       | 挂载到实例上去之后调用。[详见](https://cn.vuejs.org/v2/api/#mounted) 注意：此处并不能确定子组件被全部挂载，如果需要子组件完全挂载之后在执行操作可以使用`$nextTick`[Vue官方文档](https://cn.vuejs.org/v2/api/#Vue-nextTick) |              |          |
| beforeUpdate  | 数据更新时调用，发生在虚拟 DOM 打补丁之前。[详见](https://cn.vuejs.org/v2/api/#beforeUpdate) | 仅H5平台支持 |          |
| updated       | 由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。[详见](https://cn.vuejs.org/v2/api/#updated) | 仅H5平台支持 |          |
| beforeDestroy | 实例销毁之前调用。在这一步，实例仍然完全可用。[详见](https://cn.vuejs.org/v2/api/#beforeDestroy) |              |          |
| destroyed     | Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。[详见](https://cn.vuejs.org/v2/api/#destroyed) |              |          |

uniCloud



