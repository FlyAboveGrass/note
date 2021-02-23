### let和const

#### let

- 不存在变量提升
- 暂时性死区： let 声明的变量就“绑定”（binding）这个区域，不再受外部的影响。（即使外面也有定义，里面已已经定义的话就不可以再访问外界的同名变量了）
- 不允许重复声明

#### const

- 定义时必须赋值
- 基本类型不可修改，对象内的变量值可以改变。 
  - 使用 Object.freeze（obj）函数可以将对象也锁死，里面值不可以再改变



### [箭头函数](https://zhuanlan.zhihu.com/p/62482741)

- 语法简洁清晰
- 没有this。 
  - 箭头函数的this永远从作用域链的上层继承而来，没有自己的this
  - 不可以使用bind/apply/call 改变this指向
  - 没有arguments。 但是可以访问外部的arguments对象。
- 没有原型prototype
  - 不能作为构造函数,不能通过 new 关键词调用。
- 不能用作Generator函数



### [Symbol类型](https://www.jianshu.com/p/e36a558bec34)

#### 用途

1. 属性名
2. 类的私有属性/方法
3. 模块化机制
4. 使用`Symbol`来替代常量
5. 

#### 常用api

1. Symbol.for(key) ： key相同的情况下返回同一个symbol值，
2. Symbol.keyFor ： 找到一个symbol的 key

### 异步编程

异步编程的方法： 

- 回调函数
- 事件监听
- 发布/订阅
- Promise 对象
- generator
- async/await（终极方案，同步的方式实现异步）

### [set和map](http://caibaojian.com/es6/set-map.html)

#### Set

类似于数组，但是成员的值都是唯一的，没有重复的值。

操作方法： 

- `add(value)`：添加某个值，返回Set结构本身。
- `delete(value)`：删除某个值，返回一个布尔值，表示删除是否成功。
- `has(value)`：返回一个布尔值，表示该值是否为`Set`的成员。
- `clear()`：清除所有成员，没有返回值。

遍历方法

- `keys()`：返回键名的遍历器
- `values()`：返回键值的遍历器
- `entries()`：返回键值对的遍历器
- `forEach()`：使用回调函数遍历每个成员

#### WeakSet

1. WeakSet的成员只能是对象，而不能是其他类型的值。
2. WeakSet中的对象都是弱引用，即垃圾回收机制不考虑WeakSet对该对象的引用
3. 没有size属性，无法遍历

#### Map

类似于对象，也是键值对的集合，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

#### WeakMap

和map类似。只接受对象作为键名，且键名所指向的对象，不计入垃圾回收机制。



