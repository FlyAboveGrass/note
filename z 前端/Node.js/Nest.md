
# 基本概念

## Controller

控制器负责处理传入的**请求**和向客户端返回**响应**。


## Providers

Provider 的作用是创建或者分发 Service 服务。
Provider 可以被注入到 Controller 、模块或者其他使用依赖注入（DI）的 Provider 中，保证你的代码的模块化、可测试和方便维护。
当你想要定义一个 Provider 时，只需要将你的类使用 `@Injectable()` 装饰器装饰。

### 服务 Service

Service 常常用来进行数据的存储和检索。


### DI 依赖注入

Nest 是建立在强大的设计模式，通常称为依赖注入 （Dependency Injection， DI)。
依赖注入是一种从外部接受依赖的内容而不是直接定义它的设计模式。
依赖注入常常是通过 Provider 去实现的


基于构造函数的注入：
```typescript
import { Injectable, Optional, Inject } from '@nestjs/common';

@Injectable()
export class HttpService<T> {
  constructor(
    @Optional() @Inject('HTTP_OPTIONS') private readonly httpClient: T
  ) {}
}
```

基于属性的注入：

> 如果您的类没有扩展其他提供者，你应该总是使用基于**构造函数**的注入。


```typescript
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class HttpService<T> {
  @Inject('HTTP_OPTIONS')
  private readonly httpClient: T;
}
```


### Module 模块

模块是具有 `@Module()` 装饰器的类。 `@Module()` 装饰器提供了元数据，Nest 用它来组织应用程序结构。

`@module()` 装饰器接受一个描述模块属性的对象：

|对象| 内容 |
| :--------------: | :--------------: |
|providers|由 Nest 注入器实例化的提供者，并且可以至少在整个模块中共享|
|controllers|必须创建的一组控制器|
|imports|导入模块的列表，这些模块导出了此模块中所需提供者|
|exports|由本模块提供并应在其他模块中可用的提供者的子集。|



#### 功能模块

#### 共享模块
在 Nest 中，默认情况下，模块是**单例**，因此您可以轻松地在多个模块之间共享**同一个**提供者实例。

#### 全局模块

`@Global` 装饰器使模块成为全局作用域。 全局模块应该只注册一次，最好由根或核心模块注册。


#### 动态模块

`Nest` 模块系统包括一个称为动态模块的强大功能。此功能使您可以轻松创建可自定义的模块，这些模块可以动态注册和配置提供程序。



