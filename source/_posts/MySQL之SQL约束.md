---
title: MySQL之SQL约束
tags: MySQL
abbrlink: 7b13
date: 2021-10-27 00:19:22
password:
---

## 约束

约束, 其实就是⼀种限制条件, 让你不能超出这个控制范围。 公路上有速度限制、⻋距限制、鸣笛限制。 ⽽在数据库中的约束，就是指 表中的数据内容 不能胡乱填写，必须按照要求填写，好保证数据的 完整性与安全性 .



### 数据准备



~~~mysql
-- 准备数据
CREATE TABLE persons (
	pid int COMMENT '用户编号',
	firstname varchar(255) COMMENT '名字',
	lastname varchar(255) COMMENT '姓氏',
	address varchar(255) COMMENT '家庭住址'
);
insert into persons values(1, '星驰','周','香港');
insert into persons values(1, '德华','刘','香港');
insert into persons values(2, '德华','刘',null);
insert into persons values(null, '润发','周','香港');
~~~





### 主键约束



#### PRIMARY KEY 约束

* 主键必须包含唯⼀的值, 不能重复。
* 主键列不能包含 NULL 值。
* 每个表有且只有一个主键。

#### 添加主键约束



* ⽅式⼀：创建表时，在字段描述处，声明指定字段为主键：

* * 格式: 字段名 数据类型[长度] PRIMARY KEY

* ~~~mysql
  CREATE TABLE persons (
  	pid int primary key, -- 添加了主键约束
  	lastname varchar(255),
  	firstname varchar(255),
  	address varchar(255)
  );
  INSERT INTO persons VALUES(1, '星驰','周','香港');
  INSERT INTO persons VALUES(1, '德华','刘','香港'); -- 设置主键后, 插入失败, 值重复
  INSERT INTO persons VALUES(2, '德华','刘',NULL);
  INSERT INTO persons VALUES(NULL, '润发','周','香港'); -- 设置主键后, 插入失败, 值不能为 NULL
  ~~~

* ⽅式⼆：创建表时，在 constraint 约束区域，声明指定字段为主键：

* * 格式： [constraint 名称] primary key (字段列表)
  * 关键字 constraint 可以省略，如果需要为主键命名，constraint 不能省略，主键名称⼀般没⽤。
  * 字段列表需要使⽤⼩括号括住，如果有多字段需要使⽤逗号分隔。声明两个以上字段为主键，我们称为联合主键。

* ~~~mysql
  CREATE TABLE persons (
  	pid int,
  	lastname varchar(255),
  	firstname varchar(255),
  	address varchar(255),
  	constraint pk_persons primary key (pid) -- 添加主键约束, 单一字段
  );
  CREATE TABLE persons (
  	pid INT,
  	lastname VARCHAR(255),
  	firstname VARCHAR(255),
  	address VARCHAR(255),
  	CONSTRAINT pk_persons PRIMARY KEY (lastname, firstname) -- 添加主键约束, 多个字段, 我们称为联合主键。
  );
  
  INSERT INTO persons VALUES(1, '星驰', '周', '香港');
  INSERT INTO persons VALUES(1, '德华', '刘', '香港');
  INSERT INTO persons VALUES(2, '德华', '刘', NULL); -- 插入失败，因为刘 德华重复
  INSERT INTO persons VALUES(3, '德华', '马', '大陆');
  
  ~~~

* ⽅式三：创建表之后，通过修改表结构，声明指定字段为主键：

* * 格式： ALTER TABLE 表名 ADD [CONSTRAINT 名称] PRIMARY KEY (字段列表)

* ~~~mysql
  CREATE TABLE persons (
  	pid int,
  	lastname varchar(255),
  	firstname varchar(255),
  	address varchar(255)
  );
  alter table persons add constraint pk_persons primary key (lastname, firstname); -- 添加联合主键
  
  ~~~





#### 删除主键约束

* 格式：

* ~~~mysql
  ALTER TABLE 表名 DROP PRIMARY KEY
  ~~~

* 实例：

* ~~~mysql
  alter table persons drop primary key;
  
  ~~~









