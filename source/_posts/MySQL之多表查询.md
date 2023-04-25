---
title: MySQL之多表查询
abbrlink: '9088'
date: 2021-10-28 05:49:53
tags: MySQL
password:
---



### 外键



#### 外键（foreign key）

**：外键为某个表中的⼀列，它包含另⼀个表的主键值，定义了两个表之间的关系。** 

#### 主表（⽗表）

**：对于两个具有关联关系的表⽽⾔，相关联字段中的主键所在的那个表即是主表。**

####  从表（⼦表）

**：对于两个具有关联关系的表⽽⾔，相关联字段中的外键所在的那个表即是从表。**







### 语法



#### 格式



~~~mysql
-- 添加外键
alter table 从表 add [constraint][外键名称] foreign key (从表外键字段名)
references 主表 (主表的主键);
--[外键名称]用于删除外键约束的，一般建议“_fk”结尾
-- 也可以在建表时添加外键约束，
--CONSTRAINT orders_customers_fk FOREIGN KEY (cust_id) REFERENCES customers (cust_id)
-- 删除外键
alter table 从表 drop foreign key 外键名称
~~~





#### 实例



~~~mysql
# 添加外键
ALTER TABLE orders ADD constraint orders_customers_fk FOREIGN KEY (cust_id) REFERENCES customers (cust_id);

ALTER TABLE orders ADD CONSTRAINT orders_customers_fk FOREIGN KEY (cust_id) REFERENCES customers (cust_id);
# 删除外键
ALTER TABLE orders DROP FOREIGN KEY orders_customers_fk;
# 向主表添加数据
INSERT INTO customers (cust_id,cust_name) VALUES (666,'王老五'); -- 成功
# 向从表添加数据
INSERT INTO orders(order_date, cust_id) VALUES (now(),666); -- 成功
# 向从表添加数据
INSERT INTO orders (order_date,cust_id) VALUES (now(),111) ;-- 失败,插入数据在外键在主表不存在
ALTER TABLE orders ADD CONSTRAINT orders_customers_fk FOREIGN KEY (cust_id) REFERENCES customers (cust_id);

show CREATE TABLE orders;
SHOW CREATE TABLE customers;
~~~







### 表关系



**实际开发中，⼀个项⽬通常需要很多张表才能完成。例如：⼀个商城项⽬就需要顾客表(customers)、商品表(products)、订单表(orders) 等多张表。且这些表的数据之间存在⼀定的关系，接下来我们将在单表的基础上，⼀起学习多表⽅⾯的知识。**





#### ⼀对⼀关系

* 在实际的开发中不多.因为⼀对⼀可以创建成⼀张表
* 常⻅实例：商品表和商品描述表



**建表原则**



* 外键唯⼀：主表的主键和从表的外键（唯⼀），形成主外键关系，外键唯⼀ unique。
* 外键是主键：主表的主键和从表的外键，形成主外键关系。





#### ⼀对多关系



* 常⻅实例：客户和订单，分类和商品，部⻔和员⼯, 省份和城市
* ⼀对多建表原则：在从表(多⽅)创建⼀个字段，字段作为外键指向主表(⼀⽅)的主键.



#### 实例



~~~mysql
-- 创建省份表
DROP TABLE province;

CREATE TABLE province
(
    pid         int         NOT NULL
        PRIMARY KEY,
    pname       varchar(32) NOT NULL COMMENT '省份名称', -- 省份名称;
    description varchar(100)                         -- 描述;
)
    COMMENT '省份表';


DROP TABLE city;

-- 创建城市表
CREATE TABLE city
(
    cid         int PRIMARY KEY,
    cname       varchar(32),  -- 城市名称
    description varchar(100), -- 描述
    province_id int,
    CONSTRAINT city_province_fk FOREIGN KEY (province_id) REFERENCES province (pid)
);
~~~





#### 多对多关系



* 常⻅实例：商品和订单，学⽣和课程，⽤户和⻆⾊
* 多对多关系建表原则：需要创建第三张表,中间表中⾄少两个字段，这两个字段分别作为外键指向各⾃⼀⽅的主键





#### 实例



~~~mysql
CREATE TABLE `user`
(
    uid        int PRIMARY KEY,
    username   varchar(32),
    `password` varchar(32)
);

-- 角色表
CREATE TABLE role
(
    rid   int PRIMARY KEY,
    rname varchar(32)
);

-- 中间表
CREATE TABLE user_role
(
    user_id int,
    role_id int,
    CONSTRAINT user_role_pk PRIMARY KEY (user_id, role_id),
    CONSTRAINT user_id_fk FOREIGN KEY (user_id) REFERENCES `user` (uid),
    CONSTRAINT role_id_fk FOREIGN KEY (role_id) REFERENCES role (rid)
);
~~~





### 注意|特别提醒



* 现在这种创建外键的⽅式已经不提倡，甚⾄被禁⽌了，因为在维护数据时，限制条件太多，效率较低。关联关系通过 SQL 语句来实现。







