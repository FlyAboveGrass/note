> 参考链接
>
> [Webpack 5 知识体系 - GitMind](https://gitmind.cn/app/doc/fac1c196e29b8f9052239f16cff7d4c7)
>
> [[万字总结\] 一文吃透 Webpack 核心原理 - 掘金 (juejin.cn)](https://juejin.cn/post/6949040393165996040#heading-1)
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



## **减少前端打包体积**



### **使用生产模式**

```JavaScript
// webpack.config.js
module.exports = {
  mode: 'production',
};
```





### **启用 minification**

minification 会对你的代码进行压缩，手段包括去除多余空格、缩短变量名等



**包层级的压缩**

webpack4是默认开启的，webpack3则需要手动配置

```JavaScript
// webpack.config.js
const webpack = require('webpack');
module.exports = {
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
  ],
};
```



**特定 loader 的压缩配置**

```JavaScript
// webpack.config.js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          'style-loader',
          { loader: 'css-loader', options: { minimize: true } },
        ],
      },
    ],
  },
};
```

### **使用 ES6 模块**

使用 ES6 模块可以让webpack进行自动的摇树优化。



### **图片优化**

url-loader

svg-url-loader

Image-webpack-loader



### **[依赖优化](https://github.com/GoogleChromeLabs/webpack-libs-optimizations)**



### **启用ES模块串联**

启用之后，打包bundle时webpack 会自动将模块都打包进一个方法中，并且所有的模块都放在同一个文件中，能有效减少文件数。

```JavaScript
// webpack.config.js (for webpack 4)
module.exports = {
  optimization: {
    concatenateModules: true,
  },
};
```



## **使用长期缓存**



### **缓存的方式**

**使用带版本的包或者带缓存的请求头**

```JavaScript
<!-- Before the change -->
<script src="./index-v15.js"></script>


<!-- After the change -->
<script src="./index-v16.js"></script>

Cache-Control: max-age=31536000
```



### **选取依赖和运行时代码到不同文件中**

#### **依赖**

将依赖分离到独立的块中，有三个步骤

```JavaScript
// 1.替代输出文件名
// webpack.config.js
module.exports = {
  output: {
    // Before
    filename: 'bundle.[chunkhash].js',
    // After
    filename: '[name].[chunkhash].js',
  },
};


// 2. 将 entry 字段转换成object
module.exports = {
  // Before
  entry: './index.js',
  // After
  entry: {
    main: './index.js',
  },
};


// 3.webpack.config.js (for webpack 4)
module.exports = {
  optimization: {
    splitChunks: {
      chunks: 'all',
    }
  },
};
```



#### **运行时代码**

```JavaScript
// webpack.config.js (for webpack 4)
module.exports = {
  optimization: {
    runtimeChunk: true,
  },
};
```



### **内联运行时代码以节省http请求**

```JavaScript
// before
<script src="./runtime.79f17c27b335abc7aaf4.js"></script>

// after
<script>
!function(e){function n(r){if(t[r])return t[r].exports;…}} ([]);
</script>
```



#### **用HtmlWebpackPlugin生成html的情况**

```JavaScript
// webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const InlineSourcePlugin = require('html-webpack-inline-source-plugin');

module.exports = {
  plugins: [
    new HtmlWebpackPlugin({
      // Inline all files which names start with “runtime~” and end with “.js”.
      // That’s the default naming of runtime chunks
      inlineSource: 'runtime~.+\\.js',
    }),
    // This plugin enables the “inlineSource” option
    new InlineSourcePlugin(),
  ],
};
```



#### **用服务器生成html的情况**

**使用ManifestPlugin插件得知运行时的生成文件名**

```JavaScript
// webpack.config.js (for webpack 4)
const ManifestPlugin = require('webpack-manifest-plugin');

module.exports = {
  plugins: [
    new ManifestPlugin(),
  ],
};


// manifest.json
{
  "runtime~main.js": "runtime~main.8e0d62a03.js"
}
```



**使用node服务将运行时内联到内容里**

```JavaScript
// server.js

const fs = require('fs');
const manifest = require('./manifest.json');
const runtimeContent = fs.readFileSync(manifest['runtime~main.js'], 'utf-8');

app.get('/', (req, res) => {
  res.send(`
    …
    <script>${runtimeContent}</script>
    …
	`);
});
```



### **懒加载**

不在一开始就引入所有的文件。只先引入重要的必须文件，其他的可以进行动态的import



### **将代码拆分到路由或者页面中**

#### **单页面应用**

react-router 和 vue-router 都提供了代码分割的能力



**多页面应用**

使用 entry 选项，将页面内容分开

```JavaScript
module.exports = {
  entry: {
    home: './src/Home/index.js',
    article: './src/Article/index.js',
    profile: './src/Profile/index.js'
  },
};
```





### **让模块id更稳定**

使用计数器作为模块的id，如果在中间加入一个模块会导致后面的未改动的模块都进行重新编译。而使用哈希值作为模块id则不会产生这个问题。



使用 HashedModuleIdsPlugin 可以将模块id由计数器变成hash

```JavaScript
// webpack.config.js
module.exports = {
  plugins: [
    new webpack.HashedModuleIdsPlugin(),
  ],
};
```







## **[监控和分析应用](https://developers.google.com/web/fundamentals/performance/webpack/monitor-and-analyze)**

[分享几个 Webpack 实用分析工具 (qq.com)](https://mp.weixin.qq.com/s/A0udBhvNoA0o-kX1B0rt9A)
