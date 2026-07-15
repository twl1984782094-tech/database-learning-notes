# MySQL数据库的使用以及打开方式
## 一、数据库的使用
1.使用navicate(启动小皮)，直接点击鼠标右键来创立新的数据库，底层逻辑还是电脑帮我们生成一段代码
2.
- 打开navicate的安装路径，找到数据库并打开bin文件，在上方的路径框中直接输入cmd
![[mysql-bin文件夹.png]]
- 输入mysql -u 用户名 -p
- 输入密码
- ![[mysql命令行登录.png]]
## 二、数据库的操作
### 创建数据库
1.使用DDL语言创建
```
  create database 数据库名 default character set utf8;
```
2.使用navicate创建
![[navicat新建数据库.png|182]]
### 删除数据库
1.使用命令窗格删除数据库
```
drop database 数据库名;
```
2.navicate删除
![[navicat数据库操作菜单.png|319]]
### 使用数据库
```
USE 数据库名
```
## 三.表的操作
### ==创建表==
1.使用DDL语言创建
```
create table 表名(列名 类型，列名 类型.......);
```
![[mysql创建数据表命令.png]]

```
查看已创建的表
show tables;
```
![[mysql查看数据表命令.png]]
2.使用navicate
![[navicat新建数据表菜单.png|296]]
### ==删除表==
1.使用DDL语言删除
```
drop table 表名;
```
2.navicate
![[navicat新建数据表确认.png]]
### ==修改表名==
1.使用DDL语言修改
```
alter table 旧表名 rename 新表名;
```
2.使用navicate，**选中表按住f2**
![[navicat学生表.png|438]]
### ==修改列名==
1.使用DDL语言修改
```
alter table 表名 change column 旧列名 新列名 类型;
```
2.使用navicate
![[navicat数据表字段设计.png|508]]
### ==修改列类型==
1.使用DDL语言修改
```
alter table 表名 modify 列名 新类型;
```
2.使用navicate
![[navicat字段定义.png|521]]
### ==添加新的列==
1.使用DDL语言修改
```
alter table 表名 add column 新列名 类型;
```
2.使用navicate，向下键
![[navicat学生表数据.png]]
### ==删除指定的列==
1.使用DDL语言修改
```
alter table 表名 drop column 列名;
```
2.使用navicate，鼠标删除
![[navicat字段属性.png|533]]
## 三、MySQL中的约束
![[数据库关系模型.png]]
- 主键约束(Primary Key) PK 主键约束是使用最频繁的约束。在设计数据表时，一般情况下，都会要求表中设置一个主键。主键是表的一个特殊字段，该字段能唯一标识该表中的每条信息。例如，学生信息表中的学号是唯一的。
- 外键约束(Foreign Key) FK 外键约束经常和主键约束一起使用，用来确保数据的一致性。
- 唯一性约束(Unique) 唯一约束与主键约束有一个相似的地方，就是它们都能够确 保列的唯一性。与主键约束不同的是，唯一约束在一个表中 可以有多个，并且设置唯一约束的列是允许有空值的。
- 非空约束(Not Null) 非空约束用来约束表中的字段不能为空。
- 检查约束(Check) 检查约束也叫用户自定义约束，是用来检查数据表中，字段值是否有效的一个手段，但目前 MySQL 数据库不支持检查约束。
### ==添加主键约束==
1.使用DDL语句添加主键约束
```
alter table 表名 add primary key(列名);
```
2.主键自增长
```
alter table 表名 modify 主键 类型 auto_increment;
```
3.使用navicate
![[navicat表设计保存.png|430]]
### ==删除主键==
使用DDL语句删除主键 
```
ALTER TABLE 表名 DROP PRIMARY KEY;
```
注意： 删除主键时，如果主键列具备自动增长能力，需要先去掉自 动增长，然后在删除主键。
### ==添加外键约束==
使用DDL语句添加外键约束
```
alter table 表名 add constraint 约束名 foreing key(列名) refernces 参照的表名(参照的列名);
```
### ==删除外键约束==
使用DDL语句删除外键约束
```
alter table 表名 drop foreign key 约束名;
```
### ==添加唯一性约束==
使用DDL语句提娜佳唯一性约束

```
alter tbale 表名 add constraint 约束名 unique(列名);
```
### ==删除唯一性约束==
使用DDL删除唯一性约束
```
alter table 表名 drop key 约束名;
```
### ==修改表添加非空约束==
使用DDL语句添加非空约束
```
alter table 表名 modify 列名 类型 not null;
```
### ==删除非空约束==
使用DDL语句删除非空约束
```
alter table 表名 modify 列名 类型 null;
```
### ==创建表时添加约束==
查询表中的约束信息
```
show keys from 表名;
```
### ==添加数据==
选择插入
```
insert into 表名(列名1，列名2.....)
values("值1"，"值2"......);
```
完全插入
```
insert into 表名 values (值1，值2......);
如果主键是自动增长，需要用default 或者 null 或者 0 占位
```
### ==创建表时制动列的默认值==
```
create table 表名(列名 类型 default 默认值，.....)
```
### ==修改表添加新列并指定默认值==
```
alter table 表名 add column 列名 类型 default 默认值;
```
### ==更新数据==
```
update 表名 set 列名=值，列名=值 where 条件;
```
### ==删除数据==
```
delete from 表名 where 条件；
```
### ==truncate清空表==
```
truncate table 表名;
```

## 四、添加数据（INSERT）

`INSERT` 是 DML（数据操作语言），用于向数据表新增记录。插入前先确认当前使用的数据库和表结构：

```sql
USE 数据库名;
DESC 表名;
```

### 1. 指定列插入（推荐）

只为指定的列提供值，未写出的列会使用默认值或 `NULL`。这种写法不依赖列的排列顺序，最安全。

```sql
INSERT INTO student (name, age, gender)
VALUES ('张三', 18, '男');
```

注意：

- 字符串、日期通常用单引号，例如 `'张三'`、`'2026-07-12'`。
- 数值一般不加引号，例如 `18`。
- 列名与值必须一一对应，顺序也必须一致。
- 必填列（`NOT NULL`）不能遗漏，除非它有默认值。

### 2. 插入全部列

不写列名时，必须按照建表时的列顺序为每一列提供值，不推荐在实际项目中频繁使用。

```sql
INSERT INTO student
VALUES (1, '李四', 19, '女');
```

如果主键是 `AUTO_INCREMENT` 自增列，可以用 `DEFAULT` 或 `NULL` 占位：

```sql
INSERT INTO student
VALUES (DEFAULT, '王五', 20, '男');

-- 也可以省略自增主键列（推荐）
INSERT INTO student (name, age, gender)
VALUES ('王五', 20, '男');
```

### 3. 一次插入多条数据

多条数据使用一个 `INSERT` 语句完成，效率高于逐条执行。

```sql
INSERT INTO student (name, age, gender)
VALUES
  ('赵六', 18, '女'),
  ('孙七', 21, '男'),
  ('周八', 20, '女');
```

### 4. 使用默认值和空值

假设建表时定义了默认值：

```sql
CREATE TABLE student (
  id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(20) NOT NULL,
  age INT DEFAULT 18,
  gender VARCHAR(2)
);
```

插入时：

```sql
-- age 使用默认值 18
INSERT INTO student (name, gender)
VALUES ('陈晨', '女');

-- 明确使用默认值
INSERT INTO student (name, age, gender)
VALUES ('林林', DEFAULT, '男');

-- 只有允许为空的列才可以插入 NULL
INSERT INTO student (name, age, gender)
VALUES ('杨洋', 19, NULL);
```

> `NULL` 表示“未知或没有值”，它不是空字符串 `''`，也不能用 `= NULL` 判断；查询空值要使用 `IS NULL`。

### 5. 插入后验证

```sql
-- 查看全部数据
SELECT * FROM student;

-- 只查看需要的列
SELECT id, name, age, gender FROM student;

-- 查看刚插入的指定学生
SELECT * FROM student WHERE name = '张三';
```

### 6. 常见错误

- **列数和值数不一致**：列名有 3 个，`VALUES` 中也必须有 3 个值。
- **主键重复**：主键或 `UNIQUE` 列不能插入已有值。
- **遗漏非空列**：`NOT NULL` 列既没有值也没有默认值时会报错。
- **类型不合适**：例如向 `INT` 列插入无法转换的文本。
- **没有写列名导致错位**：表结构修改后，`INSERT INTO 表名 VALUES (...)` 很容易将数据放错列。

### 7. 练习

```sql
-- 1. 向 student 表插入一名学生：姓名“小明”、年龄 18、性别“男”
INSERT INTO student (name, age, gender)
VALUES ('小明', 18, '男');

-- 2. 一次插入两名学生
INSERT INTO student (name, age, gender)
VALUES
  ('小红', 19, '女'),
  ('小刚', 20, '男');

-- 3. 查询 student 表，检查插入结果
SELECT * FROM student;
```

### 8. 本节要点

- 优先使用“指定列名”的 `INSERT INTO 表名 (列...) VALUES (值...)`。
- 自增主键通常不手动填写。
- 多条记录可以写在同一条 `INSERT` 中。
- 插入后用 `SELECT` 检查结果。