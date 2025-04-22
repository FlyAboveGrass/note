# 面向对象编程

## 封装

```
function fn(name) {
  var age = 12
  this.name = name

  this.logAge = function logAge() {
    return age
  }
}

fn.a = 'a'
fn.prototype.b = 'b'

const fn2 = new fn('name')

console.log({ fn, a: fn.a, b: fn.b })
console.log({ fn2, a: fn2.a, b: fn2.b, age: fn2.logAge() })
```

思考一下上面的代码，他们的输出会是什么呢？ 3-2-1，揭晓谜底：

```
{
    fn: [Function: fn] { a: 'a' },
    a: 'a',
    b: undefined
}

{
  fn2: fn { name: 'name', logAge: [Function: logAge] },
  a: undefined,
  b: 'b',
  age: 12
}
```

全对了么？别急，一个个的解释给你听

**fn 和 fn2**

这个还是比较好理解的 —— `函数也是对象，是一种特殊的对象`

当 fn 没有被实例化之前，可以看作是一个普通的对象，fn 函数内部定义了什么并不重要。而后面的赋值语句才是这个普通对象的内容。

当 fn 实例化成 fn2 之后，fn2 成为了一个实例对象，函数内部的内容开始显现出来，通过 this.XX 定义的变量会出现在实例对象中，而用标识符定义的变量会成为这个实例的**私有变量**，外面无法访问到。

**a**

fn 是一个普通对象，所以 a 能够正常访问。

在 fn2 中，实例化的过程中就会把作为 fn 属性加入的变量给“甩掉”。

**b**

fn 上为什么 b 访问不到呢？b不是已经在 fn 的原型上了么？

这里其实有一个误区，在查找对象上的变量时，找不到该变量确实会沿着原型链往上查找。但是！查找的过程会以 ****proto****  往上查找，而我们的 b 却挂在了 proptotype 上。

fn 是未实例化的**构造函数**，fn 的 ****proto****  会指向 Funtion.prototype,所以 b 是没法访问的；而 fn2 已经实例化过了，属于**实例对象**，fn2 的 ****proto****  会指向 fn, 当然可以访问到 b 啦。

**age**

前面已经说了，在函数中用标识符定义的变量会作为函数的私有变量，所以我们不可以直接访问，我们只能通过**闭包**的形式，利用闭包可以访问其他函数作用域变量的特性，以函数作为中介去访问这个变量。

### **创建对象的安全模式**

我们在创建对象的时候，有时候会忘记使用 new 关键字，这会导致函数直接在 window 全局作用域上执行！产生意外的全局变量。如

```
var Book = funtion (name) {
    this.name = name
}
var book = Book('1')
console.log(book) // 实例化不成功
console.log(window.name) // '1' 意外的全局变量！
```

为了避免这种情况，我们可以在执行函数的时候进行当前 this 的判断，只有 this 是当前函数的实例时才进行后续操作。

```
var Book = funtion (name) {
    if(this instanceof Book) {
        this.name = name
    } else {
        return new Book(name)
    }
}
```

# 创建型设计模式

## 工厂模式

工厂模式： 工厂模式就是将创建对象的过程单独封装，不对外暴露实现的细节，将有多种类似场景的类进行区分和拆解，通过传入不同的参数可以获取到相应的类、对象等内容。

### 简单工厂模式

对于同一类对象的重复使用，我们可以通过简单工厂来创建这些对象，让这些对象能够共用一些相同的资源，又私有一部分资源。

我们只要传递正确的参数，就能获得所需的对象，而不需要关心其创建的具体细节。

```job
function Career(name, rank) {
    this.name = name
    this.rank = rank
}
function factory(type) {
    switch(type) {
        case 'teacher':
            return new Career('teacher', 1)
            break
        case 'doctor':
            return new Career('doctor', 0)
            breack
        default: 
            return new Career('-', 999)
            breack
    }
}
```

### 工厂方法模式

当对象的种类变多，简单工厂模式的局限就会显现出来。每增加一种对象，我们就需要先定义这个对象，然后到简单工厂中新增一个逻辑判断什么时候使用这个对象。对象越来越多，逻辑判断也会越来越多，使得工厂方法变得复杂。

工厂方法模式将创建对象的工作放到了子类当中，工厂方法就只负责将子类中的对象实例化。这样的方式避免了使用者和对象类之间的耦合，用户不用在意实现对象的具体类，只需要关注工厂方法。

判断当前this 是否属于该工厂函数，可以避免忘记使用 new 关键字意外产生全局变量。

```
function Factory(career){
    if(this instanceof Factory){
        var a = new this[career]();
        return a;
    }else{
        return new Factory(career);
    }
}
Factory.prototype={
    'java': function(){
        this.type = 'java'
        this.content = 'write code'
    },
    'ui': function(){
        this.type = 'ui'
        this.content = 'design page'
    }
}
```

### 抽象工厂模式

抽象工厂模式不同，抽象工厂模式并不直接生成实例， 而是用于对产品类簇的创建。

在抽象工厂中，类簇一般用父类定义，并在父类中定义一些抽象方法，再通过抽象工厂让子类继承父类。所以，抽象工厂其实是实现子类继承父类的方法。（通俗的说，**抽象工厂模式中的工厂方法生成的不是对象，而是新的工厂方法**）

> 抽象方法：指声明但不能使用的方法。在Javascript中，abstract是保留字，我们一般通过在类的方法中抛出错误来模拟抽象类。

```
var VehicleFactory = function(subType, superType) {
    if(VehicleFactory[superType] instanceof Function) {
            function F() {}
            F.prototype = new VehicleFactory[superType]()
            subType.prototype = new F() // 子类继承工厂类类簇
            subType.prototype.constructor = subType; // 修复 constructor
    } else {
        throw new Error('没有此抽象类')
    }
}

VehicleFactory.Car = function() {
    this.type = 'car'
}
VehicleFactory.Car.prototype = {
    getPrice: fucntion() {
        // 模拟抽象方法
        return new Error('抽象方法不能掉用')
    } 
}

// 定义类簇
VehicleFactory.Bus = function() {
    this.type = 'bus'
}
VehicleFactory.Bus.prototype = {
    getPrice: fucntion() {
        return new Error('抽象方法不能掉用')
    } 
}

// 通过工厂抽象方法创建工厂方法
var BMW = function() {
    this.price = 100
}

VehicleFactory(BMW, 'Car')

BMW.prototype.getPrice = function() {
    return this.price
}
```

## 原型模式

**原型模式**： 将可复用、可共享、耗资源的内容从基类中抽离出来放在原型中，用原型实例指向创建对象的类，让创建出来的对象共享原型中的属性和方法。（基类可以对原型中的方法进行重写）

**适用场景**：适用于创建复杂类型的对象时，有一部分的对象结构有可能发生改变或者新增的时候，我们将稳定的部分提取出来方便后面功用，

```
// 图片轮播原型
var LoopImages = function(imgArr, container) {
    this.imgArr = imgArr
    this.container = container
}
LoopImages.prototype.changeImage = function() {
    // ... change Image
}

// 滑动轮播，从轮播原型获取部分属性
var slideLoopImage = function(imgArr, container) {
    LoopImages.call(this, imgArr, container)
}
slideLoopImage.prototype = new LoopImages()
slideLoopImage.prototype.changeImage = function() {
    // ... slide change Image
}
```

原型模式还有一个特点——我们随时可以拓展原型，让原型的后代也获得新的方法或者属性。

```
var slide = new slideLoopImage([])

LoopImages.prototype.getImageLenngth = function() {
    return this.imgArr.length
}

console.log(slide.getImageLenngth()) // 0
```

## 单例模式

**单例模式**： 只允许实例化一次的类，后续再使用该类直接使用已存在的类实例。也可以用来划分一个命名空间，专注管理这一个命名空间内的属性和方法。

**如何创建一个单例**

```
var loadSingle = (function() {
    var _instance = null
    function Single() {
        // ...
    }
    return funtion() {
        if(!_instance) {
            _instance = new Single()
        }
        return _instance
    }
})()
```

# 结构型设计模式

## 外观模式

**外观模式**：为一组复杂的子系统接口提供一个更高级的统一接口，通过这个接口使得对子系统接口的访问变得更方便容易。对接口的二次封装隐藏其复杂实现，简化其使用是一种很不错的实践。在子系统的接口发生改变时，也只需要改动高级接口的实现，可维护性更上一层楼。

示例1、兼容不同浏览器的点击， 不用每次为 dom 绑定事件时都进行兼容性判断

```
function addEvent(dom, type, fn) {
    if(document.addEventListener) {
        document.addEvetListener(type, fn, false)
    } else if (dom.attachEvent) {
        document.attachEvent('on'+ type, fn)
    } else {
        dom['on'+ type] = fn
    }
}
```

示例2、 组合简化底层操作方法

```
var $ = {
    css: function(id, key, val) {
        document.getElementById(id).style[key] = val
    },
    on: function(id, type, fn) {
        document.getElementById(id).addEvetListener(type, fn, false)
    }
}
```

## 装饰器模式

**装饰器模式**： 在不改变原对象的基础上，对其进行包装拓展，使原对象拥有更多能力

一个典型的例子就是 React 中的 HOC 高阶组件：

```
import React, { Component } from 'react'

const BorderHoc = WrappedComponent => class extends Component {
  render() {
    return <div style={{ border: 'solid 1px red' }}>
      <WrappedComponent />
    </div>
  }
}
export default borderHoc
```

```
import React, { Component } from 'react'
import BorderHoc from './BorderHoc'

// 用BorderHoc装饰目标组件
@BorderHoc 
class TargetComponent extends React.Component {
  render() {
    // 目标组件具体的业务逻辑
  }
}

// export出去的其实是一个被包裹后的组件
export default TargetComponent
```

# 行为设计模式

## 观察者模式

**观察者模式**定义了一种一对多的依赖关系，让多个**观察者**对象同时监听某一个目标对象，当这个目标对象的状态发生变化时，会通知所有**观察者**对象，使它们能够自动更新。

一个典型的例子就是 vue 的响应式。响应式的原理在此不多赘述了。

## 状态模式

**状态模式**： 当一个对象的内部状态发生了更改的时候，会导致这个对象行为发生改变，看起来像是改变了对象。

```
function colorState() {
    const state = {
        default: 'red',
        blue: 'blue',
        yellow: 'yellow'
    }
    let _color = 'default'

    function changeColor(color) {
        _color = color
    }
    function show() {
        return state[_color]
    }
    return {
        changeColor,
        show
    }
}

let colorFn = colorState()
console.log(colorFn.show()) // 'red'
colorFn.changeColor('blue')
console.log(colorFn.show()) // 'blue'
```

## 策略模式

**策略模式**： 将一组算法封装独立，使他们可以互相替换。封装的算法具有一定的独立性，不会随客户端变化而变化， 同一个策略算法的产出结果是一定的。

```
function inputCheck() {
    conat rules = {
        notNull(value) {
            return value.trim().length > 0
        },
        number() {
            return /^[0-9]+(\.[0-9]+)$/.test(value)
        }
    }
    function add(type, fn) {
        rules[type] = fn
    }
    return {
        rules,
        add
    }
}
```

### 与状态模式的区别

策略模式和状态模式很相似，不同的是，策略模式不需要维护一个内部状态。策略对象内部的算法互不相干，但是又可以相互替换。

### 优点

**方便测试**

由于策略模式内部的算法彼此之间是独立的，所以在新增算法的时候我们不需要对原来的逻辑再进行二次测试，只需要对新增的算法进行测试即可。

**减少分支判断**

策略模式封装了多种算法，在使用的时候可以直接通过对应的键值取出算法，避免了 if 地狱。

**更好拓展**

策略模式内的算法彼此都是相互独立的，因此在拓展的时候不需要考虑其他的部分。

### 缺点

**使用成本高**

使用哪种算法的决定权在于使用者，所以使用者必须了解算法的作用才能进行使用。

**资源不共享**

策略模式内的算法彼此相互独立，因此即使存在部分相同的逻辑或者变量，我们也不可以对他们进行共享。

### 分支逻辑优化

目前分支逻辑优化已经总结了几种，分别是工厂模式，状态模式和策略模式。

工厂模式，它是一种创建型模式，目标是创建一个新的对象。

状态模式和策略模式都是行为设计模式。但是状态模式的核心是通过状态来控制行为，状态之间是不可以相互替代的。而策略模式是对多种算法的分割封装，不保存状态，算法之间是可以互相替代的。
