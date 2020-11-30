# MySQL

>  视频参考狂神：https://www.bilibili.com/video/BV1NJ411J79W

## 1、初识MySQL

> 只会写代码，学好数据库，基本混饭吃
>
> 操作系统，数据结构与算法，不错的程序员
>
> 离散数学，数字电路，体系结构，编译原理 + 实战经验 高级程序员

### 1.1、为什么学习数据库

1. 岗位需求
2. 大数据时代，的数据者得天下
3. 被迫需求：存数据 
4. 数据库是所有软件体系中最核心的存在 DBA（数据库管理员）

### 1.2、什么是数据库

- 数据库（DB DataBase）
- 概念：数据仓库，软件，安装在操作系统之上！SQL，可以存储大量数据。（数据500万以上要优化）
- 作用：存储数据，管理数据

### 1.3、数据库分类

- 关系型数据库（SQL）

  - MySQL,Oracle,Sql Server,DB2,SQLlite

  - 通过表和表之间，行和列之间的关系进行数据的存储，如学员信息表等

    

- 非关系型数据库（NoSQL）Not only SQL

  - Redis，MongoDB
  - 非关系型数据库，对象存储，通过对象的自身属性要决定

DBMS（数据库管理系统）

- 数据库的管理软件，科学有效的管理我们的数据。维护和获取数据
- MySQL，数据库管理系统

### 1.4、Mysql简介

- MySQL是一个关系型数据管理系统
- 前世：瑞典MySQL AB公司
- 今生：属于Oracle旗下产品
- MySQL是最好的RDBMS 关系型数据库管理系统之一
- 开源的数据库软件
- 体积小，速度快，总体拥有成本低，招人成本低，所有人必须会
- 中小型网站，或大型，集群
- 官网：https://www.mysql.com/
- 版本
  - 5.7稳定
  - 8.0最新支持nosql
- 安装建议：
  1. 不要使用exe，注册表问题，卸载残留
  2. 尽可能使用压缩包安装

### 1.5、安装Mysql

> Windows

1. 解压

2. 放到环境目录

3. 添加环境变量

4. 新疆mysql.ini配置文件

   ```ini
   [mysqld]
   basedir={yourPath}
   datadir={yourPath}\data #会自动创建
   port=3306
skip-grant-table #跳过密码验证
   ```
   
5. 启动管理员权限，切换到bin目录，输入mysqld -install（安装mysql）

6. 初始化数据库文件 mysql --initialize-insecure --user=mysql

7. 启动mysql，修改root密码，输入flush privilege 刷新权限

8. 修改my.ini 文件删除最后一句

9. 重启mysql

10. 测试结果 

- sc delete mysql,清空服务

### 1.6 安装SQLyog（或其他）

- Navicat for SQL Server for mac
- 初始教程 https://www.jianshu.com/p/326c1aaa1052
- 在线手册 http://www.navicat.com.cn/manual/online_manual/cn/navicat/win_manual/

### 1.7 连接数据库

- 命令行连接!

  ```sql
  mysql -uroot -p123456 -- 连接数据库
  
  update mysql.user set authentication_string=password('123456') where user='root' and Host='localhost'; -- 修改用户密码
  
  flush privilege; -- 刷新权限
  --------------------------------------
  -- 所有的语句都使用分号;结尾
  
  show databases; -- 查看所有数据库
  
  use database -- 切换数据库 use 数据库名
  
  show tables; -- 查看数据库中的表
  
  describe table; -- 显示数据库表的结构信息
  
  create database xxxx; -- 创建一个数据库
  
  exit; -- 退出连接
  
  -- 单行注释（SQL本来的注释）
  
  /*
  	多行注释
  */
  ```

  **CRUD**

  - DDL 数据库定义语言
  - DML 数据库操作语言
  - DQL 数据库查询语言
  - DCL 数据库控制语言

## 2.操作数据库

> 操作数据库>操作数据库中的表->操作数据库中表的数据
>
> **mysql的关键字不区分大小写**

### 2.1、操作数据库（了解）

1. 创建数据库

```sql
CREATE DATABASE [IF NOT EXISTS] test
```

2. 删除数据库

```sql
DROP DATABASE [IF EXISTS] hello
```

3. 使用数据库

```sql
-- tab 键上面，如果你的表名或者字段名是一个特殊字符，就需要带``比如`user`
USE `test`
```

4. 查看数据库

```sql
SHOW DATABASES -- 查看所有的数据库
```

- 对比 **DBMS** 可视化操作

学习思路：

1. 对照数据库操作的历史记录sql
2. 固定的语法关键字必须记住

### 2.2、数据库的列类型

> 数值

|    类型     |          说明          |    大小     |              其他               |
| :---------: | :--------------------: | :---------: | :-----------------------------: |
|   tinyint   |      十分小的数据      |   1个字节   |                                 |
|  smallint   |       较小的数据       |   2个字节   |                                 |
|  mediumint  |     中等大小的数据     |   3个字节   |                                 |
|   **int**   |     **标准的整数**     | **4个字节** |          **常用的int**          |
|   bigint    |       较大的数据       |   8个字节   |                                 |
|    float    |         浮点数         |   4个字节   |                                 |
|   double    |         浮点数         |   8个字节   |            精度问题             |
| **decimal** | **字符串形式的浮点数** |             | **金融计算的时候，使用decimal** |



> 字符串

|    类型     |       说明       |     大小     |           其他           |
| :---------: | :--------------: | :----------: | :----------------------: |
|    char     | 字符串固定大小的 |    0～255    |                          |
| **varchar** |  **可变字符串**  | **0～65535** | **对应Java的String类型** |
|  tinytext   |     微型文本     |   2^8 - 1    |                          |
|  **text**   |    **文本串**    | **2^16 - 1** |      **保存大文本**      |



> 时间日期

| 类型      | 格式说明            | 其他                   |
| --------- | ------------------- | ---------------------- |
| date      | YYYY-MM-DD          | 日期格式               |
| time      | HH:mm:ss            | 时间格式               |
| datatime  | YYYY-MM-DD HH:mm:ss | 最常用的时间格式       |
| timestamp | 时间戳              | 1970-1-1到现在的浩淼数 |
| Year      | 年份表示            |                        |



> null

- 没有值，未知
- **不要使用NULL进行运算，结果为NULL**



### 2.3、数据库字段属性（重点）

Unsigned：

- 无符号的整数
- 声明了该列不能声明为负数

zerofill：

- 0填充的
- 不足的位数，使用0来填充，int（3），5 ---> 005

自增：

- 通常理解为自增，自动在上一条记录的基础上+1（默认）
- 通常用来设计唯一的主键~ index，必须是整数类型
- 可以自定义设计主键自增的起始值和步长

非空 null ,not null

- 假设设置为 not null，如果不给他赋值，就会报错
- NULL，如果不填写值，默认就是就是null

默认：

- 设置默认的值！
- sex，默认值为男，如果不指定该列的值，则会有默认的值

拓展：

```sql
/* 每一个表，都必须存在一以下五个字段！表示一个记录存在的意义
id 主键
`version` 乐观锁
is_delete 伪删除
gmt_create 创建时间
gmt_update 修改时间
*
```



### 2.4、创建数据库表

```sql
CREATE TABLE IF NOT EXISTS `student` (
	`id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
	`name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
	`pwd` VARCHAR(255) NOT NULL DEFAULT '123456' COMMENT '密码',
	`sex` VARCHAR(2) NOT NULL DEFAULT '男' COMMENT '性别',
	`birthday` DATETIME DEFAULT NULL COMMENT '家庭住址',
	`address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址',
	`email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY(`id`)
)engine=innodb default charset=utf8
```

格式

```sql
CREATE TABLE [IF NOT EXISTS] `表名` (
	`字段名` 列类型 [属性] [索引] [注释],
   .....
)[表类型] [字符集设置] [注释]
```

常用命令

```sql
show create database test -- 查看创建数据库的语句
show create table student -- 查看student数据表的定义语句
desc student -- 显示表的结构
```

### 2.5、数据表的类型

```sql
-- 关于数据库引擎
/*
INNODB 默认使用
MYISAM 早些年使用
*/
```

|              | MYISAM         | INNODB               |
| ------------ | -------------- | -------------------- |
| 事务支持     | 不支持         | 支持                 |
| 数据行锁定   | 不支持（表锁） | 支持（行锁支持）     |
| 外键约束     | 不支持         | 支持（关联另一张表） |
| 全文索引     | 支持           | 不支持               |
| 表空间的大小 | 较小           | 较大，前者的两倍     |

常规的使用操作：

- MYISAM：节约空间、速度较快
- INNODB：安全性高，事务的处理，多表多用户操作

> 在物理空间的位置

- 所有的数据库文件都在data目录下
- 本质还是文件的存储

MySQL引擎在物理文件上的区别

- innoDB在数据库表中只有一个*.frm文件，以及上级目录下的ibdata1文件
- MYISAM对应文件
  - *.frm 	表结构的定义文件
  - *.MYD   数据文件 data
  - *.MYI     索引文件 idnex



> 设置数据库的字符集编码

```sql
CHARSET=UTF8 -- 建议使用
```

- 不设置的话，会是mysql默认的字符集编码（Latin）不支持中文
- MYSQL的默认编码是Latin1，不支持中文
- 在my.ini中配置默认的编码

```sql
charset-set-server = utf8 -- 不建议使用
```



### 2.6、修改和删除表

> 修改

```sql
-- 修改b表名 旧表名 RENAME AS 新表名
ALTER TABLE `student1` RENAME AS `student`;
-- 增加表的字段 ALTER TABLE 表名 ADD 字段名 属性列
ALTER TABLE `student` ADD age INT(3);
-- 修改表的字段 （重命名，修改约束）
ALTER TABLE `student` MODIFY age VARCHAR(11) -- 修改约束和类型
ALTER TABLE `student` CHANGE age age1 INT(3) -- 字段重命名 
-- 删除表的字段
ALTER TABLE `student` DROP age1
```

> 删除

```sql
-- 删除表（如果表存在再删除）
DROP TABLE IF EXISTS `student`
```

所有的创建和删除操作尽量加上判断，以免报错

- 注意点
  - `    字段名 ,使用飘号包裹
  - 注释 -- /**/
  - sql 关键字大小写不敏感，建议大家写小写
  - 所有的符号全部用英文



## 3、MySQL 数据管理

### 3.1、外键（了解即可）

> 方式一：在创建表的时候，增加约束（麻烦，比较复杂）

```sql
create table `grade`(
	`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
	`gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
	PRIMARY KEY (`gradeid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8

-- 学生表的gradeid字段，要去引用年级表的gradeid
-- 定义外键key
-- 给这个外键添加约束（执行引用）references引用
CREATE TABLE `student` (
  `id` int(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
  `name` varchar(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
  `pwd` varchar(255) NOT NULL DEFAULT '123456' COMMENT '密码',
  `sex` varchar(2) NOT NULL DEFAULT '男' COMMENT '性别',
  `birthday` datetime DEFAULT NULL COMMENT '家庭住址',
  `address` varchar(100) DEFAULT NULL COMMENT '家庭住址',
  `gradeid` INT(10) NOT NULL COMMENT '年级',
  `email` varchar(50) DEFAULT NULL COMMENT '邮箱',
  PRIMARY KEY (`id`),
  KEY `FK_gradeid` (`gradeid`),
  CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade`(`gradeid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

- 删除有外键的关系表的时候，必须要先删除应用别人的表（从表），在删除被引用的表（主表）

> 方式二：创建表成功后，添加外键约束

```sql
CREATE TABLE `student` (
  `id` int(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
  `name` varchar(30) NOT NULL DEFAULT '匿名' COMMENT '姓名',
  `pwd` varchar(255) NOT NULL DEFAULT '123456' COMMENT '密码',
  `sex` varchar(2) NOT NULL DEFAULT '男' COMMENT '性别',
  `birthday` datetime DEFAULT NULL COMMENT '家庭住址',
  `address` varchar(100) DEFAULT NULL COMMENT '家庭住址',
  `gradeid` INT(10) NOT NULL COMMENT '年级',
  `email` varchar(50) DEFAULT NULL COMMENT '邮箱',
  PRIMARY KEY (`id`)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8

create table `grade`(
	`gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
	`gradename` VARCHAR(50) NOT NULL COMMENT '年级名称',
	PRIMARY KEY (`gradeid`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8

-- 创建表的时候没有外键关系
ALTER TABLE `student`
ADD  CONSTRAINT `FK_gradeid` FOREIGN KEY (`gradeid`) REFERENCES `grade`(`gradeid`);

-- ALTER TABLE表 ADD CONSTRAINT 约束名 FOREIGN KEY（作为外键约束）REFERENCES 表（字段）
```

- 以上操作都是物理外键，数据库级别的外键，我们不建议使用！（避免数据库过多造成困扰）

最佳实践

- 数据库就是单纯的表，只用来存数据，只有行（数据）和列（字段）
- 我们想使用多张表的数据，想使用外键（程序级别实现）

### 3.2、DML语言（全部记住）

 数据库意义：数据库存储，数据管理

DML语言：数据操作语言

- insert
- update
- delete

### 3.3、添加

```sql
-- 插入语句（添加）
-- insert into table ([字段，字段..]) values (value1,value2..),(..),..;
INSERT INTO `grade` (`gradename`) VALUE('大三')

-- 由于主键自增我们可以省略（如果不写表的字段，他就会一一匹配）
INSERT INTO `grade` VALUE('大四') -- 错误

-- 插入多个数据
INSERT INTO `grade`(`gradename`) VALUES('大一'),('大三')

INSERT INTO `student`(`name`,`gradeid`) VALUE('Aden',1)

INSERT INTO `student`(`name`,`gradeid`,`pwd`,`sex`) 
VALUE('Aden',1,'aaa123456','女'),('Tony',2,'aa123','男')
```

- 语法：i**nsert into table ([字段，字段..]) values (value1,value2..),(..),..;**

- **注意事项**

  1. 字段和字段之间使用英文逗号隔开
  2. 字段是可以省略的，但是后面的值必须要一一对应，不能少
  3. 可以同时插入多条数据，VALUES后面的值，需要使用，隔开即可**values (value1,value2..),(..),..;**

  

### 3.4、修改

> update 修改谁（条件）set原来的值 = 新值

```sql
-- 修改学员的名字
UPDATE `student` SET `name` = '羊大大' WHERE id = 1;

-- 不指定条件的情况下，会改动所有的表！！
UPDATE `student` SET `name` = '羊小小'

-- 修改多个属性
UPDATE `student` SET `name` = '杨大大',`email`='1725236585@qq.com' WHERE id = 1;

-- 语法
-- UDPATE 表名 set colnum_name = value,[colnum_name = value,....] where [条件]
```

- 条件：where子句 运算符 id等于某个值，大于某个值，在某个区间内修改
- 操作符会返回 boolean值

|      操作符      |    含义    |    范围     | 结果  |
| :--------------: | :--------: | :---------: | :---: |
|        =         |    等于    |     5=6     | false |
|     <>或！=      |   不等于   |    5<>6     | true  |
|        >         |    大于    |     5<6     | true  |
|        <         |    小于    |     5>6     | false |
|        <=        |  小于等于  |    5<=6     | true  |
|        >=        |  大于等于  |    5>=6     | false |
| BETWEEN...AND... | 在某个范围 |    [2.5]    |       |
|       AND        |    &&与    | 5>1 AND 1>2 | false |
|        OR        |    //或    | 5>1 OR 1>2  | true  |

```sql
-- 通过多个条件定位数据
UPDATE `student` SET `name`="羊" WHERE `name`='羊大大' AND sex='男'
```

- 语法：**UDPATE 表名 set colnum_name = value,[colnum_name = value,....] where [条件]**

- 注意事项：

  1. colnum_name 是数据库的列，尽量带上``
  2. 条件，筛选的条件，如果没有指定，则会修改所有的列（需要注意）
  3. value，是一个具体的值，也可以是一个变量
  4. 多个设置属性之间，使用英文逗号隔开

  ```sql
  UPDATE `student` SET `birthday`=CURRENT_TIME WHERE `name` = '羊' and `sex` = '男'
  ```

  

### 3.5、删除

> delete命令

- 语法：delete from 表名 [where 条件]

```sql
-- 删除数据 （避免这样）
DELETE FROM  `student`

-- 删除指定的数据
DELETE FROM  `student` WHERE id = 1
```

> TRUNCATE 命令
>
> - 作用：完全清空一个数据库表，表的结构和索引约束不会变

```sql
-- 清空student表
TRUNCATE `student`
```

> DELETE 和 TRUNCATE区别

- 相同点：都能删除数据，都不会删除表结构
- 不同点：
  - TRUNCATE 重新设置 自增列 计数器会归零
  - TRUNCATE 不会影响事务

``` sql
-- 测试delete 和 truncate 区别
CREATE TABLE `test` (
  `id` int(4) NOT NULL AUTO_INCREMENT,
  `coll` varchar(20) NOT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8

INSERT INTO `test` (`coll`) VALUES ('1'),('2'),('3')

DELETE FROM `test` -- 不会影响自增

TRUNCATE TABLE `test` -- 自增辉归零
```

- DELETE删除的问题，重启数据库，现象
  - InnoDB 自增列会从1开始（存在内存当中的，断电即消失）
  - MYISAM 继续从上一个自增量开始（存在文件中的，不会丢失）



## 4、DQL查询数据（重点）

### 4.1、DQL

> Data Query Language:数据查询语言

- 所有的查询操作都用它 Select
- 简单的查询，复杂的查询都能做
- 数据库中最核心的语言，最重要的语句
- 使用频率最高的语句

> select完整语法

![NGokh6.png](https://s1.ax1x.com/2020/06/22/NGokh6.png)

### 4.2、指定查询字段

```sql
-- 查询全部的学生 SELECT 字段 FROM 表
SELECT * FROM `student` 
SELECT * FROM result

-- 查询指定字段
SELECT `studentno`,`studentname` from student

-- 别名，给结果起一个名字，AS
SELECT `studentno` AS 学号,`studentname` AS 学生姓名 FROM student

-- 函数 Concat (A，B）
SELECT CONCAT('姓名：',studentname) as 新名字 FROM student
```

语法：SELECT 字段,... FROM 表

> 有的时候，列名不是那么见名知意。我们起别名 AS  字段名 AS 别名  表名 AS 别名



> 去重 distinct

- 作用：去除select查询出来的结果中重复出现的数据，重复的数据只显示一条

```sql
-- 查询一下有哪些同学参加了考试，成绩
SELECT * FROM result -- 查询全部的考试成绩
SELECT `studentno` FROM result -- 查询哪些同学参加了考试
SELECT DISTINCT `studentno` FROM result -- 发现重复数据，去重
```

> 数据库的列（表达式）

```sql
-- 查询系统版本（函数）
SELECT VERSION()
-- 用来计算（表达式）
SELECT 100*2+1 AS result 
-- 查询自增的步长（变量）
SELECT @@auto_increment_increment

-- 学员成绩的考试+1分查看
SELECT `studentno`,`studentresult`+1 AS '提分后' FROM result

```

- 数据库中的表达式：文本值，列，null，函数，计算表达式，系统变量...
- Select **表达式** from 表

### 4.3、where条件子句

作用：检索数据中符合条件的值

搜索的条件由一个或多个表达式组成！结果布尔值

> 逻辑运算符

| 运算符 |      语法       |             描述             |
| :----: | :-------------: | :--------------------------: |
| and && | a and b.   a&&b | 逻辑与，两个都为真，结果为真 |
| or //  | a or b    a//b  | 逻辑或，其中一个为真，则为真 |
| Not !  |  Not a.    !a   |    逻辑非，真为假，假为真    |

```sql
-- where条件子句
SELECT studentno,studentresult FROM result

-- 查询成绩在95～100之间
SELECT studentno,studentresult FROM result
WHERE studentresult>=95 AND studentresult <=100

-- AND &&
SELECT studentno,studentresult FROM result
WHERE studentresult>=95 && studentresult <=100

-- 模糊查询（区间）
SELECT studentno,studentresult FROM result
WHERE studentresult BETWEEN 95 AND 100

-- 除了1000号同学之外的同学的成绩
SELECT studentno,studentresult FROM result
WHERE studentno != 1000

--!= NOT
SELECT studentno,studentresult FROM result
WHERE NOT studentno = 1000
```



> 模糊查询：比较运算符

|   运算符    |       语法        |                     描述                      |
| :---------: | :---------------: | :-------------------------------------------: |
|   IS NULL   |     a is null     |           如果操作符为NULL结果为真            |
| IS NOT NULL |   a is not null   |         如果操作符不为null，结果为真          |
|   between   | a between b and c |           若a在b和c之间，则结果为真           |
|    like     |     a like b      |         SQL匹配，如果a匹配b，结果为真         |
|     in      | a in(a1,a2,a2...) | 假设a在a1，或者a2..其中的某一个值中，结果为真 |

```sql
-- ============模糊查询=================
-- 查询性张同学
SELECT studentno,studentname FROM student
WHERE studentname LIKE '张%'

-- 查询性张同学，名字后面只有一个字的
SELECT studentno,studentname FROM student
WHERE studentname LIKE '张_'

-- 查询名字中间有伟字的同学 %伟%
SELECT studentno,studentname FROM student
WHERE studentname LIKE '%伟%'

-- in 具体的一个或多个值，不能使用通配符
SELECT studentno,studentname FROM student
WHERE studentno IN (1000,1001)

-- 查询在广东的学生
-- in
SELECT studentno,studentname FROM student
WHERE address IN ('广东深圳')

-- null  not null
-- 查询地址为空的学生 null ''
SELECT studentno,studentname FROM student
WHERE address='' OR address IS NULL

-- 查询有出生日期的同学 不为空
SELECT studentno,studentname FROM student
WHERE borndate IS NOT NULL

-- 查询有出生日期的同学 为空
SELECT studentno,studentname FROM student
WHERE borndate IS NULL
```

### 4.4、联表查询

> JOIN 对比

![NGc3sx.png](https://s1.ax1x.com/2020/06/22/NGc3sx.png)

![NGc9iQ.png](https://s1.ax1x.com/2020/06/22/NGc9iQ.png)

```sql
-- 联表查询
-- 查询了参加考试的同学（学号，姓名，科目编号，分数）
SELECT * FROM student
SELECT * FROM result


/*思路
1.分析需求，分析查询的字段来自哪些表，（连接查询）
2.确定使用哪种连接查询？ 7种
确定交叉点（这两个表种那个数据是相同的）
*/

-- INNER JOIN
SELECT s.studentno,studentname,`subjectno`,`studentresult`
FROM student AS s
INNER JOIN result AS r
WHERE s.studentno = r.studentno

-- RIGHT JOIN
SELECT s.studentno,studentname,`subjectno`,`studentresult`
FROM student AS s
RIGHT JOIN result AS r
ON s.studentno = r.studentno

-- LEFT JOIN
SELECT s.studentno,studentname,`subjectno`,`studentresult`
FROM student AS s
LEFT JOIN result AS r
ON s.studentno = r.studentno

-- join on 连接查询
-- where   等值查询
-- 查询缺考的同学
SELECT s.studentno,studentname,`subjectno`,`studentresult`
FROM student AS s
LEFT JOIN result AS r
ON s.studentno = r.studentno
WHERE studentresult IS NULL

-- 思考 查询了参加考试的同学
SELECT s.studentno,studentname,subjectname,studentresult
FROM student s
RIGHT JOIN result r
ON	r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno

-- 我要查询哪些数据 select ...
-- 从那几个表查from 表 xxx join 连接的表 on 交叉条件
-- 假设存在一种多张表查询，慢慢来，先查询两张表然后再慢慢增加

-- From a left join b
-- From a right join b
```

| 操作       | 描述                                       |
| ---------- | ------------------------------------------ |
| inner join | 如果表中至少有一个匹配，就返回行           |
| left join  | 会从左表中返回所有的值，即使右表中没有匹配 |
| right join | 会从右表返回所有值，即使左表没有匹配       |



> 自连接

- 自己的表和自己的表连接，核心：一张表拆分为两张一样的表即可

```sql
-- 查询父子信息：把一张表看为两个一摸一样的表
SELECT a.categoryname as '父栏目',b.categoryname as '子栏目'
FROM category a
INNER JOIN category b
on a.categoryid = b.pid

-- 查询学员所属的年级（学号，学生的姓名，年级姓名）
SELECT studentno,studentname,gradename
FROM student s
INNER JOIN grade g
on s.gradeid = g.gradeid

-- 查询科目所属的年级（科目名称，年级名称）
SELECT `subjectname`,gradename
FROM `subject` sub
INNER JOIN grade g
WHERE sub.gradeid = g.gradeid

-- 查询了参加 数据库结构-1 考试的同学信息：学号，学生姓名，科目名，分数
SELECT s.studentno,studentname,subjectname,studentresult
FROM student s
INNER JOIN result r
on s.studentno = r.studentno
INNER JOIN `subject` sub
on sub.subjectno = r.subjectno
```



### 4.5、分页和排序

> 排序

```sql
-- 分页limit 和排序 order by

-- 排序 ： 升序 ASC，降序 DESC 
-- 查询结果根据成绩排序，降序
SELECT s.`studentno`,`studentname`,`subjectname`,`studentresult`
FROM student s
INNER JOIN `result` r
ON s.studentno = r.studentno
INNER JOIN `subject` sub
on r.subjectno = sub.subjectno
WHERE subjectname = '高等数学-1'
ORDER BY studentresult desc
```

> 分页

```sql
SELECT s.`studentno`,`studentname`,`subjectname`,`studentresult`
FROM student s
INNER JOIN `result` r
ON s.studentno = r.studentno
INNER JOIN `subject` sub
on r.subjectno = sub.subjectno
WHERE subjectname = '高等数学-1'
ORDER BY studentresult desc
limit 0,1

-- limit(起始值startIndex,页面大小length)
```

```sql
-- 查询Java程序设计-1 年级前10的数据

SELECT s.studentno,studentname,subjectname,studentresult
FROM student s
INNER JOIN result r
ON s.studentno = r.studentno
INNER JOIN `subject` sub
on sub.subjectno = r.subjectno
WHERE `subjectname` = 'Java程序设计-1' AND studentresult >= 85
ORDER BY studentresult DESC
LIMIT 0,10
```



### 4.6 、子查询

- where(这个值是一个新的select语句，是计算出来的)
- 本质：在where语句中嵌套一个子查询语句

```sql
-- 查询 数据库结构-1 的所有考试结果，降序排列
-- 方式一：使用连接查询
SELECT studentno,r.subjectno,studentresult
FROM result r
INNER JOIN `subject` sub
on r.subjectno = sub.subjectno
WHERE subjectname = '数据库结构-1'
ORDER BY studentresult DESC

-- 方式二：子查询
SELECT studentno,subjectno,studentresult
FROM result
WHERE subjectno = (
		SELECT subjectno
		FROM `subject`
		WHERE subjectname = '数据库结构-1'
)
ORDER BY studentresult DESC

-- 分数不小于80分的学的学号和姓名
SELECT DISTINCT s.studentno,studentname
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
WHERE studentresult >= 80

-- 查询课程为 高等数学 且分数不小于80 的同学姓名 和 学号
SELECT DISTINCT s.studentno,studentname
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON sub.subjectno = r.subjectno
WHERE subjectname = '高等数学-2' and studentresult>=80

-- 分数不小于80分的学的学号和姓名 科目为Java程序设计-1
SELECT DISTINCT s.studentno,studentname
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
WHERE studentresult >= 80 AND subjectno =(
		SELECT subjectno
		FROM `subject`
		WHERE subjectname ='高等数学-2'
)

-- 再改造
SELECT studentno,studentname FROM student WHERE studentno IN(
	SELECT studentno FROM result WHERE studentresult>=80 AND subjectno=(
		SELECT subjectno FROM `subject` WHERE subjectname = '高等数学-2'
	)
)
```



### 4.7、过滤和分组

```sql
-- 查询不同课程的平均分，最高分，最低分
-- 核心： （根据不同的课程分组）
SELECT `subjectname`,AVG(studentresult) AS 平均分,MAX(studentresult) as 最高分, MIN(studentresult) AS 最低分
FROM result r
INNER JOIN `subject` sub
ON sub.`subjectno` = r.`subjectno`
GROUP BY r.`subjectno`
HAVING 平均分 > 80
```



### 4.8、select小结

![NJg2eP.png](https://s1.ax1x.com/2020/06/22/NJg2eP.png)

 

## 5、函数

官网：https://dev.mysql.com/doc/refman/5.7/en/func-op-summary-ref.html

### 5.1、常用函数

```sql
-- 常用函数

-- 数学运算
SELECT ABS(-8) -- 绝对值
SELECT CEILING(9.4) -- 向上取整
SELECT FLOOR(9.4) -- 向下取整
SELECT RAND() -- 返回一个0～1之间的随机数
SELECT SIGN(-10) -- 判断一个数的符号 0-0 负数返回 -1，正数返回 1

-- 字符串函数
SELECT CHAR_LENGTH('抬头仰望星空，路上脚踏实地') -- 字符串长度
SELECT CONCAT('我','爱','打羽毛球') -- 拼接字符串
SELECT INSERT('我爱打羽毛球',2,1,'super爱') -- 查询，从某个位置开始替换某个长度
SELECT LOWER('KiDjAyA') -- 转小写
SELECT UPPER('kidjaya') -- 转大写
SELECT REPLACE('时间就是金钱','就','必须') -- 替换指定字符串
SELECT SUBSTR('123456',4,6) -- 截取字符串
SELECT REVERSE('123456') -- 反转

-- 查询姓张的同学 ，名字改为 章
SELECT REPLACE(studentname,'张','章')
FROM student
WHERE studentname LIKE '张%'

-- 时间日期函数（记住）
SELECT CURRENT_DATE() -- 获取当前时间 （DATE）
SELECT CURDATE() -- 获取当前时间 （DATE）
SELECT NOW() -- 获取当前时间 （DATETIME）
SELECT LOCALTIME() -- 获取本地时间 （DATETIME）
SELECT SYSDATE() -- 获取系统时间 （DATETIME）

SELECT YEAR(NOW())
SELECT MONTH(NOW())
SELECT DAY(NOW())
SELECT HOUR(NOW())
SELECT MINUTE(NOW())
SELECT SECOND(NOW())

-- 系统
SELECT SYSTEM_USER()
SELECT USER()
SELECT VERSION()
```



### 5.2、聚合函数(超常用)

| 函数名称 | 描述   |
| -------- | ------ |
| COUNT()  | 计数   |
| SUM()    | 求和   |
| AVG()    | 平均值 |
| MAX()    | 最大值 |
| MIN()    | 最小值 |
| ......   | ...... |

```sql
-- 聚合函数
SELECT COUNT(borndate) FROM student -- COUNT(指定列) 忽略所有的null值
SELECT COUNT(*) FROM student -- COUNT(*) 不会忽略null值
SELECT COUNT(1) FROM student -- COUNT(1) 忽略所有的null值

SELECT SUM(`studentresult`) AS 总和 FROM result
SELECT AVG(`studentresult`) AS 总和 FROM result
SELECT MAX(`studentresult`) AS 总和 FROM result
SELECT MIN(`studentresult`) AS 总和 FROM result
```

```sql
-- 查询不同课程的平均分，最高分，最低分
-- 核心： （根据不同的课程分组）
SELECT `subjectname`,AVG(studentresult) AS 平均分,MAX(studentresult) as 最高分, MIN(studentresult) AS 最低分
FROM result r
INNER JOIN `subject` sub
ON sub.`subjectno` = r.`subjectno`
GROUP BY r.`subjectno`
HAVING 平均分 > 80
```



### 5.3、数据库级别的MD5加密(扩展)

什么是MD5？

- 主要是不可逆
- md5破解网站的原理，背后有一个字典，md5加密后的值，穷举法

```sql
CREATE TABLE `testmd5`(
	`id` INT(4) NOT NULL,
	`name` VARCHAR(20) NOT NULL,
	`pwd` VARCHAR(50) NOT NULL,
	PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

-- 明文密码 
INSERT INTO testmd5 VALUES(1,'aaa','123456'),(2,'bbb','123456'),(3,'ccc','123456')

-- 加密
UPDATE testmd5 set pwd = MD5(pwd) -- 加密全部密码

-- 插入时加密
INSERT INTO testmd5 VALUES(4,'cchealter',MD5('123456'))

-- 校验用户
SELECT * FROM testmd5 WHERE `name`='cchealter' AND `pwd`=MD5('123456')
```

  

## 6、事务

### 6.1、什么是事务

- 要么都成功，要么都失败
- 例子：银行转账
- 将sql放在一个批次去执行

> 事务原则：ACID原则 原子性 一致性 隔离性 持久性

- 原子性
  - 要么都成功，要么都失败

- 一致性
  - 事务前后的数据完整性要保持一致

![NJqn5d.png](https://s1.ax1x.com/2020/06/22/NJqn5d.png)

- 持久性 -- 事务提交
  - 事务一旦提交则不可逆，被持久化到数据库当中

![NJqtaQ.png](https://s1.ax1x.com/2020/06/22/NJqtaQ.png)

- 隔离性 isolation

  - 事务的隔离性是多个用户并发访问数据库时，数据库为每一个用户开启了事务，不能被其他事务的操作数据锁干扰，事务之间要互相隔离

  > 隔离导致的一些问题

- 脏读

  - 指一个事务读取了另一个事务未提交的数据（回滚的事务）

- 不可重复读

  - 在一个事务内读取表中的某一行数据，多次读取结果不同。
  - 与脏读的区别：脏读是读到未提交的数据，而不可重复读读到的却是已经提交的数据，但实际上是违反了事务的一致性原则。

- 虚读

  - 是指在一个事务内读取到了别的事务插入的数据，导致前后读取不一致

  

> 执行事务

```sql
-- 事务
-- mysql是默认开始事务的自动提交
set autocommit = 0 /*关闭*/
set autocommit = 1 /*开启（默认的）*/

-- 手动处理事务
SET autocommit = 0 -- 关闭事务自动提交

-- 事务开启
START TRANSACTION -- 标记一个事务的开始，从这之后的sql都在同一个事务内

UPDATE xxx
UPDATE xxx
INSERT xxx

-- 提交：持久化（成功）
COMMIT
-- 回滚： 回到原来的样子（失败）
ROLLBACK

-- 事务结束
SET autocommit = 1 -- 开启事务自动提交

-- 了解
SAVEPOINT 保存点 -- 设置一个事务的保存点
ROLLBACK TO SAVEPOINT 保存点 -- 回滚到保存点
RELEASE SAVEPOINT 保存点 -- 释放保存点
```

- 模拟转账

```sql
create TABLE `account`(
	`id` int(3) not NULL auto_increment,
	`nmae` VARCHAR(30) NOT NULL,
	`money` DECIMAL(9,2) NOT NULL,
	PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

create DATABASE shop character set utf8 COLLATE utf8_general_ci

ALTER TABLE account CHANGE nmae name VARCHAR(30)

INSERT INTO account (`name`,`money`)
VALUES('A',2000.00),('B',10000.00)

-- 模拟转帐
SET autocommit = 0 -- 关闭自动提交
START TRANSACTION -- 开启一个事务（一组事务）

UPDATE account SET money = money-500 WHERE `name` = 'A' -- A - 500
UPDATE account SET money = money+500 WHERE `name` = 'B' -- B + 500

COMMIT; -- 提交事务
ROLLBACK; -- 回滚

set autocommit = 1 -- 恢复默认值
```

## 7、索引

> MySQL官方对索引的定义为：索引（index）是帮助Mysql搞笑获取数据的数据结构
>
> 作用：提取句子主干，就可以得到索引的本质-->数据结构

### 7.1、索引的分类

- 主键索引：PRIMARY KEY
  - 唯一的标识，主键不可重复，只能有一个列作为主键
- 唯一索引：UNIQUE KEY
  - 避免重复的列出现，唯一索引可以设置多个，多个列都可以标识为唯一索引
- 常规索引：KEY/INDEX
  - 默认的，index、key关键字来设置
- 全文索引：FULLTEXT
  - 在特定的数据库引擎下才有，MyISAM
  - 快速定位数据



- 基础语法

```sql
-- 索引的使用
-- 1. 在创建表的时候给字段增加索引
-- 2. 创建完毕后，增加索引

-- 显示所有的索引信息
SHOW INDEX FROM student

-- 增加一个索引
ALTER TABLE school.student ADD FULLTEXT INDEX `studentname` (`studentname`)

-- EXPLAIN 分析sql执行的状况
EXPLAIN SELECT * FROM student; -- 非全文索引

EXPLAIN SELECT * FROM student WHERE MATCH(studentname) AGAINST('张')
```

### 7.2、测试索引

```sql
-- 索引的使用
-- 1. 在创建表的时候给字段增加索引
-- 2. 创建完毕后，增加索引

-- 显示所有的索引信息
SHOW INDEX FROM student

-- 增加一个索引
ALTER TABLE school.student ADD FULLTEXT INDEX `studentname` (`studentname`)

-- EXPLAIN 分析sql执行的状况
EXPLAIN SELECT * FROM student; -- 非全文索引

EXPLAIN SELECT * FROM student WHERE MATCH(studentname) AGAINST('张')


CREATE TABLE `app_user` (
`id` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
`name` varchar(50) DEFAULT '',
`eamil` varchar(50) NOT NULL,
`phone` varchar(20) DEFAULT '',
`gender` tinyint(4) unsigned DEFAULT '0',
`password` varchar(100) NOT NULL DEFAULT '',
`age` tinyint(4) DEFAULT NULL,
`create_time` datetime DEFAULT CURRENT_TIMESTAMP,
`update_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8


-- 插入100万数据
DELIMITER $$ -- 写函数之前必须要写，标志
CREATE FUNCTION mock_data()
RETURNS INT
BEGIN
	DECLARE num INT DEFAULT 1000000;
	DECLARE i int DEFAULT 0;
	
	WHILE i<num DO
		-- 插入语句
		INSERT INTO app_user(`name`,`email`,`phone`,`gender`,`password`,`age`)
		VALUES(CONCAT('用户',i),'123456@qq.com',CONCAT('18',FlOOR(RAND()*(99999999-100000000)+100000000)),FLOOR(RAND()*2),UUID(),FlOOR(RAND()*100));
		SET i = i + 1;
	END WHILE;
	RETURN i;
END;

SELECT mock_data()

ALTER TABLE app_user change eamil email varchar(50)

INSERT INTO app_user(`name`,`email`,`phone`,`gender`,`password`,`age`)
VALUES(CONCAT('用户',i),'123456@qq.com',CONCAT('18',FlOOR(RAND()*(99999999-100000000)+100000000)),
FLOOR(RAND()*2),UUID(),FlOOR(RAND()*100))

SELECT * FROM app_user WHERE `name` = '用户9999'; -- 0.367
SELECT * FROM app_user WHERE `name` = '用户9999'; -- 0.348
SELECT * FROM app_user WHERE `name` = '用户9999'; -- 0.348

EXPLAIN SELECT * FROM app_user WHERE `name` = '用户9999'; -- 90多万

-- 创建索引规范 id_表名_字段名
-- CREATE index 索引名 on 表（字段）
CREATE INDEX id_app_user_name ON app_user(`name`)
SHOW INDEX FROM app_user

SELECT * FROM app_user WHERE `name` = '用户9999'; -- 0秒 // 空间换时间
EXPLAIN SELECT * FROM app_user WHERE `name` = '用户9999'; -- 1  唯一定位
```

- 加了索引

![NYlXhF.png](https://s1.ax1x.com/2020/06/22/NYlXhF.png)

- 没加索引

![NY8Eyd.png](https://s1.ax1x.com/2020/06/22/NY8Eyd.png)

- 索引在小数据量的时候，用处不大，但是在大数据的时候，区别十分明显

### 7.3、索引原则

- 索引不是越多越好
- 不要对经常变动数据加索引
- 小数据量的表不需要加索引
- 索引一般加在常用来查询的字段上



> 索引的数据结构

Hash类型索引

Btree：innoDB的默认结构

博客：https://blog.codinglabs.org/articles/theory-of-mysql-index.html



## 8、权限管理和备份

### 8.1、用户管理

> 添加用户 图形化管理界面

![NaDE0x.png](https://s1.ax1x.com/2020/06/24/NaDE0x.png)

> SQL命令操作

用户表：mysql.user

本质：读这张表进行增删改查

```sql
-- 创建用户 CREATE USER 用户名 IDENTIFIED BY '密码'
创建用户 CREATE USER yang IDENTIFIED BY '123456'

-- 修改密码（当前用户）
SET PASSWORD = PASSWORD('aaa791654109')

-- 修改密码（指定用户）
SET PASSWORD FOR yang = PASSWORD('aaa791654109')

-- 重命名 RENAME USER 原来的名字 TO 新改的名字
RENAME USER yang TO yangdada

-- 用户授权 ALL PRIVILEGES 全部的权限，库.表
-- ALL PRIVILEGES 除了给别人授权（grant），其他所有权限都有
GRANT ALL PRIVILEGES ON *.* TO yangdada

-- 查看权限
SHOW GRANTS FOR yangdada -- 查看指定用户的权限
SHOW GRANTS FOR root@localhost
-- GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION

-- 撤销权限 REVOKE 哪些权限，给哪些库撤销，给谁撤销
REVOKE ALL PRIVILEGES ON *.* FROM yangdada

-- 删除用户
DROP USER yangdada
```



### 8.2、MySQL备份

为什么要备份

- 保证重要的数据不丢失
- 数据转移

MySQL数据库备份的方式

- 直接拷贝物理文件
- 在可视化数据库管理软件手动导出
  - 右键导出
  - 可以选择只要结构 或者 结构和数据
- 使用命令行导出mysqldump 命令行使用

```sql
-- mysqldump -h 主机 -u 用户名 -p 密码 数据库 [表名] > 物理磁盘/文件名
mysqldump -hlocalhost -uroot -p book_store notice >/Users/kidjaya/Downloads/notice.sql

-- 导出多张表 mysqldump -h 主机 -u 用户名 -p 密码 数据库 [表1 T2 T3...] > 物理磁盘/文件名
mysqldump -hlocalhost -uroot -p book_store notice student type >/Users/kidjaya/Downloads/muti.sql

-- 导入
-- 登陆的情况下，导入的表的话，先切入指定的数据库
-- source 备份文件（磁盘地址）
source .../dir/...
-- 未登陆情况
mysql -u用户名 -p密码 库名 < 备份文件（磁盘地址）
```

- 作用：容灾，分享



## 9、规范数据库设计

### 9.1、为什么要设计

- 当数据库比较复杂的时候，我们就要设计了

**糟糕的数据库设计：**

- 数据冗余，浪费空间
- 数据库插入和删除都会麻烦、异常
- 程序的性能差

**良好的数据库设计：**

- 节省内存空间
- 保证数据的完整性（屏蔽使用物理外键）
- 方便我们开发系统

**软件开发中，关于数据库的设计**

- 分析需求：分析业务和需要处理的数据库请求
- 概要设计：设计关系图E-R图

**设计数据库的步骤（个人博客）**

- 收集信息，分析需求
  - 用户表（用户登陆注销，用户的个人信息，写博客，创建分类）
  - 分类表（文章分类，谁创建的）
  - 评论表
  - 文章表（文章的信息）
  - 友链表（友链信息）
  - 自定义表：（系统信息，某个关键的字，或者一些主字段）key：value （ps：有点像字典）
- 标识实体（把需求落到每个字段）
- 标识实体之间的关系
  - 写博客：user->blog
  - 创建分类：user->category
  - 关注：user->user
  - 友链：links
  - 评论：user->user->blog



### 9.2、三大范示

**为什么需要数据库规范化？**

- 信息重复
- 更新异常
- 插入异常
  - 无法正常显示信息
- 删除异常
  - 丢失有用信息



> 三大范示

- 第一范式
  - 原子性：保证每一列不可再分
- 第二范式
  - 前提：满足第一范式
  - 每张表只描述一件事情
- 第三范式
  - 前提：满足第一范式和第二范式
  - 需要确保数据表中的每一列数据都和主键直接相关，而不能间接相关

- 规范数据库的设计
  - 关联查询的表不得超过三张表
    - 考虑商业化的需求和目标，数据库的性能更加重要
    - 在规范性能的问题的时候，需要适当考虑一下规范性
    - 故意给某些表增加一些冗余字段。（从多张表查询变为单表查询）
    - 故意增加一些计算列（从大数据量降低为小数据量的查询，索引（大数据占内存））



## 10、JDBC（重点）

### 10.1、数据库驱动

- 驱动：声卡、显卡、数据库

![Nd9dGq.png](https://s1.ax1x.com/2020/06/24/Nd9dGq.png)

- 驱动是什么？
  - 驱动是指**驱动**计算机里软件的程序。是添加到**操作系统**中的特殊程序，其中包含**有关硬件设备的信息**。此信息能够**使计算机与相应的设备进行通信**。驱动程序是硬件厂商根据操作系统编写的配置文件，可以说没有驱动程序，计算机中的**硬件就无法工作**。



- 我们的程序会通过数据库驱动和数据库打交道



### 10.2、JDBC

- SUN公司为了简化开发人员的（对数据的统一）操作，提供了一个（Java操作数据库的）规范，俗称JDBC。
- 这些规范由数据库厂商来实现
- 开发人员只要学习JDBC即可

- 需要的包
  - java.sql
  - Javax.sql
  - 驱动包 mysql-connector-java-xxx.xx.jar



### 10.3、第一个JDBC

> 创建测试数据库

```sql
CREATE DATABASE jdbcStudy CHARACTER SET utf8 COLLATE utf8_general_ci;

USE jdbcStudy;

CREATE TABLE `users`(
id INT PRIMARY KEY,
NAME VARCHAR(40),
PASSWORD VARCHAR(40),
email VARCHAR(60),
birthday DATE
);

INSERT INTO `users`(id,NAME,PASSWORD,email,birthday)
VALUES(1,'zhansan','123456','zs@sina.com','1980-12-04'),
(2,'lisi','123456','lisi@sina.com','1981-12-04'),
(3,'wangwu','123456','wangwu@sina.com','1979-12-04')
```

1. 创建普通项目
2. 导入mysql数据库驱动 并设置为library
3. 编写测试代码

```java
package xyz.kidjaya.lesson01;

import java.sql.*;

//我的JDCB测试程序
public class JdbcFirstDemo {
    public static void main(String[] args) throws ClassNotFoundException, SQLException {
        //1.加载驱动
        Class.forName("com.mysql.jdbc.Driver");

        //2.用户信息和url
        String url = "jdbc:mysql://localhost:3306/jdbcStudy?useUnicode=true&characterEncoding=utf8&useSSl=true";
        String username = "root";
        String password = "aaa791654109";

        //3.链接成功，数据库对象
        Connection connection = DriverManager.getConnection(url, username, password);

        //4.执行sql对象
        Statement statement = connection.createStatement();

        //5.执行sql的对象 去 执行sql语句，可能存在结果，查看返回结构
        String sql = "SELECT * FROM users";

        ResultSet resultSet = statement.executeQuery(sql);//返回的结果集,封装了查询结果

        while (resultSet.next()){
            System.out.println("id="+ resultSet.getObject("id"));
            System.out.println("NAME="+ resultSet.getObject("NAME"));
            System.out.println("PASSWORD="+ resultSet.getObject("PASSWORD"));
            System.out.println("email="+ resultSet.getObject("email"));
            System.out.println("birthday="+ resultSet.getObject("birthday"));
            System.out.println("=======");
        }

        //6.释放连接

        resultSet.close();
        statement.close();
        connection.close();
    }
}
```

- 步骤总结：
  1. 加载驱动
  2. 连接数据库DriverManger
  3. 获得执行sql的对象Statement
  4. 获得返回的结果集
  5. 释放连接

> DriverManger

```java
//DriverManager.registerDriver(new com.mysql.jdbc.Driver());//注册两次
Class.forName("com.mysql.jdbc.Driver");//固定写法，加载驱动

Connection connection = DriverManager.getConnection(url, username, password);
//cconnection 代表数据库
//数据库设置自动提交
connection.setAutoCommit()
//事务提交
connection.commit()
//事务回滚
connection.rollback()
```

> URL

```java
String url = "jdbc:mysql://localhost:3306/jdbcStudy?useUnicode=true&characterEncoding=utf8&useSSl=true";
//mysql -- 3306
// 协议://主机地址:端口号/数据库名?参数&参数&参数&参数

//oralce -- 1521
//jdbc:oracle:thin@localhost:1521:sid
```

>  Statement 执行SQL对象 PrepareStatement执行SQL对象

```java
statement.executeQuery();//查询返回resultSet
statement.execute();//执行任何sql 相对效率低 判断的过程
statement.executeUpdate();//更新，插入，删除，都是用这个，返回int 受影响行数
```

> ResultSet 查询的结果集，封装了查询到的所有数据

- 获得指定的数据类型

```java
 resultSet.getObject();//在不知道列的情况下使用
// 如果知道列的类型就使用指定类型
resultSet.getString();
resultSet.getInt();
resultSet.getDouble();
....
```

- 遍历、指针

```java
 resultSet.beforeFirst();// 移动到最前面
resultSet.afterLast();// 移动到最后面
resultSet.next();// 移动到下一个数据
resultSet.previous();// 移动到前一行
int row = 1;
resultSet.absolute(row);// 移动到指定行
```

> 释放资源

```java
 //6.释放连接

resultSet.close();
statement.close();
connection.close();//不关闭耗费资源
```



### 10.4、statement对象

- JDBC中的statement对象用于向数据发送SQL语句，想完成对数据库的增删改查，只需要通过这个对象想数据库发送增删改查语句即可
- Statement对象的executeUpdate方法，由于想数据库发送增、删、改的sql语句，executeUpdate执行完之后，将会返回一个整数（即增删改语句导致了数据库几行数据发生了变化）。
- Statement.executeQuery方法用于向数据库发送查询语句，executeQuery方法返回代表查询结果的ResultSet对象。

> CRUD操作 -create

使用executeUpdate(String sql) 方法完成数据库添加操作

```java
Statement st = conn.createStatment();
String sql = "insert into user(...) values(...)";
int num = st.executeUpdate(sql);
if(num>0){
  System.out.print("插入成功");
}
```

> CRUD操作-delete

使用executeUpdate(String sql) 方法完成数据库删除操作

```java
Statement st = conn.createStatment();
String sql = "delete from user where id = 1";
int num = st.executeUpdate(sql);
if(num>0){
  System.out.print("修改成功");
}
```

> CRUD 操作-update

使用executeUpdate(String sql) 方法完成数据库更新操作

```java
Statement st = conn.createStatment();
String sql = "update user set name = '' where name = ''";
int num = st.executeUpdate(sql);
if(num>0){
  System.out.print("修改成功");
}
```

> CRUD操作-query

使用executeQuery(String sql) 方法完成数据库查询操作

```java
Statement st = conn.createStatment();
String sql = "select * from user where id = 1";
ResultSet res = st.executeQuery(sql);
while(res.next()){
  // 根据获取列的数据类型，分别调用rs相应的方法映射到java对象中
}
```



> 代码实现

1. 提取工具类

```java
package xyz.kidjaya.lesson02.utils;

import java.io.IOException;
import java.io.InputStream;
import java.sql.*;
import java.util.Properties;

public class JdbcUtils {
    private static String driver = null;
    private static String url = null;
    private static String username = null;
    private static String password = null;
    static {
        try {
            InputStream in = JdbcUtils.class.getClassLoader().getResourceAsStream("db.properties");
            Properties properties = new Properties();
            properties.load(in);

            driver = properties.getProperty("driver");
            url = properties.getProperty("url");
            username = properties.getProperty("username");
            password = properties.getProperty("password");

            Class.forName(driver);

        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }

    public static Connection getConnection() throws SQLException {
        return DriverManager.getConnection(url,username,password);
    }

    public static void  release(Connection conn, Statement statement, ResultSet resultSet) throws SQLException {
        if (resultSet!=null){
            resultSet.close();
        }

        if (statement!=null){
            statement.close();
        }

        if (conn!=null){
            conn.close();
        }
    }
}
```

2. 编写增删改的方法：**executeUpdate**

```java
//这里以Update为例
public class TestUpdate {
    public static void main(String[] args) {
        Connection conn = null;
        Statement statement = null;
        ResultSet resultSet = null;

        try {
            conn = JdbcUtils.getConnection();
            statement = conn.createStatement();
            String sql = "update users set `NAME` = 'yangdada' where id = 1";
            int res = statement.executeUpdate(sql);
           if (res>0){
               System.out.println("更新成功");
           }


        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                JdbcUtils.release(conn,statement,resultSet);
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

3.查询

```java
public class TestInsert {
    public static void main(String[] args) {
        Connection conn = null;
        Statement statement = null;
        ResultSet resultSet = null;

        try {
            conn = JdbcUtils.getConnection();
            statement = conn.createStatement();
            String sql = "select * from users where id = 1";
            ResultSet res = statement.executeQuery(sql);
            if (res.next()){
                System.out.println(res.getObject("NAME"));
                System.out.println(res.getObject("email"));
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                JdbcUtils.release(conn,statement,resultSet);
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```



>  Sql注入的问题

- sql存在漏洞，会被攻击导致数据泄漏，**SQL会被拼接 or**

```java
public class LoginTest {
    public static void main(String[] args) {
        try {
//            login("yangdada","123456");//正常登陆
            login("'or' 1=1","'or' 1=1");//sql注入
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    
    public static void login(String username, String password) throws SQLException {
        String sql = "select * from users where NAME = '"+username+"' and  password = '"+password+"'";
        Connection connection = JdbcUtils.getConnection();
        Statement statement = connection.createStatement();
        ResultSet resultSet = statement.executeQuery(sql);
        while (resultSet.next()){
            System.out.println("resultSet.getObject(\"name\") = " + resultSet.getObject("name"));
            System.out.println("resultSet.getObject(\"password\") = " + resultSet.getObject("password"));
        }

    }
}
```

### 10.5、PrepareStatement

- PrepareStatement可以防止SQL注入。效率更好

1. 普通例子（插入数据）

```java
public class TestInsert {
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement preparedStatement = null;

        try {
            connection = JdbcUtils.getConnection();
            //区别
           String sql = "insert into users(id,`NAME`,`PASSWORD`,`email`,`birthday`) values (?,?,?,?,?)";
           preparedStatement = connection.prepareStatement(sql);//预编译sql，先写sql，然后不执行
            //手动给参数赋值
            preparedStatement.setInt(1,4);
            preparedStatement.setString(2,"cc");
            preparedStatement.setString(3,"123456");
            preparedStatement.setString(4,"123456@qq.com");
            //注意点：sql.Date 数据库
            //       util.Date Java new Date().getTime 获得时间戳
            preparedStatement.setDate(5,new Date(new java.util.Date().getTime()));

            //执行
            int i = preparedStatement.executeUpdate();
            if (i>0){
                System.out.println("插入成功");
            }
        } catch (SQLException e) {
            e.printStackTrace();
        } finally {
            try {
                JdbcUtils.release(connection,preparedStatement,null);
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```

1. 防止sql注入

```java
public class SQL {
    public static void main(String[] args) {
        try {
            login("yangdada","123456");//正常登陆
//            login("'or' 1=1","'or' 1=1");//sql注入
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }

    public static void login(String username,String password) throws SQLException {
        //PreparedStatement 防止sql注入的本质，把传递进来的参数当作字符
        //假设其中存在转义字符 比如' 就会被直接转义为字符
        String sql = "select * from users where NAME = ? and  password = ?";
        Connection connection = JdbcUtils.getConnection();
        PreparedStatement statement = connection.prepareStatement(sql);
        statement.setString(1,username);
        statement.setString(2,password);
        ResultSet resultSet = statement.executeQuery();
        while (resultSet.next()){
            System.out.println("resultSet.getObject(\"name\") = " + resultSet.getObject("name"));
            System.out.println("resultSet.getObject(\"password\") = " + resultSet.getObject("password"));
        }
    }
}
```

### 10.7、使用idea连接数据库

1. 添加数据库

![NwCMUH.png](https://s1.ax1x.com/2020/06/24/NwCMUH.png)

2. 测试数据库连接

![NwCw5j.png](https://s1.ax1x.com/2020/06/24/NwCw5j.png)

3. 选择要连接的数据库

![NwCWa4.png](https://s1.ax1x.com/2020/06/24/NwCWa4.png)

4.双击可查看表数据

![NwCvid.png](https://s1.ax1x.com/2020/06/24/NwCvid.png)

5. idea编写sql语句

![NwPZJs.png](https://s1.ax1x.com/2020/06/24/NwPZJs.png)

6. 修改表字段需要手动点击提交数据

![NwPmzq.png](https://s1.ax1x.com/2020/06/24/NwPmzq.png)

其他注意事项：

- 连接出现异常可查看版本问题

![NwPKyV.png](https://s1.ax1x.com/2020/06/24/NwPKyV.png)



### 10.8、事务

- 要么都成功要么都失败

> ACID原则

- 原子性：要么全部完成，要么都不完成
- 一致性：总数不变
- 隔离性：多个进程互不干扰
- 持久性：一旦提交不可逆，持久化到数据库

- 隔离性的问题：
  - 脏读：一个事务读取了另一个没有提交的事务
  - 不可重复度：在同一事务内，重复读取表中的数据，表数据发送了改变
  - 幻读：在一个事务内，读取到了别人插入的数据，导致前后读出来结果不一致



> 代码实现

1. 开启事务 **connection.setAutoCommit(false)**
2. 一组业务执行完毕，提交事务
3. 可以在catch语句中显式的定义回滚语句，默认失败就会回滚

- 代码实现

```java
public class TestTransaction {
    public static void main(String[] args) {
        Connection connection = null;
        PreparedStatement preparedStatement = null;
        ResultSet resultSet = null;

        try{
            connection = JdbcUtils.getConnection();
            connection.setAutoCommit(false);//开启事务

            String sql = "update account set money = money - 100 where name = 'A'";
            preparedStatement = connection.prepareStatement(sql);
            preparedStatement.executeUpdate();

//            int x = 1/0;//报错

            String sql2 = "update account set money = money + 100 where name = 'B'";
            preparedStatement = connection.prepareStatement(sql2);
            preparedStatement.executeUpdate();

            //业务完毕，提交事务
            connection.commit();
            System.out.println("success");
        } catch (SQLException e) {
            try {
                connection.rollback();
            } catch (SQLException ex) {
                ex.printStackTrace();
            }
            e.printStackTrace();
        } finally {
            try {
                JdbcUtils.release(connection,preparedStatement,resultSet);
            } catch (SQLException e) {
                e.printStackTrace();
            }
        }
    }
}
```



### 10.9、数据库连接池

数据库连接---执行完毕---释放 （连接--释放。十分浪费系统资源）

- **池化技术：准备一些预先的资源，过来就连接预先准备好的**

最小连接数：10

最大连接数：15

等待超时：100ms

编写连接池，实现一个接口DataSource

> 开源数据源实现（拿来即用）

- DBCP
- C3P0
- Druid：阿里巴巴

使用了这些数据库连接池之后，我们项目开发就不需要编写连接数据库代码了



> DBCP

- 需要用到的jar包
  - commons-dbcp.xxx.jar
  - commoms-pool.xxx.jar

 ```java
static {
		try {
			Class.forName("com.mysql.jdbc.Driver");
			Properties properties = new Properties();
      //配置文件 不用自己写Properties命令
			InputStream inputStream = JdbcDbPool.class.getClassLoader().getResourceAsStream("dbcpconfig.properties");
			properties.load(inputStream);
			basicDataSource = BasicDataSourceFactory.createDataSource(properties);
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
	}
 
	//禁止实例化
	
	private DbPool() {
		super();
		// TODO Auto-generated constructor stub
	}
	//获取连接
	public static Connection getConnection() throws SQLException {
		return basicDataSource.getConnection();
	}
	//释放连接
	public static void close(Connection conn) {
		try {
			if( conn != null && !conn.isClosed())
				conn.close();
		} catch (Exception e) {
			// TODO: handle exception
			e.printStackTrace();
		}
	}
 ```



> C3P0

- 需要的jar包
  - c3p0.xxx.jar
  - change-commons-java-xxx
- 参考博客：https://blog.csdn.net/qq_18895659/java/article/details/51813875

```java
//三种创建方式

import java.beans.PropertyVetoException;
import java.sql.Connection;
import java.sql.SQLException;
import org.junit.Test;
 
import com.mchange.v2.c3p0.ComboPooledDataSource;
 
/**
 * c3p0
 * 性能好，开源以后一般用它
 */
public class Demo1 {
	/**
	 * 代码配置
	 * @throws PropertyVetoException
	 * @throws SQLException
	 */
//	@Test
	public void connPool1() throws PropertyVetoException, SQLException {
		// 创建连接池对象
		ComboPooledDataSource dataSource = new ComboPooledDataSource();
		// 对池进行四大参数的配置
		dataSource.setDriverClass("oracle.jdbc.driver.OracleDriver");
		dataSource.setJdbcUrl("jdbc:oracle:thin:@localhost:1521:orcl");
		dataSource.setUser("chj");
		dataSource.setPassword("chj");	
		// 池配置
		//每次新增多少连接
		dataSource.setAcquireIncrement(5);
		//初始连接数多少
		dataSource.setInitialPoolSize(20);
		//最少连接数
		dataSource.setMinPoolSize(2);
		//最大连接数
		dataSource.setMaxPoolSize(50);
		Connection con = dataSource.getConnection();
		System.out.println(con);
		con.close();
	}
	
	/**
	 * 配置文件的默认配置
	 * @throws SQLException 
	 */
//	@Test
	public void connPool2() throws SQLException{
		/**
		 * 在创建连接池对象时，这个对象就会自动加载配置文件！不用我们来指定
		 */
		ComboPooledDataSource dataSource  = new ComboPooledDataSource();
		
		Connection con = dataSource.getConnection();
		System.out.println(con);
		con.close();
	}
	
	/**
	 * 使用命名配置信息
	 * @throws SQLException
	 */
	@Test
	public void connPool3() throws SQLException{
		/**
		 * 构造器的参数指定命名配置元素的名称！
		 * <named-config name="oracle-config"> 
		 */
		ComboPooledDataSource dataSource  = new ComboPooledDataSource("oracle-config");
		
		Connection con = dataSource.getConnection();
		System.out.println(con);
		con.close();
	}
}
```

```xml
<!-- c3p0-config.xml -->

<?xml version="1.0" encoding="UTF-8"?>
<c3p0-config>
	<!-- 这是默认配置信息 -->
	<default-config> 
		<!-- 连接四大参数配置 -->
		<property name="jdbcUrl">jdbc:mysql://localhost:3306/mydb3</property>
		<property name="driverClass">com.mysql.jdbc.Driver</property>
		<property name="user">root</property>
		<property name="password">123</property>
		<!-- 池参数配置 -->
		<property name="acquireIncrement">3</property>
		<property name="initialPoolSize">10</property>
		<property name="minPoolSize">2</property>
		<property name="maxPoolSize">10</property>
	</default-config>
 
	<!-- 专门为oracle提供的配置信息 -->
	<named-config name="oracle-config"> 
		<property name="jdbcUrl">jdbc:oracle:thin:@localhost:1521:orcl</property>
		<property name="driverClass">oracle.jdbc.driver.OracleDriver</property>
		<property name="user">luowg</property>
		<property name="password">luowg</property>
		<!-- 连接不足时,会自动创建连接，这里指定会创建多少连接 -->
		<property name="acquireIncrement">3</property>
		<property name="initialPoolSize">10</property>
		<property name="minPoolSize">2</property>
		<property name="maxPoolSize">10</property>
	</named-config>
```



> 结论

- 无论使用什么数据源（DataSource），本质还是一样，DataSource接口不会变，方法就不会变