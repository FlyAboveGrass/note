
#Node #Nest #后端 
# 一个普通的业务 Module 包含什么

以一个最简单的列表查询为例

## imports
### 实体类  Entity

**装饰器**
1. `@Entity()`: 用于标记一个类为数据库表的模型。
2. `@ApiProperty`： 添加对模型属性的描述
3. `@PrimaryGeneratedColumn()`: 用于标记一个字段为主键，并且该字段的值是自动生成的。
4. `@Column()`: 用于标记一个字段为数据库表的列。
5. `@CreateDateColumn()`, `@UpdateDateColumn()`: 用于自动创建和更新日期时间戳。
6. `@ManyToOne()`, `@OneToMany()`, `@OneToOne()`, `@ManyToMany()`: 用于标记实体之间的关系。
7. `@JoinColumn()`: 用于标记实体的外键。
8. `@Index()`: 用于在数据库中为特定列创建索引。

例子如下
```
import { Entity, PrimaryGeneratedColumn, Column, CreateDateColumn, UpdateDateColumn, ManyToOne, JoinColumn, Index } from 'typeorm';
import { User } from './user.entity';

@Entity('todos')
export class Todo {
  @PrimaryGeneratedColumn()
  id: number;

  @Column({ length: 500 })
  @Index()
  @ApiProperty({ description: 'todo title' })
  title: string;

  @Column('text')
  description: string;

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;

  @ManyToOne(() => User)
  @JoinColumn({ name: 'userId' })
  user: User;
}
```
## controllers
处理传入的**请求**和向客户端返回**响应**。


**装饰器**
1. 类装饰器：
    - `@ApiTags`: 用于给控制器类添加标签，这些标签在 Swagger UI 中显示，帮助组织和分类 API。
    - `@UseGuards`: 用于指定一些守卫，这些守卫会在每个路由处理器执行前运行，用于处理权限控制等逻辑。
    - `@Controller`: 用于标记一个类作为 NestJS 控制器，参数通常是路由的前缀。
2. 方法装饰器：
    - `@Get`, `@Post`, `@Put`, `@Delete`: 这些装饰器用于定义路由处理器，参数是路由的路径。
    - `@ApiOperation`: 用于定义 API 操作，参数是一个对象，包含对 API 操作的描述。
    - `@ApiResult`: 用于定义 API 返回的结果，参数是一个对象，包含对返回结果的描述。
    - `@Perm`: 这是一个自定义装饰器，用于定义该路由处理器需要的权限。
3. 参数装饰器：
    - `@Query`, `@Body`, `@IdParam`: 这些装饰器用于从请求中提取参数，分别对应查询参数、请求体和路由参数。

### Guard 守卫
根据运行时出现的某些条件（例如权限，角色，访问控制列表等）来确定给定的请求是否由路由处理程序处理。

守卫类通常使用以下装饰器和API：

1. `@Injectable()`：这是一个NestJS装饰器，用于将类标记为可以由NestJS注入器处理的服务。
2. `@CanActivate()`：这是NestJS的一个装饰器，用于定义一个守卫。它需要一个返回布尔值或Promise解析为布尔值的方法。如果返回`true`，则请求处理管道继续执行，如果返回`false`，则请求被拒绝。
3.  `ExecutionContext`：这是NestJS的一个类，提供了许多有用的方法来获取关于当前请求的信息。例如， `context.switchToHttp().getRequest()` 可以获取当前的HTTP请求。
4. `Reflector`：  这是NestJS的一个类，用于在运行时获取类、方法或属性的元数据。


### 权限控制

1. 定义接口操作的时候，定义权限点，并且注入到权限点集里面
2. 角色分配
	1. 定义角色和权限：在数据库中定义不同的角色和权限。每个角色都有一组相关的权限。
	2. 用户角色分配：每个用户都分配一个或多个角色。
3. 守卫拦截。创建一个自定义的 AuthGuard，在需要进行权限控制的路由上使用 AuthGuard。AuthGuard 将验证用户的身份和权限，如果用户没有相应的权限，AuthGuard 将拒绝用户的请求。

## providers

### services

### DTO
DTO 是 Data Transfer Object 的缩写，数据传输对象。这是一个设计模式，用于将数据从一个系统或应用的一部分传输到另一部分。
通常用于描述接口的入参。

DTO (Data Transfer Object) 定义常用的装饰器主要来自 `class-validator` 和 `class-transformer` 这两个库，用于数据验证和转换。以下是一些常用的装饰器：

1. **@IsString()**：验证字段是否为字符串。
2. **@IsInt()**：验证字段是否为整数。
3. **@IsOptional()**：标记字段为可选。
4. **@IsNotEmpty()**：验证字段是否非空。
5. **@IsArray()**：验证字段是否为数组。
6. **@IsEmail()**：验证字段是否为有效的电子邮件地址。
7. **@Min()** 和 **@Max()**：验证数字字段的最小值和最大值。
8. **@Length()**：验证字符串字段的长度。
9. **@Transform()**：用于在序列化和反序列化时转换字段的值。

下面是一个示例：
```
import { IsString, IsNotEmpty, IsOptional, IsInt, Min, Max, Length } from 'class-validator';
import { Transform } from 'class-transformer';

export class CreateTodoDto {
  @IsString()
  @IsNotEmpty()
  title: string;

  @IsString()
  @IsOptional()
  description?: string;

  @IsInt()
  @Min(0)
  @Max(5)
  @Transform(({ value }) => Number(value))
  priority: number;

  @IsString()
  @Length(0, 20)
  @IsOptional()
  tag?: string;
}
```

###  `Repository`
`Repository`  是一个可以用来操作数据库的类。

`Repository`  是 TypeORM 的核心概念之一，它实现了 Unit of Work 和 Repository 设计模式。每个实体都有自己的 repository，可以通过它来操作实体相关的数据。


### 事务
事务（Transaction）是一个执行单元，表示一系列的数据库操作。这些操作要么全部成功，要么全部失败，不会出现部分操作成功的情况。事务是数据库并发控制的基本单位。

```
import { Injectable } from '@nestjs/common';
import { InjectRepository } from '@nestjs/typeorm';
import { Repository } from 'typeorm';
import { TodoEntity } from '~/modules/todo/todo.entity';
import { OtherEntity } from '~/modules/other/other.entity';
import { TodoDto, TodoUpdateDto } from './todo.dto';

@Injectable()
export class TodoService {
  constructor(
    @InjectRepository(TodoEntity)
    private todoRepository: Repository<TodoEntity>,
    @InjectRepository(OtherEntity)
    private otherRepository: Repository<OtherEntity>,
  ) {}

  async updateMultipleTables(id: number, dto: TodoUpdateDto, otherDto: any) {
    const queryRunner = this.todoRepository.manager.connection.createQueryRunner();

    await queryRunner.connect();
    await queryRunner.startTransaction();

    try {
      await queryRunner.manager.getRepository(TodoEntity).update(id, dto);
      await queryRunner.manager.getRepository(OtherEntity).update(id, otherDto);

      await queryRunner.commitTransaction();
    } catch (err) {
      // if we have an error we rollback the transaction
      await queryRunner.rollbackTransaction();
    } finally {
      // you need to release a queryRunner which was manually instantiated
      await queryRunner.release();
    }
  }
}
```

## exports

由本模块提供并应在其他模块中可用的提供者的子集





# 常用的模块和中间件

**Modules:**

1. `ConfigModule`: 用于管理配置的模块。
2. `ThrottlerModule`: 这是一个用于处理请求限制的模块。在 Web 开发中，请求限制是一种常见的安全措施，用于防止恶意用户或脚本连续、快速地发送大量请求，从而导致服务器资源耗尽。`ThrottlerModule` 提供了一种简单的方式来在 Nest.js 应用中实现请求限制。
3. `ClsModule`: 这是一个用于处理上下文本地存储（Context-local Storage）的模块。在 Node.js 中，由于其异步的特性，传统的线程本地存储（Thread-local Storage）模式并不适用。因此，Node.js 社区提出了上下文本地存储的概念，用于在一个特定的异步上下文中存储和访问数据。
4. `SharedModule`： 全局共享模块
	1. LoggerModule
	2. HttpModule
	3. ScheduleModule： 这是一个用于处理任务调度的模块。在许多应用程序中，都需要定期执行某些任务，如清理缓存、发送通知等。`ScheduleModule`提供了一种简单的方式来在 Nest.js 应用中实现任务调度。你可以使用它来创建定时任务
	4. EventEmitterModule： 处理事件驱动编程的模块。在事件驱动的编程模型中，应用程序的执行流程是由事件（如用户操作、系统消息等）驱动的。
	5. RedisModule
	6. MailerModule
5. `DatabaseModule`： 数据库模块


**Filters:**

- `APP_FILTER`: 全局过滤器，可以捕获应用程序中的所有异常，并按照你定义的方式处理它们。
- `AllExceptionsFilter`: 特殊的过滤器，可以捕获应用程序中的所有异常，包括 HTTP 异常和 JavaScript 错误。

**Guards:**

- `APP_GUARD`: 全局守卫，可以在每个路由处理器执行之前运行一些代码，例如进行身份验证或授权。

**Interceptors:**

- `ClassSerializerInterceptor`: 拦截器，可以自动应用类转换器库的序列化功能，以便在发送响应时排除或包含特定的属性。
- `IdempotenceInterceptor`: 拦截器，可以确保重复的请求不会导致不同的结果。