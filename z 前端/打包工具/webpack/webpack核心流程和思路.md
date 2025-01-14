> 参考链接
>
> [Webpack 5 知识体系 - GitMind](https://gitmind.cn/app/doc/fac1c196e29b8f9052239f16cff7d4c7)
>
> [[万字总结] 一文吃透 Webpack 核心原理 - 掘金 (juejin.cn)](https://juejin.cn/post/6949040393165996040#heading-1)
>
> [Introduction  | Web Fundamentals  | Google Developers](https://developers.google.com/web/fundamentals/performance/webpack/)

# 核心流程



Webpack 过程核心完成了 **内容转换 + 资源合并** 两种功能，实现上主要包含三个阶段：

1. 初始化阶段：
    1. **初始化参数**：从配置文件、 配置对象、Shell 参数中读取，与默认配置结合得出最终的参数
    2. **创建编译器对象**：用上一步得到的参数创建 `Compiler` 对象
    3. **初始化编译环境**：包括注入内置插件、注册各种模块工厂、初始化 RuleSet 集合、加载配置的插件等
    4. **开始编译**：执行 `compiler` 对象的 `run` 方法
    5. **确定入口**：根据配置中的 `entry` 找出所有的入口文件，调用 `compilition.addEntry` 将入口文件转换为 `dependence` 对象
2. 构建阶段：
    1. **编译模块(make)**：根据 `entry` 对应的 `dependence` 创建 `module` 对象，调用 `loader` 将模块转译为标准 JS 内容，调用 JS 解释器将内容转换为 AST 对象，从中找出该模块依赖的模块，再 递归 本步骤直到所有入口依赖的文件都经过了本步骤的处理
    2. **完成模块编译**：上一步递归处理所有能触达到的模块后，得到了每个模块被翻译后的内容以及它们之间的 **依赖关系图**
3. 生成阶段：
    1. **输出资源(seal)**：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 `Chunk`，再把每个 `Chunk` 转换成一个单独的文件加入到输出列表，**这步是可以修改输出内容的最后机会**
    2. **写入文件系统(emitAssets)**：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统









## **构建**



构建阶段从 `entry` 开始递归解析资源与资源的依赖，在 `compilation` 对象内逐步构建出 `module` 集合以及 `module` 之间的依赖关系

![[Pasted image 20250113165931.png]]




1. 调用 `handleModuleCreate` ，根据文件类型构建 `module` 子类
2. 调用 [loader-runner](https://link.juejin.cn/?target=https://www.npmjs.com/package/loader-runner) 仓库的 `runLoaders` 转译 `module` 内容，通常是从各类资源类型转译为 JavaScript 文本
3. 调用 [acorn](https://link.juejin.cn/?target=https://www.npmjs.com/package/acorn) 将 JS 文本解析为AST
4. 遍历 AST，触发各种钩子
    1. 在 `HarmonyExportDependencyParserPlugin` 插件监听 `exportImportSpecifier` 钩子，解读 JS 文本对应的资源依赖
    2. 调用 `module` 对象的 `addDependency` 将依赖对象加入到 `module` 依赖列表中
5. AST 遍历完毕后，调用 `module.handleParseResult` 处理模块依赖
6. 对于 `module` 新增的依赖，调用 `handleModuleCreate` ，控制流回到第一步
7. 所有依赖都解析完毕后，构建阶段结束





## **生成**

生成阶段的流程：

1. 构建本次编译的 `ChunkGraph` 对象；
2. 遍历 `compilation.modules` 集合，将 `module` 按 `entry/动态引入` 的规则分配给不同的 `Chunk` 对象；
3. `compilation.modules` 集合遍历完毕后，得到完整的 `chunks` 集合对象，调用 `createXxxAssets` 方法
4. `createXxxAssets` 遍历 `module/chunk` ，调用 `compilation.emitAssets` 方法将资 `assets` 信息记录到 `compilation.assets` 对象中
5. 触发 `seal` 回调，控制流回到 `compiler` 对象



这一步的关键逻辑是将 `module` 按规则组织成 `chunks` ，webpack 内置的 `chunk` 封装规则比较简单：

- `entry` **及 entry 触达到的模块，组合成一个** `chunk`
- **使用动态引入语句引入的模块，各自组合成一个** `chunk`



# [优化思路](https://developers.google.com/web/fundamentals/performance/webpack)


## 体积优化

### Tree Shaking:

确保使用 ES6 模块语法 (import 和 export) 以便 Webpack 能够进行 tree shaking，移除未使用的代码。


### Code Splitting

- 减少初始加载时间：只加载用户当前需要的代码。
- 提高应用性能：按需加载减少了不必要的资源消耗。
- 更好的用户体验：通过懒加载，用户可以更快地访问应用的关键功能。

#### 如何实现 Code Splitting

1. 动态导入

使用动态 import() 语法可以实现按需加载模块。这种方式适用于需要在运行时加载的模块。
```
// 使用动态 import
function loadComponent() {
  return import('./SomeComponent')
    .then(module => {
      const SomeComponent = module.default;
      // 使用 SomeComponent
    })
    .catch(error => {
      console.error('Error loading component:', error);
    });
}
```

2. SplitChunksPlugin

Webpack 的 SplitChunksPlugin 可以自动将公共模块提取到单独的文件中。通过配置这个插件，可以更好地管理代码分割。

```
// webpack.config.js
module.exports = {
  // ...
  optimization: {
    splitChunks: {
      chunks: 'all', // 对所有类型的 chunks 进行分割
      minSize: 20000, // 生成 chunk 的最小大小
      maxSize: 0, // 不限制 chunk 的最大大小
      minChunks: 1, // 最小共享次数
      maxAsyncRequests: 30, // 最大异步请求数
      maxInitialRequests: 30, // 最大初始化请求数
      automaticNameDelimiter: '~', // 文件名分隔符
      cacheGroups: {
        vendors: {
          test: /[\\/]node_modules[\\/]/,
          priority: -10
        },
        default: {
          minChunks: 2,
          priority: -20,
          reuseExistingChunk: true
        }
      }
    }
  }
};
```

3. React.lazy 和 Suspense

在 React 应用中，可以使用 React.lazy 和 Suspense 来实现组件的懒加载。

```
import React, { Suspense } from 'react';

const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}
```



### 懒加载

路由级别的 Code Splitting

对于单页应用（SPA），可以在路由级别实现 Code Splitting。React Router 和 Vue Router 都支持这种方式。

```
import React, { Suspense } from 'react';
import { BrowserRouter as Router, Route, Switch } from 'react-router-dom';

const Home = React.lazy(() => import('./Home'));
const About = React.lazy(() => import('./About'));

function App() {
  return (
    <Router>
      <Suspense fallback={<div>Loading...</div>}>
        <Switch>
          <Route path="/about" component={About} />
          <Route path="/" component={Home} />
        </Switch>
      </Suspense>
    </Router>
  );
}
```



3. Minification:

使用 TerserPlugin 来压缩 JavaScript 代码，移除多余的空格、注释等。


4. CSS Minification:

使用 cssnano 或 OptimizeCSSAssetsPlugin 来压缩 CSS 文件。


5. Image Optimization:

使用 image-webpack-loader 来压缩图片文件。





### 代码压缩

对你的代码进行压缩，手段包括去除多余空格、缩短变量名

- 使用 TerserPlugin 来压缩 JavaScript 代码。
- 确保在生产环境中启用 mode: 'production'，Webpack 会自动应用一些优化。

### 图片优化

- 使用 image-webpack-loader 或 url-loader 来优化图片。
- 考虑使用 WebP 格式的图片。


### CSS 压缩

- 使用 MiniCssExtractPlugin 来提取 CSS。
- 使用 cssnano 来压缩 CSS。


### CDN

- 将第三方库通过 CDN 引入，减少打包体积。


## 构建速度优化

### 使用最新版本的 Webpack:

- 确保使用最新版本的 Webpack 和相关插件，因为新版本通常包含性能改进。

### 优化 Loader:

- 减少 Loader 的处理范围: 使用 include 和 exclude 选项来限制 Loader 处理的文件范围。
- 使用缓存: 对于一些耗时的 Loader（如 Babel），可以使用 cache-loader 或者 babel-loader 的 cacheDirectory 选项来启用缓存。

### 优化 Plugin:

- 减少不必要的 Plugin: 只使用项目中真正需要的插件。
- 使用并行化插件: 如 terser-webpack-plugin 支持多线程并行压缩。

### 代码分割:

- 使用 Webpack 的代码分割功能（如 SplitChunksPlugin）来分割代码，减少单个文件的大小。

### 持久化缓存:

- 使用 Webpack 的持久化缓存功能（cache: { type: 'filesystem' }）来加速二次构建。

### 缩小构建目标:

- 在开发环境中，使用 cheap-module-eval-source-map 代替 source-map，以加快构建速度。
- 在生产环境中，使用 production 模式来启用内置的优化。

### 减少模块解析时间:

- 使用 resolve.alias 和 resolve.extensions 来减少模块解析时间。
- 确保 node_modules 中的模块不被重复解析。
	- 明确指定需要解析的文件扩展名范围，减少不必要的尝试
	- 对于一些需要频繁解析的模块，使用缓存来

### 使用 DLLPlugin:

- 在开发环境中，使用 DLLPlugin 和 DLLReferencePlugin 来预编译不常变化的依赖库。

### 多进程/多实例构建:

- 使用 thread-loader 来启用多进程构建，尤其是对于 CPU 密集型的任务。


## **[监控和分析应用](https://developers.google.com/web/fundamentals/performance/webpack/monitor-and-analyze)**

[分享几个 Webpack 实用分析工具 (qq.com)](https://mp.weixin.qq.com/s/A0udBhvNoA0o-kX1B0rt9A)
