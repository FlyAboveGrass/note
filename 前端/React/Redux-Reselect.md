

# 概念

- Selector可以计算衍生的数据,可以让Redux做到存储尽可能少的state。
- Selector比较高效,只有在某个参数发生变化的时候才发生计算过程.
- Selector是可以组合的,他们可以作为输入,传递到其他的selector.



> 参考链接： [重新翻译版本|Redux-Reselect 文档 (juejin.cn)](https://juejin.cn/post/6844903829381595150#heading-4)



# 实例



> 注： 下面的 input-selector 指的是createSelector 的最后的结果函数前面的函数

## 缓存 selector

Reslect提供一个函数 `createSelector` 来创建一个  `记忆selectors` .  `记忆selector ` 只有在传入的 `input-selector ` 返回值发生改变才会随之更改自己的状态。

state 发生改变造成 `input-selector `  返回的值发生改变,  `记忆selector`  会调用变换函数,依据 `input-selector ` ,  返回的值做参数,返回一个结果.

如果`input-selector `  返回的值返回的结果和前面的一样,那么就会直接返回有关state, 会省略变换函数的调用.

```
// 前面两个是
const getVisibilityFilter = (state) => state.visibilityFilter
const getTodos = (state) => state.todos

export const getVisibleTodos = createSelector(
  [ getVisibilityFilter, getTodos ],
  (visibilityFilter, todos) => {
    		// 语句
    }
  }
)
```



## 组合selector

一个记忆性selector本身也可以作为另一个记忆性selector的 input-selector .

```
const getKeyword = (state) => state.keyword

const getVisibleTodosFilteredByKeyword = createSelector(
  [ getVisibleTodos, getKeyword ],
  (visibleTodos, keyword) => visibleTodos.filter(
    todo => todo.text.indexOf(keyword) > -1
  )
)
```



## 跨组件通过selector共享props





# API





## createSelector

createSelector(…inputSelectors|[inputSelectors],resultFunc)

接受一个或者多个selectors,或者一个selectors数组,计算他们的值并且作为参数传递给`resultFunc`.



`createSelector`通过判断input-selector之前调用和之后调用的返回值的全等于(===,这个地方英文文献叫reference equality,引用等于,这个单词是本质,中文没有翻译出来).经过`createSelector`创建的selector应该是immutable(不变的).

经过`createSelector`创建的Selectors有一个缓存,大小是1. 这意味着当一个input-selector变化的时候,他们总是会重新计算state,因为Selector仅仅存储每一个input-selector前一个值



## createSelectorCreator

```
createSelectorCreator(memoize, …memoizeOptions)
```



`createSelectorCreator`用来配置定制版本的`createSelector`.



### 定义一个定制的 createSelector

```
const customSelectorCreator = createSelectorCreator(
  customMemoize, // function to be used to memoize resultFunc,记忆resultFunc
  option1, // option1 will be passed as second argument to customMemoize 第二个惨呼
  option2, // option2 will be passed as third argument to customMemoize 第三个参数
  option3 // option3 will be passed as fourth argument to customMemoize   第四个参数
)

const customSelector = customSelectorCreator(
  input1,
  input2,
  resultFunc // resultFunc will be passed as first argument to customMemoize  作为第一个参数传递给customMomize
)
```



在 `customSelecotr` 内部使用memoize的函数的代码如下:

```
customMemoize(resultFunc, option1, option2, option3)
```





