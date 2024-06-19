
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



## exports

导出模块和服务