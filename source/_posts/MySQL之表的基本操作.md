---
title: MySQL之表的基本操作
tags:
  - MySQL
abbrlink: 348c
date: 2021-10-19 22:21:11
password:
---



### 说明

* MySQL 数据库的 SQL 语句不区分⼤⼩写，关键字建议使⽤⼤写，以分号结尾。例如：

* ~~~mysql
  SELECT * FROM user;
  ~~~

* 使⽤/**/、 -- 、# 的⽅式完成注释

* ~~~mysql
  /*
  多行注释
  */
  -- 单行注释
  # 单行注释
  SELECT * FROM user;
  
  ~~~











### 建表

#### 格式



~~~mysql
create table 表名 (
字段名 数据类型[长度] [约束],
字段名 数据类型[长度] [约束],
...
);
# 注:[]中的内容是可选项
~~~

#### 实例



~~~mysql
-- 创建表student, 字段包括 编号id\ 姓名name\ 年龄age
CREATE TABLE student (
id INT,
name VARCHAR(100),
age INT
);

-- 创建表users, 字段包括 编号id\ 用户名username \ 密码password
CREATE TABLE users (
id INT,
username VARCHAR(100),
password VARCHAR(100)
);


~~~





### 查表



#### 格式



~~~mysql
-- 查看所有表, 格式: show tables
SHOW TABLES;
-- 查看指定表的建表结构, 格式: show create table 表名;
SHOW CREATE TABLE users;

~~~



#### 实例



~~~mysql
-- 查看所有表, 格式: show tables
SHOW TABLES;
-- 查看指定表的建表结构, 格式: show create table 表名;
SHOW CREATE TABLE users; #查看表users
~~~







### 删除表



#### 格式与实例



~~~mysql
-- 删除表, 格式: drop table 表名;
DROP TABLE users;#删除表users
~~~





### 改表格式



~~~mysql
/*
对表中的列进行修改
1. 添加新的列, 格式: alter table 表名 add 新列名 数据类型(长度);
2. 修改列的数据类型(长度), 格式: alter table 表名 modify 列名 修改后的数据类型(长度);
3. 修改列的名称, 格式: alter table 表名 change 列名 新列名 新列名的数据类型(长度);
4. 删除指定列, 格式: alter table 表名 drop 列名;
*/
ALTER TABLE student ADD `desc` VARCHAR(100); -- 添加新的列
ALTER TABLE student MODIFY `desc` VARCHAR(50);-- 修改列的数据类型(长度)
ALTER TABLE student CHANGE `desc` description VARCHAR(100);-- 修改列的名称
ALTER TABLE student DROP description;-- 删除指定列
/*
对表进行修改
1. 修改表的名称, 格式: rename table 表名 to 新表名;
2. 修改表的字符编码, 格式: alter table 表名 character set 字符编码;
*/
RENAME TABLE student TO stu; -- 修改表student的名称为stu
ALTER TABLE stu CHARACTER SET gbk; -- 修改表stu的字符编码为gbk
~~~



### 插入表记录



~~~mysql
/*
插入表记录
方式一, 对指定的字段插入值, 格式: insert into 表名(字段1, 字段2, ...) values (值1, 值2, ...);
方式二, 对所有字段插入值, 格式: insert into 表名 values(值1, 值2, ...);
方式三, 一次性插入多条数据，格式:insert into 表名(字段1, 字段2, ...) values (值1, 值2, ...),(值1, 值2, ...);
*/
INSERT INTO student(id, name, age) VALUES(1, 'tom', 24);
INSERT INTO student(name, age) VALUES('lili', 22);
INSERT INTO student(id, name, age) VALUES(3, 'jim', NULL);
INSERT INTO student VALUES(4, 'jack', 26);
INSERT INTO student VALUES(5, 'zhangsan', 26),(6,'lisi',27);
INSERT INTO student (id ,name,age) VALUES (1,'李四',20),(2,'zhangsan',30);
~~~



#### 注意



* 1、值与字段必须对应, 个数相同, 类型相同 
* 2、值的数据⼤⼩必须在字段的指定⻓度范围内 
* 3、除了整数\⼩数类型外, 其他字段类型的值必须使⽤引号引起来 (建议单引号) 
* 4、如果要插⼊空值, 可以不写字段, 或者插⼊ null





### 更新表





~~~mysql
-- 更新表记录, 格式: update 表名 set 字段1=值, 字段2=值... where 条件;
UPDATE student SET name='lili', age=21 WHERE id=1;
UPDATE student SET age=25 WHERE age=27;

~~~



#### 注意



* 1、列名的类型与修改的值要⼀致
* 2、修改值时不能超过字段的⻓度范围
* 3、除了整数\⼩数类型外, 其他字段类型的值必须使⽤引号扩起来



### 表的复制





#### 删除表记录



~~~mysql
-- 删除表记录, 格式: delete from 表名 where 条件;
DELETE FROM student WHERE id=1;
DELETE FROM student WHERE age IS NULL;
~~~





#### 仅复制表结构



~~~mysql
CREATE TABLE 新表 SELECT * FROM 旧表 WHERE 1=2;
~~~



#### 仅复制数据



~~~mysql
INSERT INTO 新表(字段1,字段2,.......) SELECT 字段1,字段2,...... FROM 旧表;
~~~





### 复制表结构及数据到新表



~~~mysql
CREATE TABLE 新表 SELECT * FROM 旧表;
~~~





### 运行实例



~~~mysql
CREATE TABLE Student(
    id int,
    name varchar(3),
    age int
);

CREATE TABLE Student1(
    id int,
    username varchar(200),
    password varchar(200)
);

CREATE TABLE users(
    id int,
    username varchar(200),
    password varchar(200)
);

SHOW TABLES;  # 查看所有表
SHOW CREATE TABLE users;  # 查看表

DROP TABLE Student1; # 删除表

ALTER TABLE Student ADD `desc` varchar(40);  # 在表增加列，列名字为desc
ALTER TABLE Student MODIFY `desc` varchar(29); # 修改列desc的数据类型长度为29
ALTER TABLE Student CHANGE `desc` description varchar(20);  #列desc改名字为description
ALTER TABLE Student DROP description; # 删除列description

RENAME TABLE Student TO Stu; #给表Student改名字为Stu
SHOW CREATE TABLE Stu;  # 查看表
ALTER TABLE Stu CHARACTER SET utf8mb4; # 修改表的字符编码
SHOW CREATE TABLE Stu;  # 查看表

# 插入表记录
INSERT INTO Stu (id, name, age) VALUES (1, 'li', 19); #对指定字段插入值
INSERT INTO Stu VALUES (2, 'si', 20); # 对所有字段插入值，数据类型得相对应
INSERT INTO Stu VALUES (3, 'hu', 20);
ALTER TABLE Stu MODIFY name varchar(20);
INSERT INTO Stu VALUES (3, 'husd', 20);
INSERT INTO Stu VALUES (6, 'hdu', 20), (5, 'hgu', 20), (4, 'hu', 20);
INSERT INTO Stu VALUES (3, 'hu', NULL);

#修改字段
UPDATE Stu SET age = 129 WHERE id = 3; #修改id为2的记录的age为19
UPDATE Stu SET age = NULL, name = 'asd' WHERE id = 3;

DELETE FROM Stu WHERE id = 2; #删除记录
DELETE FROM Stu WHERE age is NULL; #删除记录，不能用等号
/*
 注意事项：
 1、修改长度不能超过字段的长度范围
 2、修改数据类型得相同
 */

drop TABLE Stu;
DROP TABLE users;

show variables like 'character_set_database'; # 查看数据库的编码类型
ALTER DATABASE mydb CHARACTER SET utf8mb4;  # 修改数据库的编码
CREATE TABLE teacher(
    id int,
    name varchar(200),
    age int,
    dsecription varchar(200)
);
ALTER TABLE teacher CHARACTER SET utf8mb4;  # 修改表的编码
SHOW CREATE TABLE teacher;  # 查看表
INSERT INTO teacher VALUES (1, '强哥', 18, '真帅'); # 插入记录
DROP TABLE teacher;  # 删除表

~~~

