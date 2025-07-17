
> 参考于： [如何正确使用 useMemo 和 useCallback - 掘金](https://juejin.cn/post/7122027852492439565#heading-7)

使用 memo 通常有三个原因：

1. ✅ 防止不必要的 effect。
2. ❗️防止不必要的重复计算。
3. ❗️防止不必要的 re-render。

# 防止不必要的 effect

如果一个值被 useEffect 依赖，那它可能需要被缓存，这样可以避免重复执行 effect。

```
const Component = () => {
  // 在 re-renders 之间缓存 a 的引用
  const a = useMemo (() => ({ test: 1 }), []);

  useEffect (() => {
    // 只有当 a 的值变化时，这里才会被触发
    doSomething ();
  }, [a]);

  // the rest of the code
};
```

# 防止不必要的重复计算

如 [React 文档]( https://link.juejin.cn/?target=https%3A%2F%2Fzh-hans.reactjs.org%2Fdocs%2Fhooks-reference.html%23usememo " https://zh-hans.reactjs.org/docs/hooks-reference.html#usememo" )所说，useMemo 的基本作用是，避免在每次渲染时都进行高开销的计算。

但大部分情况下，组件渲染才是性能的瓶颈，应该把 useMemo 用在程序里渲染昂贵的组件上，而不是数值计算上。当然，除非这个计算真的很昂贵，比如阶乘计算。

至于为什么不给所有的组件都使用 useMemo，那是因为useMemo 是有成本的，它会增加整体程序初始化的耗时，并不适合全局全面使用，它更适合做局部的优化。

# 防止不必要的 re-render

进入重点环节了🔔。正确的阻止 re-render 需要我们明确三个问题：

1. 组件什么时候会 re-render。
2. 如何防止子组件 re-render。
3. 如何判断子组件需要缓存。

## 1. 组件什么时候会 re-render

三种情况：

1. 当本身的 props 或 state 改变时。
2. Context value 改变时，使用该值的组件会 re-render。
3. 当父组件重新渲染时，它所有的子组件都会 re-render，形成一条 re-render 链。

第三个 re-render 时机经常被开发者忽视，**导致代码中存在大量的无效缓存**。例如：

```
const App = () => {
  const [state, setState] = useState(1);

  const onClick = useCallback(() => {
    setState(num => num + 1)
  }, []);

  return (
	<>
		<button onClick={onClick} ></button>
		
		// 无论 onClick 是否被缓存，Page 都会 re-render 
	    <Page onClick={onClick} />
    </>
  );
};

```

当使用 setState 改变 state 时，App 会 re-render，作为子组件的 Page 也会跟着 re-render。这里 useCallback 是完全无效的，它并不能阻止 Page 的 re-render。

## 2. 如何防止子组件 re-render

**必须同时缓存 onClick 和组件本身，才能实现 Page 不触发 re-render。**

```
const PageMemoized = React.memo(Page);

const App = () => {
  const [state, setState] = useState(1);

  const onClick = useCallback(() => {
    console.log('Do something on click');
  }, []);

  return (
    <button onClick={() => setState(num => num + 1)} ></button>
    // Page 和 onClick 同时 memorize
    <PageMemoized onClick={onClick} />
  );
};

```

由于使用了React.memo，PageMemoized 会浅比较 props 的变化后再决定是否 re-render。onClick 被缓存后不会再变化，所以 PageMemoized 不再 re-render。

然而，如果 PageMemoized 再添加一个未被缓存的 props，一切就前功尽弃 🤯 ：
```
const PageMemoized = React.memo(Page);

const App = () => {
  const [state, setState] = useState(1);

  const onClick = useCallback(() => {
    console.log('Do something on click');
  }, []);

  return (
    <button onClick={() => setState(num => num + 1)} ></button>
    // page WILL re-render because value is not memoized
    <PageMemoized onClick={onClick} value={[1, 2, 3]} />
  );
};

```

由于 value 会随着 App 的 re-render 重新定义，引用值发生变化，导致 PageMemoized 仍然会触发 re-render。

现在可以得出结论了，必须同时满足以下两个条件，子组件才不会 re-render：

1. 子组件自身被缓存。
2. 子组件所有的 prop 都被缓存。
