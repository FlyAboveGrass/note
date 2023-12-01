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
- forRoutes(CatsController)
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