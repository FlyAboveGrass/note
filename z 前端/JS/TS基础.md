#Typescript

[JELLY | Typescript 常用特性小结 (jd.com)](https://jelly.jd.com/article/6292d76e1ca7c3018d0bebfe)


# 基础类型

### TS的数据类型

#### 内置类型：

- 数字(number)
- 大数字（bigint）
- 字符串(string)
- 布尔值(boolean)
- 无效(void): 当函数没有返回值，返回的类型就会是 void
- 空值(null)
- 未定义(undefined)。
- symbol

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

##### 顶部类型

所有其他类型都是子类型。通常，类型是包含了其相关类型系统中所有可能的类型。

- unknow ： 不确定数据类型， 但是后续会通过类型断言或者typeof来缩小类型范围
- any： any在编译阶段跳过类型检查

##### 底部类型(never)

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
// 使用 never 避免出现新增了联合类型没有对应的实现，目的就是写出类型绝对安全的代码。
```

# 高级类型


### 条件类型
TypeScript 里的条件判断是 `extends ? :`，叫做条件类型（Conditional Type）

### 推导类型
`infer`  可以用于提取类型的一部分

```
type First<Tuple extends unknown[]> = Tuple extends [infer T,...infer R] ? T : never; 
type res = First<[1,2,3]>; // 1
```
``


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

### 索引类型（Index types）

使用索引类型，编译器就能够检查使用了动态属性名的代码。 

```
function pluck<T, K extends keyof T>(o: T, names: K[]): T[K][] {
  return names.map(n => o[n]);
}

interface Person {
    name: string;
    age: number;
}
let person: Person = {
    name: 'Jarid',
    age: 35
};
let strings: string[] = pluck(person, ['name']); // ok, string[]
```

上面的例子就是索引类型的典型示例，里面涉及了两个操作符：

- **索引类型查询操作符**： keyof T。对于任何类型 `T`， `keyof T`的结果为 `T`上已知的公共属性名的联合
- **索引访问操作符**： T[K]。在这里，类型语法反映了表达式语法。 这意味着 `person['name']`具有类型 `Person['name']` — 在我们的例子里则为 `string`类型。

### 映射类型

 在映射类型里，新类型以相同的形式去转换旧类型里每个属性。

我们可以将一个类型里面的属性的类型进行改变，例如变成可选或者只读的类型。

```
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
}
type Partial<T> = {
    [P in keyof T]?: T[P];
}

type Keys = 'option1' | 'option2';
type Flags = { [K in Keys]: boolean };
```
- keyof T 是查询索引类型中所有的索引，叫做`索引查询`。
- T[Key] 是取索引类型某个索引的值，叫做 `索引访问`。
- in 是用于遍历联合类型的运算符。


除了值可以变化，索引也可以做变化，用 as 运算符，叫做 `重映射`。
```typescript
type MapType<T> = {
    [
        Key in keyof T 
            as `${Key & string}${Key & string}${Key & string}`
    ]: [T[Key], T[Key], T[Key]]
}
```

#### 常用的映射类型

- Readonly： 转成 readonly
- Partial ： 转成可读
- Pick ： 选出类型内的其中几个属性
- Record： 生成所有属性类型一样的类型

#### 预定义的有条件类型

TypeScript 2.8在`lib.d.ts`里增加了一些预定义的有条件类型：

- `Exclude<T, U>` -- 从`T`中剔除可以赋值给`U`的类型。
- `Extract<T, U>` -- 提取`T`中可以赋值给`U`的类型。
- `NonNullable<T>` -- 从`T`中剔除`null`和`undefined`。
- `ReturnType<T>` -- 获取函数返回值类型。
- `Parameters<T>` -- 用于提取函数类型的参数类型。
- `InstanceType<T>` -- 获取构造函数类型的实例类型。
- `Uppercase、Lowercase、Capitalize、Uncapitalize` --- 分别实现大写、小写、首字母大写、去掉首字母大写的。



# 枚举

1. 使用枚举我们可以定义一些带名字的常量。 使用枚举可以清晰地表达意图或创建一组有区别的用例。

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

|      | TypeScript | JavaScript   |
| ---- | ---------- | ------------ |
| 类型   | 含有类型       | 无类型          |
| 箭头函数 | 箭头函数       | 箭头函数（ES2015） |
| 函数类型 | 函数类型       | 无函数类型        |
| 参数   | 必填和可选参数    | 所有参数都是可选的    |
| 默认参数 | 有          | 无默认参数        |
| 剩余参数 | 有          | 无            |
| 重载   | 函数重载       | 无函数重载        |

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


# 类型运算

> **模式匹配做提取，重新构造做变换。**
> **递归复用做循环，数组长度做计数。**
> **联合分散可简化，特殊特性要记清。**
> **基础扎实套路熟，类型体操可通关。**


**模式匹配做提取**
通过 infer 进行模式匹配，提取出我们需要的类型

```
type GetLast<Arr extends unknown[]> = Arr extends [...unknown[], infer Last] ? Last : never; // 数组的最后一个元素

type StartsWith<Str extends string, Prefix extends string> = Str extends `${Prefix}${string}` ? true : false; // 字符串的开头
```


**重新构造做变化**

TypeScript 类型系统支持 3 种可以声明任意类型的变量： type、infer、类型参数。

type 叫做类型别名，其实就是声明一个变量存储某个类型：

```typescript
type ttt = Promise<number>;
```

infer 用于类型的提取，然后存到一个变量里，相当于局部变量：

```typescript
type GetValueType<P> = P extends Promise<infer Value> ? Value : never;
```

类型参数用于接受具体的类型，在类型运算中也相当于局部变量：

```typescript
type isTwo<T> = T extends 2 ? true: false;
```

但是，严格来说这三种也都不叫变量，因为它们不能被重新赋值。





### 知识点整理


```
// 联合类型infer推断出来的就是它本身
// type u = 1 | 2 extends infer S ? S : never  // 1 | 2


// 如何遍历一个联合类型。构造一个泛型类型作为中间映射函数（因为分布式条件类型只有在泛型 + 条件判断时才生效
// type MyMap<T> = T extends T ? [T] : never

```



 三元表达式后一个结果拿不到对象中的 key
```
// 错误的
type Merge<F, S> = {
[K in keyof F | keyof S]: K extends keyof S ? S[K] : F[K];
};

// 正确的
type Merge<F, S> = {
[K in keyof F | keyof S]: K extends keyof S
    ? S[K]
    : K extends keyof F ? F[K] : never;
};
```


**如何判断一个类型是 never**
> 参考 [296 - Permutation (with explanations) · Issue #614 · type-challenges/type-challenges · GitHub](https://github.com/type-challenges/type-challenges/issues/614)

```
直觉上第一感觉就是：
type isNever<T> = T extends never ? true : false

然而会发现 isNever<never> 得到的是 never。

why？

1. extends 不是判断相等，而是尝试 distributes 分发它。
2. never 作为联合类型时，内容为空。 举例 never ｜ string 在分发的时候，只会得到一个 string。

所以在上面的 extends 中，尝试分发 never，是无法分发的，也就走不到条件语句里面。也就是直接得到 never。


那么如何实现真正的isNever 呢？简单，给 never 增加一个包裹内容，让他不独立为类型。最终答案：

type IsNever<T> = [T] extends [never] ? true : false;



```