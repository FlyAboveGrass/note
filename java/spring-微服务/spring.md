# 数据库

创建容器启动 mysql。
密码： 123456
```
docker run -d --name my-mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -v ~/mysql/data:/var/lib/mysql  -v ~/mysql/conf:/etc/mysql/conf.d mysql:latest
```
