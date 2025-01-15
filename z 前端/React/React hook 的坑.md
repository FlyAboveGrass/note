

# 组件的重新渲染问题

useMemo 和 useCallback 提升性能？那倒也未必。


[[useMemo 和 useCallback]]




# useState 是同步的还是异步的？

1. 在正常的 react 的事件流里（如 onClick 等合成事件和钩子函数）

setState 是异步执行的（不会立即更新 state 的结果）多次执行 setState，只会调用一次重新渲染 render
```
const [count, setCount] = useState(0)
const handleClick = () => {
	// 批处理，两次setCount合并。只会渲染一次
	setCount(1)
	setCount(2) 
}

console.log('render') // 只会打印一次
```


## 闭包陷阱

state 的值被缓存，相当于是一个快照。本次渲染中的操作，都是一样的值。

```
const [count, setCount] = useState(0)
const handleClick = () => {
	// 两次设置，count 的值都是 0。两次设置都是 0 + 1 = 1
	setCount(count + 1)
	setCount(count + 1) 

	// 解决闭包陷阱方法
	// setCount(pre => pre + 1)
	
	console.log(count) // 没有重新渲染，输出 0
}
```


2. 在 setTimeout，Promise.then 等原生异步事件中

setState 和 useState 是同步执行的（立即更新 state 的结果）多次执行 setState 和 useState，每一次的执行 setState 和 useState，都会调用一次 render

2. react 18 以前，同步执行 setState，且不自动批处理
```
const [count, setCount] = useState(0)
const handleClick = () => {
	setTimeout(() => {
		setCount(count + 1) // 立即生效， count在设值之后变成 0 + 1 = 1
		setCount(count + 1) // 立即生效， count在设值之后变成 1 + 1 = 2
		
		console.log(count) // 已经重新渲染，输出 2
	}, 0)
}
```


react 18 以后，异步执行 setState，自动批处理
```
const [count, setCount] = useState(0)
const handleClick = () => {
	setTimeout(() => {
		setCount(count => count + 1)
		setCount(count => count + 1)
		
		console.log(count) // 没有重新渲染，输出 0
	}, 0)
}
```
