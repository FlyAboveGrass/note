

# springboot学习

> 官方文档：[https://spring.io/projects/spring-boot](https://spring.io/projects/spring-boot)

# 1、简介

## 1.1、什么是spirngboot?

springboot在spring的基础之上，搭建起来的框架，能够帮助我们整合市面上最流行的框架，帮助我们快速搭建起来项目。

springboot不是新的技术，而是新的框架，是基于spring来搭建起来的。

特性：约定大于配置！

## 1.2、为什么使用springboot

-   开发效率快，内置有配置好的版本依赖。
-   基于spring。
-   轻松上手

# 2、第一个 SpringBoot 项目

**我的开发环境：**

-   java 8
-   Maven-3.6.1
-   SpringBoot 2.6.11

> 2.6.11官方文档：[https://docs.spring.io/spring-boot/docs/2.6.11/reference/htmlsingle/](https://docs.spring.io/spring-boot/docs/2.6.11/reference/htmlsingle/)

创建springboot有两种方式：

1.  在[https://start.spring.io/上创建后，下载完成，通过IDEA打开即可。](https://start.spring.io/%E4%B8%8A%E5%88%9B%E5%BB%BA%E5%90%8E%EF%BC%8C%E4%B8%8B%E8%BD%BD%E5%AE%8C%E6%88%90%EF%BC%8C%E9%80%9A%E8%BF%87IDEA%E6%89%93%E5%BC%80%E5%8D%B3%E5%8F%AF%E3%80%82)
2.  在IDEA中直接创建。

## 2.1、创建项目

1、页面创建  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy63d55b07-eddb-4a6e-87d9-424a47755014.png)

2、IDEA创建  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy7ce520ab-ecad-4765-972c-927ffaf57ee5.png)

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy6fa2724b-0b7f-4e7d-b873-84d3d476917a.png)

两种创建方式大致相同。

## 2.2、启动项目并访问

1、创建一个`HelloController.java`

1.  `@RestController`
2.  `public class HelloController {`

4.      `@GetMapping("/")`
5.      `public String hello(){`
6.          `return "Hello springboot";`
7.      `}`
8.  `}`

2、启动项目  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy26a2ae67-7926-4f99-8b82-ff094eb46f0a.png)

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudye028e17e-8c36-442e-8a7f-eb17333c105c.png)

访问：`http://localhost:8080/`  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy00b54e2a-2bee-40d0-8a0c-5433289ee4a0.png)

这样一个web应用就创建完成，是不是很快。

## 2.3、彩蛋

> 官网文档说明：[https://docs.spring.io/spring-boot/docs/2.6.12-SNAPSHOT/reference/html/features.html#features.spring-application.banner](https://docs.spring.io/spring-boot/docs/2.6.12-SNAPSHOT/reference/html/features.html#features.spring-application.banner)

可以自定义banner图  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudydfcaa657-4f5f-4bc6-9d6a-627b23f8dab1.png)

在根目录下添加`banner.txt`或者添加静态资源图片即可。

重新启动项目，即可在控制台看到效果。  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudyf759c405-efb9-46a2-b409-9ede639e4136.png)

# 3、Spring Boot启动器

我们在创建项目的时候添加了一个web的依赖。

通过`pom.xml`查看。

1.  `<dependency>`
2.      `<groupId>org.springframework.boot</groupId>`
3.      `<artifactId>spring-boot-starter-web</artifactId>`
4.  `</dependency>`
 
会发现，只添加`spring-boot-starter-web`就可以进行web开发了。并且不用声明版本号。

**启动器**包含许多依赖项，包括版本号，可以添加这些依赖项使项目快速启动并运行。  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudyb34446d9-6eeb-4054-98fb-20ead9b2e92b.png)

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudye6f47736-d3c0-4d46-ab5e-01dab671d70a.png)

**官方**启动器命名规则：`spring-boot-starter-*`，其中`*`是特定类型的应用程序。例如，`spring-boot-starter-web`。

**第三方**启动器命名规则：以项目名称开头`*-boot-starter`。例如，MyBatis-Plus。他的命名是`mybatis-plus-boot-starter`

Spring Boot 应用程序启动器

官方文档：[https://docs.spring.io/spring-boot/docs/2.6.11/reference/htmlsingle/#using.build-systems.starters](https://docs.spring.io/spring-boot/docs/2.6.11/reference/htmlsingle/#using.build-systems.starters)  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy0539b77d-29f3-4dd4-84fb-451cde4282a8.png)

整合第三方技术的两种方式

-   自定义
-   找starter

# 4、配置文件

springboot支持两种格式的配置文件，`.properties`，`.yml`

> 如果在同一位置有同时具有.properties和.yml格式的配置文件，.properties优先。
> 
> 如果存在多个相同配置，会有优先级。

## 4.1、配置文件区别

`.properties`，`.yml`区别在于语法结构不同。

-   `.properties`结构 ：key=value

1.  `server.port=8081`

-   `.yml`结构 ：key: value

1.  `server:`
2.    `port: 8081`

## 4.2、实体类获取配置信息

加载单个配置

1.  `// @Value 加载单个配置`
2.  `@Value("${student.name}")`

加载多个配置

案例：创建学生对象，用于默认就把配置信息加载进去。

1、在springboot项目中的resources目录下新建一个文件 application.yml

1.  `student:`
2.    `name: Zhang San`
3.    `birthdate: 1990/09/01`
4.    `interests: [eat, sleep]`

2、添加实体类

1.  `//注册bean到容器中`
2.  `@Component`
3.  `// 开头为student配置`
4.  `@ConfigurationProperties(prefix = "student")`
5.  `@Data`
6.  `public class Student {`
7.      `private String name;`
8.      `private Date birthdate;`
9.      `private List<String> interests;`
10.  `}`

idea提示没有找到springboot配置注解处理器。  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy9205c17a-73f5-4d67-b2bf-e643e0f41380.png)

需要添加springboot配置注解处理器，方便在Test测试。

1.  `<dependency>`
2.      `<groupId>org.springframework.boot</groupId>`
3.      `<artifactId>spring-boot-configuration-processor</artifactId>`
4.      `<optional>true</optional>`
5.  `</dependency>`

3、测试类中测试。

1.  `@SpringBootTest`
2.  `class DemoApplicationTests {`

4.      `@Autowired`
5.      `Student student;`

7.      `@Test`
8.      `void contextLoads() {`
9.          `System.out.println(student);`
10.      `}`

12.  `}`

结果：  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudya8bba307-c33d-40c2-b6bf-1d6852b3a08b.png)

## 4.3、加载指定的配置文件

日常开发中，配置文件可能存在多个。

1、我们去在resources目录下新建一个`student.properties`文件，`yaml`不生效。

1.  `student.name=Wang mou`
2.  `student.birthdate=1995/09/01`
3.  `student.interests=[sleep,dream]`

2、修改配置类

1.  `//注册bean到容器中`
2.  `@Component`
3.  `// 开头为student配置`
4.  `@ConfigurationProperties(prefix = "student")`
5.  `// 资源路径`
6.  `@PropertySource(value = "classpath:student.properties")`
7.  `@Data`
8.  `public class Student {`
9.      `private String name;`
10.      `private Date birthdate;`
11.      `private List<String> interests;`
12.  `}`

3、测试查看效果  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy9a57796b-6f93-4804-bd25-0929e0a31bb9.png)

## 4.4、yaml语法总结

-   语法要求严格
    
-   空格不能省略
    
-   以缩进来控制层级关系
-   对大小写敏感

1.  `# 普通格式：`
2.  `k: v`
3.  `# 对象格式：`
4.  `k:`
5.    `v1:`
6.    `v2:`
7.  `# 数组模式：`
8.  `k:`
9.    `- v`
10.    `- v`
11.    `- v`

13.  `# 行内写法`
14.  `k: {k1:v1,k2:v2}`
15.  `k: {v,v,v}`

## 4.5、配置文件优化级

springboot 启动会扫描以下位置的application.properties或者application.yml文件作为Spring boot的默认配置文件：

1.  `1.类路径`
2.    `1.类路径`
3.    `2.类路径/config包`
4.  `2.当前目录`
5.    `1.当前目录`
6.    `2.当前目录中的/config子目录`
7.    `3.子目录的/config直接子目录`

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy6238356a-4dad-4d3c-8a0e-550e7c20a8a7.png)

## 4.6、多环境切换

profile是Spring对不同环境提供不同配置功能的支持，可以通过激活不同的环境版本，实现快速切换环境；

我们在主配置文件编写的时候，文件名可以是 `application-{profile}.properties/yml` , 用来指定多个环境版本；

**例如：**

application-test.yml：代表测试环境配置

application-dev.yml：代表开发环境配置

但是Springboot并不会直接启动这些配置文件，它默认使用application.properties主配置文件，如果没有就会找`application.yml`。

我们需要通过一个配置来选择需要激活的环境：

1.  `spring:`
2.    `profiles:`
3.      `active: dev #使用开发环境。`

# 5、自动配置原理

SpringBoot官方文档中有大量的配置。我们无法全部记住。

我们来简单分析一下。

## 5.1、分析自动配置原理

> 这块内容，建议大家去看视频。

我们以**HttpEncodingAutoConfiguration（Http编码自动配置）**为例。

解释自动配置原理；

1.  `@Configuration //表示这是一个配置类，和以前编写的配置文件一样，也可以给容器中添加组件；`

3.  `//启动指定类的ConfigurationProperties功能；`
4.    `//进入这个HttpProperties查看，将配置文件中对应的值和HttpProperties绑定起来；`
5.    `//并把HttpProperties加入到ioc容器中`
6.  `@EnableConfigurationProperties({HttpProperties.class})` 

8.  `//Spring底层@Conditional注解`
9.    `//根据不同的条件判断，如果满足指定的条件，整个配置类里面的配置就会生效；`
10.    `//这里的意思就是判断当前应用是否是web应用，如果是，当前配置类生效`
11.  `@ConditionalOnWebApplication(type = Type.SERVLET)`

13.  `//判断当前项目有没有这个类CharacterEncodingFilter；SpringMVC中进行乱码解决的过滤器；`
14.  `@ConditionalOnClass({CharacterEncodingFilter.class})`

16.  `//判断配置文件中是否存在某个配置：spring.http.encoding.enabled；`
17.    `//如果不存在，判断也是成立的`
18.    `//即使我们配置文件中不配置spring.http.encoding.enabled=true，也是默认生效的；`
19.  `@ConditionalOnProperty(`
20.      `prefix = "spring.http.encoding",`
21.      `value = {"enabled"},`
22.      `matchIfMissing = true`
23.  `)`

25.  `public class HttpEncodingAutoConfiguration {`
26.      `//他已经和SpringBoot的配置文件映射了`
27.      `private final Encoding properties;`
28.      `//只有一个有参构造器的情况下，参数的值就会从容器中拿`
29.      `public HttpEncodingAutoConfiguration(HttpProperties properties) {`
30.          `this.properties = properties.getEncoding();`
31.      `}`

33.      `//给容器中添加一个组件，这个组件的某些值需要从properties中获取`
34.      `@Bean`
35.      `@ConditionalOnMissingBean //判断容器没有这个组件？`
36.      `public CharacterEncodingFilter characterEncodingFilter() {`
37.          `CharacterEncodingFilter filter = new OrderedCharacterEncodingFilter();`
38.          `filter.setEncoding(this.properties.getCharset().name());`
39.          `filter.setForceRequestEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.REQUEST));`
40.          `filter.setForceResponseEncoding(this.properties.shouldForce(org.springframework.boot.autoconfigure.http.HttpProperties.Encoding.Type.RESPONSE));`
41.          `return filter;`
42.      `}`
43.      `//。。。。。。。`
44.  `}`

**一句话总结 ：根据当前不同的条件判断，决定这个配置类是否生效！**

如果没有把对应的依赖引用进来，这个配置类也会不生效。

通过不同的条件来进行判断是否要启动配置。

-   一但这个配置类生效；这个配置类就会给容器中添加各种组件；
    
-   这些组件的属性是从对应的properties类中获取的，这些类里面的每一个属性又是和配置文件绑定的；
    
-   这样就可以形成我们的配置文件可以动态的修改springboot的内容。
-   所有在配置文件中能配置的属性都是在xxxxProperties类中封装着；
-   配置文件能配置什么就可以参照某个功能对应的这个属性类

## 5.2、总结

1、SpringBoot启动时会加载大量的自动配置类

2、我们看我们需要的功能有没有在SpringBoot默认写好的自动配置类当中

3、我们再来看这个自动配置类中到底配置了哪些组件；（只要我们要用的组件存在其中，我们就不需要再去手动配置了，如果不存在我们再手动配置）

4、给容器中自动配置类添加组件的时候，会从properties类中获取某些属性，我们只需要在配置文件中指定这些属性即可；

XXXXAutoConfiguration：自动配置类:给容器添加组件，这些组件要赋值就需要绑定一个XXXXProperties类

XXXXProperties：里面封装配置文件中相关属性；

# 6、Web开发

## 6.1、静态资源管理

### 6.1.1、静态资源访问

1、默认情况下，Spring Boot 从类路径中的`/static` (或`/public` 或`/resources` 或`/META-INF/resources)`目录或 `ServletContext`的根目录提供静态内容。

访问 ： 当前项目根路径/ + 静态资源名

原理： 静态映射/**。

请求进来，**先去找Controller看能不能处理。不能处理的所有请求又都交给静态资源处理器**。静态资源也找不到则**响应404页面**

添加图片到`resource`下的`static`里。  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy4f84eaea-74cb-4dc9-96af-9b446930ec1c.png)

访问 ： 当前项目根路径/ + 静态资源名  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy19cf0491-07ce-4386-92a4-7c50b93ae022.png)

原理： `静态映射/**。`

请求进来，先去找Controller看能不能处理。不能处理的所有请求又都交给静态资源处理器。静态资源也找不到则响应404页面

### 6.1.2、静态资源前缀

可以添加访问静态资源前缘。

1.  `spring:`
2.    `mvc:`
3.      `static-path-pattern: /res/**`

现在访问就是： `当前项目根路径 + /res + 静态资源名`

### 6.1.3、webjar

> webjar官网：[https://www.webjars.org/](https://www.webjars.org/)

WebJars是可以让大家以jar包的形式来使用前端的各种框架、组件。

例如，引用jquery。

1.  `<dependency>`
2.      `<groupId>org.webjars</groupId>`
3.      `<artifactId>jquery</artifactId>`
4.      `<version>3.6.1</version>`
5.  `</dependency>`

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudyc38e32a2-e66b-46ba-a1f0-4efab2b79548.png)

访问路径：`当前项目根路径/ + webjars/**。`

[http://localhost:8081/webjars/jquery/3.6.1/jquery.js](http://localhost:8081/webjars/jquery/3.6.1/jquery.js)  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy28d6c814-4554-4c62-90c5-43c7e13ead51.png)

### 6.1.4、首页支持

-   静态资源路径下 index.html
-   可以配置静态资源路径。
-   不能与静态资源前缀共用。

1.  `spring:`
2.    `resources:`
3.      `static-locations: [classpath:/haha/]`

## 6.2、请求参数处理

### 6.2.1、Rest风格

RESTFUL是一种网络应用程序的设计风格和开发方式。

对比：

功能

传统请求

Rest风格

获取用户

/user/getUser （GET请求）

/user （GET请求）

保存用户

/user/saveUse （POST请求）

/user （POST请求）

修改用户

/user/editUser （POST请求）

/user （PUT请求）

删除用户

/user/deleteUser（POST请求）

/user （DELETE请求）

springboot用法：**表单method=post，隐藏域 _method=put**。

1、开启页面表单的Rest功能

1.  `spring:`
2.    `mvc:`
3.      `hiddenmethod:`
4.        `filter:`
5.          `# 开启页面表单的Rest功能`
6.          `enabled: true`

2、添加页面请求

1.  `<!DOCTYPE html>`
2.  `<html lang="en">`
3.  `<head>`
4.      `<meta charset="UTF-8">`
5.      `<title>Title</title>`
6.  `</head>`
7.  `<script src="/webjars/jquery/3.6.1/jquery.js"></script>`
8.  `<body>`
9.      `<h1>首页</h1>`
10.      `<button id="getUser">获取用户</button>`
11.      `<button id="saveUser">保存用户</button>`
12.      `<button id="editUser">修改用户</button>`
13.      `<button id="deleteUser">删除用户</button>`
14.      `<p id="msg"></p>`
15.  `<script>`
16.      `$("#getUser").on("click",()=>{`
17.          `$.get("/user",(res)=>{`
18.              `$("#msg").text(res);`
19.          `})`
20.      `});`
21.      `$("#saveUser").on("click",()=>{`
22.          `sendAjax(null);`
23.      `});`
24.      `$("#editUser").on("click",()=>{`
25.          `sendAjax('PUT');`
26.      `});`
27.      `$("#deleteUser").on("click",()=>{`
28.          `sendAjax("DELETE");`
29.      `});`

31.      `function sendAjax(type){`
32.          `let data = {'_method':type}`
33.          `$.post("/user",data,(res)=>{`
34.              `$("#msg").text(res);`
35.          `})`
36.      `}`
37.  `</script>`
38.  `</body>`
39.  `</html>`

3、添加后端接口

1.  `// 组合注解，@Controller + RequestBody`
2.  `@RestController`
3.  `@RequestMapping("/user")`
4.  `public class UserController {`

6.      `// 普通写法`
7.      `// @RequestMapping(value = "/user",method = RequestMethod.GET)`
8.      `// 精简写法`
9.      `@GetMapping`
10.      `public String getUser(){`
11.          `return "get user";`
12.      `}`
13.      `@PostMapping`
14.      `public String saveUser(){`
15.          `return "post user";`
16.      `}`
17.      `@PutMapping`
18.      `public String editUser(){`
19.          `return "put user";`
20.      `}`
21.      `@DeleteMapping`
22.      `public String deleteUser(){`
23.          `return "delete user";`
24.      `}`
25.  `}`

**为什么明明请求方式是POST，会跑到别的接口。**

-   核心Filter；HiddenHttpMethodFilter。
-   由过滤器来判断改变。  
    ![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy48ecc395-9279-4744-8a6d-6c744caebbb2.png)

如果请求方式是直接发送Put、delete等方式请求，无需Filter。

扩展：`_method`的值可以自定义，只需要重新实现过滤器方法即可。

1.  `//自定义filter`
2.  `@Bean`
3.  `public HiddenHttpMethodFilter hiddenHttpMethodFilter(){`
4.      `HiddenHttpMethodFilter methodFilter = new HiddenHttpMethodFilter();`
5.      `methodFilter.setMethodParam("_m");`
6.      `return methodFilter;`
7.  `}`

### 6.2.2、参数注释

-   [@PathVariable](https://github.com/PathVariable "@PathVariable")、从请求路径上获取参数
-   [@RequestHeader](https://github.com/RequestHeader "@RequestHeader")、从请求头上获取参数
-   [@RequestParam](https://github.com/RequestParam "@RequestParam")、从请求参数上获取参数
-   [@CookieValue](https://github.com/CookieValue "@CookieValue")、从请求Cookie中获取参数
-   [@RequestBody](https://github.com/RequestBody "@RequestBody")、从请求body上获取参数
-   [@MatrixVariable](https://github.com/MatrixVariable "@MatrixVariable")、从请求路径上；分割获取变量
    -   SpringBoot默认禁用
    -   语法： 请求路径`/test;user=jack;age=16,interests=sleep,dream`
    -   后端接收,`[](https://github.com/MatrixVariable "@MatrixVariable")[@MatrixVariable](https://github.com/MatrixVariable "@MatrixVariable")("user") String name,[](https://github.com/MatrixVariable "@MatrixVariable")[@MatrixVariable](https://github.com/MatrixVariable "@MatrixVariable")("age") Integer age,[](https://github.com/MatrixVariable "@MatrixVariable")[@MatrixVariable](https://github.com/MatrixVariable "@MatrixVariable")("interests") List<String> interests`

3.  `@GetMapping("/{id}")`
4.  `public String getParam(@PathVariable("id") Integer id,`
5.                        `@RequestHeader("Host") String host,`
6.                        `@RequestParam("name") Integer name,`
7.                        `@CookieValue("_username") String usernmae){`

9.  `}`

11.  `@PostMapping`
12.  `public void postMethod(@RequestBody String content){`
13.  `}`

15.  `// 可以传多个值，用对象来接收，存在相同属性时，会自动封装到里面。`
16.  `@PostMapping`
17.  `public void postMethod(@RequestBody Student student){`
18.  `}`

## 6.3、数据响应

数据响应，一般分两个类型：

-   响应页面
-   响应数据

响应数据的格式可以是`json`,`xml`,`io流`等。

SpringMVC支持返回值

1.  `ModelAndView`
2.  `Model`
3.  `View`
4.  `ResponseEntity` 
5.  `ResponseBodyEmitter`
6.  `StreamingResponseBody`
7.  `HttpEntity`
8.  `HttpHeaders`
9.  `Callable`
10.  `DeferredResult`
11.  `ListenableFuture`
12.  `CompletionStage`
13.  `WebAsyncTask`
14.  `有 @ModelAttribute 且为对象类型的`
15.  `@ResponseBody 注解`

## 6.4、模板引擎

**SpringBoot默认不支持 JSP，需要引入第三方模板引擎技术实现页面渲染。**

### 6.4.1Thymeleaf模板引擎

> 官网：[https://www.thymeleaf.org/](https://www.thymeleaf.org/)

前端显示页面，是html页面。我们以前开发，做的是jsp页面，jsp可以动态渲染一些数据在页面上，可以写Java代码。JSP+Servlet+JavaBean，是我们很早之前就不用了，企业也用得少。

**现在SpringBoot推荐Thymeleaf模板引擎。**

### 6.4.2、基本语法

表达式名字

语法

用途

变量取值

${…}

获取请求域、session域、对象等值

选择变量

*{…}

获取上下文对象值

消息

#{…}

获取国际化等值

链接

@{…}

生成链接

片段表达式

~{…}

jsp:include 作用，引入公共页面片段

1.  `<!-- 常用标签，一般都是  th:XXX -->`
2.  `<!-- 需要设置头部(非标准HTML5 规范)，也可以不设置 -->`
3.  `<html xmlns:th="http://www.thymeleaf.org">`

5.  `<!-- 不设置头部的写法（符合HTML5规范） -->`
6.  `<p data-th-text="${msg}">msg</p>`

8.  `<!--设置文本-->`
9.  `<p th:text="${msg}">提醒消息</p>`

11.  `<!--设置文本-->`
12.  `<a th:href="@{href}">超链接</a>`

14.  `<!-- 设置属性值 -->`
15.  `<input type="text" th:id="${student.id}" />`

18.  `<!-- 获取session -->`
19.  `<p th:id="${#session.user}" />`

### 6.4.3、thymeleaf使用

1、添加`thymeleaf`依赖

1.  `<dependency>`
2.      `<groupId>org.springframework.boot</groupId>`
3.      `<artifactId>spring-boot-starter-thymeleaf</artifactId>`
4.  `</dependency>`

2、创建文件，springboot帮我们配置好了。我们直接开发页面即可。  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy5fac2189-c531-498b-9f50-67422c937017.png)

1.  `// 接口`
2.  `@Controller`
3.  `public class IndexController {`

5.      `@GetMapping("/thymeleaf")`
6.      `public String index(Model model) {`
7.          `model.addAttribute("msg","hello thymeleaf");`
8.          `model.addAttribute("link","www.baidu.com");`
9.          `// 返回视图层`
10.          `return "thymeleaf";`
11.      `}`
12.  `}`

在`templates`下新建`thymeleaf.html`;

1.  `<!doctype html>`
2.  `<html>`
3.  `<head>`
4.      `<meta charset="UTF-8">`
5.      `<meta name="viewport"`
6.            `content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">`
7.      `<meta http-equiv="X-UA-Compatible" content="ie=edge">`
8.      `<title>Document</title>`
9.  `</head>`
10.  `<body>`
11.      `<h1 data-th-text="${msg}">提醒消息</h1>`
12.      `<h2>`
13.          `<a data-th-href="${link}">超连接</a>`
14.      `</h2>`
15.  `</body>`
16.  `</html>`

3、效果  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy1bab9d20-8a72-4a77-a05e-dac60258adc2.png)

## 6.5、登录功能 + 拦截器

例子：访问项目，需要登录，如果没有登录就不能访问。

1、添加登录页面。`login.html`

1.  `<!DOCTYPE html>`
2.  `<html lang="en">`
3.  `<head>`
4.      `<meta charset="UTF-8">`
5.      `<meta name="viewport" content="width=device-width, initial-scale=1.0">`
6.      `<title>LOGIN</title>`
7.      `<style>`
8.          `* {`
9.              `margin: 0;`
10.              `padding: 0;`
11.          `}`
12.          `html {`
13.              `height: 100%;`
14.          `}`
15.          `body {`
16.              `height: 100%;`
17.          `}`
18.          `.container {`
19.              `height: 100%;`
20.              `background-image: linear-gradient(to right, #fbc2eb, #a6c1ee);`
21.          `}`
22.          `.login-wrapper {`
23.              `background-color: #fff;`
24.              `width: 358px;`
25.              `height: 588px;`
26.              `border-radius: 15px;`
27.              `padding: 0 50px;`
28.              `position: relative;`
29.              `left: 50%;`
30.              `top: 50%;`
31.              `transform: translate(-50%, -50%);`
32.          `}`
33.          `.header {`
34.              `font-size: 38px;`
35.              `font-weight: bold;`
36.              `text-align: center;`
37.              `line-height: 200px;`
38.          `}`
39.          `.input-item {`
40.              `display: block;`
41.              `width: 100%;`
42.              `margin-bottom: 20px;`
43.              `border: 0;`
44.              `padding: 10px;`
45.              `border-bottom: 1px solid rgb(128, 125, 125);`
46.              `font-size: 15px;`
47.              `outline: none;`
48.          `}`
49.          `.input-item:placeholder {`
50.              `text-transform: uppercase;`
51.          `}`
52.          `.btn {`
53.              `border: 0;`
54.              `font-size: 20px;`
55.              `text-align: center;`
56.              `padding: 10px;`
57.              `width: 100%;`
58.              `margin-top: 40px;`
59.              `background-image: linear-gradient(to right, #a6c1ee, #fbc2eb);`
60.              `color: #fff;`
61.          `}`
62.          `.msg {`
63.              `color:red;`
64.              `text-align: center;`
65.              `line-height: 88px;`
66.          `}`
67.          `a {`
68.              `text-decoration-line: none;`
69.              `color: #abc1ee;`
70.          `}`
71.      `</style>`
72.  `</head>`
73.  `<body>`
74.  `<div class="container">`
75.      `<div class="login-wrapper">`
76.          `<div class="header">Login</div>`
77.          `<div class="form-wrapper">`
78.              `<form data-th-action="@{/login}" method="post">`
79.                  `<input type="text" name="username" placeholder="username" class="input-item" /  >`
80.                  `<input type="password" name="password" placeholder="password" class="input-item" />`
81.                  `<input type="submit" class="btn" value="Login" />`
82.              `</form>`
83.          `</div>`
84.          `<div class="msg" data-th-text="${msg}">`
85.              `Don't have account?`
86.              `<a href="#">Sign up</a>`
87.          `</div>`
88.      `</div>`
89.  `</div>`
90.  `</body>`
91.  `</html>`

2、添加登录接口。

1.  `@Controller`
2.  `public class LoginController {`
3.      `@GetMapping("/")`
4.      `public String index() {`
5.          `// 返回视图层`
6.          `return "/login/login";`
7.      `}`

9.      `@PostMapping("/login")`
10.      `public String login(String username, String password, HttpSession session, Model model) {`
11.          `if(StringUtils.hasLength(username) && "123456".equals(password)){`
12.              `//把登陆成功的用户保存起来`
13.              `session.setAttribute("loginUserName",username);`
14.              `//登录成功重定向到 thymeleaf ;  重定向防止表单重复提交`
15.              `return "redirect:/thymeleaf";`
16.          `}else {`
17.              `model.addAttribute("msg","账号密码错误");`
18.              `//回到登录页面`
19.              `return "/";`
20.          `}`
21.      `}`
22.  `}`

3、添加拦截器。

继承HandlerInterceptor 接口。

1.  `@Slf4j`
2.  `public class LoginInterceptor implements HandlerInterceptor {`

4.      `/**`
5.       `* 目标方法执行之前`
6.       `* @param request`
7.       `* @param response`
8.       `* @param handler`
9.       `* @return`
10.       `* @throws Exception`
11.       `*/`
12.      `@Override`
13.      `public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {`
14.          `// 请求路径 request.getRequestURI();`
15.          `//登录检查逻辑`
16.          `HttpSession session = request.getSession();`
17.          `Object loginUser = session.getAttribute("loginUserName");`
18.          `if(loginUser != null){`
19.              `//放行`
20.              `return true;`
21.          `}`
22.          `//拦截住。未登录。跳转到登录页`
23.          `request.setAttribute("msg","请先登录");`

25.          `// 跳转`
26.          `request.getRequestDispatcher("/").forward(request,response);`
27.          `return false;`
28.      `}`

30.  `}`

4、配置拦截器

1.  `@Configuration`
2.  `public class AdminWebConfig implements WebMvcConfigurer {`

4.      `@Override`
5.      `public void addInterceptors(InterceptorRegistry registry) {`
6.          `registry.addInterceptor(new LoginInterceptor())`
7.                  `// 所有请求都被拦截包括静态资源`
8.                  `.addPathPatterns("/**")`
9.                  `// 放行的请求`
10.                  `.excludePathPatterns("/","/login","/css/**","/fonts/**","/images/**","/js/**");`
11.      `}`
12.  `}`

测试：  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy01a39c82-e776-41cb-9202-641d1557d715.png)

## 6.6、异常处理

**错误处理**

-   默认情况下，Spring Boot提供`/error`处理所有错误的映射
-   对于浏览器客户端，响应一个“ whitelabel”错误视图，以HTML格式呈现相同的数据
-   对于机器客户端，它将生成JSON响应，其中包含错误，HTTP状态和异常消息的详细信息。  
    ![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy890c8ab2-c089-4304-8cf4-3afa7863dac8.png)

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy8ee967c2-be4c-4c9b-b05e-877515729e26.png)

SpringBoot也为我们提供了自定义错误页的功能。

自定义错误页的话可以在静态路径(如 /static/ ）下的`error`目录。或放在模板目录(如 /templates/ )下的`error`目录，都会被SpringBootz自动解析。  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy52b6b917-6e48-4546-bd9c-bb0f59b539f2.png)

**DefaultErrorAttributes**：定义错误页面中可以包含哪些数据。

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy7839cf56-9f31-419c-b4d5-7f90ee6387ab.png)

# 7、数据库连接

按照我们学习这么久，要连接数据肯定需要添加依赖才可以连接。

## 7.1、添加JDBC依赖

1、添加jdbc依赖

1.  `<dependency>`
2.      `<groupId>org.springframework.boot</groupId>`
3.      `<artifactId>spring-boot-starter-data-jdbc</artifactId>`
4.  `</dependency>`

发现同时依赖数据池（速度快）

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy08bd72c1-98e5-4671-9cb3-37d11eeae4b7.png)

2、添加数据库驱动。

1.  `<dependency>`
2.      `<groupId>mysql</groupId>`
3.      `<artifactId>mysql-connector-java</artifactId>`
4.  `</dependency>`

默认配置的mysql驱动版本`8.0.30`

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy4b8a2d42-1cb8-4024-a100-1afb0b60273f.png)

可以进行修改。

1.  `<!-- 方法一，直接在下面写版本 -->`
2.  `<dependency>`
3.      `<groupId>mysql</groupId>`
4.      `<artifactId>mysql-connector-java</artifactId>`
5.      `<version>5.1.49</version>`
6.  `</dependency>`

8.  `<!-- 方法一，2、重新声明版本（maven的属性的就近优先原则）-->`
9.  `<properties>`
10.      `<mysql.version>5.1.49</mysql.version>`
11.  `</properties>`

## 7.2、修改配置项

1.  `spring:`
2.    `datasource:`
3.      `url: jdbc:mysql://localhost:3306/sp_boot_demo`
4.      `username: root`
5.      `password: 123456`
6.      `driver-class-name: com.mysql.cj.jdbc.Driver`

## 7.3、测试

1.  `@SpringBootTest`
2.  `class DemoApplicationTests {`

4.      `@Autowired`
5.      `JdbcTemplate jdbcTemplate;`

7.      `@Test`
8.      `void contextLoads() {`
9.          `String sql = "select count(*) from sys_user";`
10.          `Long totle = jdbcTemplate.queryForObject(sql, Long.class);`
11.          `System.out.println(totle);`
12.      `}`

14.  `}`

# 8、使用Druid数据源

> druid官方github地址： [https://github.com/alibaba/druid](https://github.com/alibaba/druid)

Druid相对于其他数据库连接池有着强大的**监控特性**，通过监控特性可以清楚知道连接池和SQl的工作情况。

## 8.1、添加依赖

1.  `<dependency>`
2.      `<groupId>com.alibaba</groupId>`
3.      `<artifactId>druid-spring-boot-starter</artifactId>`
4.      `<version>1.1.17</version>`
5.  `</dependency>`

## 8.2、添加配置

SpringBoot配置示例 ：[https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter](https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter)

配置项列表： [https://github.com/alibaba/druid/wiki/DruidDataSource%E9%85%8D%E7%BD%AE%E5%B1%9E%E6%80%A7%E5%88%97%E8%A1%A8](https://github.com/alibaba/druid/wiki/DruidDataSource%E9%85%8D%E7%BD%AE%E5%B1%9E%E6%80%A7%E5%88%97%E8%A1%A8)

1.  `spring:`
2.    `datasource:`
3.      `url: jdbc:mysql://localhost:3306/sp_boot_demo`
4.      `username: root`
5.      `password: 123456`
6.      `driver-class-name: com.mysql.cj.jdbc.Driver`

8.      `druid:`
9.        `aop-patterns: com.atguigu.admin.*  #监控SpringBean`
10.        `filters: stat,wall     # 底层开启功能，stat（sql监控），wall（防火墙）`

12.        `stat-view-servlet:   # 配置监控页功能`
13.          `enabled: true`
14.          `login-username: admin`
15.          `login-password: admin`
16.          `resetEnable: false`

18.        `web-stat-filter:  # 监控web`
19.          `enabled: true`
20.          `urlPattern: /*`
21.          `exclusions: '*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*'`

24.        `filter:`
25.          `stat:    # 对上面filters里面的stat的详细配置`
26.            `slow-sql-millis: 1000`
27.            `logSlowSql: true`
28.            `enabled: true`
29.          `wall:`
30.            `enabled: true`
31.            `config:`
32.              `drop-table-allow: false`

## 8.3、测试

访问：[http://localhost:8081/druid/login.html](http://localhost:8081/druid/login.html)

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudy0f137507-ac9e-4420-ae77-30948835a6f7.png)

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudyb4571def-0cd9-4b65-97f1-54e470bad305.png)

# 9、整合MyBatis操作

> github地址：[https://github.com/mybatis/spring-boot-starter](https://github.com/mybatis/spring-boot-starter)
> 
> SpringBoot配置文档：[https://github.com/mybatis/spring-boot-starter/blob/master/mybatis-spring-boot-autoconfigure/src/site/zh/markdown/index.md](https://github.com/mybatis/spring-boot-starter/blob/master/mybatis-spring-boot-autoconfigure/src/site/zh/markdown/index.md)

## 9.1、添加依赖

1.  `<dependency>`
2.      `<groupId>org.mybatis.spring.boot</groupId>`
3.      `<artifactId>mybatis-spring-boot-starter</artifactId>`
4.      `<version>2.2.1</version>`
5.  `</dependency>`

## 9.2、添加配置

1.  `# 配置mybatis规则`
2.  `mybatis:`
3.    `#sql映射文件位置`
4.    `mapper-locations: classpath:/mappers/*.xml`

## 9.3、测试

1、添加`mapper`接口

1.  `@Mapper`
2.  `public interface UserMapper {`
3.      `public List<SysUser> userList();`
4.  `}`

2、添加`UserMapper.xml`

1.  `<?xml version="1.0" encoding="UTF-8" ?>`

3.  `<!DOCTYPE mapper`
4.          `PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"`
5.          `"http://mybatis.org/dtd/mybatis-3-mapper.dtd">`
6.  `<mapper namespace="com.example.demo.mapper.UserMapper">`

8.      `<select id="userList" resultType="com.example.demo.entity.SysUser">`
9.          `` SELECT * FROM `sys_user` ``
10.      `</select>`
11.  `</mapper>`

3、测试

1.  `@SpringBootTest`
2.  `class DemoApplicationTests {`

4.      `@Autowired`
5.      `UserMapper userMapper;`

7.      `@Test`
8.      `void contextLoads() {`
9.          `List<SysUser> sysUsers = userMapper.userList();`
10.          `System.out.println(sysUsers);`
11.      `}`

13.  `}`

结果：  
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudydd86a459-cb8c-475b-8eba-a3e0709d68c2.png)

## 9.4、注解模式

1.  `@Mapper`
2.  `public interface UserMapper {`

4.      `public List<SysUser> userList();`

6.      `// 采用注释`
7.      ``@Select("SELECT * FROM `sys_user` where id=#{id}")``
8.      `public SysUser getById(Long id);`
9.  `}`

测试结果：

1.  `SysUser user = userMapper.getById(1L);`
2.  `System.out.println(user);`

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/09/16/kuangstudyad58ed56-9fd3-432f-bb03-66eb96ff2a7f.png)

推荐：

-   简单方法直接注解方式
-   复杂方法编写mapper.xml进行绑定映射

# 10、总结

-   springboot使用起来方便，能快速搭建spirng环境。
-   spirngboot官网帮我们配置了大部分参数。直接使用即可。
-   用JavaBean替代xml配置。配置更少了。看起来也简洁了不少。
-   添加很多新的组合注释。例如：Rest
-   能与其它框架快速集成，减少配置的编写。

参考文章：

[https://www.yuque.com/atguigu/springboot/rmxq85](https://www.yuque.com/atguigu/springboot/rmxq85)

[https://docs.spring.io/spring-boot/docs/2.6.11/reference/html/](https://docs.spring.io/spring-boot/docs/2.6.11/reference/html/)

标签：_springboot_

版权声明：本文为博主原创文章，遵循[CC 4.0 BY-SA](https://creativecommons.org/licenses/by-sa/4.0/)版权协议,转载请附上原文出处链接和本声明，KuangStudy,以学为伴，一生相伴！

[本文链接：https://www.kuangstudy.com/bbs/1570769526755663873](https://www.kuangstudy.com/bbs/1570769526755663873)
