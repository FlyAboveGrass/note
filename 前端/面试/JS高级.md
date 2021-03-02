### [原型链](https://segmentfault.com/a/1190000021232132)

>  参考链接  
>
>  [JavaScript深入之从原型到原型链 · Issue #2 · mqyqingfeng/Blog (github.com)](https://github.com/mqyqingfeng/blog/issues/2),
>
> [一张图搞定JS原型&原型链 - SegmentFault 思否](https://segmentfault.com/a/1190000021232132)



![](E:\Code\笔记\笔记图片\js原型链.jpg)

#### 基本概念

\__proto\__ 、 prototype和constructor

1. 对象中都有两个属性， \__proto\__ 和constructor
2. prototype 是函数独有的
3. 函数是一种特殊的对象，所以函数同时拥有 \__proto\__ 和constructor 和 prototype



![](E:\Code\笔记\笔记图片\__proto__ 、 prototype和constructor.jpg)

 \__proto__  属性是实例对象指向原型对象的属性

 prototype 属性指向一个对象，这个对象正是调用该构造函数而创建的**实例**的原型。

constructor属性，用于记录实例是由哪个构造函数创建；

原型对象也有一个自己的原型对象(  \___proto\___ ) ，层层向上直到一个对象的原型对象为 null。(根据定义，null 没有原型，并作为这个原型链中的最后一个环节)



#### 四个概念和两个原则

四个概念	

1. js分为**函数对**象和**普通对象**，每个对象都有\__proto\__属性，但是只有函数对象才有prototype属性
2. Object、Function都是js内置的**函数**, 类似的还有我们常用到的Array、RegExp、Date、Boolean、Number、String
3. 属性\__proto\__是一个对象，它有两个属性，constructor和\__proto\__；
4. 原型对象prototype有一个默认的constructor属性，用于记录实例是由哪个构造函数创建；



两个原则

1. Person.prototype.constructor === Person 

    **准则1：原型对象（即Person.prototype）的constructor指向构造函数本身** 

2. person01.\__proto__ == Person.prototype 

   **准则2：实例（即person01）的__proto__和原型对象指向同一一个地方**



#### 注意事项

instanceof（f1） === Function的返回值是false，因为instanceof会一直沿着__proto__寻找，Foo.prototype.__prpto__ 将直接指Object.propotype.

用function funName（）定义的函数，包括js内置的function Object（）和 function Function() 都属于同一个原型——Function.prototype.

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

#### bind/apply/call

这三个函数都可以用来改变this的指向

**bind**返回的是一个函数， call/apply 是立刻执行的

call 传入的参数是一系列的参数， apply 传入的是一个参数数组









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



















