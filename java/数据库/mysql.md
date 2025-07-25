# 数据库

## 起步

- docker 下载 mysql

```bash
docker pull mysql:latest  # 拉取最新版[6,10](@ref)
# 或指定版本（如 MySQL 5.7）：
docker pull mysql:5.7[3,8](@ref)
```

- 创建容器启动 mysql
	-  密码： 123456
```
docker run -d --name my-mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -v ~/mysql/data:/var/lib/mysql  -v ~/mysql/conf:/etc/mysql/conf.d mysql:latest
```

- 重新进入
```
docker exec -it my-mysql mysql -uroot -p
// 密码 123456
```

## 图书管理系统表

### 📚 ​**核心表结构设计（6张表）​**​

#### 1. ​**用户表（`user`）​**​ - 支持多角色和状态管理

```sql
CREATE TABLE `user` (
  `user_id` INT AUTO_INCREMENT PRIMARY KEY COMMENT '用户唯一ID',
  `name` VARCHAR(50) NOT NULL COMMENT '姓名',
  `email` VARCHAR(100) UNIQUE NOT NULL COMMENT '邮箱（登录账号）',
  `password` VARCHAR(100) NOT NULL COMMENT '加密密码（SHA-256）',
  `phone` VARCHAR(20) COMMENT '联系方式',
  `role` ENUM('USER', 'ADMIN', 'LIBRARIAN') DEFAULT 'USER' COMMENT '角色',
  `status` ENUM('ACTIVE', 'INACTIVE', 'SUSPENDED', 'INVALID') DEFAULT 'INACTIVE' COMMENT '账号状态',
  `created_at` TIMESTAMP DEFAULT CURRENT_TIMESTAMP COMMENT '注册时间'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

> ​**扩展设计**​：
>
> - 角色区分普通用户、管理员、图书馆员（权限分层）
>
> - `status`字段支持冻结账号等操作需求

---

#### 2. ​**图书表（`book`）​**​ - 支持库存动态管理

```sql
CREATE TABLE `book` (
  `book_id` INT AUTO_INCREMENT PRIMARY KEY COMMENT '图书ID',
  `isbn` VARCHAR(13) UNIQUE NOT NULL COMMENT 'ISBN号',
  `title` VARCHAR(200) NOT NULL COMMENT '书名',
  `author` VARCHAR(100) NOT NULL COMMENT '作者',
  `publisher` VARCHAR(100) COMMENT '出版社',
  `publish_date` DATE COMMENT '出版日期',
  `category_id` INT COMMENT '分类ID（外键预留）',
  `total_copies` INT DEFAULT 1 COMMENT '总副本数',
  `available_copies` INT DEFAULT 1 COMMENT '可用副本数',
  `status` ENUM('AVAILABLE', 'BORROWED', 'MAINTENANCE', 'LOST') DEFAULT 'AVAILABLE' COMMENT '状态',
  `location` VARCHAR(50) COMMENT '书架位置',
  `library_id` INT NOT NULL COMMENT '所属图书馆（外键）'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

> ​**关键优化**​：
>
> - 动态库存：`available_copies`实时更新，避免超借
>
> - 状态机：覆盖丢失、维护等场景

---

#### 3. ​**借阅记录表（`borrow_record`）​**​ - 支持续借与罚金计算

```sql
CREATE TABLE `borrow_record` (
  `borrow_id` INT AUTO_INCREMENT PRIMARY KEY COMMENT '借阅ID',
  `user_id` INT NOT NULL COMMENT '借阅用户ID',
  `book_id` INT NOT NULL COMMENT '借阅图书ID',
  `borrow_date` DATE NOT NULL COMMENT '借出日期',
  `due_date` DATE NOT NULL COMMENT '应还日期',
  `return_date` DATE COMMENT '实际归还日期',
  `renew_count` TINYINT DEFAULT 0 COMMENT '续借次数（≤2）',
  `fine_amount` DECIMAL(10,2) DEFAULT 0.0 COMMENT '罚金',
  `status` ENUM('ACTIVE', 'RETURNED', 'OVERDUE') DEFAULT 'ACTIVE' COMMENT '记录状态',
  FOREIGN KEY (`user_id`) REFERENCES `user`(`user_id`) ON DELETE CASCADE,
  FOREIGN KEY (`book_id`) REFERENCES `book`(`book_id`) ON DELETE CASCADE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

> ​**业务规则**​：
>
> - 自动罚金：通过定时任务计算超期费用（`due_date < CURDATE()`时更新）
>
> - 续借限制：`renew_count`控制最多续借2次

---

#### 4. ​**座位表（`seat`）​**​ - 支持多类型座位与状态跟踪

```sql
CREATE TABLE `seat` (
  `seat_id` INT AUTO_INCREMENT PRIMARY KEY COMMENT '座位ID',
  `code` VARCHAR(10) NOT NULL COMMENT '座位编号（如2F-A01）',
  `type` ENUM('STANDARD', 'QUIET_ROOM', 'DISCUSSION_ROOM') DEFAULT 'STANDARD' COMMENT '类型',
  `status` ENUM('AVAILABLE', 'OCCUPIED', 'MAINTENANCE') DEFAULT 'AVAILABLE' COMMENT '实时状态',
  `library_id` INT NOT NULL COMMENT '所属图书馆（外键）',
  `description` VARCHAR(200) COMMENT '座位描述（如电源位置）'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

-- 初始化默认座位数据
INSERT INTO `seat` (`code`, `library_id`, `type`) 
VALUES ('1F-A01', 1, 'STANDARD'), ('2F-B01', 1, 'QUIET_ROOM');
```

---

#### 5. ​**图书馆表（`library`）​**​ - 动态统计座位与开放状态

```sql
CREATE TABLE `library` (
  `library_id` INT AUTO_INCREMENT PRIMARY KEY COMMENT '图书馆ID',
  `name` VARCHAR(100) NOT NULL COMMENT '名称',
  `address` VARCHAR(200) COMMENT '地址',
  `open_status` ENUM('OPEN', 'CLOSED') DEFAULT 'OPEN' COMMENT '开放状态',
  `open_time` TIME DEFAULT '08:00:00' COMMENT '开放时间',
  `close_time` TIME DEFAULT '22:00:00' COMMENT '关闭时间',
  `total_seats` INT DEFAULT 0 COMMENT '总座位数',
  `available_seats` INT DEFAULT 0 COMMENT '可用座位数（动态更新）',
  `max_capacity` INT COMMENT '最大承载人数（安全限制）'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

> ​**联动设计**​：
>
> - 触发器自动更新`available_seats`（当`seat.status`变化时）
>
> - `max_capacity`支持人流管控（扩展安全需求）

---

#### 6. ​**扩展预留表（`category`）​**​ - 支持图书分类扩展

```sql
CREATE TABLE `category` (
  `category_id` INT AUTO_INCREMENT PRIMARY KEY COMMENT '分类ID',
  `name` VARCHAR(50) NOT NULL COMMENT '分类名（如计算机/文学）',
  `parent_id` INT DEFAULT 0 COMMENT '父分类ID（树形结构）'
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
```

---

### ⚙️ ​**扩展性设计**​

1. ​**外键关联预留**​

    - `book.category_id` → `category.category_id`（未来扩展分类体系）
    - `book.library_id`/`seat.library_id` → `library.library_id`（多图书馆支持）

2. ​**状态字段统一规范**​

    - 使用`ENUM`定义有限状态（如借阅状态、座位状态），避免无效值
    - 预留`description`字段（如座位表）存储扩展信息
3. ​**动态计数优化**​

    - 图书可用副本数（`book.available_copies`）
    - 图书馆实时座位数（`library.available_seats`）
        → 通过触发器或应用层逻辑同步更新
4. ​**索引加速高频查询**​

    ```sql
    CREATE INDEX idx_book_title ON book(title);
    CREATE INDEX idx_user_email ON user(email);
    CREATE INDEX idx_borrow_user ON borrow_record(user_id, status);
    ```
