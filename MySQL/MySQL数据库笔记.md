# MySQL 基础操作笔记（讲义同步）

> 对照课程讲义《Mysql.pdf》整理。当前进度：**MySQL 中 DML 操作 - 添加数据（INSERT）**。

## 一、数据库与 MySQL 基础

### 1. 基本概念

- **数据（Data）**：用于描述客观事物、可被识别的符号，如数字、文字、图片、音视频等。
- **数据库（Database）**：按一定结构组织、存储和管理数据的集合。
- **数据库管理系统（DBMS）**：定义和管理数据库的软件，例如 MySQL。
- **数据库应用系统（DBAS）**：基于 DBMS 开发、面向最终用户的应用程序。
- **数据库管理员（DBA）**：负责数据库运行、维护和权限管理的人员。

### 2. 数据库分类

- **关系型数据库**：使用二维表和表之间的关系组织数据，常用 SQL 操作；MySQL 属于此类。
- **非关系型数据库（NoSQL）**：常以键值、文档等形式存储数据，结构更灵活。

### 3. MySQL 简介

MySQL 是开源的关系型数据库管理系统，使用 SQL 进行数据的定义、查询和管理。

---

## 二、SQL 语言基础

### 1. SQL 分类

| 分类 | 全称 | 常用关键字 | 作用 |
| --- | --- | --- | --- |
| DQL | 数据查询语言 | `SELECT` | 查询数据 |
| DML | 数据操作语言 | `INSERT`、`UPDATE`、`DELETE` | 增加、修改、删除表中的数据 |
| DDL | 数据定义语言 | `CREATE`、`ALTER`、`DROP` | 创建、修改、删除数据库对象 |
| DCL | 数据控制语言 | `GRANT`、`REVOKE` | 管理用户权限 |
| TCL | 事务控制语言 | `COMMIT`、`ROLLBACK`、`SAVEPOINT` | 管理事务 |

### 2. SQL 书写规则

- SQL 关键字不区分大小写，但建议使用大写，例如 `SELECT`、`INSERT`。
- 一条 SQL 语句可以写在一行或多行，以英文分号 `;` 结尾。
- DML 面向表中的数据；DDL 面向数据库、表、索引等数据库对象。

---

## 三、连接与选择数据库

### 1. 通过 Navicat 操作

打开 phpStudy 后启动 MySQL，再用 Navicat 连接本地数据库。

![[phpstudy-mysql-bin目录.png]]

![[navicat新建数据库菜单.png]]

### 2. 通过命令行连接

进入 MySQL 的 `bin` 目录，在地址栏输入 `cmd` 打开命令行。

![[mysql-bin文件夹.png]]

```sql
mysql -u 用户名 -p
```

随后按提示输入密码；成功连接后会出现 `mysql>`。

![[mysql命令行登录.png]]

### 3. 常用命令

```sql
SHOW DATABASES;      -- 查看所有数据库
USE 数据库名;         -- 选择要操作的数据库
SELECT DATABASE();   -- 查看当前使用的数据库
SHOW TABLES;         -- 查看当前数据库中的表
DESC 表名;            -- 查看表结构
```

---

## 四、数据库操作（DDL）

### 1. 创建、查看和删除数据库

```sql
-- 讲义示例：创建 school_db 数据库
CREATE DATABASE school_db DEFAULT CHARACTER SET utf8;

SHOW DATABASES;

-- 查看指定数据库的字符集
SELECT schema_name, default_character_set_name
FROM information_schema.schemata
WHERE schema_name = 'school_db';

-- 选择数据库
USE school_db;

-- 删除数据库
DROP DATABASE school_db;
```

![[navicat新建数据库.png]]

![[navicat数据库操作菜单.png]]

> [!warning] 注意
> `DROP DATABASE` 会删除数据库中的全部表和数据，执行前务必确认名称。

---

## 五、数据类型与数据表操作（DDL）

### 1. 常用数据类型

| 类别 | 常用类型 | 示例 |
| --- | --- | --- |
| 整数 | `TINYINT`、`INT`、`BIGINT` | `employee_id INT` |
| 小数 | `FLOAT(M,D)`、`DOUBLE`、`DECIMAL(M,D)` | `salary FLOAT(8,2)` |
| 字符 | `CHAR(n)`、`VARCHAR(n)`、`TEXT` | `employee_name VARCHAR(10)` |
| 日期时间 | `DATE`、`DATETIME`、`TIMESTAMP` | `hire_date DATE` |
| 二进制 | `BLOB` | 图片、文件等二进制内容 |

### 2. 创建与查看数据表

```sql
CREATE TABLE employees (
  employee_id INT,
  employee_name VARCHAR(10),
  salary FLOAT(8,2)
);

SHOW TABLES;
DESC employees;
```

![[mysql创建数据表命令.png]]

![[mysql查看数据表命令.png]]

### 3. 使用 Navicat 创建表

设置字段名、数据类型、主键和自增后保存。

![[navicat新建数据表菜单.png]]

![[navicat数据表字段设计.png]]

![[navicat字段定义.png]]

### 4. 修改或删除数据表

```sql
-- 删除表
DROP TABLE employees;

-- 修改表名（与讲义写法一致）
ALTER TABLE employees RENAME emp;

-- 修改列名：必须同时写出新列名和类型
ALTER TABLE emp CHANGE COLUMN employee_name emp_name VARCHAR(20);

-- 修改列类型
ALTER TABLE emp MODIFY COLUMN salary FLOAT(10,2);

-- 添加新列
ALTER TABLE emp ADD COLUMN phone VARCHAR(20);

-- 删除指定列
ALTER TABLE emp DROP COLUMN phone;
```

![[navicat学生表.png]]

![[navicat学生表数据.png]]

![[navicat字段属性.png]]

---

## 六、MySQL 中的约束

约束用于保证表中数据的正确性、有效性和完整性。

![[数据库关系模型.png]]

| 约束 | 作用 |
| --- | --- |
| `PRIMARY KEY` | 唯一标识一条记录，不能重复、不能为 `NULL` |
| `FOREIGN KEY` | 与主键配合，保证表之间的数据一致性 |
| `UNIQUE` | 确保列值唯一；一个表可有多个唯一约束 |
| `NOT NULL` | 限制列值不能为空 |
| `DEFAULT` | 插入时未指定值则使用默认值 |
| `CHECK` | 检查列值是否满足条件 |

### 1. 主键与外键

```sql
-- 为 departments 表添加主键和自增
ALTER TABLE departments ADD PRIMARY KEY (department_id);
ALTER TABLE departments MODIFY department_id INT AUTO_INCREMENT;

-- 添加外键
ALTER TABLE employees
ADD CONSTRAINT fk_emp_dept
FOREIGN KEY (department_id) REFERENCES departments(department_id);

-- 删除外键
ALTER TABLE employees DROP FOREIGN KEY fk_emp_dept;
```

![[navicat表设计保存.png]]

### 2. 唯一、非空与默认值

```sql
CREATE TABLE depts (
  department_id INT PRIMARY KEY AUTO_INCREMENT,
  department_name VARCHAR(30) UNIQUE,
  location_id INT NOT NULL
);

SHOW KEYS FROM depts;
```

### 3. `CHECK` 约束提示

讲义使用的 MySQL 5.7 环境不实际执行 `CHECK` 约束，因此课程练习不要依赖它来校验数据。若使用 MySQL 8.0.16 及以上版本，`CHECK` 才会被执行。

---

## 七、MySQL 中的 DML 操作

DML 包括 `INSERT`、`UPDATE` 和 `DELETE`，它们操作的是**表中的数据**。

### 1. 添加数据（INSERT）

#### 选择插入

推荐明确写出列名，列名和值必须一一对应。

```sql
INSERT INTO departments (department_name, location_id)
VALUES ('market', 1);
```

#### 完全插入

不写列名时，必须按建表时的列顺序填写所有值。

```sql
INSERT INTO departments
VALUES (DEFAULT, 'development', 2);
```

如果主键是自动增长列，完全插入时可使用 `DEFAULT`、`NULL` 或 `0` 占位：

```sql
INSERT INTO departments VALUES (DEFAULT, 'development', 2);
INSERT INTO departments VALUES (NULL, 'human', 3);
INSERT INTO departments VALUES (0, 'teaching', 4);
```

> [!tip] 记忆
> 不写列名的“完全插入”容易因列顺序变化而出错，实际操作优先使用“选择插入”。

### 2. 默认值处理（DEFAULT）

```sql
-- 创建表时设置默认值
CREATE TABLE emp3 (
  emp_id INT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(10),
  address VARCHAR(50) DEFAULT 'Unknown'
);

-- 修改表时新增带默认值的列
ALTER TABLE emp3 ADD COLUMN job_id INT DEFAULT 0;

-- 未指定的列自动使用默认值
INSERT INTO emp3 (name) VALUES ('admin');

-- 完全插入时用 DEFAULT 占位
INSERT INTO emp3 VALUES (DEFAULT, 'oldlu', DEFAULT, DEFAULT);
```

### 3. 更新数据（UPDATE）

```sql
UPDATE emp3
SET address = 'BeiJing'
WHERE emp_id = 1;
```

> [!warning] 注意
> `UPDATE` 必须写 `WHERE` 条件；遗漏条件会更新整张表。

### 4. 删除与清空数据

```sql
-- 删除指定记录
DELETE FROM emp3
WHERE emp_id = 1;

-- 清空整张表的数据，保留表结构
TRUNCATE TABLE emp3;
```

| 对比 | `DELETE` | `TRUNCATE` |
| --- | --- | --- |
| 删除方式 | 逐行删除，可配合 `WHERE` | 整体清空，不能按条件删除 |
| 速度 | 通常较慢 | 通常较快 |
| 自增列 | 删除后通常继续累加 | 通常重置为初始值 |

> [!danger] 高风险操作
> `DELETE` 没有 `WHERE` 条件会删除表中所有数据；执行前先用同条件的 `SELECT` 确认范围。


---

## 十、MySQL 查询数据（SELECT）

`SELECT` 用于从表中返回数据。它可以选择需要的列、限制需要的行、计算表达式，并对结果排序。

### 1. 基本结构

```sql
SELECT 列名
FROM 表名;
```

最简单的查询必须包含 `SELECT` 子句和 `FROM` 子句。

```sql
-- 查询 employees 表中的所有列
SELECT *
FROM employees;

-- 查询指定列
SELECT employee_id, employee_name, salary
FROM employees;
```

> [!tip] 建议
> 学习时可以使用 `SELECT *` 快速查看整张表；实际编写业务查询时，优先明确写出需要的列名。

### 2. 列别名与算术表达式

```sql
-- 为查询结果列设置别名
SELECT employee_id, employee_name AS name
FROM employees;

-- 计算年薪
SELECT employee_id, employee_name, salary * 12 AS annual_salary
FROM employees;

-- 用括号改变运算顺序
SELECT employee_id, employee_name, (salary + 100) * 12 AS adjusted_annual_salary
FROM employees;
```

- 别名只改变查询结果中的列标题，不会修改表结构。
- `*`、`/` 的优先级高于 `+`、`-`；不确定时使用括号。
- 算术表达式中只要有一个值是 `NULL`，结果通常也是 `NULL`。

### 3. 去除重复值

```sql
-- 查询不重复的部门编号
SELECT DISTINCT department_id
FROM employees;
```

`DISTINCT` 放在 `SELECT` 后，用于去除结果中完全相同的行。

### 4. 使用 WHERE 筛选行

```sql
-- 查询部门编号为 90 的部门
SELECT department_name, location_id
FROM departments
WHERE department_id = 90;

-- 查询薪水大于等于 3000 的员工
SELECT employee_name, salary
FROM employees
WHERE salary >= 3000;
```

常用比较运算符：

| 运算符 | 含义 |
| --- | --- |
| `=` | 等于 |
| `>`、`>=` | 大于、大于等于 |
| `<`、`<=` | 小于、小于等于 |
| `<>`、`!=` | 不等于 |

### 5. 常用条件

```sql
-- 闭区间：包含 3000 和 8000
SELECT employee_id, employee_name, salary
FROM employees
WHERE salary BETWEEN 3000 AND 8000;

-- 属于给定集合
SELECT employee_id, employee_name, salary
FROM employees
WHERE salary IN (5000, 6000, 8000);

-- 名字第二个字符是 e
SELECT employee_name
FROM employees
WHERE employee_name LIKE '_e%';

-- 查询没有经理的员工
SELECT employee_name
FROM employees
WHERE manager_id IS NULL;
```

- `BETWEEN ... AND ...` 包含上下边界。
- `IN (...)` 等价于多个 `OR` 条件。
- `LIKE` 中 `%` 表示零个或多个任意字符，`_` 表示一个任意字符。
- 判断空值使用 `IS NULL` 或 `IS NOT NULL`，不能写成 `= NULL`。

### 6. 多个条件与优先级

```sql
-- 薪水为 8000 且姓名中包含 e
SELECT employee_name, salary
FROM employees
WHERE salary = 8000
  AND employee_name LIKE '%e%';

-- 工作岗位为两种之一，且薪水大于 15000
SELECT employee_name, job_id, salary
FROM employees
WHERE (job_id = 'AD_PRES' OR job_id = 'SA_REP')
  AND salary > 15000;
```

逻辑运算优先级：`NOT` > `AND` > `OR`。有多个条件时，用括号明确分组，避免理解错误。

### 7. ORDER BY 排序

```sql
-- 按薪水升序（ASC 可省略）
SELECT employee_id, employee_name, salary
FROM employees
ORDER BY salary ASC;

-- 按姓名降序
SELECT employee_id, employee_name
FROM employees
ORDER BY employee_name DESC;

-- 使用别名排序
SELECT employee_id, employee_name, salary * 12 AS annual_salary
FROM employees
ORDER BY annual_salary ASC;

-- 多列排序：先按部门升序，再按薪水降序
SELECT employee_name, department_id, salary
FROM employees
ORDER BY department_id ASC, salary DESC;
```

### 8. SELECT 的书写与执行顺序

书写顺序：

```sql
SELECT 列名
FROM 表名
WHERE 条件
ORDER BY 列名;
```

逻辑执行顺序：`FROM` → `WHERE` → `SELECT` → `ORDER BY`。

### 9. 练习

1. 查询 `employees` 表的所有列。
2. 只查询员工姓名与薪水，并将姓名列显示为 `name`。
3. 查询员工的年薪，并将结果列命名为 `annual_salary`。
4. 查询部门编号为 20 或 50 的员工姓名和部门编号，并按姓名升序排列。
5. 查询薪水在 5000 到 12000 之间的员工姓名和薪水。
6. 查询没有经理的员工姓名。
7. 查询工作岗位为 `SA_REP` 或 `ST_CLERK`，且薪水不等于 2500、3500、7000 的员工姓名、工作岗位和薪水。

### 10. 本节易错点

- `WHERE` 用于筛选行，`ORDER BY` 用于排序，二者的位置不能颠倒。
- `BETWEEN` 包含边界值。
- `LIKE` 中 `%` 是任意多个字符，`_` 是一个字符。
- 空值只能用 `IS NULL` 或 `IS NOT NULL` 判断。
- 多条件查询优先使用括号明确逻辑关系。
---

## 十一、SQL 函数

SQL 函数接收输入参数并返回结果，常用于计算、修改单个数据项、格式化日期或数字，以及转换数据类型。

### 1. 单行函数

单行函数对每一行数据分别运算，每一行都会得到一个结果。当前学习的单行函数分为：字符函数、数字函数、日期函数、转换函数和通用函数。

### 2. 字符函数

```sql
-- 大小写转换
SELECT LOWER('ADMIN'), UPPER('admin');

-- 字符串长度与拼接
SELECT LENGTH('mysql');
SELECT CONCAT('hello', ' ', 'mysql');

-- 左侧、右侧填充到指定长度
SELECT LPAD('abc', 6, '0');
SELECT RPAD('abc', 6, '*');

-- 去除空格
SELECT LTRIM('  mysql');
SELECT RTRIM('mysql  ');
SELECT TRIM('  mysql  ');

-- 替换、反转、截取
SELECT REPLACE('mysql', 'sql', 'SQL');
SELECT REVERSE('abc');
SELECT SUBSTR('MYSQL', 2, 3);
SELECT SUBSTRING('MYSQL', 2, 3);

-- 查找子字符串首次出现的位置
SELECT INSTR('database', 'base');
```

| 函数 | 作用 |
| --- | --- |
| `LOWER()` / `LCASE()` | 转为小写 |
| `UPPER()` / `UCASE()` | 转为大写 |
| `LENGTH()` | 返回字符串长度 |
| `CONCAT()` | 连接多个字符串 |
| `LPAD()` / `RPAD()` | 在左侧 / 右侧填充字符 |
| `LTRIM()` / `RTRIM()` / `TRIM()` | 去除左侧、右侧、两侧空格 |
| `REPLACE()` | 替换指定内容 |
| `REVERSE()` | 反转字符串 |
| `SUBSTR()` / `SUBSTRING()` | 截取子字符串 |
| `INSTR()` | 返回子字符串的位置；未找到时返回 `0` |

示例：查询工作岗位从第 4 个字符开始为 `REP` 的员工，并拼接姓名。

```sql
SELECT employee_id,
       CONCAT(last_name, ' ', first_name) AS name,
       job_id,
       LENGTH(last_name) AS name_length,
       INSTR(last_name, 'a') AS contains_a
FROM employees
WHERE SUBSTR(job_id, 4) = 'REP';
```

### 3. 数字函数

```sql
SELECT ABS(-5);                 -- 绝对值：5
SELECT CEIL(1.2), FLOOR(1.8);   -- 向上取整、向下取整
SELECT ROUND(45.923, 2);        -- 四舍五入：45.92
SELECT TRUNCATE(45.923, 2);     -- 截断：45.92
SELECT MOD(17, 5);              -- 余数：2
SELECT POW(2, 3), SQRT(25);     -- 幂、平方根
SELECT GREATEST(3, 12, 8);      -- 最大值：12
SELECT LEAST(3, 12, 8);         -- 最小值：3
SELECT RAND();                  -- 0 到 1 之间的随机数
```

> [!note] `ROUND` 与 `TRUNCATE`
> `ROUND(1.235, 2)` 会四舍五入；`TRUNCATE(1.235, 2)` 直接截断，不进行四舍五入。

常见的统计函数 `AVG()`、`COUNT()`、`MAX()`、`MIN()`、`SUM()` 会在后续“聚合函数”章节中进一步学习。

### 4. 日期函数

MySQL 可用字符串表示日期或日期时间，推荐使用标准格式：`'YYYY-MM-DD HH:MI:SS'`。

```sql
-- 当前日期与时间
SELECT CURDATE(), CURTIME(), NOW(), SYSDATE();

-- 取出日期的组成部分
SELECT DATE('2026-07-13 10:20:30');
SELECT YEAR('2026-07-13'), MONTH('2026-07-13'), DAY('2026-07-13');
SELECT HOUR('10:20:30'), SECOND('10:20:30');

-- 计算与判断日期
SELECT DATEDIFF('2026-07-13', '2026-07-01');
SELECT LAST_DAY('2026-07-13');
SELECT WEEK('2026-07-13'), WEEKDAY('2026-07-13');
```

示例：计算部门编号为 90 的员工从入职到今天共工作了多少周。

```sql
SELECT last_name,
       (SYSDATE() - hire_date) / 7 AS weeks
FROM employees
WHERE department_id = 90;
```

### 5. 转换函数

#### 隐式转换

MySQL 可以自动进行部分类型转换，例如将标准格式的日期字符串转换为日期类型。

#### 显式转换

```sql
-- 日期转为指定格式的字符串
SELECT DATE_FORMAT(hire_date, '%Y年%m月%d日') AS hire_date_text
FROM employees
WHERE last_name = 'King';

-- 字符串按指定格式转为日期
SELECT STR_TO_DATE('2049年05月05日', '%Y年%m月%d日');
```

常用格式符：`%Y`（四位年份）、`%m`（月份）、`%d`（日期）、`%H`（小时）、`%i`（分钟）、`%s`（秒）。

### 6. 通用函数

通用函数主要用于条件判断和空值处理。

| 函数 | 作用 |
| --- | --- |
| `IF(expr, v1, v2)` | 条件 `expr` 成立返回 `v1`，否则返回 `v2` |
| `IFNULL(v1, v2)` | `v1` 不为 `NULL` 返回 `v1`，否则返回 `v2` |
| `ISNULL(expr)` | 判断表达式是否为 `NULL`；是则返回 `1`，否则返回 `0` |
| `NULLIF(expr1, expr2)` | 两个表达式相等返回 `NULL`，否则返回 `expr1` |
| `COALESCE(expr1, expr2, ...)` | 从左到右返回第一个非空值 |
| `CASE ... WHEN ... THEN ... ELSE ... END` | 多分支条件判断 |

```sql
SELECT IF(1 > 0, '正确', '错误');
SELECT IFNULL(NULL, '备用值');
SELECT ISNULL(NULL);
SELECT NULLIF(25, 25);
SELECT COALESCE(NULL, NULL, '最终值');

SELECT CASE job_id
         WHEN 'IT_PROG' THEN '开发'
         WHEN 'ST_CLERK' THEN '文员'
         ELSE '其他'
       END AS job_type
FROM employees;
```

#### 通用函数综合示例

```sql
-- 有佣金显示 SAL+COMM，没有佣金显示 SAL
SELECT last_name,
       salary,
       commission_pct,
       IF(ISNULL(commission_pct), 'SAL', 'SAL+COMM') AS income
FROM employees
WHERE department_id IN (50, 80);

-- 计算年报酬：佣金为空时按 0 计算
SELECT last_name,
       salary,
       IFNULL(commission_pct, 0) AS commission_pct,
       salary * 12 + salary * 12 * IFNULL(commission_pct, 0) AS annual_income
FROM employees;

-- 依次选择第一个非空值
SELECT last_name,
       COALESCE(commission_pct, salary, 10) AS commission_or_salary
FROM employees
ORDER BY commission_pct;
```

### 7. 本节练习

1. 查询员工姓名，并显示其大写形式和姓名长度。
2. 查询薪水并分别使用 `ROUND`、`TRUNCATE` 保留两位小数，比较结果。
3. 查询员工入职日期，并格式化为“年-月-日”。
4. 若员工有佣金则显示佣金，否则显示薪水，两个值都为空时显示 `10`。
5. 使用 `CASE` 为不同工作岗位设置不同的涨薪比例。

### 8. 易错点速记

- `LENGTH()` 计算的是字符串长度；截取字符串使用 `SUBSTR()` 或 `SUBSTRING()`。
- `LIKE` 的通配符与字符函数不同：`%` 和 `_` 只用于模式匹配。
- `NULL` 参与算术运算通常得到 `NULL`，可用 `IFNULL()` 或 `COALESCE()` 处理。
- 日期格式符中 `%m` 是月份，`%i` 是分钟。
- `CASE` 必须以 `END` 结束。
---

## 十二、多表查询

多表查询用于从多张有关联的表中获取数据。连接时必须明确连接条件，否则会产生无意义的大量结果。

### 1. 笛卡尔积

当连接条件缺失或无效时，第一张表的每一行会与第二张表的每一行组合，形成笛卡尔积。除非确有需要，否则查询中应始终写出有效连接条件。

### 2. 等值连接与表别名

```sql
-- 查询员工及所在部门
SELECT e.last_name,
       e.employee_id,
       e.department_id,
       d.department_name,
       d.location_id
FROM employees AS e,
     departments AS d
WHERE e.department_id = d.department_id;
```

- 两张表中同名的列必须使用 `表名.列名` 或表别名限定，避免歧义。
- `n` 张表连接至少需要 `n - 1` 个连接条件。

### 3. 非等值连接

```sql
-- 按薪水范围查出员工的薪水级别
SELECT e.last_name,
       e.salary,
       j.grade_level
FROM employees AS e
JOIN job_grades AS j
  ON e.salary BETWEEN j.lowest_salary AND j.highest_salary;
```

非等值连接使用 `>`、`<`、`BETWEEN` 等非等号条件。

### 4. 自连接

```sql
-- 查询员工及其经理姓名
SELECT e.last_name AS employee_name,
       m.last_name AS manager_name
FROM employees AS e
JOIN employees AS m
  ON e.manager_id = m.employee_id;
```

同一张表在查询中扮演不同角色时，必须为它设置不同别名。

### 5. SQL99 连接写法

```sql
-- 交叉连接：会得到所有组合，慎用
SELECT *
FROM employees CROSS JOIN departments;

-- 内连接：只返回匹配行
SELECT e.last_name, d.department_name
FROM employees AS e
INNER JOIN departments AS d
  ON e.department_id = d.department_id;

-- 左外连接：保留左表全部行
SELECT e.last_name, d.department_name
FROM employees AS e
LEFT JOIN departments AS d
  ON e.department_id = d.department_id;

-- 右外连接：保留右表全部行
SELECT e.last_name, d.department_name
FROM employees AS e
RIGHT JOIN departments AS d
  ON e.department_id = d.department_id;
```

`NATURAL JOIN` 会自动用同名列连接，实际使用中不够直观，优先写 `JOIN ... ON ...`。MySQL 不支持 `FULL OUTER JOIN`，可用左连接和右连接的 `UNION` 模拟。

```sql
SELECT e.last_name, d.department_name
FROM employees AS e
LEFT JOIN departments AS d
  ON e.department_id = d.department_id
UNION
SELECT e.last_name, d.department_name
FROM employees AS e
RIGHT JOIN departments AS d
  ON e.department_id = d.department_id;
```

---

## 十三、聚合函数

聚合函数（多行函数）对一组行进行运算，并为每组返回一个结果。未分组时，整个结果集视为一组。

```sql
SELECT AVG(salary) AS avg_salary,
       SUM(salary) AS total_salary,
       MIN(salary) AS min_salary,
       MAX(salary) AS max_salary,
       COUNT(*) AS employee_count
FROM employees;
```

| 函数 | 作用 |
| --- | --- |
| `AVG()` | 平均值，只适用于数值 |
| `SUM()` | 求和，只适用于数值 |
| `MIN()` / `MAX()` | 最小值 / 最大值 |
| `COUNT(*)` | 统计行数，包括列值为 `NULL` 的行 |
| `COUNT(列名)` | 统计该列非空值的数量 |

注意：聚合函数通常忽略 `NULL`。若要把空值按某个值参与计算，可用 `IFNULL()` 或 `COALESCE()`。

```sql
SELECT AVG(IFNULL(commission_pct, 0)) AS avg_commission
FROM employees;
```

---

## 十四、数据分组（GROUP BY）

`GROUP BY` 将结果集分成多个小组，再对每个小组使用聚合函数。

```sql
-- 每个部门的人数和平均薪水
SELECT department_id,
       COUNT(*) AS employee_count,
       ROUND(AVG(salary), 2) AS avg_salary
FROM employees
GROUP BY department_id;

-- 按多个列分组
SELECT department_id,
       job_id,
       COUNT(*) AS employee_count
FROM employees
GROUP BY department_id, job_id;
```

### WHERE 与 HAVING

- `WHERE`：分组前筛选行。
- `HAVING`：分组后筛选组，可使用聚合函数。

```sql
-- 只显示最高薪水超过 10000 的部门
SELECT department_id,
       MAX(salary) AS max_salary
FROM employees
WHERE job_id NOT LIKE '%REP%'
GROUP BY department_id
HAVING MAX(salary) > 10000
ORDER BY max_salary DESC;
```

常用书写顺序：`SELECT` → `FROM` → `WHERE` → `GROUP BY` → `HAVING` → `ORDER BY`。

---

## 十五、子查询

子查询是嵌在另一条 SQL 语句中的 `SELECT`。子查询必须用圆括号包围，通常先执行内层查询，再使用其结果。

### 1. 单行子查询

单行子查询只返回一行，可配合 `=`、`>`、`<`、`>=`、`<=`、`<>` 使用。

```sql
-- 查询与 Fox 同一部门的员工
SELECT last_name, department_id
FROM employees
WHERE department_id = (
  SELECT department_id
  FROM employees
  WHERE last_name = 'Fox'
);

-- 查询薪水高于平均薪水的员工
SELECT employee_id, last_name, salary
FROM employees
WHERE salary > (
  SELECT AVG(salary)
  FROM employees
)
ORDER BY salary ASC;
```

### 2. 多行子查询

多行子查询返回多行结果，常与 `IN`、`ANY`、`ALL`、`EXISTS` 使用。

```sql
-- 查询在指定地点工作的员工
SELECT last_name, department_id, job_id
FROM employees
WHERE department_id IN (
  SELECT department_id
  FROM departments
  WHERE location_id = 1700
);

-- 薪水大于子查询全部结果中的最大值
SELECT last_name, salary
FROM employees
WHERE salary > ALL (
  SELECT salary
  FROM employees
  WHERE department_id = 60
);

-- 薪水大于子查询任意一个结果
SELECT last_name, salary
FROM employees
WHERE salary > ANY (
  SELECT salary
  FROM employees
  WHERE department_id = 60
);
```

- `> ALL (...)` 等价于大于子查询结果的最大值。
- `> ANY (...)` 表示大于其中任意一个值。
- 子查询结果中若包含 `NULL`，比较结果可能不符合直觉，必要时先排除空值。

---

## 十六、MySQL 索引

索引是对表中一列或多列值建立的排序结构，能加快查询定位。可以把它理解为书的目录：查询更快，但维护索引也有成本。

### 1. 适用与不适用场景

适合创建索引：经常出现在 `WHERE`、`JOIN`、`ORDER BY` 中，且选择性较高的列。

不适合创建索引：频繁更新的列、查询条件很少使用的列、记录极少的表。

### 2. 普通索引

```sql
-- 直接创建
CREATE INDEX emp3_name_index ON emp3(name);

-- 修改表添加
ALTER TABLE emp3 ADD INDEX emp3_name_index(name);

-- 建表时指定
CREATE TABLE emp4 (
  emp_id INT,
  name VARCHAR(30),
  address VARCHAR(50),
  INDEX emp4_name_index(name)
);

-- 删除索引
DROP INDEX emp3_name_index ON emp3;
```

### 3. 唯一索引、主键索引与组合索引

```sql
-- 唯一索引：列值不能重复，但允许空值
CREATE UNIQUE INDEX emp_name_unique_index ON emp(name);

-- 主键索引：由 PRIMARY KEY 自动创建
CREATE TABLE emp6 (
  emp_id INT PRIMARY KEY,
  name VARCHAR(30),
  address VARCHAR(50)
);

-- 组合索引
CREATE INDEX emp_name_address_index ON emp7(name, address);
```

组合索引遵循**最左前缀原则**：索引 `(name, address)` 能支持 `name`，或同时使用 `name` 与 `address` 的条件；只按 `address` 查询通常不能有效利用该索引。

---

## 十七、MySQL 事务

事务是一组作为单个逻辑工作单元执行的操作：要么全部成功，要么全部不执行。

### 1. 事务的特性（ACID）

- **原子性**：事务中的操作不可分割，要么全成功，要么全失败。
- **一致性**：事务前后数据满足既定规则。
- **隔离性**：并发事务之间相互隔离。
- **持久性**：事务提交后，修改会永久保存。

### 2. 使用事务

```sql
START TRANSACTION;

UPDATE account
SET balance = balance - 100
WHERE id = 1;

UPDATE account
SET balance = balance + 100
WHERE id = 2;

COMMIT;       -- 提交全部修改
-- ROLLBACK;  -- 发生异常时回滚全部修改
```

```sql
START TRANSACTION;
SAVEPOINT step_one;

UPDATE account SET balance = balance - 100 WHERE id = 1;
ROLLBACK TO step_one;
COMMIT;
```

### 3. 并发问题与隔离级别

常见并发问题：脏读、不可重复读、幻读。

隔离级别从低到高：

1. `READ UNCOMMITTED`
2. `READ COMMITTED`
3. `REPEATABLE READ`
4. `SERIALIZABLE`

级别越高，数据一致性越强，并发性能通常越低。InnoDB 的默认隔离级别通常为 `REPEATABLE READ`。

---

## 十八、用户与权限管理

用户由“用户名”和“允许连接的主机”共同确定，常见主机包括 `localhost`、`127.0.0.1` 和 `%`（任意主机）。

```sql
-- 创建本地用户
CREATE USER 'reader_user'@'localhost'
IDENTIFIED BY 'StrongPassword_2026';

-- 授予指定库中指定表的查询权限
GRANT SELECT
ON school_db.emp
TO 'reader_user'@'localhost';

-- 回收权限
REVOKE SELECT
ON school_db.emp
FROM 'reader_user'@'localhost';

-- 删除用户
DROP USER 'reader_user'@'localhost';
```

常见权限：`SELECT`、`INSERT`、`UPDATE`、`DELETE`、`CREATE`、`DROP`、`ALL PRIVILEGES`。遵循最小权限原则：只授予完成任务所必需的权限。

---

## 十九、图形化工具：用户与数据导入导出

### 1. 创建用户与分配权限

在图形化工具中通常可通过“用户”或“权限”页面完成：创建用户 → 设置允许连接的主机 → 勾选所需权限 → 保存。

### 2. 导出与导入 SQL 脚本

- **导出**：选择数据库或表，导出为 `.sql` 脚本，用于备份或迁移。
- **导入**：选择目标数据库，运行或导入 `.sql` 脚本。

导入前确认目标数据库、字符集和脚本内容；生产数据操作前先创建备份。

---

## 二十、MySQL 分页查询

MySQL 使用 `LIMIT` 放在查询语句末尾进行分页，起始位置从 `0` 开始。

```sql
-- 从第 0 条开始，取 10 条
SELECT employee_id, last_name, salary
FROM employees
ORDER BY employee_id
LIMIT 0, 10;

-- 等价写法
SELECT employee_id, last_name, salary
FROM employees
ORDER BY employee_id
LIMIT 10 OFFSET 0;
```

页码从 `1` 开始、每页大小为 `page_size` 时：

```text
offset = (page_number - 1) * page_size
```

例如第 3 页、每页 10 条：

```sql
SELECT employee_id, last_name, salary
FROM employees
ORDER BY employee_id
LIMIT 20, 10;
```

> [!tip] 注意
> 分页查询最好配合稳定的 `ORDER BY`，否则不同页面的数据顺序可能不稳定。
