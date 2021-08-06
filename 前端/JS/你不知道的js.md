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

2.  接下来编译器会为引擎生成运行时所需的代码，这些代码被用来处理 a = 2 这个赋值操作。引擎运行时会首先询问作用域，在当前的作用域集合中是否存在一个叫作 a 的变量。如果是，引擎就会使用这个变量；如果否，引擎会继续查找该变量（查看 1.3节）。

    

如果引擎最终找到了 a 变量，就会将 2 赋值给它。否则引擎就会举手示意并抛出一个异常！





#### 引擎寻找变量



引擎查找变量的两种方式：

1. **LHS** ：左侧查找

    ​	`a = 2` 这段代码是一个 LHS 引用查找，不关心当前的值，只想为 `= 2` 寻找一个目标。关心目标

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

![image-20210602235821537](../../笔记图片/词法作用域.png)

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

以这个表达式为例，你可能会认为这是一个生命，但是 JS 实际上会将其看作两个声明，`var a` 和 `a = 2`. 其中 `var a ` 会提升到代码的最上面，在**编译阶段**就进行；而 `a = 2` 会被留在原地，等待代码执行到这里。







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



立即执行函数不是闭包











# --------------------------------





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



# ---------------------



# 对象





### 类型 和 内容

• String

• Number

• Boolean

• Object

• Function

• Array

• RegExp

• Error



这些内置对象从表现形式来说很像其他语言中的类型（type）或者类（class），比如 Java中的 String 类。但是在 JavaScript 中，它们实际上只是一些内置函数。这些内置函数可以当作构造函数来使用，从而可以构造一个对应子类型的新对象。





在读取对象的内容时，有两种方式

1. 属性访问。 `obj.a`
2. 键访问。 `obj["a"]`



在对象中，属性名永远是字符串，如果使用 string 以外的其他值作为属性名，都会被转化成一个字符串。在数组对象中使用的的确是数字，但是也会被转换成字符串。



#### 存在性

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



### 类理论

面向对象编程强调的是数据和操作数据的行为本质上是互相关联的（当然，不同的数据有不同的行为），因此好的设计就是把数据以及和它相关的行为打包（或者说封装）起来。



类的特性： 封装、继承和多态



**多重继承**

多重继承可以让一个类同事继承多个父类。但是这样会出现如果两个父类都存在同一个方法时，子类不知道应该采用谁的。



### 混入

当进行继承和实例化的时候， JS 并不会自动执行复制行为，因为在 JS 中不存在类的概念，JS 中只有对象和函数。

因此 js 的开发者相处了一个方法模拟类的复制，这个方法就是混入。



#### 显式混入

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





#### 隐式混入



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

### Prototype

JS 所有的对象都有一个Prototype属性，几乎所有的对象在创建时 prototype 属性都会赋予一个非空值。



**Prototype 用处？**

对于我们默认的 Get 操作来说，当我们读区一个对象上的属性，如果我们不能够在对象上找到，那么会继续在对象的 prototype 上去寻找。



> ES6 中的 Proxy 超出了这个限制，当存在Proxy 的时候，这个规则就不适用



#### 属性设置和屏蔽



`myObject.foo = "bar";`



如果 myObject 对象中包含名为 foo 的普通数据访问属性，这条赋值语句只会修改已有的属性值。

如果 foo 不是直接存在于 myObject 中，[[Prototype]] 链就会被遍历，类似 [[Get]] 操作。如果原型链上找不到 foo，foo 就会被直接添加到 myObject 上。



如果 foo 存在于原型链上层，赋值语句 myObject.foo = "bar" 的行为就会有些不同

1. 如果在 [[Prototype]] 链上层存在名为 foo 的普通数据访问属性（参见第 3 章）并且没有被标记为只读（writable:false），那就会直接在 myObject 中添加一个名为 foo 的新属性，它是**屏蔽属性**
2. 如果在 [[Prototype]] 链上层存在 foo，但是它被标记为只读（writable:false），那么无法修改已有属性或者在 myObject 上创建屏蔽属性。如果运行在严格模式下，代码会抛出一个错误。否则，这条赋值语句会被忽略
3.  如果在 [[Prototype]] 链上层存在 foo 并且它是一个 setter，那就一定会调用这个 setter。foo 不会被添加到（或者说屏蔽于）myObject，也不会重新定义 foo 这 个 setter。



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



### “类”



#### 类函数

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





#### 构造函数

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



##### 构造函数还是调用

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









### 对象关联



#### 创建关联

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







# --------------------------------------





# 类型和值



## 类型



JavaScript 有七种内置类型：

• 空值（null）

• 未定义（undefined）

• 布尔值（ boolean）

• 数字（number）

• 字符串（string）

• 对象（object）

• 符号（symbol，ES6 中新增）



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

• String()

• Number()

• Boolean()

• Array()

• Object()

• Function()

• RegExp()

• Date()

• Error()

• Symbol()——ES6 中新加入的





### 内部属性



所有 typeof 返回值为 "object" 的对象（如数组）都包含一个内部属性 [[Class]]（我们可以把它看作一个内部的分类，而非传统的面向对象意义上的类）。这个属性无法直接访问，一般通过 Object.prototype.toString(..) 来查看。



```
// [[Class]] 属性值是 "Array"
Object.prototype.toString.call( [1,2,3] );// "[object Array]" 
// 正则表达式的值是 "RegExp"
Object.prototype.toString.call( /regex-literal/i );// "[object RegExp]"
```





### 包装对象

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



#### 拆封

想要得到封装对象中的基本类型值，可以使用 valueOf() 函数：

```
var a = new Boolean( false );
if (!a.valueOf()) {
 console.log( "Oops" ); // 可以执行
}
```





### 原生函数作为构造函数



#### Array

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





#### Object、Function、 RegExp



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





#### Date 和 Error



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

• undefined

• null

• false

• +0、-0 和 NaN

• ""



==除假值外的所有内容都会转成true==







## 显式强制类型转换











































