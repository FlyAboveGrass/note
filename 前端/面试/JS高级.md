### 原型链

![](E:\Code\笔记\笔记图片\js原型链.jpg)

### this的指向问题

![](E:\Code\笔记\笔记图片\this指向.jpg)

- 对于直接调用`foo`来说，不管`foo`函数被放在了什么地方，`this`一定是`window`
- 对于`obj.foo()`来说，我们只需要记住，谁调用了函数，谁就是`this`，所以在这个场景下`foo`函数中的`this`就是`obj`对象
- 对于`new`的方式来说，`this`被永远绑定在了`c`上面，不会被任何方式改变`this`

```
function foo() {
  console.log(this.a)
}
var a = 1

foo()

const obj = {
  a: 2,
  foo: foo
}

obj.foo()

const c = new foo()
```

### 深拷贝和浅拷贝

拷贝：将一个对象复制一份给新对象

> 浅拷贝： 拷贝前后对象的基本数据类型互不影响，但拷贝前后对象的引用类型因共享同一块内存，会相互影响。

> 深拷贝： 另外创造一个一模一样的对象，新对象跟原对象"完全"不共享堆内存，修改新对象不会改到原对象。

#### 浅拷贝方式

1.  = 。 将对象的引用赋值。
2.  Object.assign()。 把任意多个的源对象自身的可枚举属性拷贝给目标对象，然后返回目标对象。
3.  ... 展开运算符。
4.  slice() 和 concat()。 （针对于数组）
    1. 只是第一层的深拷贝，二级及以下没有影响

#### 深拷贝方式

1. 递归赋值

   1. 遇到循环引用，会陷入一个循环的递归过程，从而导致爆栈

2. JSON.parse(JSON.stringify())

   1. 忽略undefined、任意的函数、symbol 值、正则表达式，因为JSON.stringify不能识别这些

3. Object.create()



### 判断数组的三种方式

Object.prototype.toString.call() 、instanceof 、Array.isArray()

#### Object.prototype.toString.call()

优点： 对于所有基本的数据类型都能进行判断，即使是 null和defined

缺点： 不能判断自定义对象

#### instanceof 

优点： 能够判断自定义对象

缺点： 只能判断对象的类型，原始类型不行

#### Array.isArray

优点： 方便

缺点： 只能判断数组类型。ES5新增，存在兼容性问题，可以用Object.prototype.toString.call()代替。



### 



















