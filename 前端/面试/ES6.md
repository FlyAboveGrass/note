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



### Promise

#### Promise的不足

1、无法取消`Promise`，一旦新建它就会立即执行，无法中途取消。

2、如果不设置回调函数，`Promise`内部抛出的错误，不会反应到外部。

3、当处于`Pending`状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

#### [中断promise](https://juejin.cn/post/6847902216028848141)

```
// 手动中断
function abortWrapper(p1) {
  let abort
  let p2 = new Promise((resolve, reject) => (abort = reject))
  let p = Promise.race([p1, p2])
  p.abort = abort
  return p
}

// 超时中断
function timeoutWrapper(p, timeout = 2000) {
  const wait = new Promise((resolve, reject) => {
    setTimeout(() => {
      reject('请求超时')
    }, timeout)
  })
  return Promise.race([p, wait])
}
```

#### promise.all报错也全部返回

```
function dbFuc(p) {
    let pAll = p.map(item => item.catch(e => e))
    return Promise.all(pAll)
}
```

#### [手写promise](https://juejin.cn/post/6850037281206566919#heading-3)

```
const PENDING = 'pending';
const RESOLVED = 'resolved';
const REJECTED = 'rejected';

// // 如果返回的是一个 promise，要先对promise求值
const resolvePromise = (promise, x, resolve, reject) => {
    // 循环引用，则抛出异常
    if(promise === x) return reject(new Error('promise循环引用'))

    if((typeof x === 'object' && typeof x !== null) || typeof x === 'function') {
        // x 同时调用 resolve 函数和 reject 函数，则第一次调用优先，其他所有调用被忽略
        let called;
        try {
            let then = x.then
            if(typeof then === 'function') {
                // 不要写成 x.then，直接 then.call 就可以了 因为 x.then 会再次取值
                then.call(x, (y) => {
                    if (called) return;

                    called = true;
                    // 递归解析的过程(因为可能 promise 中还有 promise)
                    resolvePromise(promise2, y, resolve, reject); 
                }, r => {
                    if (called) return;

                    called = true;
                    reject(r);
                })
            } else {
                // 不是promise， 不用判断是否调用过
                resolve(x)
            }
        } catch(e) {
            if(called) return ;

            called = true;
            reject(e)
        }

    } else { //普通值就直接返回 resolve 作为结果
        resolve(x)
    }
}

class PromiseA {
    constructor(exector) {
        this.state = PENDING;
        this.value = null;
        this.reason = null;
        // 异步处理
        this.onResolvedCallbacks = [];
        this.onRejectedCallbacks = [];
        

        // 异步处理：当exector中的逻辑执行完毕，然后调用resolve，resolve就把then函数里面存到onResolvedCallbacks数组的函数都执行
        let resolve = (value) => {
            // 状态一旦修改，就不可以再修改
            if(this.state === PENDING) {
                this.state = RESOLVED;
                this.value = value;
                this.onResolvedCallbacks.forEach(fn => fn())
            }
            
        }

        let reject = (msg) => {
            if(this.state === PENDING) {
                this.state = REJECTED;
                this.reason = msg;
                this.onRejectedCallbacks.forEach(fn => fn())
            }
        }

        try {
            exector(resolve, reject)
        } catch(e) {
            reject(e)
        }
        
    }

    then(onResolved, onRejected) {
        // 防止 onFulfilled\ onRejected 没有传值
        onResolved = typeof onResolved === 'function' ? onResolved : v => v;
        onRejected = typeof onRejected === 'function' ? onRejected : err => { throw err };

        let promise2 = new PromiseA((resolve, reject) => {
            if(this.state === RESOLVED) {
                setTimeout(() => {
                    try{
                        onResolved(this.value)

                        // 如果返回的是一个 promise，要先对promise求值
                        resolvePromise(promise2, x, resolve, reject);
                    } catch(e) {
                        reject(e)
                    }
                }, 0)
            }
    
            if(this.state === REJECTED) {
                onRejected(this.reason)
            }
    
            // 异步处理： 如果还没有状态改变，就把回调函数暂时放进数组里面， 状态改变之后调用
            if(this.state === PENDING) {
                this.onResolvedCallbacks.push(() => {
                    onResolved(this.value)
                });
                this.onRejectedCallbacks.push(() => {
                    onRejected(this.reason)
                })
            }
        })
        return promise2
    }   
}
```



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



