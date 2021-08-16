## MySQL

### 安装

 初始密码：JjMD4Wq.vU,:

#### 启动

启动MySQL：`net start mysql`

#### 登录

```bash
mysql -h 主机名 -u 用户名 -p
```

- **-h** : 指定客户端所要登录的 MySQL 主机名, 登录本机(localhost 或 127.0.0.1)该参数可以省略;
- **-u** : 登录的用户名;
- **-p** : 告诉服务器将会使用一个密码来登录, 如果所要登录的用户名密码为空, 可以忽略此选项。

登录本机只需：

```bash
mysql -u root -p
```

### 管理

#### 启动

```bash
cd D:\MySQL\mysql-8.0.26-winx64\bin
mysqld --console
```

#### 关闭

```bash
cd D:\MySQL\mysql-8.0.26-winx64\bin
mysqladmin -uroot shutdown
```

#### 用户设置

添加 MySQL 用户，只需要在数据库中的 user 表添加新用户即可，注意使用PASSWORD()对密码进行加密。

#### 常用命令

`USE xxx`跳转至xxx数据库

`SHOW DATABASES`列出所有数据库。

`SHOW TABLES`显示当前数据库的所有表。

`SHOW COLUMNS FROM XXX`显示数据表xxx的属性等等。

`SHOW INDEX FROM XXX`显示数据表xxx的详细索引信息。

`SHOW TABLE STATUS [FROM db_name] [LIKE 'pattern'] \G`显示数据库管理系统的性能以及统计信息。

### 创建数据库

```mysql
CREATE DATABASE 数据库名;
```

### 删除数据库

```mysql
drop database <数据库名>;
```

### 选择数据库

```mysql
use 数据库名;
```

### 数据类型

#### 数值

| 类型         | 大小                                     | 范围（有符号）                                               | 范围（无符号）                                               | 用途            |
| :----------- | :--------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- | :-------------- |
| TINYINT      | 1 byte                                   | (-128，127)                                                  | (0，255)                                                     | 小整数值        |
| SMALLINT     | 2 bytes                                  | (-32 768，32 767)                                            | (0，65 535)                                                  | 大整数值        |
| MEDIUMINT    | 3 bytes                                  | (-8 388 608，8 388 607)                                      | (0，16 777 215)                                              | 大整数值        |
| INT或INTEGER | 4 bytes                                  | (-2 147 483 648，2 147 483 647)                              | (0，4 294 967 295)                                           | 大整数值        |
| BIGINT       | 8 bytes                                  | (-9,223,372,036,854,775,808，9 223 372 036 854 775 807)      | (0，18 446 744 073 709 551 615)                              | 极大整数值      |
| FLOAT        | 4 bytes                                  | (-3.402 823 466 E+38，-1.175 494 351 E-38)，0，(1.175 494 351 E-38，3.402 823 466 351 E+38) | 0，(1.175 494 351 E-38，3.402 823 466 E+38)                  | 单精度 浮点数值 |
| DOUBLE       | 8 bytes                                  | (-1.797 693 134 862 315 7 E+308，-2.225 073 858 507 201 4 E-308)，0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 0，(2.225 073 858 507 201 4 E-308，1.797 693 134 862 315 7 E+308) | 双精度 浮点数值 |
| DECIMAL      | 对DECIMAL(M,D) ，如果M>D，为M+2否则为D+2 | 依赖于M和D的值                                               | 依赖于M和D的值                                               | 小数值          |

#### 日期和时间

| 类型      | 大小 ( bytes) | 范围                                                         | 格式                | 用途                     |
| :-------- | :------------ | :----------------------------------------------------------- | :------------------ | :----------------------- |
| DATE      | 3             | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3             | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1             | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8             | 1000-01-01 00:00:00/9999-12-31 23:59:59                      | YYYY-MM-DD HH:MM:SS | 混合日期和时间值         |
| TIMESTAMP | 4             | 1970-01-01 00:00:00/2038结束时间是第 **2147483647** 秒，北京时间 **2038-1-19 11:14:07**，格林尼治时间 2038年1月19日 凌晨 03:14:07 | YYYYMMDD HHMMSS     | 混合日期和时间值，时间戳 |

#### 字符串

| 类型       | 大小                  | 用途                            |
| :--------- | :-------------------- | :------------------------------ |
| CHAR       | 0-255 bytes           | 定长字符串                      |
| VARCHAR    | 0-65535 bytes         | 变长字符串                      |
| TINYBLOB   | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                    |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535 bytes        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                    |

### 创建数据表

- 表名
- 表字段名
- 定义每个表字段

```mysql
CREATE TABLE table_name (column_name column_type);
```

```mysql
CREATE TABLE IF NOT EXISTS `runoob_tbl`(
   `runoob_id` INT UNSIGNED AUTO_INCREMENT,
   `runoob_title` VARCHAR(100) NOT NULL,
   `runoob_author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY ( `runoob_id` )
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

- 如果你不想字段为 **NULL** 可以设置字段的属性为 **NOT NULL**， 在操作数据库时如果输入该字段的数据为**NULL** ，就会报错。
- AUTO_INCREMENT定义列为自增的属性，一般用于主键，数值会自动加1。
- PRIMARY KEY关键字用于定义列为主键。 您可以使用多列来定义主键，列间以逗号分隔。
- ENGINE 设置存储引擎，CHARSET 设置编码。

### 删除数据表

```mysql
DROP TABLE table_name ;
```

### 插入数据

```mysql
INSERT INTO table_name ( field1, field2,...fieldN )
                       VALUES
                       ( value1, value2,...valueN );
```

查看数据表信息：`select * from xxx`

### 查询数据

```mysql
SELECT column_name,column_name
FROM table_name
[WHERE Clause]
[LIMIT N][ OFFSET M]
```

- 查询语句中你可以使用一个或者多个表，表之间使用逗号(,)分割，并使用WHERE语句来设定查询条件。
- SELECT 命令可以读取一条或者多条记录。
- 使用星号来代替其他字段，SELECT语句会返回表的所有字段数据
- 使用 WHERE 语句来包含任何条件。
- 使用 LIMIT 属性来设定返回的记录数。
- 通过OFFSET指定SELECT语句开始查询的数据偏移量，默认情况下偏移量为0。

### WHERE子句

```mysql
SELECT field1, field2,...fieldN FROM table_name1, table_name2...
[WHERE condition1 [AND [OR]] condition2.....
```

WHERE子句相当于if条件。

`SELECT * from runoob_tbl WHERE runoob_author='菜鸟教程';`

比较默认不区分大小写，但使用`WHERE BINARY`可以区分大小写。

### UPDATE更新

```mysql
UPDATE table_name SET field1=new-value1, field2=new-value2
[WHERE Clause]
```

### DELETE

```mysql
DELETE FROM table_name [WHERE Clause]
```

### LIKE

```mysql
SELECT field1, field2,...fieldN 
FROM table_name
WHERE field1 LIKE condition1 [AND [OR]] filed2 = 'somevalue'
```

LIKE 子句中使用百分号来表示任意字符，类似于正则表达式中的星号 *****。

如果没有使用百分号，LIKE子句与等号的效果是一样的。

`SELECT * from runoob_tbl  WHERE runoob_author LIKE '%COM';`

### UNION

```mysql
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions]
UNION [ALL | DISTINCT]
SELECT expression1, expression2, ... expression_n
FROM tables
[WHERE conditions];
```

- **expression1, expression2, ... expression_n**: 要检索的列。
- **DISTINCT:** 可选，删除结果集中重复的数据。默认情况下 UNION 操作符已经删除了重复数据，所以 DISTINCT 修饰符对结果没啥影响。
- **ALL:** 可选，返回所有结果集，包含重复数据。

用于从多张表中选择数据。

### 排序

```mysql
SELECT field1, field2,...fieldN FROM table_name1, table_name2...
ORDER BY field1 [ASC [DESC][默认 ASC]], [field2...] [ASC [DESC][默认 ASC]]
```

ASC升序，DESC降序。

```mysql
SELECT * from runoob_tbl ORDER BY submission_date ASC;
```

### GROUP BY

```mysql
SELECT column_name, function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name;
```

根据一个或多个列对结果集进行分组。

```mysql
SELECT name, COUNT(*) FROM   employee_tbl GROUP BY name;
```

将表格按照名字分组，并统计出现次数。

#### WITH ROLLUP

在分组统计数据基础上再进行相同的统计。

```mysql
SELECT coalesce(name, '总数'), SUM(singin) as singin_count FROM  employee_tbl GROUP BY name WITH ROLLUP;
```

对singin_count也进行求和，并将其作为总数的name呈现。

```mysql
select coalesce(a,b,c);
```

参数说明：如果a==null,则选择b；如果b==null,则选择c；如果a!=null,则选择a；如果a b c 都为null ，则返回为null（没意义）。

### 连接的使用

使用JOIN在多张表内查询数据。

- **INNER JOIN（内连接,或等值连接）**：获取两个表中字段匹配关系的记录。
- **LEFT JOIN（左连接）：**获取左表所有记录，即使右表没有对应匹配的记录。
- **RIGHT JOIN（右连接）：** 获取右表所有记录，即使左表没有对应匹配的记录。

```mysql
SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a INNER JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;
```

INNER JOIN返回两张表中重合的部分。

```mysql
SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a LEFT JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;
```

LEFT JOIN返回左表所有内容。

```mysql
SELECT a.runoob_id, a.runoob_author, b.runoob_count FROM runoob_tbl a RIGHT JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;
```

RIGHT JOIN返回右表所有内容。

### 处理NULL

- **IS NULL:** 当列的值是 NULL,此运算符返回 true。
- **IS NOT NULL:** 当列的值不为 NULL, 运算符返回 true。
- **<=>:** 比较操作符（不同于 = 运算符），当比较的的两个值相等或者都为 NULL 时返回 true。

不能用`xxx = NULL`来搜索NULL，需要写成`xxx IS NULL`。

### 正则表达式

```mysql
SELECT name FROM person_tbl WHERE name REGEXP '^st';
```

加上`REGXP 'XXX'`即可用XXX规则进行匹配。

### 事务

用于处理操作量大，复杂度高的数据。

一个事务包含多个SQL语句。

#### 事务控制

- BEGIN开启一个事务；
- COMMIT 提交事务，并使已对数据库进行的所有修改成为永久性的；
- ROLLBACK 回滚会结束用户的事务，并撤销正在进行的所有未提交的修改；
- SAVEPOINT identifier，SAVEPOINT 允许在事务中创建一个保存点，一个事务中可以有多个 SAVEPOINT。

#### 事务处理

1、用 BEGIN, ROLLBACK, COMMIT来实现

- **BEGIN** 开始一个事务
- **ROLLBACK** 事务回滚
- **COMMIT** 事务确认

2、直接用 SET 来改变 MySQL 的自动提交模式:

- **SET AUTOCOMMIT=0** 禁止自动提交
- **SET AUTOCOMMIT=1** 开启自动提交

### ALTER

修改数据表名或者修改数据表字段。

#### 删除/添加字段

```mysql
ALTER TABLE testalter_tbl  DROP i;
```

删除表内的i字段。

注意：如果数据表中只剩余一个字段则无法使用DROP来删除字段。

```mysql
ALTER TABLE testalter_tbl ADD i INT;
```

添加一个字段i，默认添加到末尾。

可以使用FIRST或者AFTER XXX来指定位置。

```mysql
ALTER TABLE testalter_tbl ADD i INT FIRST;
ALTER TABLE testalter_tbl ADD i INT AFTER c;
```

#### 修改字段名称/类型

MODIFY:

```mysql
ALTER TABLE testalter_tbl MODIFY c CHAR(10);
```

MODIFY + 要修改的字段名 + 类型。

CHANGE:

```mysql
ALTER TABLE testalter_tbl CHANGE i j BIGINT;
```

CHANGE + 要修改的字段名 + 新字段名 + 类型。

#### 对NULL/默认值的影响

可以指定是否包含值或者是否设置默认值。

```mysql
ALTER TABLE testalter_tbl MODIFY j BIGINT NOT NULL DEFAULT 100;
```

字段默认为 NULL。

#### 修改默认值

```mysql
ALTER TABLE testalter_tbl ALTER i SET DEFAULT 1000;
```

#### 删除默认值

```mysql
ALTER TABLE testalter_tbl ALTER i DROP DEFAULT;
```

#### 修改表名

```mysql
ALTER TABLE testalter_tbl RENAME TO alter_tbl;
```

### 索引

#### 普通索引

创建索引：

```mysql
CREATE INDEX indexName ON table_name (column_name)
```

添加索引：

```mysql
ALTER table tableName ADD INDEX indexName(columnName)
```

创建表时指定：

```mysql
CREATE TABLE mytable(  
ID INT NOT NULL,   
username VARCHAR(16) NOT NULL,  
INDEX [indexName] (username(length))  
);  
```

删除索引：

```mysql
DROP INDEX [indexName] ON mytable; 
```

#### 唯一索引

索引列的值必须唯一，但允许有空值。如果是组合索引，则列值的组合必须唯一。

创建索引：

```mysql
CREATE UNIQUE INDEX indexName ON mytable(username(length)) 
```

添加索引：

```mysql
ALTER table mytable ADD UNIQUE [indexName] (username(length))
```

创建表时指定：

```mysql
CREATE TABLE mytable(  
ID INT NOT NULL,
username VARCHAR(16) NOT NULL,  
UNIQUE [indexName] (username(length))  
);  
```

#### ALTER添加索引

```mysql
ALTER TABLE tbl_name ADD PRIMARY KEY (column_list);
```

添加一个主键，索引值必须唯一，且不能为NULL。

```mysql
ALTER TABLE tbl_name ADD UNIQUE index_name (column_list);
```

创建索引的值必须是唯一的。

```mysql
ALTER TABLE tbl_name ADD INDEX index_name (column_list);
```

添加普通索引，索引值可出现多次。

```mysql
ALTER TABLE tbl_name ADD FULLTEXT index_name (column_list);
```

指定了索引为 FULLTEXT ，用于全文索引。

#### ALTER添加/删除主键

主键作用于列上，添加主键索引时需要确保该主键默认不为空。

添加主键：

```mysql
ALTER TABLE testalter_tbl MODIFY i INT NOT NULL;
ALTER TABLE testalter_tbl ADD PRIMARY KEY (i);
```

删除主键：

```mysql
ALTER TABLE testalter_tbl DROP PRIMARY KEY;
```

#### 显示索引

```mysql
SHOW INDEX FROM table_name\G
```

`\G`用于格式化输出信息。

### 临时表

TEMPORARY TABLE

只在当前连接可见，当关闭连接时，Mysql会自动删除表并释放所有空间。

注意：`SHOW TABLES`命令看不到临时表。

### 复制表

1. 使用`SHOW CREATE TABLE xxx \G`命令获取创建数据表的语句。

2. 复制SQL语句，修改数据表名，并执行SQL语句。

3. 复制表的内容使用`INSERT INTO ... SELECT`实现。

   ```mysql
   mysql> INSERT INTO clone_tbl (runoob_id,
       ->                        runoob_title,
       ->                        runoob_author,
       ->                        submission_date)
       -> SELECT runoob_id,runoob_title,
       ->        runoob_author,submission_date
       -> FROM runoob_tbl;
   ```

### 服务器元数据

| 命令               | 描述                      |
| :----------------- | :------------------------ |
| SELECT VERSION( )  | 服务器版本信息            |
| SELECT DATABASE( ) | 当前数据库名 (或者返回空) |
| SELECT USER( )     | 当前用户名                |
| SHOW STATUS        | 服务器状态                |
| SHOW VARIABLES     | 服务器配置变量            |

### 序列使用

一张数据表只能有一个字段自增主键， 想实现其他字段也自动增加，用序列实现。

#### 创建AUTO_INCREMENT

在创建数据表时对某列使用，无需指定值即可实现自动增长。

#### 获取AUTO_INCREMENT的值

`SELECT LAST_INSERT_ID( ) `

#### 重置序列

需要删除自增的列，并再次添加。

```mysql
ALTER TABLE insect DROP id;
ALTER TABLE insect
	ADD id INT UNSIGNED NOT NULL AUTO_INCREMENT FIRST,
    ADD PRIMARY KEY (id);
```

#### 设置开始值

方法1：

创建数据表时在括号外加上`auto_increment=100`。

方法2：

```mysql
ALTER TABLE t AUTO_INCREMENT = 100;
```

### 重复数据

#### 防止出现重复数据

指定字段为主键或者唯一索引。

```mysql
CREATE TABLE person_tbl
(
   first_name CHAR(20) NOT NULL,
   last_name CHAR(20) NOT NULL,
   sex CHAR(10),
   PRIMARY KEY (last_name, first_name)
);
```

上表的first_name与last_name均为主键，均唯一。

在这种情况下插入重复数据会报错。

`INSERT IGNORE INTO`可以防止报错并插入所有不存在的数据。

`REPLACE INTO`会删除重复的数据再添加新的数据。

```mysql
CREATE TABLE person_tbl
(
   first_name CHAR(20) NOT NULL,
   last_name CHAR(20) NOT NULL,
   sex CHAR(10),
   UNIQUE (last_name, first_name)
);
```

上表将first_name和last_name作为唯一的索引。

#### 统计重复数据

```mysql
SELECT COUNT(*) as repetitions, last_name, first_name
	FROM person_tbl
    GROUP BY last_name, first_name
    HAVING repetitions > 1;
```

- 确定哪一列包含的值可能会重复。
- 在列选择列表使用COUNT(*)列出的那些列。
- 在GROUP BY子句中列出的列。
- HAVING子句设置重复数大于1。

#### 过滤重复数据

使用DISTINCT。

```mysql
SELECT DISTINCT last_name, first_name FROM person_tbl;
```

选择不重复的数据：

```mysql
SELECT last_name, first_name FROM person_tbl GROUP BY (last_name, first_name);
```

#### 删除重复数据

方法1：

```mysql
CREATE TABLE tmp SELECT last_name, first_name, sex FROM person_tbl  GROUP BY (last_name, first_name, sex);
DROP TABLE person_tbl;
ALTER TABLE tmp RENAME TO person_tbl;
```

方法2：

```mysql
ALTER IGNORE TABLE person_tbl
ADD PRIMARY KEY (last_name, first_name);
```

### SQL注入

过滤。

### 导出数据

```mysql
SELECT * FROM runoob_tbl INTO OUTFILE '/tmp/runoob.txt';
```

#### 导出表为原始数据

```bash
mysqldump -u root -p --no-create-info --tab=/tmp RUNOOB runoob_tbl
```

会将表格导出为生成这张表格所需要的脚本。

#### 导出表为SQL格式

```bash
mysqldump -u root -p RUNOOB runoob_tbl > dump.txt
```

### 导入数据

方法1：

```bash
mysql -u用户名    -p密码    <  要导入的数据库数据(runoob.sql)
```

方法2：

```mysql
create database abc;      # 创建数据库
use abc;                  # 使用已创建的数据库 
set names utf8;           # 设置编码
source /home/abc/abc.sql  # 导入备份数据库
```

方法3：

```mysql
LOAD DATA LOCAL INFILE 'dump.txt' INTO TABLE mytbl;
```

### 函数

[MySQL 函数 | 菜鸟教程 (runoob.com)](https://www.runoob.com/mysql/mysql-functions.html)

### 运算符

常见运算符均是。

# MySQL学习结束！
