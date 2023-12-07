

## 什么是 nginx



**什么是nginx**

nginx是一个高性能的反向代理服务器。



**nginx 的作用**

- 解决跨域
- 请求过滤
- 配置gzip
- 负载均衡
- 静态资源服务器



## 正向代理和反向代理

### 正向代理

**正向代理**，意思是一个位于客户端和原始服务器(origin server)之间的服务器，为了从原始服务器取得内容，客户端向代理发送一个请求并指定目标(原始服务器)，然后代理向原始服务器转交请求并将获得的内容返回给客户端。

**正向代理**是为我们服务的，即为客户端服务的，客户端可以根据正向代理访问到它本身无法访问到的服务器资源。

**正向代理**对我们是透明的，对服务端是非透明的，即服务端并不知道自己收到的是来自代理的访问还是来自真实客户端的访问

![image-20220316153610544](正向代理.png)

### 反向代理

**反向代理**（Reverse Proxy）方式是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。

**反向代理**是为服务端服务的，反向代理可以帮助服务器接收来自客户端的请求，帮助服务器做请求转发，负载均衡等。

**反向代理**对服务端是透明的，对我们是非透明的，即我们并不知道自己访问的是代理服务器，而服务器知道反向代理在为他服务。

## ![image-20220316153828592](反向代理.png)



## 基本配置

- `main`:nginx的全局配置，对全局生效。
- `events`:配置影响nginx服务器或与用户的网络连接。
- `http`：可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置。
- `server`：配置虚拟主机的相关参数，一个http中可以有多个server。
- `location`：配置请求的路由，以及各种页面的处理情况。
- `upstream`：配置后端服务器具体地址，负载均衡配置不可或缺的部分。

```
events { 

}

http 
{
    server
    { 
        location path
        {
            ...
        }
        location path
        {
            ...
        }
     }

    server
    {
        ...
    }

}
```



**内置变量**

| 变量名             | 功能                                                         |
| ------------------ | ------------------------------------------------------------ |
| `$host`            | 请求信息中的`Host`，如果请求中没有`Host`行，则等于设置的服务器名 |
| `$request_method`  | 客户端请求类型，如`GET`、`POST`                              |
| `$remote_addr`     | 客户端的`IP`地址                                             |
| `$args`            | 请求中的参数                                                 |
| `$content_length`  | 请求头中的`Content-length`字段                               |
| `$http_user_agent` | 客户端agent信息                                              |
| `$http_cookie`     | 客户端cookie信息                                             |
| `$remote_addr`     | 客户端的IP地址                                               |
| `$remote_port`     | 客户端的端口                                                 |
| `$server_protocol` | 请求使用的协议，如`HTTP/1.0`、·HTTP/1.1`                     |
| `$server_addr`     | 服务器地址                                                   |
| `$server_name`     | 服务器名称                                                   |
| `$server_port`     | 服务器的端口号                                               |



## nginx 跨域

例如：

- 前端server的域名为：`fe.server.com`
- 后端服务的域名为：`dev.server.com`

现在我在`fe.server.com`对`dev.server.com`发起请求一定会出现跨域。

现在我们只需要启动一个nginx服务器，将`server_name`设置为`fe.server.com`,然后设置相应的location以拦截前端需要跨域的请求，最后将请求代理回`dev.server.com`。如下面的配置：

```text
server {
        listen       80;
        server_name  fe.server.com;
        location / {
                proxy_pass dev.server.com;
        }
}
```

这样可以完美绕过浏览器的同源策略：`fe.server.com`访问`nginx`的`fe.server.com` 属于同源访问，而`nginx`对服务端转发的请求不会触发浏览器的同源策略。



## 请求过滤

根据状态码过滤

```text
error_page 500 501 502 503 504 506 /50x.html;
    location = /50x.html {
        #将跟路径改编为存放html的路径。
        root /root/static/html;
    }
```

根据URL名称过滤，精准匹配URL，不匹配的URL全部重定向到主页。

```text
location / {
    rewrite  ^.*$ /index.html  redirect;
}
```

根据请求类型过滤。

```text
if ( $request_method !~ ^(GET|POST|HEAD)$ ) {
        return 403;
    }
```



## 配置 gzip 

`GZIP`是规定的三种标准HTTP压缩格式之一。目前绝大多数的网站都在使用`GZIP`传输 `HTML`、`CSS`、`JavaScript` 等资源文件。



```text
 gzip                    on;
    gzip_http_version       1.1;        
    gzip_comp_level         5;
    gzip_min_length         1000;
    gzip_types text/csv text/xml text/css text/plain text/javascript application/javascript application/x-javascript application/json application/xml;
```



#### gzip

- 开启或者关闭`gzip`模块
- 默认值为`off`
- 可配置为`on / off`

#### gzip_http_version

- 启用 `GZip` 所需的`HTTP` 最低版本
- 默认值为`HTTP/1.1`

这里为什么默认版本不是`1.0`呢？

`HTTP` 运行在`TCP` 连接之上，自然也有着跟`TCP` 一样的三次握手、慢启动等特性。

启用持久连接情况下，服务器发出响应后让`TCP`连接继续打开着。同一对客户/服务器之间的后续请求和响应可以通过这个连接发送。

![image](https://lsqimg-1257917459.cos-website.ap-beijing.myqcloud.com/blog/keepalive.png)

为了尽可能的提高 `HTTP` 性能，使用持久连接就显得尤为重要了。

`HTTP/1.1`默认支持`TCP`持久连接，`HTTP/1.0` 也可以通过显式指定 `Connection: keep-alive` 来启用持久连接。对于`TCP`持久连接上的`HTTP` 报文，客户端需要一种机制来准确判断结束位置，而在 `HTTP/1.0`中，这种机制只有`Content-Length`。而在`HTTP/1.1`中新增的 `Transfer-Encoding: chunked` 所对应的分块传输机制可以完美解决这类问题。

`nginx`同样有着配置`chunked的`属性`chunked_transfer_encoding`，这个属性是默认开启的。

`Nginx`在启用了`GZip`的情况下，不会等文件 `GZip` 完成再返回响应，而是边压缩边响应，这样可以显著提高 `TTFB`(`Time To First Byte`，首字节时间，WEB 性能优化重要指标)。这样唯一的问题是，`Nginx` 开始返回响应时，它无法知道将要传输的文件最终有多大，也就是无法给出`Content-Length`这个响应头部。

所以，在`HTTP1.0`中如果利用`Nginx`启用了`GZip`，是无法获得`Content-Length`的，这导致HTTP1.0中开启持久链接和使用`GZip`只能二选一，所以在这里`gzip_http_version`默认设置为`1.1`。

#### gzip_comp_level

- 压缩级别，级别越高压缩率越大，当然压缩时间也就越长（传输快但比较消耗cpu）。
- 默认值为 `1`
- 压缩级别取值为`1-9`

#### gzip_min_length

- 设置允许压缩的页面最小字节数，`Content-Length`小于该值的请求将不会被压缩
- 默认值:`0`
- 当设置的值较小时，压缩后的长度可能比原文件大，建议设置`1000`以上

#### gzip_types

- 要采用gzip压缩的文件类型(`MIME`类型)
- 默认值:`text/html`(默认不压缩`js`/`css`)





## 负载均衡

Upstream指定后端服务器地址列表

```text
upstream balanceServer {
    server 10.1.22.33:12345;
    server 10.1.22.34:12345;
    server 10.1.22.35:12345;
}
```

在server中拦截响应请求，并将请求转发到Upstream中配置的服务器列表。

```text
    server {
        server_name  fe.server.com;
        listen 80;
        location /api {
            proxy_pass http://balanceServer;
        }
    }
```

上面的配置只是指定了nginx需要转发的服务端列表，并没有指定分配策略。



### nginx实现负载均衡的策略



**轮询策略**

默认情况下采用的策略，将所有客户端请求轮询分配给服务端。这种策略是可以正常工作的，但是如果其中某一台服务器压力太大，出现延迟，会影响所有分配在这台服务器下的用户。

```text
upstream balanceServer {
    server 10.1.22.33:12345;
    server 10.1.22.34:12345;
    server 10.1.22.35:12345;
}
```



**最小连接数策略**

将请求优先分配给压力较小的服务器，它可以平衡每个队列的长度，并避免向压力大的服务器添加更多的请求。

```text
upstream balanceServer {
    least_conn;
    server 10.1.22.33:12345;
    server 10.1.22.34:12345;
    server 10.1.22.35:12345;
}
```



**最快响应时间策略**

依赖于NGINX Plus，优先分配给响应时间最短的服务器。

```text
upstream balanceServer {
    fair;
    server 10.1.22.33:12345;
    server 10.1.22.34:12345;
    server 10.1.22.35:12345;
}
```

**客户端ip绑定**

来自同一个ip的请求永远只分配一台服务器，有效解决了动态网页存在的session共享问题。

```text
upstream balanceServer {
    ip_hash;
    server 10.1.22.33:12345;
    server 10.1.22.34:12345;
    server 10.1.22.35:12345;
}
```



## 静态资源服务器

```text
location ~* \.(png|gif|jpg|jpeg)$ {
    root    /root/static/;  
    autoindex on;
    access_log  off;
    expires     10h;# 设置过期时间为10小时          
}
```

匹配以`png|gif|jpg|jpeg`为结尾的请求，并将请求转发到本地路径，`root`中指定的路径即nginx本地路径。同时也可以进行一些缓存的设置。







> 参考链接：[前端开发者必备的nginx知识 | ConardLi的blog](http://www.conardli.top/blog/article/前端工程化/前端开发者必备的nginx知识.html#配置gzip)



























