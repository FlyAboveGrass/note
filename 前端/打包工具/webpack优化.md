

# [速度](https://jelly.jd.com/article/61179aa26bea510187770aa3)



### 使用热更新插件 

HotModuleReplacementPlugin

```
// 开发环境设置本地服务器，实现热更新
devServer: {
// 设置热替换
  hot: true,
},

// 插件配置
plugins: [
  // 热更新替换
  new webpack.HotModuleReplacementPlugin()
]
```



### 开发环境不做无意义的操作

开发环境的要求就是快速开发，不像打包生产环境需要各种压缩和优化。不做无意义优化（如代码压缩、目录内容清理、计算文件hash、提取CSS文件）可以有效提高热更新速度。





### 代码压缩ParallelUglifyPlugin代替 UglifyJsPlugin

自带的JS压缩插件UglifyJsPlugin是单线程执行的，而ParallelUglifyPlugin可以并行的执行





### 多进程构建 threadLoader

```
// worker 启动有一定的消耗，
// 可以预热 worker 池
const jsWorkerPool = {
    workerParallelJobs: 80,
    poolTimeout: 2000,
};
const cssWorkerPool = {
    workerParallelJobs: 10,
    poolTimeout: 2000,
};

threadLoader.warmup(jsWorkerPool, ["babel-loader"]);
threadLoader.warmup(cssWorkerPool, ["css-loader", "less-loader"]);
```



### babel-loader开启缓存

babel-loader在执行的时候，可能会产生一些运行期间重复的公共文件，造成代码体积大冗余，同时也会减慢编译效率.

加上cacheDirectory参数或使用 transform-runtime 插件可以有效提高编译效率。



```
// webpack.config.js
use: [{
  loader: 'babel-loader',
  options: {
  cacheDirectory: true
}]


// .bablerc
{
    "presets": [
        "env",
        "react"
    ],
    "plugins": ["transform-runtime"]
}
```







### JavaScript 模块热替换

1. 如果符合以下条件，可使用 [react-refresh-webpack-plugin](https://github.com/pmmmwh/react-refresh-webpack-plugin) 插件；

![QQ截图20210528173521.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/56d89e070df3438ca53ebcef5f478461~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

```javascript
{
	plugins: [
  	new ReactRefreshWebpackPlugin()
  ]
}
```

2. 若不符合上面的条件，可使用[react-hot-loader](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fgaearon%2Freact-hot-loader)库

```javascript
// .babelrc
{
    "plugins": [
      	...,
        "react-hot-loader/babel"
    ]
}
// index.js
import { hot } from 'react-hot-loader';
...
const App = hot(module)(() => ...);
render(<App />, document.getElementById('root'));
```







# 体积















# 分析



### webpack-bundle-analyzer





### speed-measure-webpack-plugin





