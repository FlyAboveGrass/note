
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



## 中间件
Nest 中间件实际上等价于 [express](http://expressjs.com/en/guide/using-middleware.html) 中间件。下面是 Express 官方文档中所述的中间件功能：

中间件函数可以执行以下任务:
- 执行任何代码。
- 对请求和响应对象进行更改。
- 结束请求-响应周期。
- 调用堆栈中的下一个中间件函数。
- 如果当前的中间件函数没有结束请求-响应周期, 它必须调用 `next()` 将控制传递给下一个中间件函数。否则, 请求将被挂起。

`Nest` 中间件完全支持依赖注入。就像 Provider 和 Controller 一样，它们能够**注入**属于同一模块的依赖项（通过 `constructor` ）。


中间件不能在 `@Module()` 装饰器中列出。我们必须使用模块类的 `configure()` 方法来设置它们。包含中间件的模块必须实现 `NestModule` 接口。
```
import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('cats'); // .forRoutes({ path: 'cats', method: RequestMethod.GET });
  }
}
```



### 中间件消费者

`forRoutes()` 可接受一个字符串、多个字符串、对象、一个控制器类甚至多个控制器类。例如：
- forRoutes ('cats')
	- 控制路由的前缀
- forRoutes ({ path: 'cats', method: RequestMethod. GET })
	- 准确的控制路由
- forRoutes ({ path: 'ab*cd', method: RequestMethod. ALL })
	- 路由通配符
- forRoutes (CatsController)
	- 控制器类


排除某些路由。我们可以使用该 `exclude()` 方法轻松排除某些路由
```typescript
consumer
  .apply(LoggerMiddleware)
  .exclude(
    { path: 'cats', method: RequestMethod.GET },
    { path: 'cats', method: RequestMethod.POST },
    'cats/(.*)',
  )
  .forRoutes(CatsController);
```


### 函数式中间件

当您的中间件没有任何依赖关系时，我们可以考虑使用函数式中间件。

### 多个中间件
为了绑定顺序执行的多个中间件，我们可以在 `apply()` 方法内用逗号分隔它们。

```typescript
consumer.apply(cors(), helmet(), logger).forRoutes(CatsController);
```

### 全局中间件
如果我们想一次性将中间件绑定到每个注册路由，我们可以使用由 `INestApplication` 实例提供的 `use()` 方法：

```typescript
const app = await NestFactory.create(AppModule);
app.use(logger);
```


## 异常处理

暂掠过


## 管道

管道是具有 `@Injectable()` 装饰器的类。**验证管道**要么返回一个转换后的值，要么抛出一个错误。

管道有两个典型的应用场景:

- **转换**：管道将输入数据转换为所需的数据输出(例如，将字符串转换为整数)
- **验证**：对输入数据进行验证，如果验证成功继续传递; 验证失败则抛出异常

在这两种情况下, 管道 `参数(arguments)` 会由 [控制器(controllers)的路由处理程序](https://docs.nestjs.cn/8/controllers?id=%e8%b7%af%e7%94%b1%e5%8f%82%e6%95%b0) 进行处理。Nest 会在调用这个方法之前插入一个管道，管道会先拦截方法的调用参数,进行转换或是验证处理，然后用转换好或是验证好的参数调用原方法。


> 创建 filter 管道的方式： `nest g filter error/cat`


### 内置管道

[内置管道列表](https://docs.nestjs.cn/8/techniques?id=%e9%aa%8c%e8%af%81)
`Nest` 自带九个开箱即用的管道，即

- `ValidationPipe`
- `ParseIntPipe`
- `ParseFloatPipe`
- `ParseBoolPipe`
- `ParseArrayPipe`
- `ParseUUIDPipe`
- `ParseEnumPipe`
- `DefaultValuePipe`
- `ParseFilePipe`


```typescript
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}

@Get(':id')
async findOne(
  @Param('id', new ParseIntPipe({ errorHttpStatusCode: HttpStatus.NOT_ACCEPTABLE }))
  id: number,
) {
  return this.catsService.findOne(id);
}
```


### 对象结构验证

npm install --save joi

> 生成一个管道类： nest g pipe className


### 全局管道

由于 `ValidationPipe` 被创建为尽可能通用，所以我们将把它设置为一个**全局作用域**的管道，用于整个应用程序中的每个路由处理器。

> main.ts

```typescript
async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();
```

在 [混合应用](https://docs.nestjs.cn/8/faq?id=%e6%b7%b7%e5%90%88%e5%ba%94%e7%94%a8)中 `useGlobalPipes()` 方法不会为网关和微服务设置管道, 对于标准(非混合) 微服务应用使用 `useGlobalPipes()` 全局设置管道。


### 提供默认值


## 守卫

守卫有一个单独的责任。它们根据运行时出现的某些条件（例如权限，角色，访问控制列表等）来确定给定的请求是否由路由处理程序处理。这通常称为授权。在传统的 `Express` 应用程序中，通常由中间件处理授权(以及认证)。


每个守卫必须实现一个 `canActivate()` 函数。此函数应该返回一个布尔值，用于指示是否允许当前请求。它可以同步或异步地返回响应(通过 `Promise` 或 `Observable`)。Nest 使用返回值来控制下一个行为:

- 如果返回 `true`, 将处理用户调用。
- 如果返回 `false`, 则 `Nest` 将忽略当前处理的请求。

> 生成上下文： `nest g guard`

### 授权守卫

正如前面提到的，授权是守卫的一个很好的用例，因为只有当调用者(通常是经过身份验证的特定用户)具有足够的权限时，特定的路由才可用。



### 执行上下文

[执行上下文](https://docs.nestjs.cn/10/guards)


### 守卫绑定方式

**绑定到某个 Controller**
```typescript
@Controller('cats')
@UseGuards(RolesGuard)
export class CatsController {}
```
上面的构造将守卫附加到此控制器声明的每个处理程序。如果我们希望守卫只应用于单个方法，则需在**方法级别**应用 `@UseGuards()` 装饰器

**全局守卫**

对于混合应用程序，默认情况下 `useGlobalGuards()` 方法不会为网关和微服务设置守卫(可查阅[混合应用](https://docs.nestjs.cn/10/faq?id=%E6%B7%B7%E5%90%88%E5%BA%94%E7%94%A8)以了解如何改变此行为)。

```typescript
const app = await NestFactory.create(AppModule);
app.useGlobalGuards(new RolesGuard());
```


**模块内守卫**

```typescript
import { Module } from '@nestjs/common';
import { APP_GUARD } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_GUARD,
      useClass: RolesGuard,
    },
  ],
	})
export class AppModule {}
```


## 拦截器

拦截器是使用 `@Injectable()` 装饰器注解的类。拦截器应该实现 `NestInterceptor` 接口。


拦截器具有一系列有用的功能，这些功能受面向切面编程（AOP）技术的启发。它们可以：

- 在函数执行之前/之后绑定**额外的逻辑**
- 转换从函数返回的结果
- **转换**从函数抛出的异常
- 扩展基本函数行为
- 根据所选条件完全重写函数 (例如, 缓存目的)


> 应用举例。处理响应的 data 和 msg 等、跟踪请求的流转速度、对部分接口做缓存处理。

### 截取切面

> `NestInterceptor<T，R>` 是一个通用接口，其中 `T` 表示已处理的 `Observable<T>` 的类型（在流后面），而 `R` 表示包含在返回的 `Observable<R>` 中的值的返回类型。

> 拦截器的作用与控制器，提供程序，守卫等相同，这意味着它们可以通过构造函数注入依赖项。

### 响应映射

我们已经知道, `handle()` 返回一个 `Observable`。使用 `RxJS` 运算符操作流的可能性为我们提供了许多功能。

通过控制这个 Observable 流，我们可以实现很多的有用的功能。
[操作符 · 学习 RxJS 操作符](https://rxjs-cn.github.io/learn-rxjs-operators/operators/)




## 自定义装饰器

> 关于装饰器的解释：[Exploring EcmaScript Decorators. Iterators, generators and array… | by Addy Osmani | Google Developers | Medium](https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841)


`ES2016` 装饰器是一个表达式，它返回一个可以将目标、名称和属性描述符作为参数的函数。通过在装饰器前面添加一个 `@` 字符并将其放置在你要装饰的内容的最顶部来应用它。可以为类、方法或属性定义装饰器。

### [参数装饰器](https://docs.nestjs.cn/10/customdecorators?id=%e5%8f%82%e6%95%b0%e8%a3%85%e9%a5%b0%e5%99%a8)

`Nest` 提供了一组非常实用的参数装饰器，可以结合 `HTTP` 路由处理器（`route handlers`）一起使用。下面的列表展示了`Nest` 装饰器和原生 `Express`（或 `Fastify`）中相应对象的映射。

|装饰器名称| 原生 `Express`（或 `Fastify`）中相应对象的映射|
|---|---|
|`@Request()，@Req()`|`req`|
|`@Response()，@Res()`|`res`|
|`@Next()`|`next`|
|`@Session()`|`req.session`|
|`@Param(param?: string)`|`req.params / req.params[param]`|
|`@Body(param?: string)`|`req.body / req.body[param]`|
|`@Query(param?: string)`|`req.query / req.query[param]`|
|`@Headers(param?: string)`|`req.headers / req.headers[param]`|
|`@Ip()`|`req.ip`|
|`@HostParam()`|`req.hosts`|


### 传递数据



# 基本原理

## 自定义 Provider

### DI 基本面

依赖注入是一种[控制反转(IoC)](https://en.wikipedia.org/wiki/Inversion_of_control) 技术，在这种技术中，你将依赖的实例化委托给 IoC 容器(在我们的例子中，是 NestJS 运行时系统)，而不是在你自己的代码中执行。

在这个过程中有三个关键步骤:

1. 在`cat.service.ts`中，`@Injectable()`装饰器将`CatsService`类声明为一个可以被 Nest IoC 容器管理的类。
2. 在 `cat.scontroller.ts` 中， `CatsController` 通过构造函数注入声明了对 `CatsService` 令牌的依赖: `constructor(private catsService: CatsService)`
3. 在`app.module.ts`中，我们将令牌`CatsService`与`cat.service.ts`文件中的类`CatsService`关联起来。 下面我们将确切地[看到](https://wdk-docs.github.io/fundamentals/custom-providers#standard-providers)这个关联(也称为 _登记_)是如何发生的。

当 Nest IoC 容器实例化一个 `CatsController` 时，它首先查找任何依赖项。当它找到 `CatsService` 依赖项时，它对 `CatsService` 令牌执行查找，该令牌返回 `CatsService` 类，每一个注册步骤(上面的第 3 条)。假设为 `单例模式` 范围(默认行为)，Nest 将创建一个 `CatsService` 的实例，缓存它，然后返回它，或者如果一个已经被缓存，返回现有的实例。


### 值 Provider：useValue

```
@Module({
  imports: [CatsModule],
  providers: [
    {
      provide: CatsService,
      useValue: mockCatsService,
    },
  ],
})
```

在上面这个例子中，`CatsService`令牌将解析为`mockCatsService`模拟对象。 `useValue`需要一个值-在这种情况下，一个文字对象具有与它正在替换的`CatsService`类相同的接口。


### 非基于类的 Provider

目前为止，我们使用的 Provider 都是基于类及其 constructor 的，provide 中的 token 就是我们的 Provider 的类名。
但是有时候我们需要更加灵活的注入方式，例如注入特定的字符串、枚举或者 symbol。

下面是一个简单的示例：

```
import { connection } from './connection';

@Module({
  providers: [
    {
      provide: 'CONNECTION',
      useValue: connection,
    },
  ],
})
export class AppModule {}


// 使用
@Injectable()
export class CatsRepository {
  constructor(@Inject('CONNECTION') connection: Connection) {}
}

```



### 类 Provider

`useClass` 语法允许您动态地确定一个令牌应该解析到的类。

```
const configServiceProvider = {
  provide: ConfigService,
  useClass:
    process.env.NODE_ENV === 'development'
      ? DevelopmentConfigService
      : ProductionConfigService,
};

@Module({
  providers: [configServiceProvider],
})
export class AppModule {}

```


### FactoryProvider：useFactory


`useFactory` 语法允许 _动态_ 地创建提供器。实际的提供程序将由工厂函数返回的值提供。

工厂提供程序语法有一对相关的机制:

1. 工厂函数可以接受(可选的)参数。
2. (可选)`inject`属性接受一个提供程序数组，Nest 将在实例化过程中解析并将其作为参数传递给工厂函数。 这两个列表应该是相关的:Nest 将以相同的顺序将`inject`列表中的实例作为参数传递给工厂函数。

```
const connectionFactory = {
  provide: 'CONNECTION',
  useFactory: (optionsProvider: OptionsProvider) => {
    const options = optionsProvider.get();
    return new DatabaseConnection(options);
  },
  inject: [OptionsProvider],
};

@Module({
  providers: [connectionFactory],
})
export class AppModule {}

```




### 别名 Provider： useExisting

使用别名 Provider 可以对现有的 Provider 起一个别名，使得在使用他们的时候获得的是同一个实例。
```typescript
@Injectable()
class LoggerService {
  /* implementation details */
}

const loggerAliasProvider = {
  provide: 'AliasedLoggerService',
  useExisting: LoggerService,
};

@Module({
  providers: [LoggerService, loggerAliasProvider],
})
export class AppModule {}
```


### 无 Service Provider

在前面的 Privider 中，我们都需要给 Provider 提供单独定义一个 @Injectable 装饰的 Service 。但是其实 Provider 可以支持任何形式的值，下面是一个简单示例：

```typescript
const configFactory = {
  provide: 'CONFIG',
  useFactory: () => {
    return process.env.NODE_ENV === 'development' ? devConfig : prodConfig;
  },
};

@Module({
  providers: [configFactory],
})
export class AppModule {}
```




### 异步的 Provider

异步 Provider 这个语法是和 `useFactory` 语法使用 `async/await`。工厂返回一个 `Promise`，并且工厂函数可以 `await` 异步任务。 Nest 将在实例化依赖于(注入)这样一个提供器的任何类之前等待承诺的解析。

```
{
  provide: 'ASYNC_CONNECTION',
  useFactory: async () => {
    const connection = await createConnection(options);
    return connection;
  },
}

```


## 动态的模块

考虑这样一种情况:我们有一个通用模块，它需要在不同的用例中表现不同。这类似于许多系统中的 `插件` 概念，在这些系统中，通用功能在供使用者使用之前需要进行一些配置。

使用 Nest 的一个很好的例子是 **配置模块** 。 许多应用程序发现，通过使用配置模块外部化配置细节非常有用。 这使得在不同的部署中动态更改应用程序设置变得很容易:







## 注入作用域


### [Provider 作用域](https://docs.nestjs.cn/10/fundamentals?id=%e6%8f%90%e4%be%9b%e8%80%85%e8%8c%83%e5%9b%b4)

基本上，每个提供者都可以作为一个单例，被请求范围限定，并切换到瞬态模式。请参见下表，以熟悉它们之间的区别。

|模式|描述|
|---|---|
|`DEFAULT`|每个提供者可以跨多个类共享。提供者生命周期严格绑定到应用程序生命周期。一旦应用程序启动，所有提供程序都已实例化。默认情况下使用单例范围。|
|`REQUEST`|在请求处理完成后，将为每个传入请求和垃圾收集专门创建提供者的新实例|
|`TRANSIENT`|临时提供者不能在提供者之间共享。每当其他提供者向 `Nest` 容器请求特定的临时提供者时，该容器将创建一个新的专用实例|

> 使用单例范围始终是推荐的方法。请求之间共享提供者可以降低内存消耗，从而提高应用程序的性能(不需要每次实例化类)。


```typescript
// Service 中
import { Injectable, Scope } from '@nestjs/common';

@Injectable({ scope: Scope.REQUEST })
export class CatsService {}

// 或者在模块的provider定义中
{
  provide: 'CACHE_MANAGER',
  useClass: CacheManager,
  scope: Scope.TRANSIENT,
}
```


### Controller 作用域

```typescript
@Controller({
  path: 'cats',
  scope: Scope.REQUEST,
})
export class CatsController {}
```


> 网关永远不应该依赖于请求范围的提供者，因为它们充当单例。一个网关封装了一个真正的套接字，不能多次被实例化





### 作用域层级

必须非常谨慎地使用 REQUEST 的 provider。请记住，`scope` 实际上是在注入链中冒泡的。如果您的控制器依赖于一个 REQUEST 的 provider，这意味着您的控制器实际上也是 REQUEST 范围的。



### Request provider

在一个 HTTP 应用程序中(例如使用`@nestjs/platform-express`或`@nestjs/platform-fastify`)，如果你想要获取原始的请求对象，可以手动注入 REQUEST 对象

```typescript
import { Injectable, Scope, Inject } from '@nestjs/common';
import { REQUEST } from '@nestjs/core';
import { Request } from 'express';

@Injectable({ scope: Scope.REQUEST })
export class CatsService {
  constructor(@Inject(REQUEST) private request: Request) {}
}
```




## 循环依赖

大多数时候，我们都应该避免出现循环依赖的场景，但是 Nest 依然提供了方式来处理这一特殊行为。

### 前向引用： Forward reference

前向引用允许 Nest 去引用尚未被 forwardRef() 函数定义的类。假设 `CatsService` 和 `CommonService` 存在相互引用的关系，那么他们可以通过 @Inject 和 forwardRef() 函数去解决他们之间的循环依赖。如果其中一个没有这样做，那么 Nest 将不会实例化这两个类，因为他们缺少关键的元信息。

```typescript
@Injectable()
export class CommonService {
  constructor(
    @Inject(forwardRef(() => CatsService))
    private catsService: CatsService,
  ) {}
}
```

> 实例化的顺序是不确定的。不能保证哪个构造函数会被先调用。所以你应保证你的代码中不存在依赖于某一个类先被实例化的场景。


### 模块前向引用：Module forward reference


为了处理模块( `module` )之间的循环依赖，必须在模块关联的两个部分上使用相同的 `forwardRef()`：

```typescript
// common.module.ts
@Module({
  imports: [forwardRef(() => CatsModule)],
})
export class CommonModule {}

// cat.module.ts

@Module({
  imports: [forwardRef(() => CommonModule)],
})
export class CatsModule {}
```



