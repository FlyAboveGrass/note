# 作用域和闭包

## 作用域

### 编译原理

js 通常被归类为 “解释性语言”。但是实际上js是编译型语言，但是他和其他的编译型语言不一样，它并不会提前编译，他是到了浏览器端才会编译的。

**编译步骤**

1. 分词
2. 解析

    像 `var a = 2`这样一个语句，js引擎会将它转化成一个***抽象语法树（AST）***。这个树的顶级节点是VariableDeclaration（var），接下来是一个Identifier（a）和一个NumericLiteral（2）的子节点。

3. 代码生成

### 理解作用域

有三个相关的概念

- 引擎： js程序的编译和执行
- 编译器： 语法分析/代码生成等
- 作用域

    收集并维护所有声明的标识符组成的一系列查询，确定当前代码对这些标识符的访问权限

当执行`var a = 2` 这一简单的操作时，

1. 遇到 var a，编译器会询问作用域是否已经有一个该名称的变量存在于同一个作用域的集合中。如果是，编译器会忽略该声明，继续进行编译；否则它会要求作用域在当前作用域的集合中声明一个新的变量，并命名为 a。
2. 接下来编译器会为引擎生成运行时所需的代码，这些代码被用来处理 a = 2 这个赋值操作。引擎运行时会首先询问作用域，在当前的作用域集合中是否存在一个叫作 a 的变量。如果是，引擎就会使用这个变量；如果否，引擎会继续查找该变量（查看 1.3节）。

如果引擎最终找到了 a 变量，就会将 2 赋值给它。否则引擎就会举手示意并抛出一个异常！

#### 引擎寻找变量

引擎查找变量的两种方式：

1. **LHS** ：左侧查找

    ​    `a = 2` 这段代码是一个 LHS 引用查找，不关心当前的值，只想为 `= 2` 寻找一个目标。关心目标

2. **RHS**： 右侧查找

    `console.log(a)` 这段代码对 a 的引用是一个 RHS 引用，因为 a 并没有赋予任何值。关心当前值

**为什么要区分**

当变量还未声明，这两种查找会导致截然不同的行为。参考以下代码：

```
function foo(a) {
 console.log( a + b );
 b = a;
}
foo( 2 );
```

当`console.log(a+b)`对 b 进行RHS 查找时，找不到 b 会导致一个引用错误。

若没有console.log, 当`b = a` 对 b 进行LHS 查找时，找不到 b 会继续向上查找，直到全局作用域，全局作用域没有，会自动为你创建。

#### 作用域嵌套

当一个块或者函数嵌套在其他的块中时，就会产生作用域嵌套。

**引擎从当前的执行作用域开始查找变量，如果找不到，就向上一级继续查找。当抵达最外层的全局作用域时，无论找到还是没找到，查找过程都会停止。**

## 词法作用域

词法作用域是由你在写代码时将变量和块作用域写在哪里来决定的，因此当词法分析器处理代码时会保持作用域不变（大部分情况下是这样的）。

作用域的范围类似于**一个气泡**。

![image-20210602235821537](词法作用域.png)

无论函数在哪里被调用，也无论它如何被调用，他的词法作用域都只由函数被声明的位置决定。

嵌套的作用域可以定义同名变量，这样查找的时候再第一次找到对应变量的时候就会停止。

### 欺骗词法

我们知道函数的词法作用域定义时就已经确定，那我们要修改词法作用域该怎么办呢？

有两种机制实现，但是我们应该**避免使用**它们！

> 欺骗词法作用域会导致性能下降。

1. #### eval

    `eval`可以在你写的代码中用程序生成代码并运行，就好像代码是写在那个位置的一样。

2. #### with

### 性能

js 在编译的时候会进行各种优化，以便于在查找作用域的时候能够根据代码位置进行快速的查找到标识符的位置。

如果在代码中发现了 eval 或者 with ，那么所有的优化都毫无意义，这会导致性能大幅下降。

## 函数作用域和块作用域

### 函数作用域

函数作用域是指，属于一个函数的全部变量都可以在函数的范围内使用及复用。

#### 隐藏内部实现

可以将变量和函数包裹在一个函数的作用域中，然后用这个作用域来隐藏他们。

##### **为什么要隐藏？**

**最小授权原则**

在软件设计中，应该最小限度的暴露必要内容，而将其他内容都隐藏起来

**规避冲突**

避免同名标识符的冲突

- 全局命名空间
- 模块化管理

#### **函数**

##### 匿名函数和具名函数

匿名函数最常见的场景就是回调函数

**优点**

写法简洁

**缺点**

- 不便调试
- 可读性差
- 引用自身只能使用过时的 arguments.callee

```
// 始终给函数一个名字是一个最佳实践
setTimeout（function timeHandler（） {
    // ...code
}, 1000）
```

### 块作用域

在js中其实本身并不存在块作用域的概念，你在 {} 中声明的变量还是会在外界可访问。

但是有一些技巧可以帮助我们在 js 代码中实现块作用域

#### 技巧

##### with

##### **try catch**

try / catch 的 catch 分句会创建一个块作用域，其中声明的变量仅在catch 内部可用

#### let

let 关键字将变量隐式的附加到了当前的作用域内

##### 垃圾收集

```
function process(data) {
 // 在这里做点有趣的事情
}
var someReallyBigData = { .. };
process( someReallyBigData );
var btn = document.getElementById( "my_button" );
btn.addEventListener( "click", function click(evt) {
 console.log("button clicked");
}, /*capturingPhase=*/false );
```

click的点击其实并不需要 someReallyBigData 变量，理论上到process 执行之后，内存中占用大量空间的数据就可以回收了。

但是由于click函数形成了一个覆盖整个作用域的闭包， JS 引擎有可能继续保存着这个结构。

块级作用域可以避免这样的问题

```
function process(data) {
 // 在这里做点有趣的事情
}
// 在这个块中定义的内容可以销毁了！
{
let someReallyBigData = { .. };
 process( someReallyBigData );
}
var btn = document.getElementById( "my_button" );
btn.addEventListener( "click", function click(evt){
 console.log("button clicked");
}, /*capturingPhase=*/false );
```

## 提升

### 变量提升

`var a = 2 `

以这个表达式为例，你可能会认为这是一个声明，但是 JS 实际上会将其看作两个声明，`var a` 和 `a = 2`. 其中 `var a ` 会提升到代码的最上面，在**编译阶段**就进行；而 `a = 2` 会被留在原地，等待代码执行到这里。

### 函数提升

```
// 函数声明
function foo1() {
 console.log( 1 );
}

// 函数表达式
var foo2 = function() {
 // ...
};
```

一个普通块内部的函数声明通常会被提升到所在作用域的顶部，**函数声明的优先级比变量更高**。

函数表达式也是函数声明的一种方式，但是函数表达式的提升和变量声明是一样的，并且都会在函数声明的后面。

## 作用域闭包

### 闭包实质

**当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域之外执行**

```
function foo() {
var a = 2;
function bar() { 
 console.log( a );
 }
return bar;
}
var baz = foo();
baz();
```

函数 bar（）可以正常执行，可以访问 foo（） 的内部作用域，bar（） 在自己定义的词法作用域之外的地方执行。

在 foo（） 执行之后，我们会期望foo的整个内部作用域被销毁，但是实际上不会。内部作用域此时依然存在，没有被回收，谁在使用这个内部作用域呢？是 bar（） 本身在使用。

bar（）被foo作为返回值返回到了外部 baz ， bar 是可以访问foo的整个作用域的，在bar 返回出去的时候连同着它的作用域链也带到了外面并保持住了，所以 foo 不会被垃圾回收机制回收掉。

无论使用何种方式对函数类型的值进行传递，当函数在别处被调用时都可以观察到闭包。

**闭包的作用**

它有助于减少全局变量的数量，维护代码的可维护性。此外，闭包可以封装一些变量，供内部函数调用，允许函数创建函数，而不会暴露该函数的实现细节和状态信息。


立即执行函数不是闭包


# this 和对象原型

## this

this 提供了一种更加优雅的方式来隐式的传递一个对象的引用。

### 误解

**this 的指向的就是函数本身？**

 这是错误的。

```
function foo(num) {
 console.log( "foo: " + num );
 // 记录 foo 被调用的次数
this.count++;
}
foo.count = 0;
var i;
for (i=0; i<10; i++) {
if (i > 5) {
 foo( i );
 }
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9
// foo 被调用了多少次？
console.log( foo.count ); // 0 
```

执行 foo.count = 0 时，的确向函数对象 foo 添加了一个属性 count。但是函数内部代码 this.count  中的 this 并不是指向那个函数对象，所以虽然属性名相同，根对象却并不相同，困惑随之产生。

> 这段代码在无意中创建了一个全局变量 count，它的值为 NaN

**this 指向它所在的作用域？**

在某种情况下它是正确的，但是在其他情况下它是错误的

> 在 JavaScript 内部，作用域确实和对象类似，可见的标识符都是它的属性。但是作用域“对象”无法通过 JavaScript代码访问，它存在于 JavaScript 引擎内部。当你想要把 this 和词法作用域的查找混合使用时,这是无法实现的。

### 实质

this 是在运行时擦爱进行绑定的，this 的绑定和函数声明的位置没有任何关系，只取决于函数的调用方式。

### this的调用位置

我们关心的调用位置就在当前正在执行的函数的前一个调用中。

#### 判断this的原则

1. **函数引用的位置是关注点**。以引用的维度去思考函数的位置。
2. **调用的位置是决定点**。以函数被调用的地方为准。调用是指函数的后面已经带了 `()` ,而不是在赋值传递

#### 绑定规则

**默认绑定**

最常用的函数调用类型 —— 独立函数调用。

```
function foo() { 
 console.log( this.a );
}
var a = 2;
foo(); // 2
```

这样的情况下，foo 函数是直接使用不带任何修饰的函数引用进行调用的，因此只能使用默认绑定。

*在严格模式下，this 与foo 调用的位置无关*

**隐式绑定**

```
function foo() { 
 console.log( this.a );
}
var obj = { 
 a: 2,
 foo: foo 
};
obj.foo(); // 2
```

调用位置是否有上下文对象，或者说是否被某个对象拥有或者包含。

在这个例子中，foo 函数是 obj 调用的， obj 此时拥有这个函数。

当函数引用有上下文对象时，**隐式绑定**会把函数调用的this 绑定到这个上下文对象。

**隐式丢失**

有一种比较特殊的情况

```
function foo() { 
 console.log( this.a );
}
var obj = { 
 a: 2,
 foo: foo 
};
var bar = obj.foo; // 函数别名！
var a = "oops, global"; // a 是全局对象的属性
bar(); // "oops, global"
```

虽然 bar 是 obj.foo 的一个引用，但是实际上，它引用的是 foo 函数本身，因此此时的 bar() 其实是一个不带任何修饰的函数调用，因此应用了默认绑定。

[这又适用于我们开头说的规则了](#判断this的原则)

**显式绑定**

通过 call 和 apply 强行改变 this 的指向。

**new 绑定**

使用 new 来调用函数，或者说发生构造函数调用时，会自动执行下面的操作。

1. 创建（或者说构造）一个全新的对象。
2. 这个新对象会被执行 [[ 原型 ]] 连接。
3. 这个新对象会绑定到函数调用的 this。
4. 如果函数没有返回其他对象，那么 new 表达式中的函数调用会自动返回这个新对象。

```
function foo(a) { 
this.a = a;
} 
var bar = new foo(2);
console.log( bar.a ); // 2
```

使用 new 来调用 foo(..) 时，我们会构造一个新对象并把它绑定到 foo(..) 调用中的 this上。new 是最后一种可以影响函数调用时 this 绑定行为的方法，我们称之为 new 绑定。

#### 优先级

1. 函数是否在 new 中调用（new 绑定）？如果是的话 this 绑定的是新创建的对象。

    var bar = new foo()

2. 函数是否通过 call、apply（显式绑定）或者硬绑定调用？如果是的话，this 绑定的是指定的对象。

    var bar = foo.call(obj2)

3. 函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，this 绑定的是那个上下文对象。

    var bar = obj1.foo()

4. 如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到 undefined，否则绑定到全局对象。

    var bar = foo()

### 绑定例外

this的位置判断也有例外

#### 被忽略的 this

把 null 或者 undefined等作为this的绑定对象传入call/apply/bind，那么这些值在调用时会被忽略。此时应用的还是默认绑定规则

**什么时候我们会传入 null/undefined ？**

函数柯里化

```
function foo(a,b) {
 console.log( "a:" + a + ", b:" + b );
}
// 把数组“展开”成参数
foo.apply( null, [2, 3] ); // a:2, b:3
// 使用 bind(..) 进行柯里化
var bar = foo.bind( null, 2 ); 
bar( 3 ); // a:2, b:3
```

更安全的this

`在使用函数的时候传入一个特殊的对象`

`（ null、undefined、object.create(null) ），`

`把 this 绑定到这个对象不会对你的程序产生任何副作用。`

#### 间接引用

你有可能（有意或者无意地）创建一个函数的“间接引用”，在这种情况下，调用这个函数会应用默认绑定规则。

```
function foo() { 
 console.log( this.a );
}
var a = 2; 
var o = { a: 3, foo: foo }; 
var p = { a: 4 };
o.foo(); // 3
(p.foo = o.foo)(); // 2
```

> 首先明确一点， 执行 `a = b` 这个表达式的时候，返回值是 b

当 执行`p.foo = o.foo` ，返回的是 foo 的引用，根据前面说到的原则，在`(p.foo = o.foo)()`这里函数应用的会是默认绑定规则

#### 软绑定

硬绑定会大大降低函数的灵活性，使用硬绑定之后就无法使用隐式绑定或者显式绑定来修改 this。

可以给默认绑定指定一个全局对象和 undefined 以外的值，那就可以实现和硬绑定相同的效果，同时保留隐式绑定或者显式绑定修改 this 的能力

```
if (!Function.prototype.softBind) { 
 Function.prototype.softBind = function(obj) {
 var fn = this;
 // 捕获所有 curried 参数
var curried = [].slice.call( arguments, 1 ); 
var bound = function() {
return fn.apply(
 (!this || this === (window || global)) ?
 obj : this
 curried.concat.apply( curried, arguments )
 ); 
 };
 bound.prototype = Object.create( fn.prototype );
return bound; 
 };
}
```


# 对象

## 类型 和 内容

- String
- Number
- Boolean
- Object
- Function
- Array
- RegExp
- Error

这些内置对象从表现形式来说很像其他语言中的类型（type）或者类（class），比如 Java中的 String 类。但是在 JavaScript 中，它们实际上只是一些内置函数。这些内置函数可以当作构造函数来使用，从而可以构造一个对应子类型的新对象。

在读取对象的内容时，有两种方式

1. 属性访问。 `obj.a`
2. 键访问。 `obj["a"]`

在对象中，属性名永远是字符串，如果使用 string 以外的其他值作为属性名，都会被转化成一个字符串。在数组对象中使用的的确是数字，但是也会被转换成字符串。

### 存在性

myObject.a 的属性访问返回值可能是 undefined，但是这个值有可能是属性中存储的 undefined，也可能是因为属性不存在所以返回 undefined。如何区分呢？

```
// in 会在原型链中查找
("a" in myObject); // true
("b" in myObject); // false 

// hasOwnProperty 不会在原型链中查找
myObject.hasOwnProperty( "a" ); // true
myObject.hasOwnProperty( "b" ); // false
```

# 类

## 类理论

面向对象编程强调的是数据和操作数据的行为本质上是互相关联的（当然，不同的数据有不同的行为），因此好的设计就是把数据以及和它相关的行为打包（或者说封装）起来。

类的特性： 封装、继承和多态

**多重继承**

多重继承可以让一个类同事继承多个父类。但是这样会出现如果两个父类都存在同一个方法时，子类不知道应该采用谁的。

## 混入

当进行继承和实例化的时候， JS 并不会自动执行复制行为，因为在 JS 中不存在类的概念，JS 中只有对象和函数。

因此 js 的开发者相处了一个方法模拟类的复制，这个方法就是混入。

### 显式混入

```
// 非常简单的 mixin(..) 例子 :
function mixin( sourceObj, targetObj ) { 
  for (var key in sourceObj) {
   // 只会在不存在的情况下复制
  if (!(key in targetObj)) { 
       targetObj[key] = sourceObj[key];
   } 
  }
  return targetObj; 
}

var Vehicle = { 
 engines: 1,
 ignition: function() {
     console.log( "Turning on my engine." );
 },
 drive: function() { 
    this.ignition();
     console.log( "Steering and moving forward!" );
 }
};
var Car = mixin( Vehicle, {
 wheels: 4,
 drive: function() { 
   Vehicle.drive.call( this ); 
   console.log(
   "Rolling on all " + this.wheels + " wheels!" 
   );
 } 
} );
```

这种情况下，值的复制是直接复制的，但是函数的复制是引用的复制，当函数改变所有的函数定义都会改变。并且，这种方式实现了对父类的重写。

**再说多态**

- 显式多态： `Vehicle.drive.call( this )`
- 相对多态： `inherited:drive()`

由于存在标识符重叠，为了区分它们必须使用更加复杂的显式伪多态方法。

 JavaScript 中（由于屏蔽）使用显式伪多态会在所有需要使用（伪）多态引用的地方创建一个函数关联，这会极大地增加维护成本。此外，由于显式伪多态可以模拟多重继承，所以它会进一步增加代码的复杂度和维护难度。

**混合复制**

mixin(..) 的工作原理。它会遍历 sourceObj（本例中是 Vehicle）的属性，如果在 targetObj（本例中是 Car）没有这个属性就会进行复制。

我们一定要小心不要覆盖目标对象的原有属性。

即使我们成功的复制了。由于两个对象引用的是同一个函数，因此这种复制（或者说混入）实际上并不能完全模拟面向类的语言中的复制。

> 只在能够提高代码可读性的前提下使用显式混入，避免使用增加代码理解难度或者让对象关系更加复杂的模式。

如果使用混入时感觉越来越困难，那或许你应该停止使用它了。实际上，如果你必须使用一个复杂的库或者函数来实现这些细节，那就标志着你的方法是有问题的或者是不必要的。

**寄生继承**

### 隐式混入

```
var Something = { 
 cool: function() {
    this.greeting = "Hello World";
    this.count = this.count ? this.count + 1 : 1; 
 }
};

var Another = {
 cool: function() {
 // 隐式把 Something 混入 Another
 Something.cool.call( this ); 
}
```

这样子，我们可以成功的借用了父类的方法，最终的结果是父类的操作应用在了我们的子类对象上。

但是我们依旧无法让函数变成相对引用。

# 原型

![原型链.png](https://segmentfault.com/img/remote/1460000021232137)

## Prototype

JS 所有的对象都有一个Prototype属性，几乎所有的对象在创建时 prototype 属性都会赋予一个非空值。

**Prototype 用处？**

对于我们默认的 Get 操作来说，当我们读区一个对象上的属性，如果我们不能够在对象上找到，那么会继续在对象的 prototype 上去寻找。

> ES6 中的 Proxy 超出了这个限制，当存在Proxy 的时候，这个规则就不适用

### 属性设置和屏蔽

`myObject.foo = "bar";`

如果 myObject 对象中包含名为 foo 的普通数据访问属性，这条赋值语句只会修改已有的属性值。

如果 foo 不是直接存在于 myObject 中，[[Prototype]] 链就会被遍历，类似 [[Get]] 操作。如果原型链上找不到 foo，foo 就会被直接添加到 myObject 上。

如果 foo 存在于原型链上层，赋值语句 myObject.foo = "bar" 的行为就会有些不同

1. 如果在 [[Prototype]] 链上层存在名为 foo 的普通数据访问属性（参见第 3 章）并且没有被标记为只读（writable:false），那就会直接在 myObject 中添加一个名为 foo 的新属性，它是**屏蔽属性**
2. 如果在 [[Prototype]] 链上层存在 foo，但是它被标记为只读（writable:false），那么无法修改已有属性或者在 myObject 上创建屏蔽属性。如果运行在严格模式下，代码会抛出一个错误。否则，这条赋值语句会被忽略
3. 如果在 [[Prototype]] 链上层存在 foo 并且它是一个 setter，那就一定会调用这个 setter。foo 不会被添加到（或者说屏蔽于）myObject，也不会重新定义 foo 这 个 setter。

>  如果你希望在第二种和第三种情况下也屏蔽 foo，那就不能使用 = 操作符来赋值，而是使用 Object.defineProperty(..)

> 第二种情况可能是最令人意外的，只读属性会阻止 [[Prototype]] 链下层隐式创建（屏蔽）同名属性。这样做主要是为了模拟类属性的继承。你可以把原型链上层的 foo 看作是父类中的属性，它会被 myObject 继承（复制），这样一来 myObject 中的 foo 属性也是只读，所以无法创建。

**隐式屏蔽**

```
var anotherObject = { 
 a:2
};
var myObject = Object.create( anotherObject ); 

anotherObject.a; // 2
myObject.a; // 2 

anotherObject.hasOwnProperty( "a" ); // true
myObject.hasOwnProperty( "a" ); // false 

myObject.a++; // 隐式屏蔽！
anotherObject.a; // 2 
myObject.a; // 3
myObject.hasOwnProperty( "a" ); // true
```

在这样的情况下，此 ++ 操作首先会通过 [[Prototype]]查找属性 a 并从 anotherObject.a 获取当前属性值 2，然后给这个值加 1，接着用 [[Put]]将值 3 赋给 myObject 中新建的屏蔽属性 a

修改委托属性时一定要小心。如果想让 anotherObject.a 的值增加，唯一的办法是anotherObject.a++

## “类”

### 类函数

JavaScript 中有一种奇怪的行为一直在被无耻地滥用，那就是模仿类。

```
function Foo() { 
 // ...
}
var a = new Foo();
Object.getPrototypeOf( a ) === Foo.prototype; // true
```

是在 JavaScript 中，并没有类似的复制机制。你不能创建一个类的多个实例，只能创建多个对象，它们 [[Prototype]] 关联的是同一个对象。但是在默认情况下并不会进行复制，因此这些对象之间并不会完全失去联系，它们是互相关联的。

绝大多数 JavaScript 开发者不知道的秘密是，new Foo() 这个函数调用实际上并没有直接创建关联，这个关联只是一个意外的副作用。new Foo() 只是间接完成了我们的目标：一个关联到其他对象的新对象。

### 构造函数

```
function Foo() { 
 // ...
}
var a = new Foo();
```

这样的函数写法会让我们误会成 Foo 是一个类。但是这样的想法其实是错误的。

```
function Foo() { 
 // ...
}
Foo.prototype.constructor === Foo; // true
var a = new Foo(); 
a.constructor === Foo; // true
```

实际上 a 本身并没有 .constructor 属性。而且，虽然 a.constructor 确实指向 Foo 函数，但是这个属性并不是表示 a 由 Foo“构造”。

.constructor 引用同样被委托给了 Foo.prototype，而 Foo.prototype.constructor 默认指向 Foo。当在 a 上没有找到 constructor 属性时，会到他的原型上去查找，所以说 a 上面并没有 constructor 属性，这个属性是在 prototype 上的。

#### 构造函数还是调用

当你在普通的函数调用前面加上 new 关键字之后，就会把这个函数调用变成一个“构造函数调用”。实际上，new 会劫持所有普通函数并用构造对象的形式来调用它。

```
function NothingSpecial() { 
 console.log( "Don't mind me!" );
}
var a = new NothingSpecial();
// "Don't mind me!" 
a; // {}
```

如果你创建了一个新对象并替换了函数默认的 .prototype 对象引用，那么新对象并不会自动获得 .constructor 属性。

.constructor 是一个非常不可靠并且不安全的引用。通常来说要尽量避免使用这些引用。

## 对象关联

### 创建关联

Object.create(..) 会创建一个新对象（bar）并把它关联到我们指定的对象（foo），这样我们就可以充分发挥 [[Prototype]] 机制的威力（委托）并且避免不必要的麻烦（比如使用 new 的构造函数调用会生成 .prototype 和 .constructor 引用）。

>  Object.create(null) 会 创 建 一 个 拥 有 空（ 或 者 说 null）[[Prototype]]链接的对象，这个对象无法进行委托。由于这个对象没有原型链，所以instanceof 操作符（之前解释过）无法进行判断，因此总是会返回 false。这些特殊的空 [[Prototype]] 对象通常被称作“字典”，它们完全不会受到原型链的干扰，因此非常适合用来存储数据。

# 行为委托

## 面向委托的设计

### 委托理论

```
Task = {
 setID: function(ID) { this.id = ID; }, 
 outputID: function() { console.log( this.id ); }
};
// 让 XYZ 委托 Task
XYZ = Object.create( Task );
XYZ.prepareTask = function(ID,Label) { 
this.setID( ID );
this.label = Label;
};
XYZ.outputTaskDetails = function() { 
this.outputID();
 console.log( this.label ); 
};
```

在 这 段 代 码 中，Task 和 XYZ 并 不 是 类（ 或 者 函 数 ）， 它 们 是 对 象。XYZ 通 过 Object.create(..) 创建，它的 [[Prototype]] 委托了 Task 对象。我们和 XYZ 进行交互时可以使用 Task 中的通用方法，因为 XYZ 委托了 Task。

委托行为意味着某些对象（XYZ）在找不到属性或者方法引用时会把这个请求委托给另一个对象（Task）。这是一种极其强大的设计模式，和父类、子类、继承、多态等概念完全不同。在你的脑海中对象并不是按照父类到子类的关系垂直组织的，而是通过任意方向的委托关联并排组织的。


# 类型和值

## 类型

JavaScript 有七种内置类型：

- 空值（null）
- 未定义（undefined）
- 布尔值（ boolean）
- 数字（number）
- 字符串（string）
- 对象（object）
- 符号（symbol，ES6 中新增）

**特殊的**：

typeof null // ‘object’

typeof function // ‘function’

**值和类型：**

JavaScript 中的变量是没有类型的，只有值才有。变量可以随时持有任何类型的值。

**undefined 和 undeclared**

已在作用域中声明但还没有赋值的变量，是 undefifined 的。相反，还没有在作用域中声明过的变量，是 undeclared 的。

## 值

**字符串**

JavaScript 中字符串是不可变的，而数组是可变的。

字符串不可变是指字符串的成员函数不会改变其原始值，而是创建并返回一个新的字符串。而数组的成员函数都是在其原始值上进行操作。

**数字**

JavaScript 只有一种数值类型：number（数字），包括“整数”和带小数的十进制数。

JavaScript 使用的是“双精度”格式（即 64 位二进制）的数字，他没有真正意义上的整数。

JavaScript 中的数字常量一般用十进制表示。数字前面的 0 可以省略，数字后面的 0 也可以省略(为了更好的可读性，这样的写法不鼓励)。

```
var b = .42;
var c = 42.0
```

有时 JavaScript 程序需要处理一些比较大的数字，如数据库中的 64 位 ID 等。由于JavaScript 的数字类型无法精确呈现 64 位数值，所以必须将它们保存（转换）为字符串。

# 原生函数

常用的原生函数有：

- String()
- Number()
- Boolean()
- Array()
- Object()
- Function()
- RegExp()
- Date()
- Error()
- Symbol()——ES6 中新加入的

## 内部属性

所有 typeof 返回值为 "object" 的对象（如数组）都包含一个内部属性 [[Class]]（我们可以把它看作一个内部的分类，而非传统的面向对象意义上的类）。这个属性无法直接访问，一般通过 Object.prototype.toString(..) 来查看。

```
// [[Class]] 属性值是 "Array"
Object.prototype.toString.call( [1,2,3] );// "[object Array]" 
// 正则表达式的值是 "RegExp"
Object.prototype.toString.call( /regex-literal/i );// "[object RegExp]"
```

## 包装对象

由 于 基 本 类 型 值 没 有 .length和 .toString() 这样的属性和方法，需要通过封装对象才能访问，此时 JavaScript 会自动为基本类型值包装（box 或者 wrap）一个封装对象：

```
var a = "abc";

a.length; // 3
a.toUpperCase(); // "ABC"
```

**注意**：

我们为 false 创建了一个封装对象，然而该对象隐式转换是真值，所以这里使用封装对象得到的结果和使用 false 截然相反。

```
var a = new Boolean( false );
if (!a) {
 console.log( "Oops" ); // 执行不到这里
}
```

### 拆封

想要得到封装对象中的基本类型值，可以使用 valueOf() 函数：

```
var a = new Boolean( false );
if (!a.valueOf()) {
 console.log( "Oops" ); // 可以执行
}
```

## 原生函数作为构造函数

### Array

>  构造函数 Array(..) 不要求必须带 new 关键字。不带时，它会被自动补上。因此 Array(1,2,3) 和 new Array(1,2,3) 的效果是一样的。

> 我们将包含至少一个“空单元”的数组称为“稀疏数组”。

如若一个数组没有任何单元，但它的 length 属性中却显示有单元数量，这样奇特的数据结构会导致一些怪异的行为。

```
var a = new Array( 3 );
var b = [ undefined, undefined, undefined ];
var c = [];
c.length = 3;
```

 a 和 b 的行为有时相同，有时又大相径庭：

```
a.join( "-" ); // "--"
b.join( "-" ); // "--"
a.map(function(v,i){ return i; }); // [ undefined x 3 ]
b.map(function(v,i){ return i; }); // [ 0, 1, 2 ]
```

我们可以通过下述方式来创建包含 undefined 单元（而非“空单元”）的数组：

```
var a = Array.apply( null, { length: 3 } );
```

Array.apply(..) 调用 Array(..) 函数，并且将 { length: 3 } 作为函数的参数。我们可以设想 apply(..) 内部有一个 for 循环（与上述 join(..) 类似），从 0 开始循环到length（即循环到 2，不包括 3）。假设在 apply(..) 内部该数组参数名为 arr，for 循环就会这样来遍历数组：arr[0]、arr[1]、arr[2]。 然 而， 由 于 { length: 3 } 中 并 不 存 在 这 些 属 性， 所 以 返 回 值 为undefined。

虽然 Array.apply( null, { length: 3 } ) 在创建 undefined 值的数组时有些奇怪和繁琐，但是其结果远比 Array(3) 更准确可靠。

==永远不要创建和使用空单元数组。==

### Object、Function、 RegExp

除非万不得已，否则尽量不要使用 Object(..)/Function(..)/RegExp(..)

**Object**

实际情况中没有必要使用 new Object() 来创建对象，因为这样就无法像常量形式那样一次设定多个属性，而必须逐一设定。

**Function**

构造函数 Function 只在极少数情况下很有用，比如动态定义函数参数和函数体的时候。

**RegExp**

强烈建议使用常量形式（如 /^a*b+/g）来定义正则表达式，这样不仅语法简单，执行效率也更高，因为 JavaScript 引擎在代码执行前会对它们进行预编译和缓存。

与前面的构造函数不同，RegExp(..) 有时还是很有用的，比如动态定义正则表达式时：

```
var name = "Kyle";
var namePattern = new RegExp( "\\b(?:" + name + ")+\\b", "ig" );
var matches = someText.match( namePattern );
```

### Date 和 Error

**Date**

创建日期对象必须使用 new Date()。Date(..) 可以带参数，用来指定日期和时间，而不带参数的话则使用当前的日期和时间。

**Error**

创建错误对象（error object）主要是为了获得当前运行栈的上下文（大部分 JavaScript 引擎通过只读属性 .stack 来访问）。栈上下文信息包括函数调用栈信息和产生错误的代码行号，以便于调试（debug）。

# 强制类型转换

显式的情况将值从一种类型转换为另一种类型通常称为类型转换（type casting）；隐式的情况称为强制类型转换（coercion）。

也可以这样来区分：类型转换发生在静态类型语言的编译阶段，而强制类型转换则发生在动态类型语言的运行时（runtime）。

```
var a = 42;

var b = a + ""; // 隐式强制类型转换
var c = String( a ); // 显式强制类型转换
```

## 抽象值操作

### toString

**基本类型**

基本类型值的字符串化规则为：

null 转换为 "null"，undefined 转换为 "undefined"，true转换为 "true"。

数字的字符串化则遵循通用规则，极小和极大的数字使用指数形式：

```
// 1.07 连续乘以七个 1000
var a = 1.07 * 1000 * 1000 * 1000 * 1000 * 1000 * 1000 * 1000;

// 七个1000一共21位数字
a.toString(); // "1.07e21"
```

**简单对象**

除非自行定义，否则 toString()（Object.prototype.toString()）返回内部属性 [[Class]] 的值，如 "[object Object]"。

如果对象有自己的 toString() 方法， toString 就不必追寻到原型链的源头，字符串化时就会调用该方法并使用其返回值。

例如数组的默认 toString() 方法经过了重新定义，将所有单元字符串化以后再用 "," 连接起来：

```
var a = [1,2,3];

a.toString(); // "1,2,3"
```

**JSON 字符串化**

所有安全的 JSON 值（JSON-safe）都可以使用 JSON.stringify(..) 字符串化。安全的 JSON 值是指能够呈现为有效 JSON 格式的值。

不安全的 JSON 值

- undefined
- function
- symbol
- 循环引用的对象

JSON.stringify(..) 在对象中遇到 undefined、function 和 symbol 时会自动将其忽略，在数组中则会返回 null（以保证单元位置不变）。

如果要对含有非法 JSON 值的对象做字符串化，或者对象中的某些值无法被序列化时，就需要定义 toJSON() 方法来返回一个安全的 JSON 值。对象中定义了 toJSON() 方法，JSON 字符串化时会首先调用该方法。

### toNumber

**基本类型**

true 转换为 1，false 转换为 0。undefined 转换为 NaN，null 转换为 0。

处理字符串时遵循数字常量的相关规则 / 语法。值得注意的是：==处理失败==

==时返回 NaN==，==对以 0 开头的====十六进制数并不按十六进制处理而是按十进制==

​

**对象**

对象（包括数组）会首先被转换为相应的基本类型值，如果返回的是非数字的基本类型值，则再遵循以上规则将其强制转换为数字。

为了将值转换为相应的基本类型值，抽象操作 ToPrimitive会首先检查该值是否有 valueOf() 方法。如果有并且返回基本类型值，就使用该值进行强制类型转换。如果没有就使用 toString()的返回值来进行强制类型转换。valueOf() 和 toString() 均不返回基本类型值，会产生 TypeError 错误。

### toBoolean

**假值**

- undefined
- null
- false
- +0、-0 和 NaN
- ""

==除假值外的所有内容都会转成true==

## 显式强制类型转换

### 字符串与数字

比较通用的方法是 String（） 和 Number（）

```
var a = 42;
var b = a.toString();

var c = "3.14";
var d = +c;

b; // "42"
d; // 3.14
```

> 在 JavaScript 开源社区中，一元运算 + 被普遍认为是显式强制类型转换。

有时候也会产生误会，当一元运算 + 和 - 与其他操作符用会产生意料之外的结果, **必须用空格隔开**。**尽量不要把一元操作符和其他运算符一起使用**

```
var c = "3.14";


var d = 5++c; // error
var d = 5+ +c; // 8.14 
```

#### 日期转换成数字

一元操作符的另一个常见的用途是将日期强制转换成数字，结果为时间戳。

> JavaScript 有一处奇特的语法，即构造函数没有参数时可以不用带 ()。于是我们可能会碰到 var timestamp = +new Date; 这样的写法。这样能否提高代码可读性还存在争议，因为这仅用于 new fn()，对一般的函数调用 fn() 并不适用。

### 显式转化成布尔值

通用的方式是 Boolean（）

与前面的 + 一元操作符类似， 一元操作符 ！显式的将值转换成布尔值。所以显式强制类型转换为布尔值最常用的方法是 !!

## 隐式强制类型转换

### 字符串与数字隐式转换

对于 + 运算符， 如果其中一个操作数是对象（包括数组），则首先对其调用ToPrimitive 抽象操作，该抽象操作再调用 [[DefaultValue]]，以数字作为上下文。

如果 + 的其中一个操作数是字符串（或者通过以上步骤可以得到字符串），则执行字符串拼接；否则执行数字加法。

```
var a = [1,2];
var b = [3,4];
a + b; // "1,23,4"
```

**\- 是数字减法运算符，因此 a - 0 会将 a 强制类型转换为数字**。也可以使用 a * 1 和 a / 1，因为这两个运算符也只适用于数字，只不过这样的用法不太常见。

```
’3.14‘ - 0 // 3.14

var a = [3];
var b = [1];
a - b; // 2
```

### 布尔值到数字的隐式强制类型转换

由布尔值转成数字可以用于处理复杂的判断逻辑。true 是 1 ，false  是 0。

```
// 判断只有一个为true
function onlyOne(a,b,c) {
 return !!((a && !b && !c) ||
 (!a && b && !c) || (!a && !b && c));
}

function onlyOne() { // 可读性大大提高了
 var sum = 0;
 for (var i=0; i < arguments.length; i++) {
   // 跳过假值，和处理0一样，但是避免了NaN
   if (arguments[i]) {
    sum += arguments[i];
   }
 }
 return sum == 1;
}
```

### 抽象相等

有几个非常规的情况需要注意。

- NaN 不等于 NaN
- +0 等于 -0

# 语法

## 语句和表达式

### 语句的结果值

语句都有一个结果值

获得结果值最直接的方法是在浏览器开发控制台中输入语句，默认情况下控制台会显示所执行的最后一条语句的结果值。

代码块的结果值就如同一个隐式的返回，即返回最后一个语句的结果值。但是语法不允许我们将语句的结果值赋值给其他变量。

### 表达式的副作用

++ 在前面时，如 ++a，它的副作用（将 a 递增）产生*在表达式返回结果值之前*，而 a++ 的副作用则产生在之后

```
var a = 42;
var b = a++;
a; // 43
b; // 42
```

有人误以为可以用括号 ( ) 将 a++ 的副作用封装起来。事实并非如此。( ) 本身并不是一个封装表达式，不会在表达式 a++ 产生副作用之后执行。

```
var a = 42;
var b = (a++);
a; // 43
b; // 42
```

但也不是没有办法，可以使用 , 语句系列逗号运算符（statement-series comma operator）将多个独立的表达式语句串联成一个语句：

```
var a = 42, b;
b = ( a++, a );
a; // 43
b; // 43
```

### 上下文规则

有一个坑常被提到（涉及强制类型转换，参见第 4 章）：

```
[] + {}; // "[object Object]"

{} + []; // 0
```

表面上看 + 运算符根据第一个操作数（[] 或 {}）的不同会产生不同的结果，实则不然。

第一行代码中，{} 出现在 + 运算符表达式中，因此它被当作一个值（空对象）来处理。第4 章讲过 [] 会被强制类型转换为 ""，而 {} 会被强制类型转换为 "[object Object]"。

但在第二行代码中，{} 被当作一个独立的空代码块（不执行任何操作）。代码块结尾不需要分号，所以这里不存在语法上的问题。最后 + [] 将 [] 显式强制类型转换（参见第 4 章）为 0。

很多人误以为 JavaScript 中有 else if，因为我们可以这样来写代码：

```
if (a) { 
 // ..
}
else if (b) {
 // .. 
}
else { 
 // ..
}
```

事实上 JavaScript 没有 else if，但 if 和 else 只包含单条语句的时候可以省略代码块的 { }。所以我们经常用的 else if 其实本质是这样的：

```
if (a) { 
 // ..
} 
else {
 if (b) { 
 // ..
 } 
 else {
 // .. 
 }
}
```

## 运算符的优先级

 , 来连接一系列语句的时候，它的优先级最低，其他操作数的优先级都比它高。

**条件运算符**：  && 运算符的优先级高于 ||，而 || 的优先级又高于 ? :


# 异步

## 异步： 现在和未来

### 事件循环

JavaScript 引擎并不是独立运行的，它运行在宿主环境中，对多数开发者来说通常就是Web 浏览器。经过最近几年（不仅于此）的发展，JavaScript 已经超出了浏览器的范围，进入了其他环境，比如通过像 Node.js 这样的工具进入服务器领域。实际上，JavaScript 现如今已经嵌入到了从机器人到电灯泡等各种各样的设备中。

JavaScript 引擎本身并没有时间的概念，只是一个按需执行 JavaScript 任意代码片段的环境。“事件”（JavaScript 代码执行）调度总是由包含它的环境进行。

setTimeout(..) 并没有把你的回调函数挂在事件循环队列中。它所做的是设定一个定时器。当定时器到时后，环境会把你的回调函数放在事件循环中，这样，在未来某个时刻的 tick 会摘下并执行这个回调。此时若队列中已经存在了其他的事件，那么setTimeout还需要排队，这也解释了为什么 setTimeout(..) 定时器的精度可能不高。

### 并行线程

异步是关于现在和将来的时间间隙，而并行是关于能够同时发生的事情。

并行计算最常见的工具就是进程和线程。进程和线程独立运行，并可能同时运行：在不同的处理器，甚至不同的计算机上，但多个线程能够共享单个进程的内存。

与之相对的是，事件循环把自身的工作分成一个个任务并顺序执行，不允许对共享内存的并行访问和修改。通过分立线程中彼此合作的事件循环，并行和顺序执行可以共存。

并行线程的交替执行和异步事件的交替调度，其粒度是完全不同的。

### 并发

**非交互**

两个或多个“进程”在同一个程序内并发地交替运行它们的步骤 / 事件时，如果这些任务彼此不相关，就不一定需要交互。如果进程间没有相互影响的话，不确定性是完全可以接受的。

**交互**

更常见的情况是，并发的“进程”需要相互交流，通过作用域或 DOM 间接交互。如果出现这样的交互，就需要对它们的交互进行协调以避免竞态的出现。

处理顺序相关竞态条件（协调交互顺序）：

```
var res = []; 
function response(data) { 
 if (data.url == "http://some.url.1") { 
 res[0] = data; 
 } 
 else if (data.url == "http://some.url.2") { 
 res[1] = data; 
 } 
} 
// ajax(..)是某个库中提供的某个Ajax函数
ajax( "http://some.url.1", response ); 
ajax( "http://some.url.2", response );
```

处理多资源竟态：

```
// ajax(..)是某个库中的某个Ajax函数
ajax( "http://some.url.1", foo ); 
ajax( "http://some.url.2", bar );
```

```
// 同时需要多个资源
var a, b; 
function foo(x) { 
 a = x * 2; 
 if (a && b) { 
 baz(); 
 } 
} 
function bar(y) { 
 b = y * 2; 
 if (a && b) { 
 baz(); 
 } 
} 
function baz() { 
 console.log( a + b ); 
} 
```

```
// 竞争单个资源
var a; 
function foo(x) { 
 if (!a) { 
 a = x * 2; 
 baz(); 
 } 
} 
function bar(x) { 
 if (!a) { 
 a = x / 2; 
 baz(); 
 } 
} 
function baz() { 
 console.log( a ); 
}
```

### 协作

还有一种并发合作方式，称为并发协作（cooperative concurrency）。这里的重点不再是通过共享作用域中的值进行交互。这里的目标是取到一个长期运行的“进程”，并将其分割成多个步骤或多批任务，使得其他并发“进程”有机会将自己的运算插入到事件循环队列中交替运行。

当处理大量数据的时候，如果我们一致处理这部分的数据，这会导致期间的其他必须的js都得不到执行，这是非常糟糕的。我们可以通过 *异步地批处理* 这些结果。每次处理之后返回事件循环，让其他等待事件有机会运行。

```
// response(..)从Ajax调用中取得结果数组
function response(data) { 
 // 一次处理1000个
 var chunk = data.splice( 0, 1000 ); 
 // 添加到已有的res组
 res = res.concat( 
 // 创建一个新的数组把chunk中所有值加倍
 chunk.map( function(val){ 
 return val * 2; 
 } ) 
 ); 
 // 还有剩下的需要处理吗？
 if (data.length > 0) { 
 // 异步调度下一次批处理
 setTimeout( function(){ 
 response( data ); 
 }, 0 ); 
 } 
}
```

### 任务

为对于任务队列最好的理解方式就是，它是挂在事件循环队列的每个 tick 之后的一个队列。在事件循环的每个 tick 中，可能出现的异步动作不会导致一个完整的新事件添加到事件循环队列中，而会在当前 tick 的任务队列末尾添加一个项目（一个任务）。

## Promise

基础知识可以参考： [ES6 Promise对象 - ES6文档 (caibaojian.com)](http://caibaojian.com/es6/promise.html)

### 具有 then 方法的鸭子类型

是如何确定某个值是不是真正的 Promise？

既然 Promise 是通过 new Promise(..) 语法创建的，那你可能就认为可以通过 p instanceof Promise 来检查。但遗憾的是，这并不足以作为检查方法，原因有许多。

1. 最主要的是，Promise 值可能是从其他浏览器窗口（iframe 等）接收到的。这个浏览器窗口自己的 Promise 可能和当前窗口 /frame 的不同，因此这样的检查无法识别 Promise实例。
2. 库或框架可能会选择实现自己的 Promise，而不是使用原生 ES6 Promise 实现。实际上，很有可能你是在早期根本没有 Promise 实现的浏览器中使用由库提供的 Promise。

正确识别 Promise（或者行为类似于 Promise 的东西）的方式就是定义某种称为 thenable 的东西，将其定义为任何具有 then(..) 方法的对象和函数。我们认为，任何这样的值就是 Promise 一致的 thenable。

> 根据一个值的形态（具有哪些属性）对这个值的类型做出一些假定。这种类型检查（type check）一般用术语鸭子类型（duck typing）来表示——“如果它看起来像只鸭子，叫起来像只鸭子，那它一定就是只鸭子”

如果你试图使用恰好有 then(..) 函数的一个对象或函数值完成一个 Promise，但并不希望它被当作 Promise 或 thenable，那就有点麻烦了，因为它会自动被识别为 thenable，并被按照特定的规则处理（参见本章后面的内容）。

如果有任何其他代码无意或恶意地给 Object.prototype、Array.prototype 或任何其他原生原型添加 then(..)，你无法控制也无法预测。并且，如果指定的是不调用其参数作为回调的函数，那么如果有 Promise 决议到这样的值，就会永远挂住！

标准决定劫持之前未保留的——听起来是完全通用的——属性名 then。这意味着所有值（或其委托），不管是过去的、现存的还是未来的，都不能拥有 then(..) 函数，不管是有意的还是无意的；否则这个值在 Promise 系统中就会被误认为是一个 thenable，这可能会导致非常难以追踪的 bug。

### 错误处理

Promise 的错误处理易于出错

```
var p = Promise.resolve( 42 ); 
p.then( 
 function fulfilled(msg){ 
 // 数字没有string函数，所以会抛出错误
 console.log( msg.toLowerCase() ); 
 }, 
 function rejected(err){ 
 // 永远不会到达这里
 } 
);
```

如果 msg.toLowerCase() 合法地抛出一个错误（事实确实如此！），为什么我们的错误处理函数没有得到通知呢？正如前面解释过的，这是因为那个错误处理函数是为 promise p 准备的，而这个 promise 已经用值 42 填充了。promise p 是不可变的，所以唯一可以被通知这个错误的 promise 是从 p.then(..) 返回的那一个，但我们在此例中没有捕捉。

> 如果通过无效的方式使用 Promise API，并且出现一个错误阻碍了正常的 Promise 构造，那么结果会得到一个立即抛出的异常，而不是一个被拒绝的 Promise。这里是一些错误使用导致 Promise 构造失败的例子：new Promise(null)、Promise.all()、Promise.race(42)，等等。
>
> 如果一开始你就没能有效使用 Promise API 真正构造出一个 Promise，那就无法得到一个被拒绝的 Promise ！

#### 处理未捕获的错误

**以catch结束promise**

为了避免丢失被忽略和抛弃的 Promise 错误，一些开发者表示，Promise 链的一个最佳实践就是最后总以一个 catch(..) 结束

**全局未处理拒绝**

有些 Promise 库增加了一些方法，用于注册一个类似于“全局未处理拒绝”处理函数的东西，这样就不会抛出全局错误，而是调用这个函数。

但它们辨识未捕获错误的方法是定义一个某个时长的定时器，比如 3 秒钟，在拒绝的时刻启动。如果 Promise 被拒绝，而在定时器触发之前都没有错误处理函数被注册，那它就会假定你不会注册处理函数，进而就是未被捕获错误。

但是这种模式可能会有些麻烦，因为 3 秒这个时间太随意了（即使是经验值），也因为确实有一些情况下会需要 Promise 在一段不确定的时间内保持其拒绝状态。而且你绝对不希望因为这些误报（还没被处理的未捕获错误）而调用未捕获错误处理函数。

**done 函数标示 Promise 结束**

Promsie 应该添加一个 done(..) 函数，从本质上标识 Promsie 链的结束。done(..) 不会创建和返回 Promise，所以传递给 done(..) 的回调显然不会报告一个并不存在的链接 Promise 的问题。

但目前最大的问题是，它并不是 ES6 标准的一部分，所以不管听起来怎么好，要成为可靠的普遍解决方案，它还有很长一段路要走。

### 成功的坑

接下来的内容只是理论上的，关于未来的 Promise 可以变成什么样。

还有，如果你认真对待的话，它可能是可以 polyfifill/prollyfifill 的。我们来看一下。

- 默认情况下，Promsie 在下一个任务或时间循环 tick 上（向开发者终端）报告所有拒绝，如果在这个时间点上该 Promise 上还没有注册错误处理函数。
- 如果想要一个被拒绝的 Promise 在查看之前的某个时间段内保持被拒绝状态，可以调用defer()，这个函数优先级高于该 Promise 的自动错误报告。

### Promise 局限性

#### 顺序错误处理

由于一个 Promise 链仅仅是连接到一起的成员 Promise，没有把整个链标识为一个个体的实体，这意味着没有外部方法可以用于观察可能发生的错误。没有这样的为 Promise 链序列的中间步骤保留的引用，你就无法关联错误处理函数来可靠地检查错误。

如果构建了一个没有错误处理函数的 Promise 链，链中任何地方的任何错误都会在链中一直传播下去，直到被查看。

#### 单一值

Promise 只能有一个完成值或一个拒绝理由

一般的建议是构造一个对象或数组来保持这样的多个信息，但要在 Promise 链中的每一步都进行封装和解封，就十分丑陋和笨重了。

**单决议**

Promise 最本质的一个特征是：Promise 只能被决议一次（完成或拒绝）

**无法取消**

一旦创建了一个 Promise 并为其注册了完成和 / 或拒绝处理函数，如果出现某种情况使得这个任务悬而未决的话，你也没有办法从外部停止它的进程。

## 生成器 Generator

基础知识参考： [ES6 Generator函数 - ES6文档 (caibaojian.com)](http://caibaojian.com/es6/generator.html)

### 打破完成运行

在普通的 js 代码中，一个函数一旦开始执行，就会运行到结束，期间不会有其他代码能够打断它并插入其间。

ES6 引入了一个新的函数类型，它并不符合这种运行到结束的特性。这类新的函数被称为生成器。

#### 输入和输出

```
function *foo(x) { 
 var y = x * (yield "Hello"); // <-- yield一个值！
 return y; 
} 
var it = foo( 6 ); 
var res = it.next(); // 第一个next()，并不传入任何东西
res.value; // "Hello" 
res = it.next( 7 ); // 向等待的yield传入7
res.value; // 42
```

首先，传入 6 作为参数 x。然后调用 it.next()，这会启动 *foo(..)。

 在 *foo(..) 内部，开始执行语句 var y = x ..，但随后就遇到了一个 yield 表达式。它就会在这一点上暂停 *foo(..)**（在赋值语句中间！）**，并在本质上要求调用代码为 yield表达式提供一个结果值。

接下来，调用 it.next( 7 )，这一句把值 7 传回作为被暂停的 yield 表达式的结果。

消息是**双向传递**的——yield.. 作为一个表达式可以发出消息响应 next(..) 调用，next(..) 也可以向暂停的 yield 表达式发送值。

#### 多个迭代器

有一个细微之处很容易忽略：每次构建一个迭代器，实际上就隐式构建了生成器的一个实例，通过这个迭代器来控制的是这个生成器实例。

同一个生成器的多个实例可以同时运行，它们甚至可以彼此交互

```
function *foo() { 
 var x = yield 2; 
 z++; 
 var y = yield (x * z); 
 console.log( x, y, z ); 
} 
var z = 1; 
var it1 = foo(); 
var it2 = foo(); 
var val1 = it1.next().value; // 2 <-- yield 2 
var val2 = it2.next().value; // 2 <-- yield 2 
val1 = it1.next( val2 * 10 ).value; // 40 <-- x:20, z:2 
val2 = it2.next( val1 * 5 ).value; // 600 <-- x:200, z:3 
it1.next( val2 / 2 ); // y:300 
 // 20 300 3 
it2.next( val1 / 4 ); // y:10 
 // 200 10 3
```

在普通的 js 函数当中，函数的执行顺序一般是固定的。但是使用生成器的话，**交替执行**（甚至在语句当中！）显然是可能的。

### 生成器产生值

#### 生产者和迭代器

假定你要产生一系列值，其中每个值都与前面一个有特定的关系。要实现这一点，需要一个有状态的生产者能够记住其生成的最后一个值。

我们可以通过两种方式自动迭代标准迭代器：

```
// 自动调用 next()，它不会向 next() 传入任何值，并且会在接收到 done:true 之后自动停止。
for (var v of something) { 
 console.log( v ); 
 // 不要死循环！
 if (v > 500) break;
} 
// 1 9 33 105 321 969


// 语法丑陋，但其优点是，这样就可以在需要时向 next() 传递值
for ( 
 var ret; 
 (ret = something.next()) && !ret.done; 
) { 
 console.log( ret.value ); 
 // 不要死循环！
 if (ret.value > 500) break;
} 
```

#### iterable

一个对象被称为迭代器，因为它的接口中有一个 next() 方法。而与其紧密相关的一个术语是 iterable（可迭代），即指一个包含可以在其值上迭代的迭代器的对象。

从 ES6 开始，从一个 iterable 中提取迭代器的方法是：iterable 必须支持一个函数，其名称是专门的 ES6 符号值 Symbol.iterator。调用这个函数时，它会返回一个迭代器。通常每次调用会返回一个全新的迭代器，虽然这一点并不是必须的。

#### 生成器迭代器

可以把生成器看作一个值的生产者，我们通过迭代器接口的 next() 调用一次提取出一个值。

严格说来，生成器本身并不是 iterable，尽管非常类似——当你执行一个生成器，就得到了一个迭代器。

> 在实际的 JavaScript 程序中使用 while..true 循环是非常糟糕的主意，至少如果其中没有 break 或 return 的话是这样，因为它有可能会同步地无限循环，并阻塞和锁住浏览器 UI。
>
> 但是，如果在生成器中有 yield 的话，使用这样的循环就完全没有问题。因为生成器会在每次迭代中暂停，通过 yield 返回到主程序或事件循环队列中。

### 异步迭代生成器

在平时的请求当中

```
function foo(x,y,cb) { 
 ajax( 
 "http://some.url.1/?x=" + x + "&y=" + y, 
 cb 
 ); 
} 
foo( 11, 31, function(err,text) { 
 if (err) { 
 console.error( err ); 
 } 
 else { 
 console.log( text ); 
 } 
} );
```

当用生成器实现：

```
function foo(x,y) { 
 ajax( 
 "http://some.url.1/?x=" + x + "&y=" + y, 
 function(err,data){ 
 if (err) { 
 // 向*main()抛出一个错误
 it.throw( err ); 
 } 
 else { 
 // 用收到的data恢复*main() 
 it.next( data ); 
 } 
 } 
 ); 
} 
function *main() { 
 try { 
 var text = yield foo( 11, 31 ); 
 console.log( text ); 
 } 
 catch (err) { 
 console.error( err ); 
 } 
} 
var it = main(); 
// 这里启动！
it.next();
```

我们在生成器内部有了看似完全同步的代码（除了 yield 关键字本身），但隐藏在背后的是，在 foo(..) 内的运行可以完全异步。

在我们的大脑中，将异步请求放在回调中是一个混乱的方式，并且这很容易导致回调地狱。

生成器是巨大的改进！对于我们前面陈述的回调无法以顺序同步的、符合我们大脑思考模式的方式表达异步这个问题，这是一个近乎完美的解决方案。

#### 同步的错误处理

 yield 暂停时也使得生成器能够捕获错误。

在异步代码中实现看似同步的错误处理（通过 try..catch）在可读性和合理性方面都是一个巨大的进步。

### 生成器 +Promise

ES6 中最完美的世界就是生成器（看似同步的异步代码）和 Promise（可信任可组合）的结合。

获得 Promise 和生成器最大效用的最自然的方法就是 yield 出来一个 Promise，然后通过这个 Promise 来控制生成器的迭代器。

```
function foo(x,y) { 
 return request( 
 "http://some.url.1/?x=" + x + "&y=" + y 
 ); 
} 
function *main() { 
 try { 
 var text = yield foo( 11, 31 ); 
 console.log( text ); 
 } 
 catch (err) { 
 console.error( err ); 
 } 
}

var it = main(); 
var p = it.next().value; 
// 等待promise p决议
p.then( 
 function(text){ 
     it.next( text ); 
 }, 
 function(err){ 
     it.throw( err ); 
 } 
);
```

#### 支持 Promise 的 Generator Runner

```
function run(gen) {
  const args = [].slice.call(arguments, 1), it
  // 在当前上下文中初始化生成器
  it = gen.apply(this, args)
  // 返回一个promise用于生成器完成
  return Promise.resolve()
    .then(function handleNext(value) {
    // 对下一个yield出的值运行
      const next = it.next(value)
      return (function handleResult(next) {
      // 生成器运行完毕了吗？
        if (next.done) {
          return next.value
        }
        return Promise.resolve(next.value)
          .then(
          // 成功就恢复异步循环，把决议的值发回生成器
            handleNext,
            // 如果value是被拒绝的 promise，
            // 就把错误传回生成器进行出错处理
            function handleErr(err) {
              return Promise.resolve(
                it.throw(err)
              ).then(handleResult)
            }
          )
      }(next))
    })
}
```

这种运行 run(..) 的方式，它会自动异步运行你传给它的生成器，直到结束。

#### 生成器中的 Promise 并发

当我们需要并发发送请求并且统一处理结果的时候，generator 的一般写法会是：

```
function *foo() { 
 var r1 = yield request( "http://some.url.1" ); 
 var r2 = yield request( "http://some.url.2" ); 
 var r3 = yield request( 
 "http://some.url.3/?v=" + r1 + "," + r2 
 ); 
 console.log( r3 ); 
} 
// 使用前面定义的工具run(..) 
run( foo );
```

这样的方式会导致请求不是一起发送的，慢了很多，有了 promise 我们就可以很方便的解决这个问题。

```
function *foo() { 
 // 让两个请求"并行"，并等待两个promise都决议
 var results = yield Promise.all( [ 
 request( "http://some.url.1" ), 
 request( "http://some.url.2" ) 
 ] ); 
 var r1 = results[0]; 
 var r2 = results[1]; 
 var r3 = yield request( 
 "http://some.url.3/?v=" + r1 + "," + r2 
 ); 
 console.log( r3 ); 
}
```

#### 生成器委托

```
function *foo() { 
  console.log( "*foo() starting" ); 
  yield 3;
  yield 4;
  console.log( "*foo() finished" );
}
function *bar() {
  yield 1;
  yield 2;
  yield *foo(); // yield委托！
  yield 5;
}
var it = bar(); 
it.next().value; // 1 
it.next().value; // 2 
it.next().value; // *foo()启动
 // 3 
it.next().value; // 4 
it.next().value; // *foo()完成
 // 5
```

调用 foo() 创建一个迭代器。然后 yield * 把迭代器实例控制（当前 *bar() 生成器的）委托给 / 转移到了这另一个 *foo() 迭代器。一旦 it 迭代器控制消耗了整个 *foo() 迭代器，it 就会自动转回控制 *bar()。

**非重点内容仅作了解 。。。。。**

## 程序性能

### Web Worker

如果你有一些处理密集型的任务要执行，但不希望它们都在主线程运行（这可能会减慢浏览器 /UI），可能你就会希望 JavaScript 能够以多线程的方式运行。

你还需要注意一些问题。一个就是，你会想要知道在独立的线程运行是否意味着它可以并行运行（在多 CPU/ 核心的系统上），这样第二个线程的长时间运行就不会阻塞程序主线程。否则，相比于JavaScript 中已有的异步并发，“虚拟多线程”并不会带来多少好处。

像你的浏览器这样的环境，很容易提供多个 JavaScript 引擎实例，各自运行在自己的线程上，这样你可以在每个线程上运行不同的程序。程序中每一个这样的独立的多线程部分被称为一个（Web）Worker。这种类型的并行化被称为任务并行，因为其重点在于把程序划分为多个块来并发运行。

Worker 之间以及它们和主程序之间，不会共享任何作用域或资源，那会把所有多线程编程的噩梦带到前端领域，而是通过一个基本的事件消息机制相互联系。

```
// 创建 web worker
var w1 = new Worker( "http://some.url.1/mycoolworker.js" );
w1.addEventListener( "message", function(evt){ 
 // evt.data 
} );
w1.postMessage( "something cool to say" );

// "mycoolworker.js" 
addEventListener( "message", function(evt){ 
 // evt.data 
} ); 
postMessage( "a really cool reply" );
```

#### worker 环境

 Worker 内部是无法访问主程序的任何资源的。这意味着你不能访问它的任何全局变量，也不能访问页面的 DOM 或者其他资源。记住，这是一个完全独立的线程。

你可以执行网络操作（Ajax、WebSockets）以及设定定时器。还有，Worker 可以访问几个重要的全局变量和功能的本地复本，包括 navigator、location、JSON 和 applicationCache。

你还可以通过 importScripts(..) 向 Worker 加载额外的 JavaScript 脚本：

```
// 在Worker内部
importScripts( "foo.js", "bar.js" ); 
```

**Web Worker 用途**

- 处理密集型数学计算
- 大数据集排序
- 数据处理（压缩、音频分析、图像处理等）
- 高流量网络通信

#### 数据传递

需要在线程之间通过事件机制传递大量的信息，可能是双向的。

在早期的 Worker 中，唯一的选择就是把所有数据序列化到一个字符串值中。除了双向序列化导致的速度损失之外，另一个主要的负面因素是数据需要被复制，这意味着两倍的内存使用

如果要传递一个对象，可以使用 [结构化克隆算法](https://developer.mozilla.org/enUS/docs/Web/Guide/API/DOM/The_structured_clone_algorithm)把这个对象复制到另一边。这样就不用付出 to-string 和 from-string 的性能损失了，但是这种方案还是要使用双倍的内存。

更好的选择，特别是对于大数据集而言，就是使用[Transferable 对象](http://updates.html5rocks.com/2011/12/Transferable-Objects-Lightning-Fast)。这时发生的是对象所有权的转移，数据本身并没有移动。一旦你把对象传递到一个 Worker 中，在原来的位置上，它就变为空的或者是不可访问的，这样就消除了多线程编程作用域共享带来的混乱。当然，所有权传递是可以双向进行的。

#### 共享 Worker

如果你的站点或 app 允许加载同一个页面的多个 tab（一个常见的功能），那你可能非常希望通过防止重复专用 Worker 来降低系统的资源使用。

创建一个整个站点或 app 的所有页面实例都可以共享的中心 Worker（只有 Firefox 和 Chrome 支持这一功能）：

```
var w1 = new SharedWorker( "http://some.url.1/mycoolworker.js" );
w1.port.addEventListener( "message", handleMessages ); 
// .. 
w1.port.postMessage( "something cool" );
w1.port.start();
```

共享 Worker 可以与站点的多个程序实例或多个页面连接，所以这个 Worker 需要通过某种方式来得知消息来自于哪个程序。这个唯一标识符称为端口（port），可以类比网络 socket 的端口。因此，调用程序必须使用 Worker 的 port 对象用于通信

共享 Worker 内部，必须要处理额外的一个事件："connect"。这个事件为这个特定的连接提供了端口对象。

```
// 在共享Worker内部
addEventListener( "connect", function(evt){ 
 // 这个连接分配的端口
 var port = evt.ports[0]; 
 port.addEventListener( "message", function(evt){ 
 // .. 
 port.postMessage( .. ); 
 // .. 
 } ); 
 // 初始化端口连接
 port.start(); 
} );
```

### SIMD

单指令多数据（SIMD）是一种数据并行（data parallelism）方式，与 Web Worker 的任务并行（task parallelism）相对，因为这里的重点实际上不再是把程序逻辑分成并行的块，而是并行处理数据的多个位。

。。。 只做了解

# 性能测试与调优

## 性能测试

```
var start = (new Date()).getTime(); // 或者Date.now() 
// 进行一些操作
var end = (new Date()).getTime(); 
console.log( "Duration:", (end - start) );
```

如果报告的时间是 0，可能你会认为它的执行时间小于 1ms。但是，这并不十分精确。有些平台的精度并没有达到 1ms，而是以更大的递增间隔更新定时器。比如，Windows（也就是 IE）的早期版本上的精度只有 15ms，这就意味着这个运算的运行时间至少需要这么长才不会被报告为 0 ！

### 重复

那就用一个循环把它包起来，这样整个测试的运行时间就会更长一些了。”如果重复一个运算 100 次，然后整个循环报告共消耗了 137ms，那你就可以把它除以 100，得到每次运算的平均用时为 1.37ms，是这样吗？

**并不完全是这样！**

重复执行的时间长度应该根据使用的定时器的精度而定，专门用来最小化不精确性。定时器的精度越低，你需要运行的时间就越长，这样才能确保错误率最小化。

15ms 的定时器对于精确的性能测试来说是非常差劲的。要最小化它的不确定性（也就是出错率）到小于 1%，需要把你的每轮测试迭代运行 750ms。而 1ms 定时器时只需要每轮运行 50ms就可以达到同样的置信度。

> 也就是说，要验证一个函数的运行时间，需要将运行的时间放大到浏览器定时器时间的50倍才可以认定数据可靠

### [Benchmark.js](https://calendar.perfplanet.com/2010/bulletproof-javascript-benchmarks/)
