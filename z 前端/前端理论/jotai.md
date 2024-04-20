
# core

## atom

关键词：
- atom config
- Read-only atom
- Write-only atom
- Read-Write atom



在一个组件内部定义 atom 的时候。需要使用 useMemo 或者画 useRef 来保证 atom 的引用是固定的，避免不必要的 rerender。



atom 中有一个 onMounted 的属性，在 atom 首次被订阅的时候触发，它是一个函数，接收一个 setAtom 函数作为参数，可选地返回一个 onUnmount 函数。当这个 atom 不再被订阅的时候，会触发 onUnmount。

> 如果组件 A 引用了组件 B（没有渲染），组件 A 中使用了 setAtom 修改 atom 的值，此时 atom 并没有被订阅。那么当组件 B 被渲染的时候，读到的值是新的还是旧的？



## useAtom

关键词：
- useAtom
- useAtomValue
- useSetAtom


## store

关键词：
- createStore。包含 get、set、subscribe 操作，可以传递给 Provider
- createDefaultStore。provider-less mode


## Provider

Providers are useful for three reasons:

1. To provide a different state for each sub tree.
2. To accept initial values of atoms.
3. To clear all atoms by remounting.


# UTILITIES

## Storage

持久化

关键词：
- atomWithStorage
- createJSONStorage。自定义序列化和反序列化函数
- `RESET` symbol


## Async
异步 atom 同步化


关键词：
- loadable / unwrap 异步 atom 同步化
- atomWithObservable



## Resettable

可重置的 atom

- atomWithReset / useResetAtom


## Family

`atomFamily` 的主要作用是创建和管理一组相关的原子（atoms），这些原子是 Jotai 中用于存储和更新状态的最小单元。通过 id 来标识并缓存这些最小单元 atom。

关键词：
- atomFamily
- memory leaks

## Reducer

像 redux 的 reducer 一样，通过 action 更新 atom

## select

关键词：
- selectAtom
	- 性能优化，仅当 equalityFn 判断值发生变化才会重新计算派生值


## Split

- SplitAtom
	- 创造一个对应于 atom 包含列表时，列表中每一项的 atom 列表。提供 remove，insert， move 操作从而方便地修改数组