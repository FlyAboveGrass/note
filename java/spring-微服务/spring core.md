

Spring Core是Spring框架的基础和核心模块，提供了控制反转（IoC）容器、依赖注入（DI）和面向切面编程（AOP）等核心功能，这些机制共同实现了对象生命周期管理、解耦和横切关注点分离，奠定了Spring生态系统的基石

关键词：
- IoC (控制反转)
- DI (依赖注入)
- Bean
- AOP (面向切面编程)
- 注解

# IOC 控制反转

IoC（Inversion of Control，控制反转）容器是 Spring 框架的核心组件，负责管理对象的生命周期和依赖关系。它通过依赖注入（DI）实现控制反转，将对象的创建和依赖管理从应用程序代码转移到容器中，从而降低耦合度，提高代码的可维护性和可测试性。

## 🛠️ 1. IoC 容器的核心概念

- ​**控制反转（IoC）​**​：传统编程中，对象主动创建依赖（如 `new`关键字）；而 IoC 将控制权交给容器，由容器负责对象的创建和依赖注入
- ​**依赖注入（DI）​**​：容器通过构造函数、Setter 方法或字段注入的方式，将依赖对象注入到目标对象中。这是实现 IoC 的主要手段。
- ​**Bean**​：被 IoC 容器管理的对象称为 Bean，其生命周期由容器控制

## 📦 2. Spring 容器的两种接口

Spring 提供了两种 IoC 容器接口：

- ​**BeanFactory**​：基础容器接口，支持懒加载（使用时才创建 Bean），适合资源受限的环境
- ​**ApplicationContext**​：`BeanFactory`的子接口，提供更多企业级功能（如国际化、事件发布），默认预加载单例 Bean，是实际开发中的首选

## ⚙️ 3. IoC 容器的配置方式

Spring 支持三种配置方式：

1. ​**XML 配置**​：传统方式，通过 XML 文件定义 Bean 及其依赖。
2. ​**注解配置**。 使用注解（如 `@Component`, `@Service`, `@Repository`, `@Autowired`）来标识 Bean 和注入点，并通过 `@ComponentScan` 让 Spring 自动扫描并组装它们。这是现代 Spring 开发中最常用、最简洁的方式
	3. ​**Java 配置类**​：使用 `@Configuration`和 `@Bean`注解在 Java 类中显式定义 Bean，提供了类型安全的配置方式，完全无需 XML

## 🧩 4. 依赖注入（Dependencies Injection）的三种方式

1. ​**构造器注入（Constructor Injection）​**​（推荐）
通过类的构造函数传入依赖对象。这是 ​**Spring 官方推荐**的方式，尤其适用于强制依赖（即对象正常运行所必需的依赖）

- ​**优点**​：保证依赖不可变（`final`修饰），确保 Bean 在构造完成后就处于完全初始化的状态；易于进行单元测试；能避免循环依赖问题
    ```
    public class UserService {
        private final UserDao userDao;
        // 构造器注入
        public UserService(UserDao userDao) {
            this.userDao = userDao;
        }
    }
    ```
2. ​**Setter 注入（Setter Injection）​**​​

通过对象的 Setter 方法设置依赖对象。适用于可选依赖或可能存在动态变更的场景
- ​**优点**​：灵活性高，可以在对象创建后重新配置依赖。
- ​**缺点**​：无法保证依赖在整个生命周期中不变，可能导致对象在部分依赖未注入时处于不完整状态
    ```
    public class UserService {
        private UserDao userDao;
        // Setter 注入
        public void setUserDao(UserDao userDao) {
            this.userDao = userDao;
        }
    }
    ```
3. ​**字段注入 (Field Injection）​**​​（不推荐）
通过反射机制直接在字段上注入依赖。这种方式代码简洁，但**不推荐使用**​

- ​**缺点**​：破坏了封装性（字段通常应为私有）；不利于单元测试，必须通过 Spring 容器才能完成依赖注入；无法将依赖声明为 `final`，缺少不变性保证
    ```
    public class UserService {
        @Autowired
        private UserDao userDao;
    }
    ```

## 🧪 5. IoC 容器的配置方式——代码示例

### 示例 1：基于 XML 配置

1. ​**定义 Bean 类**​：


    ```
    // UserDao.java
    public class UserDao {
        public void save() {
            System.out.println("用户已保存");
        }
    }
    // UserService.java
    public class UserService {
        private UserDao userDao;
        // Setter 方法，用于注入
        public void setUserDao(UserDao userDao) {
            this.userDao = userDao;
        }
        public void register() {
            userDao.save();
        }
    }
    ```
2. ​**XML 配置文件（applicationContext.xml）​**​：

    ```
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://www.springframework.org/schema/beans
            http://www.springframework.org/schema/beans/spring-beans.xsd">
        <bean id="userDao" class="com.example.UserDao"/>
        <bean id="userService" class="com.example.UserService">
            <property name="userDao" ref="userDao"/> <!-- Setter 注入 -->
        </bean>
    </beans>
    ```
3. ​**启动容器并使用 Bean**​：


    ```
    import org.springframework.context.ApplicationContext;
    import org.springframework.context.support.ClassPathXmlApplicationContext;
    public class MainApp {
        public static void main(String[] args) {
            ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
            UserService userService = (UserService) context.getBean("userService");
            userService.register(); // 输出："用户已保存"
        }
    }
    ```

### 示例 2：基于注解配置

1. ​**使用注解定义 Bean**​：


    ```
    // UserDao.java
    @Repository // 标记为 Dao 层 Bean
    public class UserDao {
        public void save() {
            System.out.println("用户已保存");
        }
    }
    // UserService.java
    @Service // 标记为 Service 层 Bean
    public class UserService {
        @Autowired // 自动注入 UserDao
        private UserDao userDao;
        public void register() {
            userDao.save();
        }
    }
    ```
2. ​**配置类（替代 XML）​**​：


    ```
    @Configuration
    @ComponentScan("com.example") // 扫描指定包下的注解
    public class AppConfig {
    }
    ```
3. ​**启动容器**​：


    ```
    public class Main {
        public static void main(String[] args) {
            ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
            UserService userService = context.getBean(UserService.class);
            userService.register(); // 输出："用户已保存"
        }
    }
    ```

### 示例 3：基于 Java 配置类

1. ​**定义配置类和 Bean**​：


    ```
    @Configuration
    public class AppConfig {
        @Bean // 声明一个 Bean，方法名即为 Bean 的 ID
        public UserDao userDao() {
            return new UserDao();
        }
        @Bean
        public UserService userService() {
            // 通过构造器注入
            return new UserService(userDao());
        }
    }
    ```
2. ​**使用方式与注解配置相同**。

## 🔄 6. Bean 的生命周期

IoC 容器管理 Bean 的完整生命周期，包括：

1. ​**实例化**​：通过反射创建 Bean 实例。
2. ​**属性赋值**​：注入依赖（DI）。
3. ​**初始化**​：调用 `@PostConstruct`方法或 `InitializingBean`的 `afterPropertiesSet()`。
4. ​**使用**​：Bean 处于就绪状态。
5. ​**销毁**​：容器关闭时调用 `@PreDestroy`方法或 `DisposableBean`的 `destroy()`

## 💡 7. 最佳实践

- ​**推荐使用构造器注入**​：避免 `NullPointerException`，保证依赖不可变
- ​**优先选择注解配置**​：减少 XML 的冗余，提高开发效率
- ​**理解作用域**​：单例（Singleton）是默认作用域，适合无状态 Bean；原型（Prototype）每次提供新实例，适合有状态 Bean

# AOP （面向切面编程）

AOP（面向切面编程）是一种编程范式，旨在将横切关注点（如日志记录、事务管理、权限校验等）从核心业务逻辑中分离出来，通过动态代理技术在运行时将通用功能模块化地织入到目标方法中，从而提升代码的可维护性、复用性和内聚性

。以下是AOP的详细说明和代码示例：

## 1. ​**AOP核心概念**​

- ​**切面（Aspect）​**​：封装横切关注点的模块，包含通知和切点（例如日志记录模块）
- ​**连接点（Join Point）​**​：程序执行过程中可插入切面的点（如方法调用、异常抛出）
- ​**切点（Pointcut）​**​：通过表达式匹配需要拦截的连接点（例如匹配特定包下的所有方法）
- ​**通知（Advice）​**​：切面在连接点执行的具体动作，分为以下类型：
    - ​**前置通知（Before）​**​：在目标方法执行前执行。
- ​**后置通知（After）​**​：在目标方法执行后执行（无论是否异常）。
- ​**返回通知（AfterReturning）​**​：在目标方法正常返回后执行。
- ​**异常通知（AfterThrowing）​**​：在目标方法抛出异常后执行。
- ​**环绕通知（Around）​**​：在目标方法执行前后执行，可控制方法是否执行。
- ​**织入（Weaving）​**​：将切面应用到目标对象创建代理对象的过程

## 2. ​**AOP实现机制**​

Spring AOP基于动态代理实现

- ​**JDK动态代理**​：要求目标对象实现接口，代理对象与目标对象是兄弟关系（实现相同接口）
- ​**CGLIB代理**​：通过继承目标类生成子类代理，适用于无接口的类（无法代理final类或方法）
    Spring自动选择代理方式，优先使用JDK动态代理，若无接口则使用CGLIB

## 3. ​**代码示例（基于Spring Boot和注解）​**​

### 步骤1：添加依赖

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

### 步骤2：定义业务类（目标对象）

```
@Service
public class UserService {
    public String createUser(String name) {
        System.out.println("核心业务：创建用户 " + name);
        return "用户ID:1001";
    }
    public void deleteUser(int id) {
        System.out.println("核心业务：删除用户ID:" + id);
    }
}
```

### 步骤3：定义切面类

```
import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;
import java.util.Arrays;

@Aspect
@Component
public class LoggingAspect {
    // 定义切点：匹配UserService中所有公共方法
    @Pointcut("execution(* com.example.service.UserService.*(..))")
    public void userServiceMethods() {}
    // 前置通知
    @Before("userServiceMethods()")
    public void logBefore(JoinPoint joinPoint) {
        String methodName = joinPoint.getSignature().getName();
        Object[] args = joinPoint.getArgs();
        System.out.println("[前置通知] 方法 " + methodName + " 开始执行，参数：" + Arrays.toString(args));
    }
    // 返回通知
    @AfterReturning(pointcut = "userServiceMethods()", returning = "result")
    public void logAfterReturning(JoinPoint joinPoint, Object result) {
        System.out.println("[返回通知] 方法 " + joinPoint.getSignature().getName() + " 返回结果：" + result);
    }
    // 异常通知
    @AfterThrowing(pointcut = "userServiceMethods()", throwing = "ex")
    public void logAfterThrowing(JoinPoint joinPoint, Exception ex) {
        System.out.println("[异常通知] 方法 " + joinPoint.getSignature().getName() + " 抛出异常：" + ex.getMessage());
    }
    // 环绕通知（功能最强大）
    @Around("userServiceMethods()")
    public Object logAround(ProceedingJoinPoint joinPoint) throws Throwable {
        String methodName = joinPoint.getSignature().getName();
        long startTime = System.currentTimeMillis();
        System.out.println("[环绕通知-前] 开始执行方法：" + methodName);
    try {
            // 执行目标方法
            Object result = joinPoint.proceed();
            long endTime = System.currentTimeMillis();
            System.out.println("[环绕通知-后] 方法 " + methodName + " 执行耗时：" + (endTime - startTime) + "ms");
            return result;
        } catch (Throwable e) {
            System.out.println("[环绕通知-异常] 方法执行失败");
            throw e;
        }
    }
}
```

### 步骤4：启用AOP支持（Spring Boot自动配置，无需手动启用）

## 4. ​**切入点表达式详解**​

- ​**语法结构**​：`execution(访问修饰符? 返回值类型 包名.类名.方法名(参数类型) 异常类型?)`
- ​**示例**​：
    - `execution(* com.example.service.*.*(..))`：匹配service包下所有类的所有方法。
	- `execution(* com.example.service.UserService.createUser(..))`：匹配UserService的createUser方法。
	- `@annotation(com.example.annotation.Log)`：匹配带有@Log注解的方法。

## 5. ​**AOP的适用场景**​

- ​**日志记录**​：统一记录方法调用参数、返回值和执行时间
- ​**事务管理**​：通过@Transactional注解实现声明式事务（Spring事务基于AOP）
- ​**权限控制**​：在方法执行前校验用户权限
- ​**性能监控**​：统计方法执行耗时
- ​**异常处理**​：统一捕获和转换异常

## 6. ​**注意事项**​

- ​**同类方法调用失效**​：同类内方法调用（如A方法调用B方法）不会经过代理对象，因此B方法上的AOP通知不生效。解决方案：
    - 通过ApplicationContext获取代理对象调用。
- 使用AspectJ编译时织入。
- ​**代理方式选择**​：可通过`@EnableAspectJAutoProxy(proxyTargetClass = true)`强制使用CGLIB代理
- ​**优先级控制**​：多个切面作用于同一方法时，使用`@Order(数字)`指定优先级（数字越小优先级越高）
