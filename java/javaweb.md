> 课程来自：[https://www.bilibili.com/video/BV12J411M7Sj?spm_id_from=333.999.0.0&vd_source=345a382f2c86d3441cc342a80fc25545](https://www.bilibili.com/video/BV12J411M7Sj?spm_id_from=333.999.0.0&vd_source=345a382f2c86d3441cc342a80fc25545)

# 前言

提示：这里可以添加本文要记录的大概内容：
例如：随着人工智能的不断发展，机器学习这门技术也越来越重要，很多人都开启了学习机器学习，本文就介绍了机器学习的基础内容。

# 1.基本概念

## 1.1 基本概念

1.  静态web：html，css（提供给所有人看的数据，始终不会发生变化）
2.  动态web：Servlet/JSP，ASP，PHP，淘宝等几乎所有的网站
    （提供给所有人看的数据，始终会发生变化，每个人在不同时间，不同地方看到的都不同）

## 1.2 web应用程序

web应用程序：可以提供浏览器访问的程序

-   a.html、b.html等多个web资源，这些web资源可以被外界访问，对外界提供服务。
-   你们能访问到的任何一个页面或者资源，都存在于这个世界的某一个角落的计算机上。
-   URL：统一资源定位符。
-   这个统一的web资源会被放在同一个文件夹下，web应用程序—>Tomcat：服务器。
-   web应用程序编写完毕后，若想提供给外界访问：需要一个服务器来统一管理。

## 1.3 静态web

_.htm，_.html，这些都是网页的后缀，如果服务器上一直存在这些东西，我们就可以直接进行读取。
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/06/kuangstudy6ab7a752-fa17-431e-be07-163b8865e4b1.jpg)

## 1.4 动态web

页面会动态展示：Web的页面展示的效果因人而异。
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/06/kuangstudy5f53e972-3ea0-4539-b0de-27be95784eda.jpg)

# 2.web服务器

服务器是一种被动的操作，用来处理用户的一些请求和给用户一些响应信息。
IIS:
微软的服务器

tomcat:
Tomcat是Apache软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，由Apache、Sun和其他一些公司及个人共同开发而成。由于有了Sun 的参与和支持，最新的Servlet 和JSP规范总是能在Tomcat 中得到体现，Tomcat 5支持最新的Servlet 2.4和JSP2.0规范。
Tomcat服务器是一个免费的开放源代码的Web应用服务器，属于轻量级应用服务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP程序的首选。
对于一个初学者来说，可以这样认为，当在一台机器上配置好Apache服务器，可利用它响应HTML（标准通用标记语言下的一个应用）页面的访问请求。实际上Tomcat是Apache服务器的扩展，但运行时它是独立运行的，所以当你运行tomcat时，它实际上作为一个与Apache 独立的进程单独运行的。

# 3.tomcat详解

## 3.1 安装

## 3.2 启动

## 3.3 配置

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/06/kuangstudy48420cfc-3029-4923-8ad7-ed9ad217a000.jpg)

`高难度面试题`
请你谈谈网站是如何访问的？

1.  输入一个域名，回车
2.  检查本机的C:Windows\System32\drivers\etc\hosts配置文件下有没有这个域名映射
    有：直接返回对应的ip地址，这个地址中，有我们需要访问的web程序，可以直接访问。

    没有：去DNS服务器找，找到的话就返回，找不到就返回找不到。
    ![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/06/kuangstudy6de04f31-84b6-43c1-8d81-ed0665f49ae5.jpg)


## 3.4 发布一个网站

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/06/kuangstudyc157c9e0-2313-486e-b0ab-aef80e710d2d.jpg)

# 4.Http

## 4.1 什么是http

HTTP（超文本传输协议）是一个简单的请求-响应协议，它通常运行在TCP之上。

-   文本：html，字符串…
-   超文本：图片，音乐，视频，定位，地图…
-   默认端口：80
    HTTPS：安全的协议
-   默认端口：443

## 4.2 http的两个时代

-   http1.0：客户端可以与web服务器连接后，只能获得一个web资源，断开连接。
-   http2.0：客户端可以与web服务器连接后，可以获得多个web资源。

## 4.3 http请求

客户端—->发请求(request)—->服务器（比如：访问百度）
`General`

1.  `// 请求地址`
2.  `Request URL: https://www.baidu.com/`
3.  `// 请求方法`
4.  `Request Method: GET`
5.  `// 状态代码`
6.  `Status Code: 200 OK`
7.  `// 远程地址`
8.  `Remote Address: 14.215.177.38:443`
9.  `// 引用站点策略`
10.  `Referrer Policy: strict-origin-when-cross-origin`

`Request Headers`

1.  `Accept: text/html`
2.  `Accept-Encoding: gzip, deflate, br`
3.  `// 语言`
4.  `Accept-Language: zh-CN,zh;q=0.9`
5.  `Cache-Control: max-age=0`
6.  `Connection: keep-alive`
1.  请求行

-   请求行中的请求方式：GET
-   请求方式：Get，Post，HEAD，DELETE，PUT…
    GET:请求能够携带的参数比较少，大小有限制，会在浏览器的URL地址栏显示数据内容，不安全，但高效。
    POST:请求能够携带的参数没有限制，大小没有限制，不会在浏览器的URL地址栏显示数据内容，安全，但不高效。

1.  请求头（消息头）

    1.  `Accept: 告诉浏览器，它所支持的数据类型`
    2.  `Accept-Encoding: 告诉浏览器，它支持哪种编码格式：GBK,UTF-8,GB2312,ISO8859-1`
    3.  `Accept-Language: 告诉浏览器，它的语言环境`
    4.  `Cache-Control: 缓存控制`
    5.  `Connection: 告诉浏览器，请求完成是断开还是保持`
    6.  `HOST：主机`


## 4.4 http响应

`Response Headers`

1.  `// 缓存控制`
2.  `Cache-Control: no-cache`
3.  `// 保持连接（http1.1）`
4.  `Connection: keep-alive`
5.  `// 文本编码类型`
6.  `Content-Encoding: gzip`
7.  `// 响应类型`
8.  `Content-Type: text/html;charset=utf-8`
1.  响应体

    1.  `Accept: 告诉浏览器，它所支持的数据类型`
    2.  `Accept-Encoding: 告诉浏览器，它支持哪种编码格式：GBK,UTF-8,GB2312,ISO8859-1`
    3.  `Accept-Language: 告诉浏览器，它的语言环境`
    4.  `Cache-Control: 缓存控制`
    5.  `Connection: 告诉浏览器，请求完成是断开还是保持`
    6.  `HOST：主机`
    7.  `Refrush：告诉客户端，多久刷新一次`
    8.  `Location：让网页重新定位`

2.  响应状态码


-   200：响应成功
-   3xx：请求重定向（304等等）
-   4xx：找不到资源（404等等）
-   5xx：服务器代码错误（500代码错误，502网关错误）

`常见面试题`
当你的浏览器中地址栏输入地址并回车的一瞬间到页面能够展示回来，经历了什么？

# 5.Maven

# 6.Servlet

## 6.1 servlet简介

-   Servlet就是sun公司开发动态web的一门技术。
-   Sun在这些API中提供一个接口叫做：Servlet，如果你想开发一个Servlet程序，只需要完成两个小步骤：
    1.  编写一个类，实现Servlet接口；
    2.  把开发好的Java类部署到web服务器中。
        把实现了Servlet接口Java程序叫做，Servlet

## 6.2 HelloServlet

Serlvet接口Sun公司有两个默认的实现类：HttpServlet，GenericServlet
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/07/kuangstudya8b378f6-e3bd-40ba-865c-bd61670f3f51.jpg)

1.  构建一个普通的Maven项目，删掉里面的src目录，以后我们的学习就在这个项目里面建立Moudel；这个空的工程就是Maven主工程；
2.  关于Maven父子工程的理解：
    父工程中会有：

    1.       `<modules>`
    2.           `<module>servlet-01</module>`
    3.       `</modules>`

    子项目中会有：

    1.       `<parent>`
    2.           `<groupId>org.example</groupId>`
    3.           `<artifactId>javaweb-servlet</artifactId>`
    4.           `<version>1.0-SNAPSHOT</version>`
    5.       `</parent>`

    父项目中的jar包，子项目可以直接使用。反之，不可。

3.  Maven环境优化
4.  编写一个Servlet程序
    1.编写一个普通的类
    2.实现Servlet接口，这里我们直接继承HttpServlet类

    1.   `public class HelloServlet extends HttpServlet {`

    3.       `[@Override](https://github.com/Override "@Override")`
    4.       `protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    5.           `System.out.println("hello servlet");`
    6.           `PrintWriter writer = resp.getWriter();`
    7.           `writer.println("Hello Servlet");`
    8.       `}`

    10.       `[@Override](https://github.com/Override "@Override")`
    11.       `protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    12.           `this.doGet(req,resp);`
    13.       `}`
    14.   `}`

5.  编写Servlet的映射
    为什么需要映射：我们写的是JAVA程序，但是要通过浏览器访问，而浏览器需要连接web服务器，所以我们需要在web服务中注册我们写的Servlet，还需给他一个浏览器能够访问的路径。

    1.  `<!--注册servlet-->`
    2.  `<servlet>`
    3.   `<servlet-name>helloservlet</servlet-name>`
    4.   `<servlet-class>com.sunyiwenlong.servlet.HelloServlet</servlet-class>`
    5.  `</servlet>`
    6.  `<!--servlet请求路径-->`
    7.  `<servlet-mapping>`
    8.   `<servlet-name>helloservlet</servlet-name>`
    9.   `<url-pattern>/hello</url-pattern>`
    10.  `</servlet-mapping>`

6.  配置tomcat
    注意：配置项目发布路径就可以了

7.  启动测试

## 6.3 Servlet原理

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/07/kuangstudybf07068f-3fe7-4651-ba6d-88f522085994.jpg)

## 6.4 Mapping问题

1.  一个Servlet可以指定一个映射路径
2.  一个Servlet可以指定多个映射路径
3.  一个Servlet可以指定通用映射路径
4.  指定一些后缀或者前缀等等…

## 6.5 ServletContext

web容器在启动的时候，它会为每个web程序都创建一个对应的ServletContext对象，它代表了当前的web应用。

1.  共享数据：在这个Servlet中保存的数据，可以在另一个Servlet中拿到
    ![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/07/kuangstudy8761b223-9619-4d95-83c3-bc83b21a0658.jpg)

    1.  `public class HelloServlet extends HttpServlet {`
    2.   `[@Override](https://github.com/Override "@Override")`
    3.   `protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    4.       `// this.getInitParameter(); 获取初始化参数（web.xml文件中的初始化参数）`
    5.       `// this.getServletConfig(); 获取servlet的配置（web.xml文件中的配置）`
    6.       `// this.getServletContext(); 获取servlet上下文`
    7.       `ServletContext context = this.getServletContext();`
    8.       `String username = "张三";`
    9.       `context.setAttribute("username",username);// 将一个数据保存在了ServletContext中`
    10.   `}`
    11.  `}`
    1.  `public class GetServlet extends HttpServlet {`
    2.   `[@Override](https://github.com/Override "@Override")`
    3.   `protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    4.       `ServletContext context = this.getServletContext();`
    5.       `String username = (String) context.getAttribute("username");`
    7.       `resp.setContentType("text/html");`
    8.       `resp.setCharacterEncoding("utf-8");`
    9.       `resp.getWriter().println("名字"+username);`
    10.   `}`
    12.   `[@Override](https://github.com/Override "@Override")`
    13.   `protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    14.       `this.doGet(req,resp);`
    15.   `}`
    16.  `}`
    1.  `// web.xml文件`
    2.  `<?xml version="1.0" encoding="UTF-8"?>`
    3.  `<web-app version="2.4"`
    4.        `xmlns="http://java.sun.com/xml/ns/j2ee"`
    5.        `xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"`
    6.        `xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">`
    8.  `<servlet>`
    9.   `<servlet-name>helloservlet1</servlet-name>`
    10.   `<servlet-class>com.sunyiwenlong.servlet.HelloServlet</servlet-class>`
    11.  `</servlet>`
    12.  `<servlet-mapping>`
    13.   `<servlet-name>helloservlet1</servlet-name>`
    14.   `<url-pattern>/hello</url-pattern>`
    15.  `</servlet-mapping>`
    17.  `<servlet>`
    18.   `<servlet-name>getservlet</servlet-name>`
    19.   `<servlet-class>com.sunyiwenlong.servlet.GetServlet</servlet-class>`
    20.  `</servlet>`
    21.  `<servlet-mapping>`
    22.   `<servlet-name>getservlet</servlet-name>`
    23.   `<url-pattern>/getc</url-pattern>`
    24.  `</servlet-mapping>`
    25.  `</web-app>`
2.  获取初始化参数

    1.  `// web.xml文件`
    2.  `<!--配置一些web应用一些初始化参数-->`
    3.  `<context-param>`
    4.   `<param-name>url</param-name>`
    5.   `<param-value>jdbc:mysql://localhost:3306/mybatis</param-value>`
    6.  `</context-param>`
    1.  `public class ServletDemo03 extends HttpServlet {`
    2.   `[@Override](https://github.com/Override "@Override")`
    3.   `protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    4.       `String url = this.getInitParameter("url");`
    5.       `resp.getWriter().println(url);`
    6.   `}`
    8.   `[@Override](https://github.com/Override "@Override")`
    9.   `protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    10.       `this.doGet(req, resp);`
    11.   `}`
    12.  `}`
3.  请求转发

    1.  `// web.xml文件`
    2.  `// 请求sd4`
    3.  `<servlet>`
    4.   `<servlet-name>gp</servlet-name>`
    5.   `<servlet-class>com.sunyiwenlong.servlet.ServletDemo03</servlet-class>`
    6.  `</servlet>`
    7.  `<servlet-mapping>`
    8.   `<servlet-name>gp</servlet-name>`
    9.   `<url-pattern>/gp</url-pattern>`
    10.  `</servlet-mapping>`
    12.  `<servlet>`
    13.   `<servlet-name>sd4</servlet-name>`
    14.   `<servlet-class>com.sunyiwenlong.servlet.ServletDemo04</servlet-class>`
    15.  `</servlet>`
    16.  `<servlet-mapping>`
    17.   `<servlet-name>sd4</servlet-name>`
    18.   `<url-pattern>/sd4</url-pattern>`
    19.  `</servlet-mapping>`
    1.  `// 请求/sd4找到ServletDemo04，ServletDemo04进行请求转发到/gp，到/gp的页面`
    2.  `// (浏览器路径是sd4的路径，页面拿到的是/gp的数据)`
    3.  `public class ServletDemo04 extends HttpServlet {`
    4.   `[@Override](https://github.com/Override "@Override")`
    5.   `protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    6.       `ServletContext context = this.getServletContext();`
    7.       `System.out.println("进入了demo04");`
    8.       `// RequestDispatcher requestDispatcher = context.getRequestDispatcher("/gp");// 转发的路径`
    9.       `// requestDispatcher.forward(req,resp);// 调用forward请求转发`
    10.       `context.getRequestDispatcher("/gp").forward(req,resp);`
    11.   `}`
    13.   `[@Override](https://github.com/Override "@Override")`
    14.   `protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    15.       `this.doGet(req, resp);`
    16.   `}`
    17.  `}`
4.  读取资源文件
    Properties


-   在java目录下新建properties
-   在resources目录下新建properties发现：都被打包到了同一个路径下：classes，我们俗称这个路径为classpath。

思路：需要一个文件流

1.  `public class ServletDemo05 extends HttpServlet {`
2.      `@Override`
3.      `protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
4.          `InputStream stream = this.getServletContext().getResourceAsStream("/WEB-INF/CLASSES/db.properties");`
5.          `Properties properties = new Properties();`
6.          `properties.load(stream);`
7.          `String username = properties.getProperty("username");`
8.          `String password = properties.getProperty("password");`
9.          `resp.getWriter().println(username+":"+password);`
10.      `}`
12.      `@Override`
13.      `protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
14.          `this.doGet(req, resp);`
15.      `}`
16.  `}`

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/07/kuangstudyf9f97b5a-f7cc-4575-b74d-a3f71cfe1865.jpg)

## 6.5 HttpServletResponse

web服务器接收到客户端的http请求，针对这个请求，分别创建一个代表请求的HttpServletRequest对象，代表响应的一个HttpServletResponse；|

-   如果要获取客户端请求过来的参数：找HttpServletRequest
-   如果要给客户端响应一些信息：找HttpServletResponse

1.  负责向浏览器发送数据的方法

    1.  `public ServletOutputStream getOutputStream() throws IOException;`
    2.  `public PrintWriter getWriter() throws IOException;`

2.  响应的状态码

    1.  `public static final int SC_CONTINUE = 100;`
    2.   `/**`
    3.    `* Status code (200) indicating the request succeeded normally.`
    4.    `*/`
    5.   `public static final int SC_OK = 200;`

    7.   `/**`
    8.    `* Status code (302) indicating that the resource has temporarily`
    9.    `* moved to another location, but that future references should`
    10.    `* still use the original URI to access the resource.`
    11.    `*`
    12.    `* This definition is being retained for backwards compatibility.`
    13.    `* SC_FOUND is now the preferred definition.`
    14.    `*/`
    15.   `public static final int SC_MOVED_TEMPORARILY = 302;`

    17.   `/**`
    18.   `* Status code (302) indicating that the resource reside`
    19.   `* temporarily under a different URI. Since the redirection might`
    20.   `* be altered on occasion, the client should continue to use the`
    21.   `* Request-URI for future requests.(HTTP/1.1) To represent the`
    22.   `* status code (302), it is recommended to use this variable.`
    23.   `*/`
    24.   `public static final int SC_FOUND = 302;`

    26.   `/**`
    27.    `* Status code (304) indicating that a conditional GET operation`
    28.    `* found that the resource was available and not modified.`
    29.    `*/`
    30.   `public static final int SC_NOT_MODIFIED = 304;`

    32.   `/**`
    33.    `* Status code (404) indicating that the requested resource is not`
    34.    `* available.`
    35.    `*/`
    36.   `public static final int SC_NOT_FOUND = 404;`

    38.   `/**`
    39.    `* Status code (500) indicating an error inside the HTTP server`
    40.    `* which prevented it from fulfilling the request.`
    41.    `*/`
    42.   `public static final int SC_INTERNAL_SERVER_ERROR = 500;`

    44.   `/**`
    45.    `* Status code (502) indicating that the HTTP server received an`
    46.    `* invalid response from a server it consulted when acting as a`
    47.    `* proxy or gateway.`
    48.    `*/`
    49.   `public static final int SC_BAD_GATEWAY = 502;`

    51.   `// …`


`常见应用`

1.  向浏览器输出消息
2.  下载文件

    1.  `public class FileServlet extends HttpServlet {`
    2.   `[@Override](https://github.com/Override "@Override")`
    3.   `protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    4.       `// 1.要获取下载文件的路径`
    5.       `String realPath = "E:\\dev\\StudyProjects\\javaweb-servlet\\response\\src\\main\\resources\\大乔.jpg";`
    6.       `// 2.下载的文件名是啥？`
    7.       `String filename = realPath.substring(realPath.lastIndexOf("\\") + 1);`
    8.       `// 3.设置想办法让浏览器能够支持下载我们需要的东西`
    9.       `resp.setHeader("Content-disposition","attachment;filename="+ URLEncoder.encode(filename,"utf-8"));`
    10.       `// 4.获取下载文件的输入流`
    11.       `FileInputStream in = new FileInputStream(realPath);`
    12.       `// 5.创建缓冲区`
    13.       `int len = 0;`
    14.       `byte[] buffer = new byte[1024];// 每次读取的长度`
    15.       `// 6.获取OutputStream对象`
    16.       `ServletOutputStream out = resp.getOutputStream();`
    17.       `// 7.将FileOutputStream流写入到bufer缓冲区`
    18.       `while ((len = in.read(buffer))>0){// 每次读取的长度大于0的情况下，就写出去`
    19.           `out.write(buffer,0,len);// 写出字节，从0写到len`
    20.       `}`
    21.       `// 8.使用OutputStream将缓冲区中的数据输出到客户端！`
    22.       `in.close();`
    23.       `out.close();`
    24.   `}`

    26.   `[@Override](https://github.com/Override "@Override")`
    27.   `protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    28.       `this.doGet(req, resp);`
    29.   `}`
    30.  `}`

3.  验证码功能

    1.  `public class ImageServlet extends HttpServlet {`
    2.   `[@Override](https://github.com/Override "@Override")`
    3.   `protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    4.       `// 让浏览器3秒刷新一次`
    5.       `resp.setHeader("refresh", "3");`

    7.       `// 在内存中创建一个图片`
    8.       `BufferedImage image = new BufferedImage(80, 20, BufferedImage.TYPE_INT_RGB);// 宽、高、颜色`
    9.       `// 得到图片`
    10.       `Graphics2D g = (Graphics2D) image.getGraphics();// 得到一只2D的笔`
    11.       `// 设置图片的背景颜色`
    12.       `g.setColor(Color.white);`
    13.       `g.fillRect(0, 0, 80, 20);// 填充颜色`
    14.       `// 换个背景颜色`
    15.       `g.setColor(Color.BLUE);`
    16.       `// 设置字体样式：粗体，20`
    17.       `g.setFont(new Font(null,Font.BOLD,20));`
    18.       `// 画一个字符串（给图片写数据）`
    19.       `g.drawString(makeNum(),0,20);`
    20.       `// 告诉浏览器，这个请求用图片的方式打开`
    21.       `resp.setContentType("image/jpeg");`
    22.       `// 网站存在缓存，不让浏览器缓存`
    23.       `resp.setDateHeader("expires",-1);`
    24.       `resp.setHeader("Cache-Control","no-cache");`
    25.       `resp.setHeader("Pragma","no-cache");`
    26.       `// 把图片写给浏览器`
    27.       `boolean write = ImageIO.write(image, "jpg",resp.getOutputStream());`
    28.   `}`

    30.   `// 生成随机数`
    31.   `private String makeNum() {`
    32.       `Random random = new Random();`
    33.       `String num = random.nextInt(9999999) + "";// 随机数，最大七位，[0,9999999)`
    34.       `StringBuffer sb = new StringBuffer();`
    35.       `for (int i = 0; i < 7 - num.length(); i++) {// 不足七位，则添加0`
    36.           `sb.append("0");`
    37.       `}`
    38.       `num = sb.toString()+num;// 不足七位，在随机数前面添加0`
    39.       `return num;`
    40.   `}`

    42.   `[@Override](https://github.com/Override "@Override")`
    43.   `protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    44.       `this.doGet(req, resp);`
    45.   `}`
    46.  `}`

4.  实现请求重定向

    1.  `public class RedirectServlet extends HttpServlet {`
    2.   `[@Override](https://github.com/Override "@Override")`
    3.   `protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    4.       `/*resp.setHeader("Location","/response_war/image");`
    5.       `resp.setStatus(HttpServletResponse.SC_NOT_MODIFIED);*/`
    6.       `resp.sendRedirect("/response_war/image");// 重定向相当于上面两行代码`
    7.   `}`

    9.   `[@Override](https://github.com/Override "@Override")`
    10.   `protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    11.       `this.doGet(req, resp);`
    12.   `}`
    13.  `}`


## 6.5 HttpServletRequest

HttpServletRequest代表客户端的请求，用户通过Http协议访问服务器，HTTP请求中的所有信息会被封装到HttpServletRequest，通过这个HttpServletRequest的方法，获得客户端的所有信息。

1.  获取前端传递的参数
    ![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/09/kuangstudyfbb80cd9-dac2-49f1-aea8-625e2b074aa9.jpg)
2.  请求转发
    前端：

    1.  `<%@ page contentType="text/html;charset=UTF-8" language="java" %>`
    2.  `<html>`
    3.  `<head>`
    4.   `<title>首页</title>`
    5.  `</head>`
    6.  `<body>`
    7.  `<form action="${pageContext.request.contextPath}/login" method="post">`
    8.   `用户名：<input type="text" name="username"><br>`
    9.   `密码：<input type="password" name="password"><br>`
    10.   `爱好：`
    11.   `<input type="checkbox" name="hobbys" value="代码"> 代码`
    12.   `<input type="checkbox" name="hobbys" value="唱歌"> 唱歌`
    13.   `<input type="checkbox" name="hobbys" value="女孩"> 女孩`
    14.   `<input type="checkbox" name="hobbys" value="电影"> 电影`
    15.   `<br>`
    16.   `<input type="submit" name="提交">`
    17.  `</form>`
    18.  `</body>`
    19.  `</html>`

    后端：

    1.  `public class LoginServlet extends HttpServlet {`
    2.   `[@Override](https://github.com/Override "@Override")`
    3.   `protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    4.       `this.doPost(req, resp);`
    5.   `}`

    7.   `[@Override](https://github.com/Override "@Override")`
    8.   `protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
    9.       `// 处理请求中文乱码（后期可以使用过滤器来解决）`
    10.       `req.setCharacterEncoding("utf-8");`
    11.       `resp.setCharacterEncoding("utf-8");`

    13.       `String username = req.getParameter("username");`
    14.       `String password = req.getParameter("password");`
    15.       `String[] hobbys = req.getParameterValues("hobbys");`
    16.       `System.out.println(username);`
    17.       `System.out.println(password);`
    18.       `System.out.println(Arrays.toString(hobbys));`
    19.       `// 这里的 / 代表当前的web应用，所以不需要再加上/request_war这个上下文路径了，否则会出现404错误`
    20.       `req.getRequestDispatcher("/success.jsp").forward(req,resp);`
    21.   `}`
    22.  `}`


web.xml
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/09/kuangstudy591711cf-1d5c-4ea3-9d80-4001b924f6df.jpg)

`面试题：请你聊聊重定向和转发的区别？`
相同点：页面都会实现跳转
不同点：

-   请求转发的时候，url地址栏不会产生变化。307
-   重定向时候，url地址栏会发生变化。302

# 7.cookie/session

## 7.1 会话

无状态的会话：用户打开一个浏览器，点击了很多超链接，访问多个web资源，关闭浏览器，这个过程可以称之为会话。
有状态的会话：一个用户打开一个浏览器，访问某些资源（网站），下次再来访问该资源（网站），我们会知道这个用户曾经来过，称之为有状态会话；

`一个网站，怎么证明你来过？`

1.  服务端给客户端一个信件，客户端下次访问服务端带上信件就可以了；cookie（客户端）
2.  服务器登记你来过了，下次你来的时候我来匹配你；seesion（服务端）

## 7.2 保存会话的两种技术

cookie：

-   客户端技术，（响应、请求）

session：

-   服务端技术，利用这个技术，可以保存用户的会话信息？我们可以把信息或者数据放在Session中。

## 7.3 Cookie

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/09/kuangstudy436007a6-2fe3-4f8b-a276-ba59867e156f.jpg)

1.  从请求中拿到cookie
2.  服务器响应给客户端cookie

    1.  `Cookie[] cookies = req.getCookies();// 获得cookie`
    2.  `cookie.getName();// 获得cookie中的key`
    3.  `cookie.getValue();// 获得cookie中的value`
    4.  `new Cookie("lastLoginTime",System.currentTimeMills()+"");// 新建一个cookie`
    5.  `cookie.setMaxAge(24*60*60);// 设置cookie的有效期，单位：秒`
    6.  `resp.addCookie(cookie);// 响应给客户端一个cookie`


`cookie：一般会保存在本地的用户目录下appdata`
`一个网站cookie是否存在上限！聊聊细节问题`

-   一个Cookie只能保存一个信息；
-   一个web站点可以给浏览器发送多个cookie，最多存放20个cookie；
-   Cookie大小有限制4kb
-   300个cookie浏览器上限

`删除cookie`

1.  不设置有效期，关闭浏览器，自动失效
2.  设置有效期时间为0

`编码解码，解决中文乱码问题`

1.  `URLEncoder.encode("张三","UTF-8")`
2.  `URLDecoder.decode("张三","UTF-8")`

保存上一次登录时间

1.  `// 保存用户上一次访问的时间`
2.  `public class CookieDemo01 extends HttpServlet {`
3.      `@Override`
4.      `protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
5.          `// 服务器告诉你，你来的时间，把这个时间封装成一个信息，你下次带来，我就知道你上次来的时间`
6.          `// 解决中文乱码`
7.          `req.setCharacterEncoding("utf-8");`
8.          `resp.setCharacterEncoding("utf-8");`
10.          `PrintWriter out = resp.getWriter();`
11.          `// Cookie，服务器端从客户端获取cookie`
12.          `Cookie[] cookies = req.getCookies();// 数组，说明cookie可以有多个`
13.          `// 判断cookie是否`
14.          `if (cookies != null) {`
15.              `out.write("你上一次登录的时间是：");`
16.              `for (int i = 0; i < cookies.length; i++) {`
17.                  `// 获取cookie的名字`
18.                  `if (cookies[i].getName().equals("lastLoginTime")) {`
19.                      `// 获取cookie的值`
20.                      `long l = Long.parseLong(cookies[i].getValue());`
21.                      `Date date = new Date(l);`
22.                      `out.write(date.toLocaleString());`
23.                  `}`
24.              `}`
25.          `} else {`
26.              `out.write("你是第一次登录！");`
27.          `}`
28.          `Cookie cookie = new Cookie("lastLoginTime", System.currentTimeMillis() + "");`
29.          `cookie.setMaxAge(24*60*60);// 设置cookie的有效期为一天，单位是：秒`
30.          `resp.addCookie(cookie);`
32.      `}`
33.      `@Override`
34.      `protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
35.          `this.doGet(req, resp);`
36.      `}`
37.  `}`

## 7.4 session（重点）

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/09/kuangstudyeb1e8d24-496a-4939-a2ce-4d9e9817da6c.jpg)
什么是Session：

-   服务器会给每一个用户（浏览器）创建一个Seesion对象。
-   一个session独占一个浏览器，只要浏览器没有关闭，这个session就存在。
-   用户登录之后，整个网站它都可以访问。（保存用户的信息；也可以保存购物车的信息）

`session和cookie的区别`

-   Cookie是把用户的数据写给用户的浏览器，浏览器保存（可以保存多个）
-   Session把用户的数据写到用户独占Session中，服务器端保存（保存重要的信息，减少服务器资源的浪费）
-   Session对象由服务创建；

`使用场景`

-   保存一个登录用户的信息；
-   购物车信息；
-   在整个网站中经常会使用的数据，我们将它保存在Session中；

`会话自动过期`

1.    `<!--设置session默认的失效时间-->`
2.    `<session-config>`
3.      `<!--15分钟后session自动失效，以分钟为单位-->`
4.      `<session-timeout>15</session-timeout>`
5.    `</session-config>`

`session的使用`

1.  `public class SessionDemo01 extends HttpServlet {`
2.      `@Override`
3.      `protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
4.          `// 解决中文乱码`
5.          `req.setCharacterEncoding("UTF-8");`
6.          `resp.setCharacterEncoding("UTF-8");`
7.          `resp.setContentType("text/html;charset=utf-8");`
9.          `// 得到session`
10.          `HttpSession session = req.getSession();`
11.          `// 给session中存东西`
12.          `session.setAttribute("name", "张三");`
13.          `// 获取session的id`
14.          `String sessionId = session.getId();`
15.          `// 判断session是不是新创建`
16.          `if (session.isNew()) {`
17.              `resp.getWriter().write("session创建成功，ID:" + sessionId);`
18.          `} else {`
19.              `resp.getWriter().write("session已经存在了，ID：" + sessionId);`
20.          `}`
21.          `// session创建的时候做了什么事情`
22.          `/*Cookie cookie = new Cookie("JSESSIONID", sessionId);`
23.          `resp.addCookie(cookie);*/`
25.          `//------------------`
26.          `// 从session中获取数据`
27.          `String name = (String) session.getAttribute("name");`
28.          `//------------------`
29.          `// 从session中删除指定name的数据`
30.          `session.removeAttribute("name");`
31.          `// 手动注销session`
32.          `session.invalidate();`
33.      `}`
34.      `@Override`
35.      `protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {`
36.          `this.doGet(req, resp);`
37.      `}`
38.  `}`

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/09/kuangstudyd1fb94cc-5e84-4cc5-a1af-49babe8ea2f0.jpg)

# 8.jsp

## 8.1 什么是jsp

Java Server Pages：Java服务端页面，和servlet一样，用于动态web技术
最大的特点：

-   写jsp就像在写html
-   区别：
    -   html只给用户提供静态的数据
    -   jsp页面中可以嵌入Java代码，为用户提供动态数据

## 8.2 jsp原理

思路：jsp是怎样执行的？

-   代码层面没有任何问题
-   服务器内部工作
    -   tomcat中有一个work目录；IDEA中使用tomcat的会在IDEA的tomcat中生产一个work目录
-   目录地址
    ![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/11/kuangstudy785da300-879f-434f-9b19-ad10fa0cd9c7.jpg)

我电脑上的地址：

1.  `C:\Users\Administrator\AppData\Local\JetBrains\IntelliJIdea2021.2`

发现页面会被转换成为一个Java类！
![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/11/kuangstudy807345c3-c71c-4562-a197-729dfb29ffee.jpg)

`浏览器向服务器发送请求，不管访问什么资源，其实都是在访问Servlet！`

`JSP最终也会被转换成为一个Java类！`

`JSP本质上就是一个Servlet`

1.  `// 初始化`
2.  `public void _jspInit() {`
3.    `}`
4.  `// 销毁`
5.    `public void _jspDestroy() {`
6.    `}`
7.  `// JSPservice`
8.    `public void _jspService(final javax.servlet.http.HttpServletRequest request, final javax.servlet.http.HttpServletResponse response)`
1.  判断请求的方式
2.  内置一些对象（9个）

    1.  `final javax.servlet.jsp.PageContext pageContext;    // 页面上下文`
    2.  `javax.servlet.http.HttpSession session = null;        // session`
    3.  `final javax.servlet.ServletContext application;        // applicationContext`
    4.  `final javax.servlet.ServletConfig config;            // config`
    5.  `javax.servlet.jsp.JspWriter out = null;                // out`
    6.  `final java.lang.Object page = this;                    // page：当前`
    7.  `HttpServletRequest request;                            // 请求`
    8.  `HttpServletResponse response;                        // 响应`

3.  输出页面前增加的代码

    1.  `response.setContentType("text/html;charset=UTF-8");// 设置响应的页面类型`
    2.  `pageContext = _jspxFactory.getPageContext(this, request, response,`
    3.           `null, true, 8192, true);`
    4.  `_jspx_page_context = pageContext;`
    5.  `application = pageContext.getServletContext();`
    6.  `config = pageContext.getServletConfig();`
    7.  `session = pageContext.getSession();`
    8.  `out = pageContext.getOut();`
    9.  `_jspx_out = out;`

4.  以上的内置对象可以在jsp页面中直接使用

5.  原理图
    ![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/11/kuangstudy91ca1997-e219-4e14-9457-6c0f4ce3ade4.jpg)


在JSP页面中；
只要是JAVA代码就会原封不动的输出；
如果是HTML代码，就会被转换为：

1.  `out.write("<html>\r\n");`
2.  `out.write("<head>\r\n");`

这样的格式，输出到前端！

## 8.3 JSP基础语法

任何语言都有自己的语法，JAVA中有；
JSP作为java技术的一种应用，它拥有一些自己扩充的语法（了解，知道即可！），Java所有语法它都支持！

1.  jsp表达式

    1.  `<%--jsp表达式`
    2.  `作用：用来将程序的输出，输出到客户端`
    3.  `<%= 变量或表达式%>`
    4.  `--%>`
    5.  `<%= new java.util.Date() %>`

2.  jsp脚本片段

    1.  `<%--jsp脚本片段--%>`
    2.  `<%`
    3.   `int sum = 0;`
    4.   `for (int i = 0; i < 100; i++) {`
    5.     `sum+=i;`
    6.   `}`
    7.   `out.print("<h1>sum="+sum+"</h1>");`
    8.  `%>`
    9.  `//--------------------------`
    10.  `<%--EL表达式：${变量} --%>`
    11.  `<%`
    12.   `for (int i = 0; i < 3; i++) {`
    13.  `%>`
    14.  `<h1>Hello World! ${i}</h1>`
    15.  `<%`
    16.   `}`
    17.  `%>`

3.  jsp声明

    1.  `<%!`
    2.  `static {`
    3.   `System.out.println("loading Servlet!");`
    4.  `}`
    5.  `private int globalVar =0;`

    7.  `public void kuang(){`
    8.   `System.out.println("进入了该方法！");`
    9.  `}`
    10.  `%>`


JSP声明：会被编译到jsp生成java的类中！
其他的，就会被生成到_jspService方法中！
在jsp，嵌入Java代码即可！

## 8.4 jsp指令

1.  定制错误页面

    1.  `<%--定制错误页面--%>`
    2.  `<%@ page errorPage="error/500.jsp" %>`

    或者在web.xml定制全局的错误页面
    ![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/11/kuangstudycfd43edb-06bc-4796-b7d6-0b5022cc3a88.jpg)

2.  包含头部和尾部
    ![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/11/kuangstudyb64caaa0-79ff-44e6-9a17-5371bfad0057.jpg)


## 8.5 九大内置对象

-   PageContext `存东西`
-   Request `存东西`
-   Response
-   Session `存东西`
-   Application（ServletContext） `存东西`
-   Config（ServletConfig）
-   out
-   page
-   exception

`存东西的区别`

1.  `pageContext.setAttribute("name1","张三1");// 保存的数据只在一个页面中有效`
2.  `request.setAttribute("name2","张三2");// 保存的数据只在一次请求中有效，请求转发会携带这个数据`
3.  `session.setAttribute("name3","张三3");// 保存的数据只在一次会话中有效，从打开浏览器到关闭浏览器`
4.  `application.setAttribute("name4","张三4");// 保存的数据只在服务器中有效，从打开服务器到关闭服务器`

![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/11/kuangstudy8374695c-80b2-4c0a-96ee-e588b6604359.jpg)

request：客户端向服务器发送请求，产生的数据，用户看完就没用了，比如：新闻，用户看完没用的！
session：客户端向服务器发送请求，产生的数据，用户用完一会还有用，比如：购物车；
application：客户端向服务器发送请求，产生的数据，一个用户用完了，其他用户还可能使用，比如：聊天数据；

## 8.6 jsp标签、JSTL标签、EL表达式

# 9.mvc三层架构

# 10.过滤器（Filter）

Filter：过滤器，用来过滤网站的数据：

-   处理中文乱码。
-   登录验证。

Filter开发步骤：

1.  导包

    1.  `<!-- https://mvnrepository.com/artifact/javax.servlet/javax.servlet-api -->`
    2.       `<!--servlet依赖-->`
    3.       `<dependency>`
    4.           `<groupId>javax.servlet</groupId>`
    5.           `<artifactId>javax.servlet-api</artifactId>`
    6.           `<version>4.0.1</version>`
    7.           `<scope>provided</scope>`
    8.       `</dependency>`
    9.       `<!-- https://mvnrepository.com/artifact/javax.servlet.jsp/javax.servlet.jsp-api -->`
    10.       `<!--jsp依赖-->`
    11.       `<dependency>`
    12.           `<groupId>javax.servlet.jsp</groupId>`
    13.           `<artifactId>javax.servlet.jsp-api</artifactId>`
    14.           `<version>2.3.3</version>`
    15.           `<scope>provided</scope>`
    16.       `</dependency>`

2.  编写过滤器（自定义类实现javax.servlet.Filter接口）

    1.  `public class CharaterEncodingFilter implements Filter {`
    2.   `/**`
    3.    `* 初始化：服务器启动的时候，就已经初始化了，随时等待过滤对象出现！`
    4.    `* [@param](https://github.com/param "@param") filterConfig`
    5.    `* [@throws](https://github.com/throws "@throws") ServletException`
    6.    `*/`
    7.   `[@Override](https://github.com/Override "@Override")`
    8.   `public void init(FilterConfig filterConfig) throws ServletException {`
    9.       `System.out.println("CharaterEncodingFilter初始化");`
    10.   `}`
    11.   `[@Override](https://github.com/Override "@Override")`
    12.   `public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {`
    13.       `request.setCharacterEncoding("utf-8");`
    14.       `response.setCharacterEncoding("utf-8");`
    15.       `response.setContentType("text/html;charset=utf-8");`
    16.       `System.out.println("CharaterEncodingFilter执行前");`
    17.       `// 让我们的请求继续走，如果不写，程序到这里就被拦截停止`
    18.       `chain.doFilter(request,response);`
    19.       `System.out.println("CharaterEncodingFilter执行后");`
    20.   `}`
    21.   `/**`
    22.    `* 销毁：web服务器关闭的时候，过滤会被销毁`
    23.    `*/`
    24.   `[@Override](https://github.com/Override "@Override")`
    25.   `public void destroy() {`
    26.       `System.out.println("CharaterEncodingFilter销毁");`
    27.   `}`
    28.  `}`

    ![](https://kuangstudy.oss-cn-beijing.aliyuncs.com/bbs/2022/07/12/kuangstudyec776842-89f3-4f5e-8d4a-0777548366fb.jpg)
3.  在web.xml中配置Filter

    1.   `<filter>`
    2.       `<filter-name>filter</filter-name>`
    3.       `<filter-class>com.sunyiwenlong.filter.CharaterEncodingFilter</filter-class>`
    4.   `</filter>`
    5.   `<filter-mapping>`
    6.       `<filter-name>filter</filter-name>`
    7.       `<！--只要是/servlet的任何请求，会经过这个过滤器-->`
    8.       `<url-pattern>/servlet/*</url-pattern>`
    9.   `</filter-mapping>`


# 11.监听器

实现一个监听器的接口；（有N种监听器）

1.  编写一个监听器

    1.  `public class OnlineCountListener implements HttpSessionListener {`
    2.   `/**`
    3.    `* 创建session的监听：创建一个session就会触发一次这个事件。`
    4.    `* [@param](https://github.com/param "@param") se`
    5.    `*/`
    6.   `[@Override](https://github.com/Override "@Override")`
    7.   `public void sessionCreated(HttpSessionEvent se) {`
    8.       `ServletContext context = se.getSession().getServletContext();`
    9.       `Integer onlineCount = (Integer) context.getAttribute("OnlineCount");`
    10.       `if (onlineCount==null){`
    11.           `onlineCount = new Integer(1);`
    12.       `}else {`
    13.           `int count = onlineCount.intValue();`
    14.           `onlineCount = new Integer(count+1);`
    15.       `}`
    16.       `context.setAttribute("OnlineCount",onlineCount);`
    17.   `}`

    19.   `/**`
    20.    `* 销毁session监听`
    21.    `* 销毁Session就会触发一次这个事件！`
    22.    `* [@param](https://github.com/param "@param") se`
    23.    `*/`
    24.   `[@Override](https://github.com/Override "@Override")`
    25.   `public void sessionDestroyed(HttpSessionEvent se) {`
    26.       `ServletContext context = se.getSession().getServletContext();`
    27.       `Integer onlineCount = (Integer) context.getAttribute("OnlineCount");`
    28.       `if (onlineCount==null){`
    29.           `onlineCount = new Integer(0);`
    30.       `}else {`
    31.           `int count = onlineCount.intValue();`
    32.           `onlineCount = new Integer(count-1);`
    33.       `}`
    34.       `context.setAttribute("OnlineCount",onlineCount);`
    35.   `}`
    36.  `}`

2.  在web.xml中注册监听器

    1.   `<listener>`
    2.       `<listener-class>com.sunyiwenlong.listener.OnlineCountListener</listener-class>`
    3.   `</listener>`

3.  看情况是否使用

# 12. 过滤器、监听器常见应用

1.  `public class TestPanel {`
2.      `public static void main(String[] args) {`
3.          `// 新建一个窗口`
4.          `Frame frame = new Frame("中秋节快乐");`
5.          `frame.setLayout(null);// 设置窗口的布局`
6.          `// 新建一个面板`
7.          `Panel panel = new Panel(null);`
8.          `// 设置窗口的坐标长宽`
9.          `frame.setBounds(300,300,500,500);`
10.          `// 设置背景颜色`
11.          `frame.setBackground(new Color(0,0,255));`
12.          `// 设置面板的坐标长宽`
13.          `panel.setBounds(50,50,300,300);`
14.          `// 设置面板背景色`
15.          `panel.setBackground(new Color(0,255,0));`
16.          `// 把面板放到窗口中`
17.          `frame.add(panel);`
18.          `// 设置可见性 true`
19.          `frame.setVisible(true);`
21.          `// 监听事件，监听关闭事件`
22.          `frame.addWindowListener(new WindowAdapter() {`
23.              `@Override`
24.              `public void windowClosing(WindowEvent e) {`
25.                  `System.exit(0);`
26.              `}`
27.          `});`
28.      `}`
29.  `}`

# 总结

提示：这里对文章进行总结：
例如：以上就是今天要讲的内容，本文仅仅简单介绍了pandas的使用，而pandas提供了大量能使我们快速便捷地处理数据的函数和方法。

标签：_JavaWeb_

版权声明：本文为博主原创文章，遵循[CC 4.0 BY-SA](https://creativecommons.org/licenses/by-sa/4.0/)版权协议,转载请附上原文出处链接和本声明，KuangStudy,以学为伴，一生相伴！

[本文链接：https://www.kuangstudy.com/bbs/1544669921622335489](https://www.kuangstudy.com/bbs/1544669921622335489)
