# SQL 学习笔记

[toc]

## 基本概念

- **数据库：** 一种专门管理数据的软件。
- **SQL：** Structured Query Language，结构化查询语言，用来访问和操作数据库系统的一种语言。基本所有数据库都支持 SQL。
  - **方言：** 虽然有 SQL 标准，但是不同的数据库有不同版本的 SQL，称为「方言」，其间并不完全兼容。
  - **三种类型操作：**
    - **DDL（Data Definition Language）：** 数据定义（创建表、删除表、修改表结构等）。通常由数据库管理员执行。
    - **DML（Data Manipulation Language）：** 数据维护（添加、删除、更新），一般由应用程序执行。
    - **DQL（Data Query Language）：**数据查询，一般由应用程序执行。
- **语法：** SQL 语言关键字不区分大小写。但是大小写规则非常混乱：针对不同的数据库，对于表名和列名，有的数据库区分大小写，有的数据库不区分大小写。同一个数据库，有的在 Linux 上区分大小写，在 Windows 上不区分大小写。

### MySQL

- MySQL 是目前应用最广泛的开源关系数据库。
- **历史：** MySQL 最早是由瑞典的 MySQL AB 公司开发，该公司在 2008 年被 SUN 公司收购，SUN 公司在 2009 年被 Oracle 公司收购，所以 MySQL 最终就变成了 Oracle 旗下的产品。
- **数据引擎：** MySQL 本身实际上只是一个 SQL 接口，它的内部还包含了多种数据引擎。
  - **InnoDB：** 由 Innobase Oy 公司开发的一款支持事务的数据库引擎，2006 年被 Oracle 收购。
  - **MyISAM：** MySQL 早期集成的默认数据库引擎，不支持事务。
- **衍生版本：**
  - **MariaDB：** MySQL 最早的创始人出走开发了一个分支 MariaDB，使用 XtraDB 引擎。宣称完全兼容 MySQL 且性能更强。
  - ……

## 关系模型

- 关系模型是若干个存储数据的二维表。
- **记录（Record）：** 表的每一行称为记录，记录是一个逻辑意义上的数据。
- **字段（Column）：** 表的每一列称为字段，同一个表的每一行记录都拥有相同的若干字段。字段定义了数据类型、是否允许为`NULL`。
  - `NULL` 表示该字段不存在，并不是为 0 或者空字符串。通常情况下，字段应该避免允许为 `NULL`。不允许为 `NULL` 可以简化查询条件，加快查询速度，也减少判断。
- 基本操作：增 INSERT、删 DELETE、改 UPDATE、查 SELECT（CRUD：Create、Retrieve、Update、Delete）。

### 主键

- **主键：** 唯一区分不同字段的记录，在同一张表中不允许重复。
  - 主键的作用十分重要，如果修改会引起一系列麻烦。
  - 选取主键的一个基本原则是：不使用任何业务相关的字段作为主键。
- 通常将一个业务无关的字段 `id` 作为主键。
- 常见的主键类型：
  - **自增整数类型：** 数据库会在插入数据时自动为每一条记录分配一个自增整数。
    - 使用 INT 自增类型时，一张表记录数量不能超过 2147483647。使用 BIGINT 自增类型则最多可以存储约 922 亿亿条记录。
  - **全局唯一 GUID 类型：** 使用一种全局唯一的字符串作为主键，类似`8f55d96b-8acc-4636-8cb8-76bf8abc2f57`。GUID 算法通过网卡 MAC 地址、时间戳和随机数保证任意计算机在任意时间生成的字符串都是不同的。大部分编程语言都内置了GUID算法，可以自己预算出主键。
- **联合主键：** 允许多个字段都作为主键，通过多个字段唯一标识记录。
  - 允许一列重复，只要没有两条数据所有主键记录都相同即可。
  - 并不常用，因为会使数据库变复杂。

### 外键

- **外键：** 在一个表中，通过一个字段将记录与另一张表关联起来，这种字段称为外键。

  - 外键可以形成一对多、多对一、多对多关系。
  - 通过定义外键约束，关系数据库可以保证无法插入无效的数据。即如果 `classes` 表不存在 `id=99` 的记录，`students` 表就无法插入 `class_id=99` 的记录。

- **外键约束：** 设置外键约束来定义外键：

  ```sql
  ALTER TABLE students
  ADD CONSTRAINT fk_class_id
  FOREIGN KEY (class_id)
  REFERENCES classes (id);
  ```

  - `fk_class_id` 是外键的名称，可以任意。
  - `FOREIGN KEY (class_id) ` 指定了 `class_id` 作为外键。
  - `REFERENCES classes (id)` 指定了这个外键将关联到 `classes` 表的 `id` 列（即 `classes` 表的主键）。

- **删除外键约束：**

  ```sql
  ALTER TABLE students
  DROP FOREIGN KEY fk_class_id;
  ```

  这个操作不会删除 `fk_class_id` 这个字段，只会删除其作为外键的属性。

- **无约束外键：** 外键约束会降低数据库的性能，所以大部分互联网应用程序为了追求速度，并不设置外键约束，而是仅靠应用程序自身来保证逻辑的正确性。

- **多对多关系：** 通过一个中间表可以实现多对多关系。

- **一对一关系：** 除了特殊的逻辑性用途之外，将一个表拆为多个一对一的表可以增强查询性能。

### 索引

- **索引：** 关系数据库中对某一列或多个列的值进行预排序的数据结构。

  - **优化查询速度：** 通过使用索引，可以让数据库系统不必扫描整个表，而是直接定位到符合条件的记录。
  - **拖慢更新速度：** 插入、更新和删除记录时，需要同时修改索引，因此，索引越多，修改记录的速度就越慢。
  - **索引效率：** 索引的效率取决于索引列的值是否散列，即该列的值如果越互不相同，那么索引效率越高。如果记录的列存在大量相同的值，例如`gender`列，大约一半的记录值是`M`，另一半是`F`，对该列创建索引就没有意义。
  - **主键索引：** 关系数据库会自动对主键创建主键索引。使用主键索引的效率是最高的，因为主键会保证绝对唯一。

- **创建索引：**

  ```sql
  ALTER TABLE students
  ADD INDEX idx_score (score);
  ```

  - 使用 `ADD INDEX idx_score (score)` 创建一个名称为 `idx_score`，使用列 `score` 的索引。
  - 索引名称是任意的。
  - 创建多列索引： `ADD INDEX idx_name_score (name, score)`。

- **唯一索引：** 对于不允许重复的字段，可以设为唯一索引。

  ```sql
  ALTER TABLE students
  ADD UNIQUE INDEX uni_name (name);
  ```

  通过  `UNIQUE` 关键字创建唯一索引。也可以只添加唯一约束，不创建索引：

  ```sql
  ADD CONSTRAINT uni_name UNIQUE (name);
  ```

## 数据查询 SELECT

- **基本查询：** `SELECT * FROM <表名>`
  - 查询结果也是一个二维表，它包含列名和每一行的数据。
  - 可以不用 `FROM` 关键字。一般可以用于测试连接。

- **条件查询：** 使用 `WHERE` 关键字。例如 `SELECT * FROM <表名> WHERE <条件表达式>`。
  - 逻辑运算符：`NOT`，`AND`，`OR`。
    - 优先级：`NOT>AND>OR`，可以用小括号调整优先级。
  - 「不等于」表示为 `<>`。
  - 使用 `LIKE` 判断相似：例如 `name LIKE 'ab%'`，`%` 表示任意字符。

- **投影查询：** `SELECT <列1>,<列2> FROM ...` 使结果仅包含指定列。

  - 为列设定别名：`SELECT <列1> <别名1>, <列2> <别名2> FROM ...`，使得结果集中的列名以别名返回。

- **排序：** `ORDER BY` 关键字使返回集针对某个字段排序。
  - 在字段名后加上 `DESC` 使之降序。默认是升序（`ASC` 可以省略）
  - 可以多关键字。

- **分页：** `LIMIT <N> OFFSET <M>` 子句表示对结果集从 M 开始最多取 N 条记录。
  - MySQL 中，可以简写为 `LIMIT N, M`。
  - `OFFSET` 可以忽略，默认值为 0。
  - 第 i 页的 `OFFSET` 应该是 `每页显示数量 - (i-1)`。
  - N 越来越大，查询效率越来越低。

- **聚合查询：** SQL 提供了一些聚合函数进行聚合查询。
  - `COUNT()` 查询记录的数量。`COUNT(*)` 查询所有列的行数，实际上等价于 `COUNT(id)`。
    - 其返回值也是一个 1×1 的二维表，唯一的列名是 `COUNT(*)`。
    - 通常给列名设置一个别名方便处理：`SELECT COUNT(*) num FROM students;`。
    - 同时支持 `WHERE` 子句。如果 `WHERE` 没有匹配到行，会返回 0。
  - `SUM/AVG/MAX/MIN()`
    - 如果 `WHERE` 没有匹配到行，会返回 `NULL` 而不是 0。
    - `MAX/MIN()` 也支持对字符串操作。
  - **分组聚合： ** `COUNT(*) ... GROUP BY id`，将 id 列的相同值分组 COUNT，对每个 id 返回多个 COUNT 的结果。相当于更改 `WHERE` 条件多次聚合查询。
    - 可以设置多个分组条件。

- **多表查询（笛卡尔查询）：** `SELECT * FROM <表1> <表2>` 从多张表查询数据。

  - 返回两个表的「乘积」：两个表中的记录两两配对，产生 $n+m$ 个字段、$n*m$ 个记录。结果可能非常大，要谨慎使用。
  - 可以像列一样对表设置别名。
  - 用 `表名.列名` 来表示一个列，可以对其设置别名。

- **连接查询：** 根据某种关系跨表查询记录。

  - 内连接 INNER JOIN：

    ```sql
    SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
    FROM students s
    INNER JOIN classes c
    ON s.class_id = c.id;
    ```

    - 查询写法：主表 `FROM`、需要连接的表 `INNER JOIN`、连接条件 `ON`、其他子句。
    - 只返回同时存在于两张表的数据

  - 外连接 OUTER JOIN：

    - INNER JOIN 返回两表都存在的记录。
    - RIGHT OUTER JOIN 返回右表都存在的记录。左表不存在的，用 NULL 填充。
    - LEFT OUTER JOIN 返回左表都存在的记录。右表不存在的，用 NULL 填充。
    - FULL OUTER JOIN 返回两张表的所有记录。


## 数据插入 INSERT

- 基本语法：`INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);`
  - 可以一次增加多条记录：`INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1.1, 值2.1, ...), VALUES (值2.1, 值2.2, ...);`
- 值的给出顺序必须与字段给出顺序一致，但是字段给出顺序无需与表中字段顺序一致。
- 如果一个字段有默认值，则可以不用指定值。

## 数据更新 UPDATE

- 基本语法：`UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;`
- 如果没有找到数据，不会报错。MySQL 会返回修改的记录数。

## 数据删除 DELETE

- 基本语法：`DELETE FROM <表名> WHERE ...;`
- MySQL 中会返回删除的记录数。

## MySQL 管理命令

MySQL 中 `information_schema`、`mysql`、`performance_schema` 和 `sys` 是系统库，用户不宜更改。

- 断开连接：`EXIT`

### 数据库管理

- 显示所有数据库：`SHOW DATABASES;`
- 创建一个数据库：`CREATE DATABASE test;`
- 删除一个数据库：`DROP DATABASE test;`
- 切换到一个数据库：`USE test;`

### 表管理

- 列出表：`SHOW TABLES;`
- 查看表结构：`DESC students;`
- 查看创建表的语句：`SHOW CREATE TABLE students`
- 创建：`CREATE TABLE test;`
- 删除：`DROP TABLE test;`
- 修改表结构：`ALTER TABLE test;`
  - ADD 增添字段：`ALTER TABLE students ADD COLUMN birth VARCHAR(10) NOT NULL;`
  - DROP 删除字段：`ALTER TABLE students DROP COLUMN birthday;`
  - CHANGE 修改字段类型：`ALTER TABLE students CHANGE COLUMN birth birthday VARCHAR(20) NOT NULL;`

## 最佳实践

