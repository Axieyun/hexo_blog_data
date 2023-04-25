---
title: MySQL之事务
tags: MySQL
abbrlink: 80b5
date: 2021-11-02 21:44:21
password:
---





### 定义





* 事务: 指的是 逻辑上的⼀组操作 ，组成这组操作的各个单元要么全都成功，要么全都失败。

* ~~~mysql
  begin; #开启事务
  ~~~

* ~~~mysql
  rollback; # 事务回滚
  ~~~

* ~~~mysql
  commit;  # 事务提交
  ~~~



#### 隐式提交



**当我们使⽤ start transaction 或者 begin 语句时开启了⼀个事务。或者把系统变量的值设置为 off 时，事物就不会进⾏⾃动提交。 如果我们输⼊了某些语句，且这些语句会导致之前的事物，悄悄的提交（就像输⼊了 commit 命令⼀样），那么因为某种特殊的语句⽽导 致，事务提交的情况称为 隐式提交 。**



**会导致隐式提交的语句有：**

* 数据库定义语⾔ DDL，像 create、alter、drop
* 事务控制或关于锁定的语句，⽐如，前⼀个事务未提交，⼜开启了⼀个新的事务
* 加载数据的语句，⽐如 load data 
* 关于 MySQL 复制的⼀些语句 ：slave





#### 保存点



**如果你已经开启了⼀个事物，并且输⼊了很多语句，这是忽然发现前⾯已经执⾏完的某个语句。参数写错了，只好使⽤ rollback 语句， 让数据库状态恢复到事务执⾏之前的样⼦，然后⼀切从头再来。 这种感觉很不爽，因此就有了保存点的概念。**



**语法：**

* ~~~mysql
  -- 定义保存点
  savepoint 保存点名称;
  -- 回滚到某个保存点，如果rollback后面不跟随保存点名称，则直接回滚到事务之前的状态
  rollback [work] to [savepoint] 保存点名称 ;
  -- 删除保存点
  release savepoint 保存点名称;
  
  ~~~



### 作用



* 事务作⽤：保证在⼀个事务中多次 SQL 操作要么全都成功,要么全都失败。



### 事务的四⼤特性（ACID）

#### 原子性（Atomicity）

* 原⼦性是指事务包含的所有操作要么全部成功，要么全部失败回滚，



#### ⼀致性（Consistency）

* ⼀致性是指事务必须使数据库从⼀个⼀致性状态变换到另⼀个⼀致性状态，也就是说⼀个事务执⾏之前和执⾏之后都必须处于⼀致性状态。
* 拿转账来说，假设⽤户 A 和⽤户 B 两者的钱加起来⼀共是 5000，那么不管 A 和 B 之间如何转账，转⼏次账，事务结束后两个⽤户的钱 相加起来应该还得是 5000，这就是事务的⼀致性。



#### 隔离性（Isolation）

* 隔离性是当多个⽤户并发访问数据库时，⽐如操作同⼀张表时，数据库为每⼀个⽤户开启的事务，不能被其他事务的操作所⼲扰，多个并 发事务之间要相互隔离。
*  ⼀个事务的执⾏不能被其他事务⼲扰。⼀个事务内部的操作及使⽤的数据，对并发的其他事务是隔离的，并发执⾏的各个事务之间不能互 相⼲扰。
*  关于事务的隔离性数据库提供了多种隔离级别。



#### 持久性（Durability）



* 持久性是指⼀个事务⼀旦被提交了，那么对数据库中的数据的改变就是永久性的，即便是在数据库系统遇到故障的情况下也不会丢失提交 事务的操作。







### 实例



* ⽐如：银⾏转账，⼩明有 50 元，⼩红有 10 元，⼩明向⼩红转账 10 元，在数据库操作中相当于执⾏了两条 SQL 语句。
* 我们想让这两个操作要么全成功，要么全失败。如何保证呢？就需要把他们放到同⼀个事务⾥⾯进⾏操作。





~~~mysql
SHOW VARIABLES LIKE 'autocommit'; # 查看事务是否默认提交
SET AUTOCOMMIT = off; # 设置不默认提交
SET AUTOCOMMIT = on; # 设置不默认提交

CREATE TABLE account(
    id int primary key auto_increment comment '卡号',
    name varchar(20) comment '姓名',
    money double comment '余额'
);
drop TABLE account;
insert into account(id, name, money) values (1, '小明', 1213);
insert into account(id, name, money) values (2, '小红', 3234);
insert into account(id, name, money) values (3, '小1红', 3234);
insert into account(id, name, money) values (4, '小s明', 1213);
insert into account(id, name, money) values (5, '小a明', 1213);


select * from account;

BEGIN; # 开启事务

update account SET money = money + 10 where id = 1;

savepoint up1; # 定义保存点up1

select * from account;

UPDATE account set money = money - 10 WHERE id = 2;

SELECT * FROM account;

rollback to up1; # 回滚到保存点状态
release savepoint up1;  # 删除保存点

SELECT * from account;

rollback; # 回滚到事务开始前的状态
commit;   # 事务提交



~~~

