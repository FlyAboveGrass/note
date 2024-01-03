

前置内容：

启动 mysql：
`docker run -p 3306:3306 --name root -e MYSQL_ROOT_PASSWORD=12345678 -d mysql`




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














  
  

## 模块参考

  

Nest 提供了一个 `ModuleRef` 类来导航到内部提供者列表，并使用注入令牌作为查找键名来获取一个引用。`ModuleRef` 类也提供了一个动态实例化静态和范围的提供者的方法。

  
  

### 获取实例

  

`ModuleRef` 实例(下文称为**模块引用**) 拥有 `get()` 方法。该方法获取一个提供者，控制器或者通过注入令牌/类名获取一个在当前模块中可注入对象(例如守卫或拦截器等)。

  

```typescript

@Injectable()

export class CatsService implements OnModuleInit {

private service: Service;

constructor(private moduleRef: ModuleRef) {}

  

onModuleInit() {

this.service = this.moduleRef.get(Service);

// this.moduleRef.get(Service, { strict: false });// { strict: false } 获取全局上下文的 Provider

}

}

```

  

>不能通过 `get()` 方法获取一个范围的提供者(暂态的或者请求范围的)。

  
  
  

### 处理作用域 Provider

  

要动态处理一个范围提供者(瞬态的或请求范围的)，使用 `resolve()` 方法并将提供者的注入令牌作为参数提供给方法。

  

```typescript

@Injectable()

export class CatsService implements OnModuleInit {

private transientService: TransientService;

constructor(private moduleRef: ModuleRef) {}

  

async onModuleInit() {

this.transientService = await this.moduleRef.resolve(TransientService);

}

}

```

  
  
  

**独一无二的实例**

  

要在不同的 `resolve()` 调用之间产生一个单例，并保证他们共享同样生成的 DI 容器子树，向 `resolve()` 方法传递一个上下文引用，使用 `ContextIdFactory` 类来生成上下文引用。该类提供了一个 `create()` 方法，返回一个合适的独一无二的引用。

  

```typescript

@Injectable()

export class CatsService implements OnModuleInit {

constructor(private moduleRef: ModuleRef) {}

  

async onModuleInit() {

const contextId = ContextIdFactory.create();

const transientServices = await Promise.all([

this.moduleRef.resolve(TransientService, contextId),

this.moduleRef.resolve(TransientService, contextId),

]);

console.log(transientServices[0] === transientServices[1]); // true

}

}

```

  
  

### 注册 REQUEST Provider

  

通过 ContextIdFactory.create() 可以手动的生成上下文标识符，以此去表示 DI 子树中还没有在 Nest 的依赖注入时未示例化并管理的 REQUEST Provider。

  

```typescript

const contextId = ContextIdFactory.create();

this.moduleRef.registerRequestByContextId(/* YOUR_REQUEST_OBJECT */, contextId);

```

  
  

### 获取当前子树

  

有时候你可能会想创建一个当前请求上下文中的一个 REQUEST 范围的 Provider。

为了共享同一个 DI 容器子树，你必须获取当前上下文标识符而不是重新生成一个。为了获取这个上下文标识符，你需要一个通过 @Inject 装饰器注入的 request 对象

  

```typescript

@Injectable()

export class CatsService {

constructor(

@Inject(REQUEST) private request: Record<string, unknown>,

) {

const contextId = ContextIdFactory.getByRequest(this.request);

const catsRepository = await this.moduleRef.resolve(CatsRepository, contextId);

}

}

```

  
  
  

### [动态实例化自定义类](https://docs.nestjs.cn/10/fundamentals?id=%e5%8a%a8%e6%80%81%e5%ae%9e%e4%be%8b%e5%8c%96%e8%87%aa%e5%ae%9a%e4%b9%89%e7%b1%bb)

  

要动态实例化一个之前未注册的类作为提供者，使用模块引用的 `create()` 方法。

  

```typescript

@Injectable()

export class CatsService implements OnModuleInit {

private catsFactory: CatsFactory;

constructor(private moduleRef: ModuleRef) {}

  

async onModuleInit() {

this.catsFactory = await this.moduleRef.create(CatsFactory);

}

}

```

  
  
  

## 懒加载模块

  

默认情况下，模块都是实时加载的，这意味着当应用加载完成的时候模块也已经加载好了。

随着这样的默认行为在多数的状况下是可行的，但是当遇到了 Serverless 应用的时候，这就成为了一个性能瓶颈。

  
  

### 开始

  

为了命令式的加载模块，Nest 提供了 LazyModuleLoader 类，这个类可以正常的注入到一个类中。

  

```typescript

@Injectable()

export class CatsService {

constructor(private lazyModuleLoader: LazyModuleLoader) {}

}

```

  
  

另外一个方式是，在你的应用启动文件中可以获取到一个对 LazyModuleLoader Provider 的引用。

  

```typescript

// 启动文件。"app" represents a Nest application instance

const lazyModuleLoader = app.get(LazyModuleLoader);

  
  

// 应用

const { LazyModule } = await import('./lazy.module');

const moduleRef = await this.lazyModuleLoader.load(() => LazyModule);

```

  
  

### 懒加载 Controller、网关和解析器

  

因为 Nest 中的控制器(或 GraphQL 应用程序中的解析器)表示一组 router/path/topic(或查询参数/mutation)，你不能使用 `LazyModuleLoader` 类来惰性加载它们。

  
  
  

## 上下文

  
  

### ArgumentsHost 类

  

`ArgumentsHost`类提供了检索传递给处理程序的参数的方法。 它允许选择适当的上下文(例如，HTTP、RPC(微服务)或 WebSockets)来检索参数。 框架提供了一个`ArgumentsHost`的实例，通常作为`host`参数引用，在你想要访问它的地方。

  

`ArgumentsHost` 简单地抽象为处理程序参数。例如，在 HTTP 应用中(使用 `@nestjs/platform-express` 时),host 对象封装了 Express 的 `[request, response, next]` 数组

  

> 此外，在GraphQL应用中，host包含`[root, args, context, info]`数组。

  
  

#### 当前应用上下文

  

当构建通用的守卫，过滤器和拦截器时，意味着要跨应用上下文运行，我们需要在当前运行时确定请求的应用类型。可以使用 `ArgumentsHost` 的 `getType()` 方法。

  

```typescript

if (host.getType() === 'http') {

// do something that is only important in the context of regular HTTP requests (REST)

} else if (host.getType() === 'rpc') {

// do something that is only important in the context of Microservice requests

} else if (host.getType<GqlContextType>() === 'graphql') {

// do something that is only important in the context of GraphQL requests

}

```

  

#### 处理程序参数

  

要获取传递给处理程序的参数数组，使用host对象的`getArgs()`方法。

  

```typescript

const [req, res, next] = host.getArgs();

```

  

可以使用`getArgByIndex()`根据索引获取指定参数:

  

```typescript

const request = host.getArgByIndex(0);

const response = host.getArgByIndex(1);

```

  
  
  

还有另一种获取参数的 `switchToHttp` () 方法。

  

```typescript

const ctx = host.switchToHttp();

const request = ctx.getRequest<Request>();

const response = ctx.getResponse<Response>();

```

  
  

#### [反射和元数据](https://docs.nestjs.cn/10/fundamentals?id=%e5%8f%8d%e5%b0%84%e5%92%8c%e5%85%83%e6%95%b0%e6%8d%ae)

  

Nest 提供了通过 `@SetMetadata()` 装饰器将自定义元数据附加在路径处理程序的能力。我们可以在类中获取这些元数据来执行特定决策。

  

```typescript

@Post()

@SetMetadata('roles', ['admin'])

async create(@Body() createCatDto: CreateCatDto) {

this.catsService.create(createCatDto);

}

```

  

```typescript

  

// roles.decorator.ts

import { SetMetadata } from '@nestjs/common';

  

export const Roles = (...roles: string[]) => SetMetadata('roles', roles); // 更清晰，且强类型

  
  
  

// cats.controller.ts

@Post()

@Roles('admin')

async create(@Body() createCatDto: CreateCatDto) {

this.catsService.create(createCatDto);

}

```

  
  
  
  

## 生命周期

  
  

### 生命周期函数

  
  

![[Pasted image 20231206234432.png]]

  
  

|生命周期钩子方法|生命周期时间触发钩子方法调用|

|---|---|

|`OnModuleInit()`|初始化主模块依赖处理后调用一次|

|`OnApplicationBootstrap()`|在应用程序完全启动并监听连接后调用一次|

|`OnModuleDestroy()`|收到终止信号(例如SIGTERM)后调用|

|`beforeApplicationShutdown()`|在`onModuleDestroy()`完成(Promise被resolved或者rejected)；一旦完成，将关闭所有连接(调用app.close() 方法).|

|`OnApplicationShutdown()`|连接关闭处理时调用(app.close())|

  

> 上述列出的生命周期钩子没有被请求范围类触发。请求范围类并没有和生命周期以及不可预测的寿命绑定。他们为每个请求单独创建，并在响应发送后通过垃圾清理系统自动清理。

  
  

使用示例：

```typescript

@Injectable()

class UsersService implements OnApplicationShutdown {

onApplicationShutdown(signal) {

console.log(signal); // e.g. "SIGINT"

}

}

```

  
  

### [Application Shutdown](https://docs.nestjs.cn/10/fundamentals?id=application-shutdown)

  
  

`onModuleDestroy()`, `beforeApplicationShutdown()` 和 `onApplicationShutdown()` 钩子程序响应系统终止信号(当应用程序通过显示调用 `app.close()` 或者收到 `SIGTERM` 系统信号时)，以优雅地关闭 `Nest` 应用程序。这一功能通常用于 `Kubernetes` 、`Heroku` 或类似的服务。

  

系统关闭钩子消耗系统资源，因此默认是禁用的。要使用此钩子，必须通过 `enableShutdownHooks()` 激活侦听器。

如果要在一个单独 Node 线程中运行多个 Nest 应用(例如，使用多个 Jest 运行测试)，Node 会因为监听者太多分身乏术。要在单个 Node 进程中运行多个实例时尤其要注意这一点。

  

```typescript

import { NestFactory } from '@nestjs/core';

import { AppModule } from './app.module';

  

async function bootstrap() {

const app = await NestFactory.create(AppModule);

// Starts listening to shutdown hooks

app.enableShutdownHooks();

await app.listen(3000);

}

bootstrap();

```


# 技术

## 数据库

### TypeORM 集成

为了与 `SQL` 和 `NoSQL` 数据库集成，`Nest` 提供了 `@nestjs/typeorm` 包。`Nest` 使用 [TypeORM](https://github.com/typeorm/typeorm) 是因为它是 `TypeScript` 中最成熟的对象关系映射器( `ORM` )。因为它是用 `TypeScript` 编写的，所以可以很好地与 `Nest` 框架集成。

```typescript
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { User } from './users/user.entity';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      type: 'mysql',
      host: 'localhost',
      port: 3306,
      username: 'root',
      password: 'root',
      database: 'test',
      entities: [User
      
      
      
      ],
      synchronize: true,
    }),
  ],
})
export class AppModule {}
```

> 警告：设置 `synchronize: true` 不能被用于生产环境，否则您可能会丢失生产环境数据



一旦完成，`TypeORM` 的 `DataSource` 和 `EntityManager` 对象就可以在整个项目中注入(不需要导入任何模块)，例如:

```typescript
import { DataSource } from 'typeorm';

@Dependencies(DataSource)
@Module({
  imports: [TypeOrmModule.forRoot(), UsersModule],
})
export class AppModule {
  constructor(dataSource) {
    this.dataSource = dataSource;
  }
}
```



### [存储库模式](https://docs.nestjs.cn/10/techniques?id=%e5%ad%98%e5%82%a8%e5%ba%93%e6%a8%a1%e5%bc%8f)

`TypeORM` 支持存储库设计模式，因此每个实体都有自己的存储库。可以从数据库连接获得这些存储库。


要开始使用 `user` 实体，我们需要在模块的 `forRoot()` 方法的选项中（除非你使用一个静态的全局路径）将它插入 `entities` 数组中来让 `TypeORM` 知道它的存在。



### [关系](https://docs.nestjs.cn/10/techniques?id=%e5%85%b3%e7%b3%bb)

关系是指两个或多个表之间的联系。关系基于每个表中的常规字段，通常包含主键和外键。

关系有三种：

|名称|说明|
|---|---|
|一对一|主表中的每一行在外部表中有且仅有一个对应行。使用`@OneToOne()`装饰器来定义这种类型的关系|
|一对多/多对一|主表中的每一行在外部表中有一个或多的对应行。使用`@OneToMany()`和`@ManyToOne()`装饰器来定义这种类型的关系|
|多对多|主表中的每一行在外部表中有多个对应行，外部表中的每个记录在主表中也有多个行。使用`@ManyToMany()`装饰器来定义这种类型的关系|



### [自动载入实体](https://docs.nestjs.cn/10/techniques?id=%e8%87%aa%e5%8a%a8%e8%bd%bd%e5%85%a5%e5%ae%9e%e4%bd%93)

手动将实体一一添加到连接选项的`entities`数组中的工作会很无聊。此外，在根模块中涉及实体破坏了应用的域边界，并可能将应用的细节泄露给应用的其他部分。针对这一情况，可以使用静态全局路径（例如, dist/*_/_.entity{.ts,.js})。

注意，`webpack`不支持全局路径，因此如果你要在单一仓库(Monorepo)中构建应用，可能不能使用全局路径。针对这一问题，有另外一个可选的方案。在配置对象的属性中(传递给`forRoot()`方法的)设置`autoLoadEntities`属性为`true`来自动载入实体，示意如下：

> app.module.ts

```typescript
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';

@Module({
  imports: [
    TypeOrmModule.forRoot({
      ...
      autoLoadEntities: true,
    }),
  ],
})
export class AppModule {}
```

通过配置这一选项，每个通过`forFeature()`注册的实体都会自动添加到配置对象的`entities`数组中。

> 注意，那些没有通过 `forFeature()` 方法注册，而仅仅是在实体中被引用（通过关系）的实体不能通过 `autoLoadEntities` 配置被包含。



### [事务](https://docs.nestjs.cn/10/techniques?id=%e4%ba%8b%e5%8a%a1)

数据库事务代表在数据库管理系统（DBMS）中针对数据库的一组操作，这组操作是有关的、可靠的并且和其他事务相互独立的。一个事务通常可以代表数据库中的任何变更（[了解更多](https://zh.wikipedia.org/wiki/%E6%95%B0%E6%8D%AE%E5%BA%93%E4%BA%8B%E5%8A%A1))。

在 [TypeORM 事务](https://typeorm.io/#/transactions)中有很多不同策略来处理事务，我们推荐使用 `QueryRunner` 类，因为它对事务是完全可控的。

### [订阅者](https://docs.nestjs.cn/10/techniques?id=%e8%ae%a2%e9%98%85%e8%80%85)

使用 TypeORM[订阅者](https://typeorm.io/#/listeners-and-subscribers/what-is-a-subscriber)，你可以监听特定的实体事件。

```typescript
import {
  DataSource,
  EntitySubscriberInterface,
  EventSubscriber,
  InsertEvent,
} from 'typeorm';
import { User } from './user.entity';

@EventSubscriber()
export class UserSubscriber implements EntitySubscriberInterface<User> {
  constructor(dataSource: DataSource) {
    dataSource.subscribers.push(this);
  }

  listenTo() {
    return User;
  }

  beforeInsert(event: InsertEvent<User>) {
    console.log(`BEFORE USER INSERTED: `, event.entity);
  }
}
```


	### [多个数据库](https://docs.nestjs.cn/10/techniques?id=%e5%a4%9a%e4%b8%aa%e6%95%b0%e6%8d%ae%e5%ba%93)

某些项目可能需要多个数据库连接。这也可以通过本模块实现。要使用多个连接，首先要做的是创建这些连接。在这种情况下，连接命名成为必填项。

> 如果未为连接设置任何 `name` ，则该连接的名称将设置为 `default`。请注意，不应该有多个没有名称或同名的连接，否则它们会被覆盖。


```typescript
const defaultOptions = {
  type: 'postgres',
  port: 5432,
  username: 'user',
  password: 'password',
  database: 'db',
  synchronize: true,
};

@Module({
  imports: [
    TypeOrmModule.forRoot({
      ...defaultOptions,
      host: 'user_db_host',
      entities: [User],
    }),
    TypeOrmModule.forRoot({
      ...defaultOptions,
      name: 'albumsConnection',
      host: 'album_db_host',
      entities: [Album],
    }),
  ],
})
export class AppModule {}
```
### [自定义存储库](https://docs.nestjs.cn/10/techniques?id=%e5%ae%9a%e5%88%b6%e5%ad%98%e5%82%a8%e5%ba%93)

`TypeORM` 提供称为自定义存储库的功能。要了解有关它的更多信息，请访问此[页面](https://typeorm.io/#/custom-repository)。基本上，自定义存储库允许您扩展基本存储库类，并使用几种特殊方法对其进行丰富。

你可以创建一个自定义的存储库，来包含一系列的关于数据库操作的方法。


要创建自定义存储库，请使用 `@EntityRepository()` 装饰器和扩展 `Repository` 类。

```typescript
@EntityRepository(Author)
export class AuthorRepository extends Repository<Author> {}
```

`@EntityRepository()` 和 `Repository` 来自 `typeorm` 包。

创建类后，下一步是将实例化责任移交给 `Nest`。为此，我们必须将 `AuthorRepository` 类传递给 `TypeOrm.forFeature()` 函数。

```typescript
@Module({
  imports: [TypeOrmModule.forFeature([AuthorRepository])],
  controller: [AuthorController],
  providers: [AuthorService],
})
export class AuthorModule {}
```

之后，只需使用以下构造注入存储库：

```typescript
@Injectable()
export class AuthorService {
  constructor(private readonly authorRepository: AuthorRepository) {}
}
```

### [异步配置](https://docs.nestjs.cn/10/techniques?id=%e5%bc%82%e6%ad%a5%e9%85%8d%e7%bd%ae)

通常，您可能希望异步传递模块选项，而不是事先传递它们。

方法一，使用 useFactory：

```typescript
TypeOrmModule.forRootAsync({
  useFactory: () => ({
    type: 'mysql',
    host: 'localhost',
    port: 3306,
    username: 'root',
    password: 'root',
    database: 'test',
    entities: [__dirname + '/**/*.entity{.ts,.js}'],
    synchronize: true,
  }),
});
```

方法二，使用 useClass：

```typescript
TypeOrmModule.forRootAsync({
  useClass: TypeOrmConfigService,
});
```



### ------------

### Sequelize 集成

暂时不学习，需要的时候再学习补充。先专注于 typeorm

## [配置](https://docs.nestjs.cn/10/techniques?id=%e9%85%8d%e7%bd%ae)

应用程序通常在不同的**环境**中运行。根据环境的不同，应该使用不同的配置设置。例如，通常本地环境依赖于特定的数据库凭据，仅对本地 DB 实例有效。生产环境将使用一组单独的 DB 凭据。由于配置变量会更改，所以最佳实践是将[配置变量](https://12factor.net/config)存储在环境中。

在 `Nest` 中使用这种技术的一个好方法是创建一个 `ConfigModule` ，它暴露一个 `ConfigService` ，根据 `$NODE_ENV` 环境变量加载适当的 `.env` 文件。虽然您可以选择自己编写这样的模块，但为方便起见，Nest 提供了开箱即用的 `@ nestjs/config` 软件包。我们将在本章中介绍该软件包。

#### [自定义 env 文件路径](https://docs.nestjs.cn/10/techniques?id=%e8%87%aa%e5%ae%9a%e4%b9%89-env-%e6%96%87%e4%bb%b6%e8%b7%af%e5%be%84)

默认情况下，程序在应用程序的根目录中查找`.env`文件。 要为`.env`文件指定另一个路径，请配置`forRoot()`的配置对象 envFilePath 属性(可选)，如下所示：

```typescript
ConfigModule.forRoot({
  envFilePath: '.development.env',
});
```

您还可以像这样为.env 文件指定多个路径：

```typescript
ConfigModule.forRoot({
  envFilePath: ['.env.development.local', '.env.development'],
});
```

如果在多个文件中发现同一个变量，则第一个变量优先。

#### [全局使用](https://docs.nestjs.cn/10/techniques?id=%e5%85%a8%e5%b1%80%e4%bd%bf%e7%94%a8)

当您想在其他模块中使用 `ConfigModule` 时，需要将其导入（这是任何 Nest 模块的标准配置）。或者，通过将 `options` 对象的 `isGlobal` 属性设置为 `true`，将其声明为[全局模块](https://docs.nestjs.cn/8/modules?id=%E5%85%A8%E5%B1%80%E6%A8%A1%E5%9D%97)，

```typescript
ConfigModule.forRoot({
  isGlobal: true,
});
```


#### [自定义配置文件](https://docs.nestjs.cn/10/techniques?id=%e8%87%aa%e5%ae%9a%e4%b9%89%e9%85%8d%e7%bd%ae%e6%96%87%e4%bb%b6)

对于更复杂的项目，您可以利用自定义配置文件返回嵌套的配置对象。这使您可以按功能对相关配置设置进行分组（例如，与数据库相关的设置），并将相关设置存储在单个文件中，以帮助独立管理它们

```typescript
// config/configuration.ts
export default () => ({
  port: parseInt(process.env.PORT, 10) || 3000,
  database: {
    host: process.env.DATABASE_HOST,
    port: parseInt(process.env.DATABASE_PORT, 10) || 5432
  }
});
```

```typescript
// app.modules.ts

import configuration from './config/configuration';
@Module({
  imports: [
    ConfigModule.forRoot({
      load: [configuration],
    }),
  ],
})
export class AppModule {}
```

```typescript
// feature.module.ts
@Module({
  imports: [ConfigModule],
  ...
})
```

```typescript
// feature.service.ts
	const dbUser = this.configService.get<string>('DATABASE_USER');
```




## [验证](https://docs.nestjs.cn/10/techniques?id=%e9%aa%8c%e8%af%81)

验证网络应用中传递的任何数据是一种最佳实践。为了自动验证传入请求， Nest 提供了几个开箱即用的管道。

- `ValidationPipe`
- `ParseIntPipe`
- `ParseBoolPipe`
- `ParseArrayPipe`
- `ParseUUIDPipe`


### [自动验证](https://docs.nestjs.cn/10/techniques?id=%e8%87%aa%e5%8a%a8%e9%aa%8c%e8%af%81)

绑定 `ValidationPipe` 到整个应用程序，因此，将自动保护所有接口免受不正确的数据的影响。

```typescript
// app.module.ts
async function bootstrap() {
  const app = await NestFactory.create(ApplicationModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();
```

```typescript

// service
@Post()
create(@Body() createUserDto: CreateUserDto) {
  return 'This action adds a new user';
}
```

```typescript
// dto
import { IsEmail, IsNotEmpty } from 'class-validator';

export class CreateUserDto {
  @IsEmail()
  email: string;

  @IsNotEmpty()
  password: string;
}
```


> 当你导入你的 DTO 时，你不能使用仅类型的导入，因为类型会在运行时被擦除，记得用 `import { CreateUserDto }` 而不是 `import type { CreateUserDto }` 。



### [禁用详细错误](https://docs.nestjs.cn/10/techniques?id=%e7%a6%81%e7%94%a8%e8%af%a6%e7%bb%86%e9%94%99%e8%af%af)

错误消息有助于解释请求中的错误。然而，一些生产环境倾向于禁用详细的错误。通过向 `ValidationPipe` 传递一个选项对象来做到这一点:

```typescript
app.useGlobalPipes(
  new ValidationPipe({
    disableErrorMessages: true,
  })
);
```

现在，不会将错误消息返回给最终用户。


### [剥离属性](https://docs.nestjs.cn/10/techniques?id=%e5%89%a5%e7%a6%bb%e5%b1%9e%e6%80%a7)

我们的 `ValidationPipe` 还可以过滤掉方法处理程序不应该接收的属性。在这种情况下，我们可以对可接受的属性进行**白名单**，白名单中不包含的任何属性都会自动从结果对象中删除。例如，如果我们的处理程序需要 `email` 和 `password`，但是一个请求还包含一个 `age` 属性，那么这个属性可以从结果 `DTO` 中自动删除。要启用这种行为，请将 `whitelist` 设置为 `true` 。

```typescript
app.useGlobalPipes(
  new ValidationPipe({
    whitelist: true,
  })
);
```

当设置为 `true` 时，这将自动删除非白名单属性


### [负载对象转换(Transform)](https://docs.nestjs.cn/10/techniques?id=%e8%b4%9f%e8%bd%bd%e5%af%b9%e8%b1%a1%e8%bd%ac%e6%8d%a2transform)

来自网络的有效负载是普通的 JavaScript 对象。`ValidationPipe` 可以根据对象的 `DTO` 类自动将有效负载转换为对象类型。若要启用自动转换，请将 `transform` 设置为 `true`。这可以在方法级别使用：

> cats.control.ts

```typescript
@Post()
@UsePipes(new ValidationPipe({ transform: true }))
async create(@Body() createCatDto: CreateCatDto) {
  this.catsService.create(createCatDto);
}
```

要在全局启用这一行为，将选项设置到一个全局管道中：

```typescript
app.useGlobalPipes(
  new ValidationPipe({
    transform: true,
  })
);
```

要使能自动转换选项，`ValidationPipe` 将执行简单类型转换。


### [转换和验证数组](https://docs.nestjs.cn/10/techniques?id=%e8%bd%ac%e6%8d%a2%e5%92%8c%e9%aa%8c%e8%af%81%e6%95%b0%e7%bb%84)

TypeScript 不存储泛型或接口的元数据，因此当你在 DTO 中使用它们的时候， `ValidationPipe` 可能不能正确验证输入数据。例如，在下列代码中， `createUserDto` 不能正确验证。

```typescript
@Post()
createBulk(@Body() createUserDtos: CreateUserDto[]) {
  return 'This action adds new users';
}
```

要验证数组，创建一个包裹了该数组的专用类，或者使用 `ParseArrayPipe` 。

```typescript
@Post()
createBulk(
  @Body(new ParseArrayPipe({ items: CreateUserDto }))
  createUserDtos: CreateUserDto[],
) {
  return 'This action adds new users';
}
```

此外， `ParseArrayPipe` 可能需要手动解析查询参数。让我们考虑一个返回作为查询参数传递的标识的 `users` 的 `findByIds()` 方法：

```typescript
@Get()
findByIds(
  @Query('id', new ParseArrayPipe({ items: Number, separator: ',' }))
  ids: number[],
) {
  return 'This action returns users by ids';
}
```


## 缓存

暂时不需要


## [序列化（Serialization）](https://docs.nestjs.cn/10/techniques?id=%e5%ba%8f%e5%88%97%e5%8c%96%ef%bc%88serialization%ef%bc%89)

序列化(`Serialization`)是一个在网络响应中返回对象前的过程。这是一个适合转换和净化要返回给客户的数据的地方。例如，应始终从最终响应中排除敏感数据（如用户密码）。



### [排除属性](https://docs.nestjs.cn/10/techniques?id=%e6%8e%92%e9%99%a4%e5%b1%9e%e6%80%a7)

我们假设要从一个用户实体中自动排除`password`属性。我们给实体做如下注释：

```typescript
import { Exclude } from 'class-transformer';

export class UserEntity {
  id: number;
  firstName: string;
  lastName: string;

  @Exclude()
  password: string;

  constructor(partial: Partial<UserEntity>) {
    Object.assign(this, partial);
  }
}
```

然后，直接在控制器的方法中调用就能获得此类的实例。

```typescript
@UseInterceptors(ClassSerializerInterceptor)
@Get()
findOne(): UserEntity {
  return new UserEntity({
    id: 1,
    firstName: 'Kamil',
    lastName: 'Mysliwiec',
    password: 'password',
  });
}
```

我们必须返回一个类的实体。如果你返回一个普通的 JavaScript 对象，例如，`{user: new UserEntity()}`,该对象将不会被正常序列化。


### [公开属性](https://docs.nestjs.cn/10/techniques?id=%e5%85%ac%e5%bc%80%e5%b1%9e%e6%80%a7)

您可以使用 `@Expose()` 装饰器来为属性提供别名，或者执行一个函数来计算属性值(类似于 `getter` 函数)，如下所示。

```typescript
@Expose()
get fullName(): string {
  return `${this.firstName} ${this.lastName}`;
}
```

### [变换](https://docs.nestjs.cn/10/techniques?id=%e5%8f%98%e6%8d%a2)

您可以使用 `@Transform()` 装饰器执行其他数据转换。

`@Transform()` 是 class-transformer 库中的一个装饰器，它允许你在将普通 JavaScript 对象转换为特定类的实例时，对特定属性进行自定义的转换。

例如，假设你有一个 `UserDto` 类，其中的 `birthDate` 属性应该是 `Date` 类型，但客户端发送的数据中 `birthDate` 是一个字符串。你可以使用 `@Transform()` 装饰器将这个字符串转换为 `Date` 对象：

```
class UserDto {
  @Transform(({ value }) => new Date(value))、
  birthDate: Date;
}
```



在这个例子中，`@Transform()` 装饰器接收一个函数，这个函数接收一个包含 `value` 属性的对象作为参数，`value` 是原始的属性值。这个函数应该返回转换后的值。



## [定时任务](https://docs.nestjs.cn/10/techniques?id=%e5%ae%9a%e6%97%b6%e4%bb%bb%e5%8a%a1)

定时任务允许你按照指定的日期/时间、一定时间间隔或者一定时间后单次执行来调度(`scheduling`)任意代码（方法/函数）。


### [声明计时工作(cron job)](https://docs.nestjs.cn/10/techniques?id=%e5%a3%b0%e6%98%8e%e8%ae%a1%e6%97%b6%e5%b7%a5%e4%bd%9ccron-job)

一个计时工作调度任何函数（方法调用）以自动运行， 计时工作可以：

- 单次，在指定日期/时间
- 重复循环：重复工作可以在指定周期中指定执行（例如，每小时，每周，或者每 5 分钟）

在包含要运行代码的方法定义前使用 `@Cron()` 装饰器声明一个计时工作，如下：

```typescript
import { Module } from '@nestjs/common';
import { ScheduleModule } from '@nestjs/schedule';

@Module({
  imports: [ScheduleModule.forRoot()],
})
export class AppModule {}
```

```typescript
import { Injectable, Logger } from '@nestjs/common';
import { Cron } from '@nestjs/schedule';

@Injectable()
export class TasksService {
  private readonly logger = new Logger(TasksService.name);

  @Cron('45 * * * * *')
  handleCron() {
    this.logger.debug('Called when the current second is 45');
  }
}
```

在这个例子中，`handleCron()`方法将在当前时间为`45秒`时定期执行。换句话说，该方法每分钟执行一次，在第 45 秒执行。

`@Cron()`装饰器支持标准的[cron patterns](http://crontab.org/):

- 星号通配符 (也就是 *)
- 范围（也就是 1-3,5)
- 步长（也就是 */2)

在上述例子中，我们给装饰器传递了`45 * * * * *`，下列键展示了每个位置的计时模式字符串的意义：

```bash
* * * * * *
| | | | | |
| | | | | day of week
| | | | month
| | | day of month
| | hour
| minute
second (optional)
```

一些示例的计时模式包括：

|名称|含义|
|---|---|
|* * * * * *|每秒|
|45 * * * * *|每分钟第 45 秒
|_ 10 _ * * *|每小时，从第 10 分钟开始|
|0 _/30 9-17 _ * *|上午 9 点到下午 5 点之间每 30 分钟|
|0 30 11 * * 1-5|周一至周五上午 11:30|


### [声明间隔](https://docs.nestjs.cn/10/techniques?id=%e5%a3%b0%e6%98%8e%e9%97%b4%e9%9a%94)

要声明一个以一定间隔运行的方法，使用`@Interval()`装饰器前缀。以毫秒单位的`number`传递间隔值，如下：

```typescript
@Interval(10000)
handleInterval() {
  this.logger.debug('Called every 10 seconds');
}
```

本机制在底层使用`JavaScript`的`setInterval()`函数。你也可以使用定期调度工作来应用一个定时任务。

如果你希望在声明类之外通过[动态 API](https://docs.nestjs.com/techniques/task-scheduling#dynamic-schedule-module-api)控制你声明的时间间隔。使用下列结构将名称与间隔关联起来。

```typescript
@Interval('notifications', 2500)
handleInterval() {}
```


### [声明延时任务](https://docs.nestjs.cn/10/techniques?id=%e5%a3%b0%e6%98%8e%e5%bb%b6%e6%97%b6%e4%bb%bb%e5%8a%a1)

要声明一个在指定时间后运行（一次）的方法，使用`@Timeout()`装饰器前缀。将从应用启动的相关时间偏移量（毫秒）传递给装饰器，如下：

```typescript
@Timeout(5000)
handleTimeout() {
  this.logger.debug('Called once after 5 seconds');
}
```

本机制在底层使用 JavaScript 的 `setTimeout()` 方法


### 动态任务 

### 动态超时


### 动态间隔



## [队列](https://docs.nestjs.cn/10/techniques?id=%e9%98%9f%e5%88%97)

队列是一种有用的设计模式，可以帮助你处理一般应用规模和性能的挑战。


（等待补充

## [Cookies](https://docs.nestjs.cn/10/techniques?id=cookies)

一个 `HTTP cookie` 是指存储在用户浏览器中的一小段数据。

使用：
```TypeScript
import * as cookieParser from 'cookie-parser';
// somewhere in your initialization file
app.use(cookieParser());
```

获取：
```TypeScript
@Get()
findAll(@Req() request: Request) {
  console.log(request.cookies); // or "request.cookies['cookieKey']"
  // or console.log(request.signedCookies);
}
```

响应设置：
```TypeScript
@Get()
findAll(@Res({ passthrough: true }) response: Response) {
  response.cookie('key', 'value')
}
```



## [事件](https://docs.nestjs.cn/10/techniques?id=%e4%ba%8b%e4%bb%b6)

[Event Emitter 事件发射器](https://www.npmjs.com/package/@nestjs/event-emitter) 包(`@nestjs/event-emitter`)提供了一个简单的观察者实现，允许你订阅和监听在你应用中发生的不同事件。


## [压缩](https://docs.nestjs.cn/10/techniques?id=%e5%8e%8b%e7%bc%a9)

压缩可以大大减小响应主体的大小，从而提高 `Web` 应用程序的速度。

在大业务量的生产环境网站中，强烈推荐将压缩功能从应用服务器中卸载——典型做法是使用反向代理（例如 Nginx)。在这种情况下，你不应该使用压缩中间件。


## [文件上传](https://docs.nestjs.cn/10/techniques?id=%e6%96%87%e4%bb%b6%e4%b8%8a%e4%bc%a0)

为了处理文件上传，Nest 提供了一个内置的基于 [multer](https://github.com/expressjs/multer) 中间件包的 Express 模块。Multer 处理以 `multipart/form-data` 格式发送的数据，该格式主要用于通过 HTTP `POST` 请求上传文件。

```typescript
@Post('upload')
@UseInterceptors(FileInterceptor('file'))
uploadFile(@UploadedFile() file: Express.Multer.File) {
  console.log(file);
}
```



# 安全

## [CORS](https://docs.nestjs.cn/10/security?id=cors)

跨源资源共享（`CORS`）是一种允许从另一个域请求资源的机制。在底层，`Nest` 使用了 Express 的[cors](https://github.com/expressjs/cors) 包，它提供了一系列选项，您可以根据自己的要求进行自定义。

### [开始](https://docs.nestjs.cn/10/security?id=%e5%bc%80%e5%a7%8b)

为了启用 `CORS`，必须调用 `enableCors()` 方法。

```typescript
const app = await NestFactory.create(AppModule);
app.enableCors();
await app.listen(3000);
```

`enableCors()`方法需要一个可选的配置对象参数。这个对象的可用属性在官方 [CORS](https://github.com/expressjs/cors#configuration-options) 文档中有所描述。另一种方法是传递一个[回调函数](https://github.com/expressjs/cors#configuring-cors-asynchronously)，来让你根据请求异步地定义配置对象。

或者通过 `create()` 方法的选项对象启用 CORS。将 `cors`属性设置为`true`，以使用默认设置启用 CORS。又或者，传递一个 [CORS 配置对象](https://github.com/expressjs/cors#configuration-options) 或 [回调函数](https://github.com/expressjs/cors#configuring-cors-asynchronously) 作为 `cors` 属性的值来自定义其行为。

```typescript
const app = await NestFactory.create(AppModule, { cors: true });
await app.listen(3000);
```



# 微服务



