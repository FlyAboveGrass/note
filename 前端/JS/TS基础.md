



# 基础类型



### TS的数据类型

#### 内置类型：

- 数字(number)
- 字符串(string)
- 布尔值(boolean)
- 无效(void)
- 空值(null)
- 未定义(undefined)。

#### 自定义类型：

- 枚举(enums)
- 类(classes)
- 接口(interfaces)
- 数组(arrays)
- 元组(tuple)。



### 类型断言



在编译阶段告诉TS 我们已经进行了必须的检查，我们可以暂时将它指定为某种类型。



有两种类型断言方式：

```
// 尖括号语法
let strLength: number = (<string>someValue).length;

// as 语法
let strLength: number = (someValue as string).length;
```





添加 `!`后缀可以将一个变量断言不是 null 或者 undefined





### 顶部类型和底部类型

##### **顶部类型**

所有其他类型都是子类型。通常，类型是包含了其相关类型系统中所有可能的类型。

-  unknow ： 不确定数据类型， 但是后续会通过类型断言或者typeof来缩小类型范围
-  any： any在编译阶段跳过类型检查



##### **底部类型**(never) 

所有其他类型都不可以赋值给这个类型

- never

>  Javascript里不存在的东西通通是没有用的，因为typescript最后会被转义成javascript来执行。那些“没有用”的东西通常都是给编译器看的。
>
>  never也是给编译器看的，目的是为了防止你的代码不小心到达了这里。 

例如:

```
interface Foo {
  type: 'foo'
}

interface Bar {
  type: 'bar'
}

type All = Foo | Bar

function handleValue(val: All) {
  switch (val.type) {
    case 'foo':
      // 这里 val 被收窄为 Foo
      break
    case 'bar':
      // val 在这里是 Bar
      break
    default:
      // val 在这里是 never
      const exhaustiveCheck: never = val
      break
  }
}

// 当val传入了一个非Foo 和 Bar 的类型，就会赋值给一个never类型的 exhaustiveCheck 然后报错
```

#### 

# 高级类型



### 交叉类型

将多个类型合并为一个类型，交叉类型必须**包含所有类型**的特性。

```
interface IPerson {
  id: string;
  age: number;
}

interface IWorker {
  companyId: string;
}

type IStaff = IPerson & IWorker;

const staff: IStaff = {
  id: 'E1006',
  age: 33,
  companyId: 'EXE'
};
```



### 联合类型

联合类型表示一个值可以是**几种类型之一**。 我们用竖线（ `|`）分隔每个类型，所以 `number | string | boolean`表示一个值可以是 `number`， `string`，或 `boolean`。







### [类型保护](https://juejin.cn/post/6844904182843965453#heading-18)

类型保护是可执行运行时检查的一种表达式，用于确保该类型在一定的范围内

要定义一个类型保护，我们只要简单地定义一个函数，它的返回值是一个 *类型谓词*



##### in

##### typeof

##### instanceof

##### 自定义类型保护

谓词为 `parameterName is Type`这种形式， `parameterName`必须是来自于当前函数签名里的一个参数名。

```
function isFish(pet: Fish | Bird): pet is Fish {
    return (<Fish>pet).swim !== undefined;
}
```





### 可辨识联合



#### 三要素

1. 具有普通的单例类型属性— *可辨识的特征*。
2. 一个类型别名包含了那些类型的联合— *联合*。
3. 此属性上的类型保护。











### 类型别名

类型别名会给一个类型起个新名字。 类型别名有时和接口很像，但是可以作用于原始值，联合类型，元组以及其它任何你需要手写的类型。















# 枚举

1. 

使用枚举我们可以定义一些带名字的常量。 使用枚举可以清晰地表达意图或创建一组有区别的用例。

### 数字枚举

不指定编码会默认0，指定一个后面的会递增
指定编码时如果使用的是常量、计算值或者函数，那么后面的编码都需要手动指定

**获取编码**

+ .操作符

+ []方括号

    

### 字符串枚举

字符串枚举里，每个成员都必须用字符串字面量，或另外一个字符串枚举成员进行初始化

### 异构枚举

 枚举可以混合字符串和数字成员（一般不会使用）



### 计算的和常量成员

每个枚举成员都带有一个值，它可以是 常量或 计算出来的。

满足如下条件时，枚举成员被当作是常量：

1. 它是枚举的第一个成员且没有初始化器
2. 它不带有初始化器且它之前的枚举成员是一个 数字常量
3. 枚举成员使用 常量枚举表达式初始化
4. 所有其它情况的枚举成员被当作是需要计算得出的值



### 联合枚举和枚举成员的类型

##### 反向映射

枚举类型被编译成一个对象，它包含了正向映射（ name -> value）和反向映射（ value -> name） 

不会为字符串枚举成员生成反向映射。

```
enum Enum { A } 
let a = Enum.A; 
let nameOfA = Enum[a]; // "A"
```





### const枚举

常量枚举只能使用常量枚举表达式，并且不同于常规的枚举，它们在编译阶段会被删除
常量枚举不允许包含计算成员



### 外部枚举





# 函数



|          | TypeScript     | JavaScript         |
| -------- | -------------- | ------------------ |
| 类型     | 含有类型       | 无类型             |
| 箭头函数 | 箭头函数       | 箭头函数（ES2015） |
| 函数类型 | 函数类型       | 无函数类型         |
| 参数     | 必填和可选参数 | 所有参数都是可选的 |
| 默认参数 | 有             | 无默认参数         |
| 剩余参数 | 有             | 无                 |
| 重载     | 函数重载       | 无函数重载         |

#### 



# 接口



接口的作用就是为类型命名和为你的代码或第三方代码定义契约



### 属性



1. 可选属性
2. 只读属性
3. 额外的属性

```
interface SquareConfig {
  color?: string;
  readonly width: number;
  [propName: string]: any;
}
```



### 函数类型

```
// 指定参数类型和返回值类型
interface SearchFunc {
  (source: string, subString: string): boolean;
}
```



### 继承





# 类



### 属性和方法



#### public 

#### private

#### protected

#### readonly

#### static







### setter / getter

















































