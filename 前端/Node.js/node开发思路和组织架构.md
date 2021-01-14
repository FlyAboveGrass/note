## 项目目录的组织方式

1. 首先是入口文件 bin/www.js。 这个文件负责创建这个server

2. 然后是 请求和响应的处理文件 app.js

3. router中的是核心的请求处理，用于对各种的路由进行响应

4. contoller中将各种逻辑组合成一个个的功能模块，负责逻辑的处理

5. model中存放着各种模型，规范数据的格式



## 开发的时候的思路

在对项目的文件结构进行组织之后，就开始从上往下开始编写代码了

上层的代码不关心下层代码是如何实现的，只知道下层代码返回的统一的数据格式就可以了

1. 服务器层： www.js创建服务器，对与服务器进行各种配置

2. 数据处理层： app.js将请求的内容进行处理，将请求的内容解构出来； 同时将响应的内容进行设置，设置响应头和响应报文格式等的处理

3. 路由层： 从数据处理层获得请求得路由数据，并进行匹配，一旦匹配就执行相应的路由需要执行的操作，并且返回数据。

4. 逻辑层： 路由层确定要执行什么样的操作之后， 从逻辑层调用相应的方法执行，获得数据并返回，然后路由层将方法返回的数据再返回回去

 



## 项目工具

###  [路径别名的设置](https://blog.csdn.net/Rotten_LKZ/article/details/109263290)

1. 安装module-alias npm install module-alias --save

2. 在package.json文件中设置路径别名

```
"_moduleAliases": {

  "@": "src"

},
```

3. 在入口文件引入（**一定要放在所有自定义模块引入之前	**）

```
require('module-alias/register'); // 这是路径别名模块的导入
```

### 登录持久化

[cookie, session, token](https://segmentfault.com/a/1190000017831088)

[cookie, session和token的工作方式](https://www.cnblogs.com/cxuanBlog/p/12635842.html)

|                cookie                 | session        |
| :-----------------------------------: | -------------- |
|              用户通行证               | 用户信息档案表 |
| 存储在客户端,每次请求都要在请求中携带 | 存储在服务端   |
|            容量小，只有5k             | 容量大         |
|     不安全，有可能被禁用或者伪造      |                |
|                                       |                |

> 如果浏览器端禁用了cookie，那么可以把请求的内容放在url里面

#### 使用cookie和session的流程

http协议是无状态的，如果要关联两次请求，就可以使用cookie和session。客户端访问服务器的过程

1. 客户端会发送http请求到服务器端

2. 服务器端接受客户端请求后，建立一个session，并发送一个http响应到客户端，这个响应头，其中就包含Set-Cookie头部。该头部包含了sessionId。Set-Cookie格式如下，具体请看[Cookie详解](http://bubkoo.com/2014/04/21/http-cookies-explained/)
   `Set-Cookie: value[; expires=date][; domain=domain][; path=path][; secure]`

3. 在客户端发起的第二次请求，浏览器会自动在请求头中添加cookie

4. 服务器接收请求，分解cookie，验证信息，核对成功后返回response给客户端

   

一般来说，**cookie和session是需要一起使用**的

- 用session只需要在客户端保存一个id，实际上大量数据都是保存在服务端。如果全部用cookie，数据量大的时候客户端是没有那么多空间的。
- 如果只用cookie不用session，那么账户信息全部保存在客户端，一旦被劫持，全部信息都会泄露。并且客户端数据量变大，网络传输的数据量也会变大

### redis缓存

#### 为什么需要redis：

+ 进程内存有限，访问量暴增会导致内存暴增
+ 线上是多线程，多线程之间的数据不能共享

#### 什么是redis

redis是一个缓存数据库，数据存放在内存中。因为在内存中存储所以访问速度很快，但是成本也更高



#### redis使用

[redis常用命令](https://www.cnblogs.com/javastack/p/9854489.html)

```
> redis-server [--port 6379]  // 启动server
    > redis-cli [-h 127.0.0.1 -p 6379]  // 启动redis

```





### nginx

#### 什么是nginx

 	Nginx是一款开源免费的轻量级的Web服务器，也是一款轻量级的反向代理服务器。特点是： 高稳定、高性能、资源占用少、功能丰富、模块结构化、支持热部署

#### 为什么需要nginx

1. 直接支持Rails和PHP的程序      
2. 作为HTTP反向代理服务器     （[关于正向代理和反向代理](https://juejin.cn/post/6844904064266960903)）
3. 作为负载均衡服务器      ([关于负载均衡](https://zhuanlan.zhihu.com/p/32841479))
4. 作为邮件代理服务器      
5. 帮助实现前端动静分离



#### [如何使用nginx](https://juejin.cn/post/6844903938508980231)





## 项目部分代码

#### 请求参数处理

```
// 处理请求参数
if(req.method === 'POST') {
	req.body = await formatPostData(req)
}
    
// 合理的写法
const formatPostData = (req) => {
    let postData = ''
    return new Promise((resolve, reject) => {
        if(req.method !== 'POST' || req.headers['content-type'] !== 'application/json'){
            resolve({})
            return 
        }
        req.on('data', chunk => {
            postData += chunk.toString()
        })
        req.on('end', () => {
            // JSON.parse这个方法要求是严格的json格式，必须是用 "" 来定义变量名和值，单引号不可以
            resolve(JSON.parse(postData))
        })
    })
}
```

#### express跨域处理

```
// cores跨域
const cors = require('cors')
// 1、一定要设置{credentials: true, origin: 'http://127.0.0.1:8080'}， 否则跨域失败
app.use(cors({credentials: true, origin: 'http://127.0.0.1:8080'}));
app.all('*', function (req, res, next) {
  res.header("Access-Control-Allow-Credentials", true);
  // 2、一定要设置准确的协议。域名和端口，否则跨域失败
  res.header("Access-Control-Allow-Origin", "http://127.0.0.1:8080");
  res.header('Access-Control-Allow-Headers', "*");
  res.header("Access-Control-Allow-Methods", "*");
  next();
});
```

#### cookie和session处理

```
// redis客户端
const { REDIS_CONF } = require('../conf/enviroment')
const redis = require('redis')

// 创建redis客户端
const redisClient = redis.createClient(REDIS_CONF.port, REDIS_CONF.host)
redisClient.on('error', (err) => {
    console.log('error', err);
})

module.exports = redisClient


// app.js代码
var expressSession = require('express-session')
const RedisStore = require('connect-redis')(expressSession)
const redisClient = require('./db/redis')

// session 保存
app.use(cookieParser(SESSION_SECRET));
const sessionStore = new RedisStore({
  client: redisClient
})
// 从cookie过来（secret的值一定要和cookieParser的一致），拿到cookie的值然后去找session，session存在则放到req.session, 如果存在store则通过store存起来
app.use(expressSession({
  secret: SESSION_SECRET,
  name: 'username',
  resave: true,
  saveUninitialized: false,
  cookie: {
    // path: '/',
    // httpOnly: true,
    maxAge: 24 * 60 * 60 * 1000
  },
  store: sessionStore
}))
```



#### 



