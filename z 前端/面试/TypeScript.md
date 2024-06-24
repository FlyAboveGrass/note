### TS和JS的区别

TS是JS的超级，TS最后还是会被编译成JS代码执行的，

| TypeScript | JavaScript |
| ---------- | ---------- |
| 面向对象的语言    | 脚本语言       |
| 静态类型       | 没有静态类型     |
| 支持模块       | 不支持模块      |
| 支持可选参数     | 不支持可选参数    |



#### TS的缺点

1. TS编译代码比JS慢
2. 还不支持抽象类
3. 是JS的超集，没有带来新东西
4. 

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

#### 类型断言



#### 顶部类型和底部类型

##### **顶部类型**

所有其他类型都是子类型。通常，类型是包含了其相关类型系统中所有可能的类型。

-  unknow ： 不确定数据类型， 但是后续会通过类型断言或者typeof来缩小类型范围
- any： any在编译阶段跳过类型检查



##### **底部类型**(never) 

所有其他类型都不可以赋值给这个类型

- never

>  Javascript里不存在的东西通通是没有用的，因为typescript最后会被转义成javascript来执行。那些“没有用”的东西通常都是给编译器看的。
>
> never也是给编译器看的，目的是为了防止你的代码不小心到达了这里。 

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

#### type 和 interface

|              interface              |                             type                             |
| :---------------------------------: | :----------------------------------------------------------: |
|     接口声明引入了命名对象类型      | 类型别名声明为任何类型的类型引入名称(基本类型，联合类型和交集类型) |
| 可以在extends或implements子句中命名 |       对象类型文字的类型别名不能在扩展或实现子句中命名       |
| 接口创建一个新名称，该名称随处可见  |          他们没有创建任何新名称，仅是类型的一个别名          |
|       它可以有多个合并的声明        |                    它不能有多个合并的声明                    |



Type 

- ​适合变量，这个变量的类型可能比较**复杂**，例如是函数、由多种类型联合而来
- 方便对类型进行组合，复杂度进一步提升。
- 没有创建新类型，只是类型联合的一个别名
- 重名的type 会互相覆盖



interface

- **适合对象**，指定一个对象中的各个属性的类型，**范围可能比较大**。
- 方便继承派生出更大范围的类型。
- 创建了新类型，随处可见
- 重名的接口会合并



当是一个对象的时候使用 interface，否则应该尽量使用 type











