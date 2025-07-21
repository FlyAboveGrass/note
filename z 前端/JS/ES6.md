# Generator

## 概念

​	Generator函数是一个状态机，封装了多个内部状态。

​	执行Generator函数会返回一个遍历器对象，也就是说，Generator函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历Generator函数内部的每一个状态。

​	调用Generator函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象。必须调用 这个指针的 `next（）` 函数，才可以使generator 函数向后移动，直到遇到新的 `yield`停下。

​

## Yield 语句

​		每次遇到`yield`，函数暂停执行，下一次再从该位置继续向后执行，而`return`语句不具备位置记忆的功能。

​		一个函数里面，只能执行一次（或者说一个）`return`语句，但是可以执行多次（或者说多个）`yield`语句。正常函数只能返回一个值，因为只能执行一次`return`；Generator函数可以返回一系列的值，因为可以有任意多个`yield`。





`yield`语句如果用在一个表达式之中，必须放在圆括号里面。

用作函数参数或赋值表达式的右边，可以不加括号

```
console.log('Hello' + yield 1); // SyntaxError
console.log('Hello' + (yield 1)); // OK

foo(yield 'a', yield 'b'); // OK
```

## next 方法

​		`yield`句本身没有返回值，或者说总是返回`undefined`。`next`方法可以带一个参数，该参数就会被当作上一个`yield`语句的返回值。由于`next`方法的参数表示上一个`yield`语句的返回值，所以第一次使用`next`方法时，不能带有参数。

​		因为 yield 的返回值实际上是返回给外部的， 所以不能给函数里面赋值。

## for of 循环

​		`for…of`循环可以自动遍历Generator函数时生成的`Iterator`对象，且此时不再需要调用`next`方法。

​		但是一旦`next`方法的返回对象的`done`属性为`true`，`for…of`循环就会中止，且不包含该返回对象

​		除了`for…of`循环以外，扩展运算符（`…`）、解构赋值和`Array.from`方法内部调用的，都是遍历器接口。这意味着，它们都可以将Generator函数返回的Iterator对象，作为参数。

```
function* numbers () {
  yield 1
  yield 2
  return 3
  yield 4
}

// 扩展运算符
[...numbers()] // [1, 2]

// Array.from 方法
Array.from(numbers()) // [1, 2]

// 解构赋值
let [x, y] = numbers();
x // 1
y // 2

// for...of 循环
for (let n of numbers()) {
  console.log(n)
}
// 1
// 2
```

​

## catch 和 return

## yield*

​		在Generater函数内部，调用另一个Generator函数，默认情况下是没有效果的。

​		`yield*`语句，可以在一个Generator函数里面执行另一个Generator函数。
