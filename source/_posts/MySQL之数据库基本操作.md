---
title: MySQL之数据库基本操作
tags: MySQL
abbrlink: b2db
date: 2021-10-21 00:16:35
password:
---



### 创建数据库



#### 方式一

* 使⽤指定的字符编码表，创建数据库。

* 语法：CREATE DATABASE 数据库名 CHARACTER SET 字符编码；

* ~~~mysql
  CREATE DATABASE mydb CHARACTER SET utf8mb4;
  
  ~~~

* 注：

* * mysql 中有 utf8 和 utf8mb4 两种编码，在 mysql 中请⼤家忘记 utf8，永远使⽤ utf8mb4 。
  * 这是 mysql 的⼀个遗留问题， mysql 中的 utf8 最多只能⽀持 3bytes ⻓度的字符编码，对于⼀些需要占据 4bytes 的⽂字，mysql 的 utf8 就不⽀持了，要使⽤ utf8mb4 才⾏。



#### 方式二



* 使⽤默认的字符编码表（ latin1 ）,创建数据库. 

* 格式: CREATE DATABASE 数据库名;

* ~~~mysql
  CREATE DATABASE mydb2;
  ALTER DATABASE mydb CHARACTER SET utf8mb4;  -- 修改数据库编码
  ~~~





#### 思考



~~~mysql
如果创建⼀个已经存在的数据库会怎样？
执⾏报错，如果我们想让：当数据库不存在时再创建，该如何做呢？

CREATE DATABASE IF NOT EXISTS mydb2 CHARACTER SET utf8mb4;
~~~







### 查看数据库



#### 以列表形式返回所有的数据库

~~~mysql
SHOW DATABASES ;

~~~





##### 解释



**show databases;这个命令返回可⽤数据库的⼀个列表。包含在这个列表中的可能是 MySQL 内部使⽤的数据库。**



* information_schema 信息架构库，存储元数据信息
* mysql ⽤户信息表，存储了⽤户相关的信息
* performance_schema 性能库，存储性能监控相关的信息（较专业）
* sys 相当于 performance_schema 的简化，⽅便我们查看



#### 查看当前数据库



~~~mysql
SELECT DATABASE();

~~~



#### 查看当前数据库字符编码

* 格式：SHOW CREATE DATABASE 数据库名;

* ~~~mysql
  SHOW CREATE DATABASE mydb; -- 查看数据库mydb字符编码
  SHOW CREATE DATABASE mydb2;
  
  ~~~







### 选择数据库



~~~mysql
USE mysql; -- 切换数据库为mysql
~~~





### 修改数据库字符编码



~~~mysql
ALTER DATABASE mydb CHARACTER SET utf8mb4;
~~~





### 删除数据库



~~~mysql
DROP DATABASE mydb;
--同理，为了防止报错，删除数据库是也可以加一个判断，如果存在再删除
DROP DATABASE IF EXISTS mydb;
~~~

