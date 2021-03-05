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

