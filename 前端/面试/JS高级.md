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

### 箭头函数和普通函数

1. 语法简洁清晰
2. this永远从作用域链的上一层继承，没有自己的this
3. this继承来的this指向不可变， bind/call/apply 无法改变他的指向
4. 不能作为构造函数
5. 没有原型prototype
6. 不能用作Generator函数

































