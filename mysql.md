## 配置`mysql`

### 配置文件

在根目录创建 `my.ini` 文件，并将下边的内容粘贴进入

```python
[mysqld]
basedir ="E:\mysql"	 # 文件路径
datadir ="E:\mysql\data"
port=3306	# 端口号
server_id =10
character-set-server=utf8
character_set_filesystem=utf8
[client]
port=3306
default-character-set=utf8
[mysqld_safe]
timezone="CST"
[mysql]
default-character-set=utf8
```

### 配置环境变量

将 `bin` 目录配置到系统环境变量中 `E:\mysql\bin`

### 下载

以管理员身份运行`PowerShell` ，

 `mysqld --install`

### 初始化数据库

`mysqld --initialize --console`

### 数据库用户名、密码

```
username: root
password: cOMTCtey%6fy
```

### 启动服务器

`net start mysql`

### 登陆服务器

`mysql -u root -p`

### 修改密码

`ALTER user USER() IDENTIFIED BY '新密码'`

`root`

## 登陆退出

### 登陆命令

`mysql [-h 数据库服务器地址] -u 用户名 -p密码`

`mysql -h localhost -u root -p`

### 退出命令

`\q`

`exit`

`quit`

### 登陆成功提示

```python
Welcome to the MySQL monitor.  # 欢迎来到 mysql 终端
Commands end with ; or \g.	# 命令执行符是;或者\g
Your MySQL connection id is 9	# mysql 被连接次数
Server version: 8.0.16 MySQL Community Server - GPL	# mysql 版本号

Type 'help;' or '\h' for help. 	# mysql 帮助命令 \h 或者 help
Type '\c' to clear the current input statement.	# 清除等待命令 \c
```

## `mysql` 常见的符号

`->` 表示当前命令没有命令执行符或者说等待命令执行符来确认

`'>` 表示当前的 `SQL` 命令缺少单引号

`">` 表示当前的 `SQL` 命令缺少双引号

## 修改 `mysql` 提示符

| 参数 | 描述       |
| ---- | ---------- |
| \D   | 完整的日期 |
| \d   | 当前数据库 |
| \h   | 服务器名称 |
| \u   | 当前用户   |

连接客户端时通过参数指定：

`mysql -u root -p --prompt 提示符 `

连接上客户端后，通过`prompt` 命令修改：

`PROMPT 提示符`

例如： `PROMPT \u@\h \d>` 当前用户@服务器名称 当前数据库

## `SQL` 语句介绍及编码规范

### `SQL` 语句结构化查询语句

主要分为 4 大类：

- `DDL` 数据库定义语言（CREATE、DROP、ALTER等）
- `DML` 数据库操纵语言（INSERT、DELETE、UPDATE等）
- `DQL` 数据库查询语言（SELECT、WHERE等）
- `DCL` 数据库控制语言（了解）（GRANT、REVOKE等）

### 编码规范

- 关键字与函数名称全部大写
- 数据库名称、表名称、字段名称全部小写
- `SQL` 语句必须以分号结尾

## 字符集

### 常用中文字符集

- `GB2312` 双字节编码 早期标准 不推荐使用
- `GBK` 双字节编码 中期标准 不是国标 但支持系统很多 而且在 `gb2312` 基础上增加了很多偏僻生字
- `UTF-8` 1~4字节的编码 互联网广泛使用 亚洲通用字符集国际标准化 支持任何语言，在 `MYSQL` 中写成 `utf8`

### `utf-8` 与 `gbk` 的区别

存储长度不一样，`GB` 系统一个汉字占位2个字节，`utf-8` 占位3个字节，推荐使用`UTF-8` (支持语言更多)

### 数据库字符集依赖关系

内容字符集 -> 字段字符集 -> 表字符集 -> 库字符集

## 常用命令及建库语句

- 显示当前服务器版本`SELECT VERSION()`

- 显示当前用户 `SELECT USER()`

- 显示当前日期时间 `SELECT NOW()`

- 建库语句(`DDL`)：

  `CREATE {DATABASE | SCHEMA}[IF NOT EXISTS] db_NAME`

  `[DEFAULT] CHARACTER SET [=] charset_name`

- 显示数据库创建命令(`DDL`) `SHOW CREATE DATABASE 库名`
- 修改数据库 `ALTER {DATABASE | SCHEMA}[db_name] [DEFAULT] CHARACTER SET [=] charset_name`
- 删库语句(`DDL`) `DROP {DATABASE | SCHEMA} [IF EXISTS] db_NAME`
- 查看数据库命令 `SHOW DATABASES`

## `MYSQL` 基础数据类型

### 数值型

| 数据列类型  | 存储空间 | 说明                     | 取值范围                                                     |
| ----------- | -------- | ------------------------ | ------------------------------------------------------------ |
| `TINYINT`   | 1字节    | 非常小的整数             | 带符号值：`-128~127`<br />无符号值：`0~255`                  |
| `SMALLINT`  | 2字节    | 较小的整数               | 带符号值：`-32768~32767`<br />无符号值：`0~65535`            |
| `MEDIUMINT` | 3字节    | 中等大小的整数           | 带符号值：`-8388608~8388607`<br />无符号值：`0~16777215`     |
| `INT`       | 4字节    | 标准整数                 | 带符号值：`-2147483648~2147483647`<br />无符号值：`0~4294967295` |
| `BIGINT`    | 8字节    | 大整数                   | 带符号值：`-9223372036854775808~9233372036854775807`<br />无符号值：`0~18446744073709551615` |
| `FLOAT`     | 4字节    | 单精度浮点数             | 最小非零值：`±1.175494351E-38`<br />最大非零值：`±3.402823466E+38` |
| `DOUBLE`    | 8字节    | 双精度浮点数             | 最小非零值：`±2.2250738585072014E-308`<br />最大非零值：`±1.7976931348623157E+308` |
| `DECIMAL`   | 自定义   | 以字符串形式表示的浮点数 | 取决于存储单元字节数                                         |

### 时间类型

| 类型          | 存储空间 | 说明                                | 最大长度                                  |
| ------------- | -------- | ----------------------------------- | ----------------------------------------- |
| `DATE`        | 3字节    | `"YYYY-MM-DD"` 格式表示的日期值     | `1000-01-01~9999-12-31`                   |
| `TIME`        | 3字节    | `"hh:mm:ss"`格式表示的时间值        | `-838:59:59~838:59:59`                    |
| `DATETIME`    | 8字节    | `"YYYY-MM-DD hh:mm:ss" `格式        | `1000-01-01 00:00:00~9999-12-31 23:59:59` |
| `TIMESTAMP`   | 4字节    | `"YYYYMMDDhhmmss" `格式表示的时间戳 | `19700101000000~2037年的某个时刻`         |
| `EAR（弃用）` | 1字节    | `"YYYY" `格式的年份值               | `1901~2155`                               |

### 字符串类型

| 类型                          | 存储空间          | 说明                                   | 最大长度    |
| ----------------------------- | ----------------- | -------------------------------------- | ----------- |
| `CHAR[(M)]`                   | M字节             | 定长字符串                             | 0~255       |
| `VARCHAR[(M)]`                | L+1字节           | 可变长字符串                           | 65535       |
| `TINYBLOD, TINYTEXT`          | L+1字节           | 非常小的BLOB（二进制数大对象）和文本串 | 2⁸-1字节    |
| `BLOB, TEXT`                  | L+2字节           | 小的BLOB和文本串                       | 2¹⁶-1字节   |
| `MEDIUMBLOB, MEDIUMTEXT`      | L+3字节           | 中等的BLOB和文本串                     | 2²⁴-1字节   |
| `LONGBLOB, LONGTEXT`          | L+4字节           | 大的BLOB和文本串                       | 2³²-1字节   |
| `ENUM('value1','value2',...)` | 1或2字节          | 枚举：可赋予某个枚举成员               | 65535个成员 |
| `SET('value1','value2',...)`  | 1、2、3、4或8字节 | 集合：可赋予多个集合成员               | 64个成员    |

## 数据表操作

### 建立数据表

- 打开数据库 `USE 数据库名称`

- 查看当前所在的数据库  `SELECT DATABASE()`

- 创建数据表

  ```sh
  CREATE TABLE [IF NOT EXISTS] table_name(column_name data_type,... ...)[ENGINE=表引擎[DEFAULT] CHARSET=utf8]
  ```

### 查看数据表

```sh
SHOW TABLES [FROM db_name]
```

### 查看数据表结构

```sh
DESC table_name
SHOW COLUMNS FROM table_name
```

### 插入数据

```sh
INSERT [INTO] table_name [(col_name,......)] VALUES(val,......)
```

### 查找数据

```sh
SELECT * FROM table_name
SELECT col_name,... FROM table_name
```

 ### `MySQL` 字段约束

有时只定义了字段的类型还不够，还要设置其他一些附加属性，如自动增量的设置、自动补 0 的设置和默认值的设置等一些特殊的设置。下面具体介绍特殊字段的属性。

- `UNSIGNED` 该属性只能用于设置数值类型，不允许数列出现负数。如果不需要向某字段中插入负数。如果不需要向某字段中插入负数，则使用该属性修饰可以使该字段的最大存储长度增加一倍。例如，正常情况下数据类型 `TINYINT` 的数值范围在-128~127，而使用 `UNSIGNED` 属性修饰以后最小值为 0，最大值可以达到 255。
- `ZEROFILL` 该属性也只能用于设置数值类型，在数值之前自动用 0 补齐不足的位数。例如，将 5 插入一个声明为 `int(3) ZEROFILL` 的字段，在之后查询输出时，输出的数据将是 ’005‘。当给一个字段使用 `ZEROFILL` 修饰时，该字段自动应用 `UNSIGNED` 属性。
- `AUTO_INCREMENT` 该属性用于设置字段的自动增量属性，当数值类型的字段设置自动增量时，每增加一条新记录，该字段的值就自动加 1 ，而且此字段的值不允许重复。此修饰符只能修饰整数类型的字段。插入新纪录时自增字段可以为 `NULL`、0 或留空，这时自增字段自动使用上次此字段的值加 1，作为此次的值。插入时也可以为自增字段指定某一非零数值，这时，如果表中已经存在此值将出错；否则使用指定数值作为自增字段的值，并且下次插入时，下个字段的值将在此值的基础上加 1。
- `NULL 和 NOT NULL` 默认为 `NULL`,既没有在此字段插入值。如果指定了 `NOT NULL`,则必须在此字段插入值。
- `DEFAULT` 可以通过此属性来指定一个默认值，如果没有在此列添加值，那么默认添加此值。例如，在用户表 `users` 中，可以将性别字段的默认值设置为 “男”。在为该列插入数据时，只在当用户为 “女” 时才需要指定，否则可以不为该字段指定值，默认值就为 “男”。

### `MySQL` 字段约束之主键约束

- 主键约束 `PRIMARY KEY`
- 每张数据表中只能存在一个主键
- 主键保证记录的唯一性
- 逐渐自动为 `NOT NULL`
- `AUTO_INCREMENT` 必须和 `PRINARY KEY` 一起使用，但 `PRIMARY KEY` 不需要