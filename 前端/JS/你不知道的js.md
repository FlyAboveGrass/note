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

























































