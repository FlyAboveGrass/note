> 参考链接：
>
> [[源码解读\] Webpack 插件架构深度讲解 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/367931462)
>
> [[万字总结\] 一文吃透 Webpack 核心原理 - 掘金 (juejin.cn)](https://juejin.cn/post/6949040393165996040#heading-18)



## Plugin



### 什么是 Plugin

插件通常是一个带有 `apply` 函数的类。`apply` 函数运行时会得到参数 `compiler` ，以此为起点可以调用 `hook` 对象注册各种钩子回调，webpack 的插件架构基于这种模式构建而成，插件开发者可以使用这种模式在钩子回调中，插入特定代码。



webpack 的插件体系是一种基于 [Tapable](https://link.zhihu.com/?target=https://github.com/webpack/tapable) 实现的**强耦合**架构，它在特定时机触发钩子时会附带上足够的上下文信息，插件定义的钩子回调中，能也只能与这些上下文背后的数据结构、接口交互产生 side effect，进而影响到编译状态和后续流程。



钩子的核心逻辑定义在 [Tapable](https://link.juejin.cn/?target=https://github.com/webpack/tapable) 仓库，内部定义了如下类型的钩子：

```JavaScript
const {
        SyncHook,
        SyncBailHook,
        SyncWaterfallHook,
        SyncLoopHook,
        AsyncParallelHook,
        AsyncParallelBailHook,
        AsyncSeriesHook,
        AsyncSeriesBailHook,
        AsyncSeriesWaterfallHook
 } = require("tapable");
```



不同类型的钩子根据其并行度、熔断方式、同步异步，调用方式会略有不同，插件开发者需要根据这些的特性，编写不同的交互逻辑





### 如何使用 plugin

Tapable 使用时通常需要经历如下步骤：

- 创建钩子实例

- 调用订阅接口注册回调，包括：`tap、tapAsync、tapPromise`

- 调用发布接口触发回调，包括：`call、callAsync、promise`



#### Tapable 钩子类型

Tabable 提供如下类型的钩子(统计数据来自 webpack@5.37.0)：

|                          | 简介               | 统计                                                         |
| ------------------------ | ------------------ | ------------------------------------------------------------ |
| SyncHook                 | 同步钩子           | Webpack 共出现 86 次，如 Compiler.hooks.compilation          |
| SyncBailHook             | 同步熔断钩子       | Webpack 共出现 90 次，如 Compiler.hooks.shouldEmit           |
| SyncWaterfallHook        | 同步瀑布流钩子     | Webpack 共出现 26 次，如 Compilation.hooks.assetPath         |
| SyncLoopHook             | 同步循环钩子       | Webpack 中未使用                                             |
| AsyncParallelHook        | 异步并行钩子       | Webpack 仅出现 6 次：Compiler.hooks.make                     |
| AsyncParallelBailHook    | 异步并行熔断钩子   | Webpack 中未使用                                             |
| AsyncSeriesHook          | 异步串行钩子       | Webpack 共出现 32 次，如 Compiler.hooks.done                 |
| AsyncSeriesBailHook      | 异步串行熔断钩子   | Webpack 共出现 9 次，如 Compilation.hooks.optimizeChunkModules |
| AsyncSeriesLoopHook      | 异步串行循环钩子   | Webpack 中未使用                                             |
| AsyncSeriesWaterfallHook | 异步串行瀑布流钩子 | Webpack 共出现 3 次，如 ContextModuleFactory.hooks.beforeResolve |



##### 钩子分类依据两条规则：

- 按回调逻辑，分为：
    - 基本类型，名称不带 `Waterfall/Bail/Loop` 关键字，与通常 **「订阅/回调」** 模式相似，按钩子注册顺序，逐次调用回调

- `waterfall` 类型：前一个回调的返回值会被带入下一个回调

- `bail` 类型：逐次调用回调，若有任何一个回调返回非 `undefined` 值，则终止后续调用

- `loop` 类型：逐次、循环调用，直到所有回调函数都返回 `undefined`

- 按执行回调的并行方式，分为：
    - `sync` ：同步执行，启动后会按次序逐个执行回调，支持 `call/tap` 调用语句

- `async` ：异步执行，支持传入 `callback` 或 `promise` 风格的异步回调函数，支持 `callAsync/tapAsync` 、`promise/tapPromise` 两种调用语句



### 什么时候触发钩子



常用的钩子示例：

- compiler.hooks.compilation：
    - 时机：启动编译创建出 compilation 对象后触发

- 参数：当前编译的 compilation 对象

- compiler.hooks.make：
    - 时机：正式开始编译时触发

- 参数：同样是当前编译的 `compilation` 对象

- compilation.hooks.optimizeChunks：
    - 时机： `seal` 函数中，`chunk` 集合构建完毕后触发

- 参数：`chunks` 集合与 `chunkGroups` 集合

- compiler.hooks.done：
    - 时机：编译完成后触发

- 参数： `stats` 对象，包含编译过程中的各类统计信息



对于钩子，我们需要重点去关注它的触发时机和参数。



#### [触发时机](https://juejin.cn/post/6949040393165996040#heading-16)



##### `compiler` 对象逐次触发如下钩子



![img](https://supermonkey.feishu.cn/space/api/box/stream/download/asynccode/?code=MjNmZjczYTRkNTdjNDkwMGRkMTBmYTViYTk0MzllZjdfQ29Gcm12alNpUFJnNEV1TEJQOTdHVGFCWGs5N1JrRXBfVG9rZW46Ym94Y25aTjNsR3NxdlNPTXpRS3A0ak55bWFkXzE2NDQ5MDU1MjI6MTY0NDkwOTEyMl9WNA)



##### `compilation` 对象逐次触发如下钩子

![img](https://supermonkey.feishu.cn/space/api/box/stream/download/asynccode/?code=NTNlYWRiMjg5OTFmMjQ3NzQ1YjY3MzFhZmU4YWMyOTJfbFZ0R3lycXU5T3JQNHdicnh4cW43UXB4MG14QjRlSmpfVG9rZW46Ym94Y25oZ1JDVnMxV0RqUjE3WUVMZjZvenFjXzE2NDQ5MDU1MjI6MTY0NDkwOTEyMl9WNA)







### 如何影响编译状态

hooks 回调由 webpack 决定何时，以何种方式执行；webpack 会将上下文信息以参数或 `this` (compiler 对象) 形式传递给钩子回调，在回调中可以调用上下文对象的方法或者直接修改上下文对象属性的方式，对原定的流程产生 **side effect**。



例如：

- `compilation.addModule`：添加模块，可以在原有的 `module` 构建规则之外，添加自定义模块

- `compilation.emitAsset`：直译是“提交资产”，功能可以理解将内容写入到特定路径

- `compilation.addEntry`：添加入口，功能上与直接定义 `entry` 配置相同

- `module.addError`：添加编译错误信息

- ……







### [如何编写 Plugin](https://webpack.wuhaolin.cn/5原理/5-4编写Plugin.html)



`webpack` 插件由以下组成：

- 确定 Plugin 的形式
    - 函数形式。 定义一个 JavaScript 命名函数。在插件函数的 prototype 上定义一个 `apply` 方法。

- 类形式。 定义一个 Plugin 的类，定义 constructor（获取参数） 和 apply 方法。

- 指定一个绑定到 webpack 自身的[事件钩子](https://www.webpackjs.com/api/compiler-hooks/)。

- 处理 webpack 内部实例的特定数据。

- 功能完成后调用 webpack 提供的回调。

```JavaScript
class Plugin {
  apply(compiler) {
    compiler.plugin('emit', function (compilation, callback) {
      // compilation.chunks 存放所有代码块，是一个数组
      compilation.chunks.forEach(function (chunk) {
        // chunk 代表一个代码块。代码块由多个模块组成，通过 chunk.forEachModule 能读取组成代码块的每个模块
        chunk.forEachModule(function (module) {
          // module 代表一个模块。module.fileDependencies 存放当前模块的所有依赖的文件路径，是一个数组
          module.fileDependencies.forEach(function (filepath) {
          });
        });



        // Webpack 会根据 Chunk 去生成输出的文件资源，每个 Chunk 都对应一个及其以上的输出文件
        // 例如在 Chunk 中包含了 CSS 模块并且使用了 ExtractTextPlugin 时，
        // 该 Chunk 就会生成 .js 和 .css 两个文件
        chunk.files.forEach(function (filename) {
          // compilation.assets 存放当前所有即将输出的资源
          // 调用一个输出资源的 source() 方法能获取到输出资源的内容
          let source = compilation.assets[filename].source();
        });
      });



      // 这是一个异步事件，要记得调用 callback 通知 Webpack 本次事件监听处理结束。
      // 如果忘记了调用 callback，Webpack 将一直卡在这里而不会往后执行。
      callback();
    })
  }
}
```







## [Loader](https://www.webpackjs.com/contribute/writing-a-loader/)

`webpack` 只认识 `JavaScript` 这们语言，对于其他的资源通过 `loader` 后可以转化做预处理。

**runLoaders** 会调用用户所配置的 loader 集合读取、转译资源，此前的内容可以千奇百怪，但转译之后理论上应该输出标准 JavaScript 文本或者 AST 对象，webpack 才能继续处理模块依赖。



### Loader 的特性



**loader特点**

- loader的执行顺序和代码书写的顺序是相反的，即：最后一个loader最先执行，第一个loader最后执行

- 第一个执行的loader会接收源文件做为参数，下一次执行的loader会接收前一个loader执行的返回值做为参数



**loader 编写原则**

- **简单易用**。loaders 应该只做单一任务

- 使用**链式**传递。Webpack 会按顺序链式调用每个 Loader

- **模块化**的输出。

- 确保**无状态**。输入与输出均为字符串，各个 Loader 完全独立，即插即用

- 使用 **loader utilities**。

- 记录 **loader 的依赖**。

- 解析**模块依赖关系**。

- 提取**通用代码**。

- 避免**绝对路径**。

- 使用 **peer dependencies**。





### 如何编写 Loader

```JavaScript
// ./loader/replaceLoader.js
// 最简单的 loader
module.exports = function(source) {
    return source.replace('a', 'b')
}


// 使用 options 对象或者 loader-utils 传入参数
const loaderUtil = require('loader-utils')
module.exports = function(source) {
  console.log(this.query.name) // 林一一
  const options = loaderUtil.getOptions(this)
  return source.replace('a', 'b')
}





// 异步的 Loader
const loaderUtils = require('loader-utils')
module.exports = function(source) {
  const options = loaderUtils.getOptions(this)
  const callback = this.async()
  setTimeout(()=>{
      console.log(options.name)
      let res = source.replace('a', options.name)
      callback(null, res, sourceMaps, ast)
  }, 4000)
}
```

#### 如何使用

```JavaScript
module.exports = {
    module:{
        rules:[{
            test:"/\.js$/",
            use:[{
                    loader: path.resolve(__dirname, './loader/replaceLoader.js')
                    options:{
                        name: '你好呀'   
                    }
                }
            ]
        }]
    }
}
```