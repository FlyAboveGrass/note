# 数据类型



js 中的数据类型非为分为基本类型和引用类型两种

**基本类型**

- null
- undefined
- number
    - NaN属于number类型的特殊（不等于任何值，包括自身）
- string
- boolean
- symbol： 唯一且不可变
- bigInt（ES10）



**引用类型**

- Object
    - Function/Array/RegExp/Date 都是特殊的 object







## 堆和栈

|          |          栈          |           堆           |
| -------- | :------------------: | :--------------------: |
| 值大小   |       大小固定       | 大小不固定可以动态调整 |
| 值类型   |     存储基本类型     |      存储引用类型      |
| 占空间   |          小          |           大           |
| 直接操作 | 可以直接操作，效率高 | 不可以直接操作，效率低 |
| 空间分配 |     系统自动分配     |      代码手动分配      |





### 栈

栈中的内存空间大小是固定的，所以栈中的变量是`不可变的`. 当我们对基本类型的变量进行修改的时候，实际上会开辟一块新的内存空间，然后把变量指向新的内存空间。



### 堆

引用类型会存储在堆中，但是会在栈当中存储这个堆内存的地址，变量的值为这个地址。





## 基本类型和引用类型的关系



### 复制

```
let a = 1
let b = a
```

基本类型的复制是值的复制，a和b两者的值是相同的，但是指向的是不同的内存空间，因此在进行操作的时候两个变量不会相互影响。



```
let a = [1, 2]
let b = a
```

引用类型的复制是内存地址的复制，a 和 b 的值实际上只是一个地址，最终指向的还是堆中的同一块内存空间，因此在进行操作的时候两个变量还是会会相互影响。



### 比较

```
let a = 1
let b = 1
a === b // true
```

对于基本类型，比较时只进行值的比较，值相等则二者相等。



```
let a = [1,2]
let b = a
let c = [1,2]

a === b // true
a=== c // false
```

引用类型，比较时会比较它们的引用地址，地址相同则相等；地址不同，即使对象内容一样也不想等。





## 包装类型



```
let a = 'abcde'
a.substring(2) // "cde"
```

这段代码看着没什么问题，平时工作中也是这么用的。但是联系前面的内容仔细一想，为什么 a 明明是一个值，但是却可以在它上面掉用函数呢？



这就是**包装对象**的作用了。当我们试图此时 JavaScript 会自动为基本类型值包装一个封装对象，使它暂时具备对象的能力。例如 上面的代码，会先给 a 包装一层 （new String(a)) .

那有了包装对象之后，是不是基本类型就和引用类型没区别了呢？那倒也不。引用类型在创建之后被销毁前，都会一直存在于内存中。而基本类型的包装对象只会存在于你掉用方法的那一刻，随即销毁。这样的特性导致我们不能够给基本类型添加属性或者方法。

```
let a = 1
a.color = 'red'
a.color // undefined
```



### 装箱和拆箱



- 装箱转换：把基本类型转换为对应的包装类型
- 拆箱操作：把引用类型转换为基本类型



在基本类型上掉用方法的时候，我们会自动进行装箱操作。

如果想要对包装对象进行拆箱，可以使用 valueOf 或者 toString 方法。


# 代码质量

## Polyfill 和转译器

JavaScript 语言在稳步发展。也会定期出现一些对语言的新提议，它们会被分析讨论，如果认为有价值，就会被加入到 [https://tc39.github.io/ecma262/](https://tc39.github.io/ecma262/) 的列表中，然后被加到 [规范](http://www.ecma-international.org/publications/standards/Ecma-262.htm) 中。

因此，一个 JavaScript 引擎只能实现标准中的一部分是很常见的情况。如何让我们现代的代码在还不支持最新特性的旧引擎上工作？

### [转译器（Transpilers）](https://zh.javascript.info/polyfills#zhuan-yi-qi-transpilers)

[转译器](https://en.wikipedia.org/wiki/Source-to-source_compiler) 是一种可以将源码转译成另一种源码的特殊的软件。

例如，在 ES2020 之前没有“空值合并运算符” `??`。所以，如果访问者使用过时了的浏览器访问我们的网页，那么该浏览器可能就不明白 `height = height ?? 100` 这段代码的含义。

转译器会分析我们的代码，并进行重写
```
// 转移前
height = height ?? 100

// 转译后
height = (height !== null && height !== undefined) ? height : 100
```

[Babel](https://babeljs.io/) 是最著名的转译器之一。


## [垫片（Polyfills）](https://zh.javascript.info/polyfills#dian-pian-polyfills)

新的语言特性可能不仅包括语法结构和运算符，还可能包括内建函数。


例如， 在一些（非常过时的）JavaScript 引擎中没有 `Math.trunc` 函数，所以这样的代码会执行失败。
由于我们谈论的是新函数，而不是语法更改，因此无需在此处转译任何内容。我们只需要声明缺失的函数。
更新/添加新函数的脚本被称为“polyfill”。它“填补”了空白并添加了缺失的实现。


```
if (!Math.trunc) { // 如果没有这个函数
  // 实现它
  Math.trunc = function(number) {
    // Math.ceil 和 Math.floor 甚至存在于上古年代的 JavaScript 引擎中
    // 在本教程的后续章节中会讲到它们
    return number < 0 ? Math.ceil(number) : Math.floor(number);
  };
}
```



































