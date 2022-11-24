​	

# 基本概念



## **vite 是什么**

Vite是一种新型前端构建工具，能够显著提升前端开发体验。它主要由两部分组成：

- 一个开发服务器，它基于 [原生 ES 模块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) 提供了 [丰富的内建功能](https://cn.vitejs.dev/guide/features.html)，如速度快到惊人的模块热更新（HMR）。
- 一套构建指令，它使用 [Rollup](https://rollupjs.org/) 打包你的代码，并且它是预配置的，可输出用于生产环境的高度优化过的静态资源。



## 为什么要用 vite





### **[Bundle vs Bundleless](https://juejin.cn/post/6937223515288371214)**

Bundle和Bundleless，这是两种不同的开发方式，在过去我们一般都会使用Webpack这个打包构建工具来打包构建我们的代码。



**为什么要打包成 bundle**

- HTTP/1.1 各浏览器有并行连接限制
- 浏览器不支持模块系统（如 CommonJS 包不能直接在浏览器运行）
- 代码依赖关系与顺序管理

​	

**为什么开始尝试不打包**

- 随着项目不断变大，启动和热更新所等待的时间越来越长
- HTTP/2.0 多路并用
- 各大浏览器逐一支持 ESM
- 原生的解决了代码依赖和复用的问题
- 越来越多的 npm 包拥抱 ESM（尽管很多包的依赖并不是）

|          | Bundle（Webpack）               | Bundleless(Vite/Snowpack)      |
| -------- | ------------------------------- | ------------------------------ |
| 启动时间 | 长，完成打包项目                | 短，只启动Server 按需加载      |
| 构建时间 | 随项目体积线性增长              | 构建时间复杂度O(1)             |
| 加载性能 | 打包后加载对应Bundle            | 请求映射至本地文件             |
| 缓存能力 | 缓存利用率一般，受split方式影响 | 缓存利用率近乎完美             |
| 文件更新 | 重新打包                        | 重新请求单个文件               |
| 调试体验 | 通常需要SourceMap进行调试       | 不强依赖SourceMap,可单文件调试 |
| 生态     | 非常完善                        | 目前先对不成熟，但是发展很快   |



### server 快速启动

冷启动开发服务器时，基于打包器的方式启动必须优先抓取并构建你的整个应用。



Vite 通过在一开始将应用中的模块区分为 **依赖** 和 **源码** 两类，改进了开发服务器启动时间。

- **依赖** 大多为在开发时不会变动的纯 JavaScript。一些较大的依赖（例如有上百个模块的组件库）处理的代价也很高。依赖也通常会存在多种模块化格式（例如 ESM 或者 CommonJS）。

    Vite 将会使用 [esbuild](https://esbuild.github.io/) [预构建依赖](https://cn.vitejs.dev/guide/dep-pre-bundling.html)。esbuild 使用 Go 编写，并且比以 JavaScript 编写的打包器预构建依赖快 10-100 倍。

- **源码** Vite 以 [原生 ESM](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) 方式提供源码。这实际上是让浏览器接管了打包程序的部分工作：Vite 只需要在浏览器请求源码时进行转换并按需提供源码。



### 极速的热更新

在 Vite 中，HMR 是在原生 ESM 上执行的。当编辑一个文件时，Vite 只需要精确地使已编辑的模块与其最近的 HMR 边界之间的链失活（大多数时候只是模块本身），使得无论应用大小如何，HMR 始终能保持快速更新。

Vite 同时利用 HTTP 头来加速整个页面的重新加载（再次让浏览器为我们做更多事情）：源码模块的请求会根据 `304 Not Modified` 进行协商缓存，而依赖模块请求则会通过 `Cache-Control: max-age=31536000,immutable` 进行强缓存，因此一旦被缓存它们将不需要再次请求。







# vite 原理



vite 分为开发模式和生产模式。

**开发模式**

Vite提供了一个开发服务器，然后结合原生的[ESM](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fen-US%2Fdocs%2FWeb%2FJavaScript%2FReference%2FStatements%2Fimport)，当代码中出现import的时候，发送一个资源请求，Vite开发服务器拦截请求，根据不同文件类型，在服务端完成模块的改写（比如单文件的解析编译等）和请求处理，实现真正的按需编译，然后返回给浏览器。请求的资源在服务器端按需编译返回，完全跳过了打包这个概念，不需要生成一个大的bundle。



Vite Server 所有逻辑基本都依赖中间件实现。这些中间件，拦截请求之后，完成了如下内容：

- 处理 ESM 语法，比如将业务代码中的 import 第三方依赖路径转为浏览器可识别的依赖路径；
- 对 .ts、.vue 等文件进行即时编译；
- 对 Sass/Less 的需要预编译的模块进行编译；
- 和浏览器端建立 socket 连接，实现 HMR。



**生产模式：** 

利用Rollup来构建源代码。



## 为什么生产环境仍需打包

嵌套导入会导致额外的网络往返，在生产环境中发布未打包的 ESM 仍然效率低下（即使使用 HTTP/2）。为了在生产环境中获得最佳的加载性能，最好还是将代码进行 tree-shaking、懒加载和 chunk 分割（以获得更好的缓存）。

## 为何不用 ESBuild 打包？

虽然 `esbuild` 快得惊人，并且已经是一个在构建库方面比较出色的工具，但一些针对构建 *应用* 的重要功能仍然还在持续开发中 —— 特别是代码分割和 CSS 处理方面。就目前来说，Rollup 在应用打包方面更加成熟和灵活。尽管如此，当未来这些功能稳定后，我们也不排除使用 `esbuild` 作为生产构建器的可能。





# 功能特性



## NPM 依赖解析和预构建

原生 ES 导入不支持下面这样的裸模块导入：

```
import { someMethod } from 'my-dep'
```

上面的代码会在浏览器中抛出一个错误。Vite 将会检测到所有被加载的源文件中的此类裸模块导入，并执行以下操作:

1. [预构建](https://cn.vitejs.dev/guide/dep-pre-bundling.html) 它们可以提高页面加载速度，并将 CommonJS / UMD 转换为 ESM 格式。预构建这一步由 [esbuild](http://esbuild.github.io/) 执行，这使得 Vite 的冷启动时间比任何基于 JavaScript 的打包器都要快得多。
2. 重写导入为合法的 URL，例如 `/node_modules/.vite/my-dep.js?v=f3sf2ebd` 以便浏览器能够正确导入它们。



### 预构建

1. **CommonJS 和 UMD 兼容性:** 

    开发阶段中，Vite 的开发服务器将所有代码视为原生 ES 模块。因此，Vite 必须先将作为 CommonJS 或 UMD 发布的依赖项转换为 ESM。

    当转换 CommonJS 依赖时，Vite 会执行智能导入分析，这样即使导出是动态分配的（如 React），按名导入也会符合预期效果：

    ```
    // 符合预期
    import React, { useState } from 'react'
    ```

2. **性能：**

     Vite 将有许多内部模块的 ESM 依赖关系转换为单个模块，以提高后续页面加载性能。

    

    一些包将它们的 ES 模块构建作为许多单独的文件相互导入。例如，[`lodash-es` 有超过 600 个内置模块](https://unpkg.com/browse/lodash-es/)！当我们执行 `import { debounce } from 'lodash-es'` 时，浏览器同时发出 600 多个 HTTP 请求！尽管服务器在处理这些请求时没有问题，但大量的请求会在浏览器端造成网络拥塞，导致页面的加载速度相当慢。通过预构建 `lodash-es` 成为一个模块，我们就只需要一个 HTTP 请求了！



**自动依赖搜寻**

如果没有找到相应的缓存，Vite 将抓取你的源码，并自动寻找引入的依赖项（即 "bare import"，表示期望从 `node_modules` 解析），并将这些依赖项作为预构建包的入口点。预构建通过 `esbuild` 执行，所以它通常非常快。



### 缓存

**文件系统缓存**

Vite 会将预构建的依赖缓存到 `node_modules/.vite`。它根据几个源来决定是否需要重新运行预构建步骤:

- `package.json` 中的 `dependencies` 列表
- 包管理器的 lockfile，例如 `package-lock.json`, `yarn.lock`，或者 `pnpm-lock.yaml`
- 可能在 `vite.config.js` 相关字段中配置过的

只有在上述其中一项发生更改时，才需要重新运行预构建。

**浏览器缓存**

解析后的依赖请求会以 HTTP 头 `max-age=31536000,immutable` 强缓存，以提高在开发时的页面重载性能。一旦被缓存，这些请求将永远不会再到达开发服务器。



## 模块热加载





## 构建优化



### css 代码分割

Vite 会自动地将一个异步 chunk 模块中使用到的 CSS 代码抽取出来并为其生成一个单独的文件。这个 CSS 文件将在该异步 chunk 加载完成时自动通过一个 `<link>` 标签载入，该异步 chunk 会保证只在 CSS 加载完毕后再执行，避免发生 [FOUC](https://en.wikipedia.org/wiki/Flash_of_unstyled_content#:~:text=A flash of unstyled content,before all information is retrieved.) 。



### 预加载指令生成

Vite 会为入口 chunk 和它们在打包出的 HTML 中的直接引入自动生成 `<link rel="modulepreload">` 指令。



### [异步chunk 加载优化](https://cn.vitejs.dev/guide/features.html#async-chunk-loading-optimization)







## TS 支持





## Vue 支持







## Jsx 支持







### 



