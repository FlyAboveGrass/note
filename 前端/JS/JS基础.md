### 数据类型

基础数据类型：Undefined、Null、Boolean、String、Number、Symbol

引用数据类型：Object、Array、Date、RegExp、Function

#### 类型判断

##### typeof

判断一个变量的类型

1. 返回值是代表类型的字符串
2. 检测基本数据类型
3. 函数判断结果是‘function’， 对象判断结果都是 ‘object’
4. 判断null 返回的结果也是一个 'object'



##### instanceof

判断一个对象是否是另一个对象的子类型

1. 返回值是true和false

2. 检测对象

   

##### 手写typeof/instanceof

```
// 实现typeof
function typeof(obj) {
	return Object.prototype.toString.call(a).slice(8,-1).toLowerCase();
}

// 实现instanceof
function instanceof(left,right){
    left=left.__proto__
    right=right.prototype
    while(true){
       if(left==null)
       	  return false;
       if(left===right)
          return true;
       left=left.__proto__
    }
}
```

### for of 和for in区别

#### for in 

**枚举**。 适合遍历对象

1. 可以枚举对象，获得对象的key值

2. 可以遍历数组，数组获得**下标**，通过下标可以得到值。

   - 但是不能保证遍历的顺序
- 性能不好， 要遍历原型上的属性
   - 对于[稀疏数组](https://zhuanlan.zhihu.com/p/268534702)，会跳过已删除或未初始化的项
   
   
   
   

#### for...of 

**迭代**。适合遍历数组

1. 不可以枚举对象
2. 可以迭代数组，获得数组的**值**
3. 可以迭代类数组， 如字符串、arguments、nodeList、map/set、generator
4. 可以用break中断遍历











