# 图书管理系统

## **项目结构概览**​

```bash
src/
├── main/
│   ├── java/
│   │   └── com/example/library/
│   │       ├── entity/       # 实体类（类似 TS 的 interface）
│   │       ├── mapper/       # 数据库操作（类似 API Service）
│   │       ├── service/      # 业务逻辑层
│   │       └── controller/   # 控制器（路由）
│   └── resources/
│       ├── application.yml   # 配置文件
│       └── mapper/           # MyBatis 的 SQL 映射文件
```

---
