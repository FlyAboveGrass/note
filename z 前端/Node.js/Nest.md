
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

### 动态模块的示例

使用 Nest 的一个很好的例子是 **配置模块** 。许多应用程序发现，通过使用配置模块外部化配置细节非常有用。这使得在不同的部署中动态更改应用程序设置变得很容易.

动态模块使我们能够向被导入的模块传递参数，这样我们就可以更改它的行为。使用动态模块特性，我们可以使配置模块 **动态** ，以便消费模块可以使用 API 来控制在导入配置模块时如何定制配置模块。

换句话说，动态模块提供了一个 API，用于将一个模块导入到另一个模块，并在导入时定制该模块的属性和行为，而不是使用我们目前看到的静态绑定。

```
import { Module } from '@nestjs/common';
import { AppController } from './app.controller';
import { AppService } from './app.service';
import { ConfigModule } from './config/config.module';

@Module({
  imports: [ConfigModule.register({ folder: './config`})],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {}

```

`register()` 方法是由我们定义的，所以我们可以接受任何我们喜欢的输入参数。`register()` 方法将返回一个 `DynamicModule`。动态模块只不过是在运行时创建的模块，具有与静态模块完全相同的属性，外加一个名为 `module` 的额外属性。




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