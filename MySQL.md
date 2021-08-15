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

