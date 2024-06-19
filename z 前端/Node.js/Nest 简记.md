
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
## providers（services）


## exports

导出模块和服务