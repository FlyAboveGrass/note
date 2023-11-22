

使用 webpack-bundle-analyzer 分析打包体积
- 打包体积前 5 
	1. @ant-design 4.3
	2. echarts 3.12
	3. antd 1.6
	4. xlsx 1.46
	5. moment 0.673



使用 SpeedMeasurePlugin 分析 plugins 和 loader 的时间消耗





- 代码压缩
	- 调整为生产环境下压缩，开发环境下不再压缩
- thread-loader
	- workers 调整为 10 （项目成员的电脑多为 M1 Pro，有10核心）
	- scss 文件也使用 thread-loader 多进程处理
- 文件缓存（代替 hard-source）
	- `cache: {type: 'filesystem', },`




**速度对比**

	｜ 内容 ｜ 更新前 ｜ 更新后 ｜
	
