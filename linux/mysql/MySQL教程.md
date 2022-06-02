# MySQL教程
[TOC]
##一、安装与卸载
### 1.Ubuntu在线安装MySQL

    #安装MySQL服务器、核心程序
    sudo apt-get install mysql-server
    #安装MySQL客户端
    sudo apt-get install mysql-client
### 2.Ubuntu彻底卸载MySQL

    #首先删除mysql
    sudo apt-get remove mysql-*
    #清理残留数据
    dpkg -l|grep ^rc|awk'{print $2}'|sudo xargs dpkg -P
### 3.运行/停止/重启MySQL服务器

    sudo service mysql start
    sudo service mysql stop
    sudo service mysql restart
### 4.Ubuntu下mysql5.7修改密码

```
show databases；
use mysql;
update user set authentication_string=PASSWORD("yourpassword") where user='root';
update user set plugin="mysql_native_password";
flush privileges;
quit;
    
```






### 5.尝试MySQL
#### （1）打开MySQL

    #启动MySQL服务
    sudo service mysql start
    #使用root用户登录，******为密码
    mysql -uroot -p******
#### （2）基本操作

    #查看所有数据库
    show databases;
    #连接数据库
    use <database_name>;
    #查看表
    show tables;
    #退出
    quit;
    exit;
## 二、创建数据库并插入数据
### 1.示例

    #创建数据库
    CREATE DATABASE <database_name>;
    SHOW DATABASES；#检查是否创建成功
    USE <database_name>;#连接新建数据库
    #创建表,其中数据长度是指最大可能显示长度
    CREATE TABLE <table_name>(
    <column_name1> 数据类型(数据长度),
    <column_name2> 数据类型(数据长度),
    <column_name3> 数据类型(数据长度)
    );
    #向表插入数据
    INSERT INTO 
    <table_name>(<column_name1>,<column_name2>,<column_name3>)
    VALUES(<value1>,<value2>,<value3>);
    
    #<column_name>可省略
    INSERT INTO
    <table_name> VALUES(<value1>,<value2>,<value3>);
    
    #<column_name>可少写（注意与<value>对应）
    INSERT INTO
    <table_name>(<column_name1>,<column_name3>)
    VALUES(<value1>,<value3>);

### 2.常用数据类型

|  类型     | 大小（字节）|   用途   | 格式		|
| :-------- | :--------:| :------ | :-------- |
| **INT**	| **4**		|**整数**	|			|
| **FLOAT**	| **4**		|**单精度浮点数**|		|
| **DOUBLE**| **8**		|**双精度浮点数**|		|
| **ENUM**	| **--**	|**单选，比如性别**|**ENUM('a','b','c')**|
| **SET**	| **--**	|**多选**		|**SET('1','2','3')**|
| **DATE**	| **3**		|**日期**		|**YYYY-MM-DD**	|
| **TIME**	| **3**		|**时间点或持续时间**|**HH:MM:SS**|
| **YEAR**	| **1**		|**年份值**		|**YYYY**		|
| **CHAR**	| **0~255**	|**定长字符串**	|	|
| **VARCHAR**|**0~255**	|**变长字符串**	|	|
| **TEXT**	| **0~65535**|**长文本数据**	|	|




## 三、SQL的约束
### 1.约束分类

| 类型      |    主键 | 默认值    | 唯一      | 外键      |  非空    |
| :-------: | :-----:| :------: | :------: | :------: | :------: |
| **关键字** |**PRIMARY KEY**|**DEFAULT**|**UNIQUE**|**FOREIGN KEY**|**NOT NULL**|

### 2.PRIMARY KEY
**主键 (PRIMARY KEY)是用于约束表中的一行，作为这一行的唯一标识符，在一张表中通过主键就能准确定位到一行，因此主键十分重要，主键不能有重复记录且不能为空。**

    #方法一
    CREATE TABLE employee(
    id INT(10) PRIMARY KEY
    );
    #方法二,my_id是自定义的主键名
    CREATE TABLE employee(
    id INT(10),
    name CHAR(20),
    CONSTRAINT my_id PRIMARY KEY(id)
    );
    #方法三，还有一种特殊的主键——复合主键。主键不仅可以是表中的一列，也可以由表中的两列或多列来共同标识，比如：
    CREATE TABLE employee(
    id INT(10),
    name CHAR(20),
    CONSTRAINT my_id PRIMARY KEY(id,name)
    );

### 3.默认值约束
**默认值约束 (DEFAULT) 规定，当有 DEFAULT 约束的列，插入数据为空时，将使用默认值。**

    CREATE TABLE employee(
    id INT(10) PRIMARY KEY,
    age INT(3) DEFAULT '18'
    ); 
### 4.唯一约束
**唯一约束 (UNIQUE) 比较简单，它规定一张表中指定的一列的值必须不能有重复值，即这一列每个值都是唯一的。**

    CREATE TABLE employee(
    id INT(10) PRIMARY KEY,
    phone INT(11),
    UNIQUE (phone)
    );
### 5.外键约束
**外键 (FOREIGN KEY) 既能确保数据完整性，也能表现表之间的关系。一个表可以有多个外键，每个外键必须 REFERENCES (参考) 另一个表的主键，被外键约束的列，取值必须在它参考的列中有对应值。**

    #my_fk为自定义的外键名，another_table表示参考的表
    CREATE TABLE employee(
    id INT(10),
    department_name CHAR(10),
    CONSTRAINT my_fk FOREIGN KEY(department_name)
    REFERENCES <another_table>(<dpt_name>)
    );
### 6.非空约束
**非空约束 (NOT NULL),听名字就能理解，被非空约束的列，在插入值时必须非空。否则报错。**

    CREATE TABLE employee{
    id INT(10),
    salary INT(10) NOT NULL
    };


## 四、SELECT语句
### 1.基本语句和条件形式

    #基本格式
    SELECT <column_name> FROM <table_name> WHERE <case>;
    #数学符号条件(=,<,>,>=,<=)
    SELECT name,age FROM employee WHERE age>25;
    SELECT name,age,phone FROM employee WHERE name='Mary';
    #逻辑关系连接条件(AND 和 OR)
    SELECT name,age, FROM employee WHERE age<25 OR age>30;
    #关键词IN和NOT IN
    SELECT name,age,phone,in_dpt FROM employee 
    WHERE in_dpt in ('dpt3','dpt4');
### 2.通配符
**关键字 LIKE 可用于实现模糊查询，常见于搜索功能中。**
**和 LIKE联用的通常还有通配符，代表未知字符。SQL中的通配符是 _ 和 % 。其中 _ 代表一个未指定字符，% 代表不定个未指定字符**

    #查找1101开头的6位数电话号码
    SELECT name,age,phone FROM employee WHERE phone LIKE '1101__';
### 3.对结果排序(ORDER BY)
**用关键字ORDER BY 排序，关键字ASC指定升序，DESC降序，默认升序。**

    #按薪资降序查询
    SELECT name,age,salary FROM employee ORDER BY salary DESC;
### 4.SQL内置函数和计算

| 函数名     |   COUNT  |   SUM    | AVG   |  MAX    | MIN     |
| :------:  | :-------:| :------: | :---: | :----:  | :-----: |
| 作用	    |   计数    |  求和    | 求平均数| 最大值	|最小值    |

    #AS后接新列名，max_salary替代MAX(salary)作为查询列名
    SELECT MAX(salary) AS max_salary,MIN(salary) FROM employee;

### 5.子查询
**有时必须处理多个表才能获得所需的信息。例如：想要知道名为 "Tom" 的员工所在部门做了几个工程。员工信息储存在 employee 表中，但工程信息储存在 project 表中。**

	SELECT of_dpt,COUNT(proj_name) AS count_project FROM project 
	GROUP BY of_dpt
	HAVING of_dpt IN
	(SELECT in_dpt FROM employee WHERE name='Tom');
- **上面代码包含两个 SELECT 语句，第二个 SELECT 语句将返回一个集合的数据形式，然后被第一个 SELECT 语句用 in 进行判断。**
- **HAVING 关键字可以的作用和 WHERE 是一样的，都是说明接下来要进行条件筛选操作。区别在于 HAVING 用于对分组后的数据进行筛选。**
- **能用WHERE(相当于迭代器)的地方都可以用HAVING(先将所有数据分组)反之不可以，HAVING查询效率较低。**

### 6.连接查询(JOIN ON)
**在处理多个表时，子查询只有在结果来自一个表时才有用。但如果需要显示两个表或多个表中的数据，这时就必须使用连接 (join) 操作。 连接的基本思想是把两个或多个表当作一个新的表来操作**

    SELECT id,name,
    FROM employee,department
    WHERE employee.in_dpt = department.dpt_name
    ORDER BY id;
    #下面的的语句等同于上面的语句，都是默认的内连接(inner join)
    SELECT id,name
    FROM employee JOIN department
    ON employee.in_dpt = department.dpt_name
    ORDER BY id;

**sql的left join 、right join 、inner join之间的区别:**
- **left join(左联接) 返回包括左表中的所有记录和右表中联结字段相等的记录**
- **right join(右联接) 返回包括右表中的所有记录和左表中联结字段相等的记录**
- **inner join(等值连接) 只返回两个表中联结字段相等的行**

## 四、修改与删除
### 1.数据库的删除

    SHOW DATABASES;
    #删除一个数据库
    DROP DATABASE <database_name>;
- **目前 Mysql 没有提供修改数据库名称的方法，因为这曾导致一系列安全问题。**

### 2.表的删除与修改
**重命名一张表的语句有多种形式，以下 3 种格式效果是一样的：**

    #三种形式
    RENAME TABLE <old_name> TO <new_name>;
    ALTER TABLE <old_name> RENAME <NEW_name>;
    ALTER TABLE <old_name> RENAME TO <new_name>;
**删除一张表**

    DROP TABLE <table_name>;

### 3.表结构的修改
**增加一列**

    #法一
    ALTER TABLE 表名字 ADD COLUMN 列名字 数据类型 约束;
    #法二
    ALTER TABLE 表名字 ADD 列名字 数据类型 约束;
    #eg
    ALTER TABLE employee ADD height INT(4) DEFAULT 170;
**删除一列**

	 #法一：
	 ALTER TABLE 表名字 DROP COLUMN 列名字;
	 #法二：
	 ALTER TABLE 表名字 DROP 列名字;
	 #eg：
	 ALTER TABLE employee DROP col1;
**重命名一列 **
**当原列名和新列名相同的时候，指定新的数据类型或约束，就可以用于修改数据类型或约束。需要注意的是，修改数据类型可能会导致数据丢失，所以要慎重使用。**

```
#这条重命名语句后面的 “数据类型” 不能省略，否则重命名失败
ALTER TABLE 表名字 CHANGE 原列名 新列名 数据类型 约束;
#eg
ALTER TABLE employee CHANGE height shengao INT(4) DEFAULT 170;
```
**改变数据类型**

    ALTER TABLE 表名字 MODIFY 列名字 新数据类型;


### 4.表内容的修改

**修改表中某个值**

    UPDATE 表名字 SET 列1=值1,列2=值2 WHERE 条件;
    #eg
    UPDATE employee SET age=21,salary=3000 WHERE name='Tom';
**删除一行记录**

    #删除表中的一行数据，也必须加上 WHERE 条件，否则整列的数据都会被删除。删除语句：
    DELETE FROM 表名字 WHERE 条件;
    #删除Tom的数据
    DELETE FROM employee WHERE name='Tom';
