---
title: MySQL之组合查询
tags:
  - MYSQL
abbrlink: 69ce
date: 2021-10-28 00:50:10
password:
---

### UNION 操作符

**UNION 操作符⽤于连接两个以上的 SELECT 语句的结果组合到⼀个结果集合中。默认：多个 SELECT 语句会删除重复的数据。**



#### 格式

~~~mysql
SELECT 列名, 列名,...
FROM tables [WHERE 条件]
UNION [ALL | DISTINCT]
SELECT 列名, 列名,...
FROM tables [WHERE 条件];
~~~



#### 解读

* DISTINCT: 可选，删除结果集中重复的数据。默认情况下 UNION 操作符已经删除了重复数据，所以 DISTINCT 修饰符对结果没什么 影响。
* ALL: 可选，返回所有结果集，包含重复数据。





~~~mysql
# 准备数据：先复制一张表 customers2
CREATE TABLE customers2 SELECT * FROM customers;
# 复制表结构
CREATE TABLE cus LIKE customers;
# 复制数据
INSERT INTO cus(cust_id, cust_name, cust_address,
                cust_city, cust_state, cust_zip,
                cust_country, cust_contact, cust_email)
SELECT cust_id,
       cust_name,
       cust_address,
       cust_city,
       cust_state,
       cust_zip,
       cust_country,
       cust_contact,
       cust_email
FROM customers;

# 准备数据：在customers2 中修改几条数据
UPDATE customers2 SET cust_name = '杨过' WHERE cust_id = 10001 ;
UPDATE customers2 SET cust_name = '小龙女' WHERE cust_id = 10002;
UPDATE customers2 SET cust_name = '金庸' WHERE cust_id = 10003 ;
# 查询 customers 表中的顾客名称
SELECT cust_name
FROM customers;
# 查询 customers2 表中的顾客名称
SELECT cust_name
FROM customers2;
# 查询 customers 和 customers2 表中的顾客名称
SELECT cust_name
FROM customers
UNION
SELECT cust_name
FROM customers2;

# 结果变成了8条数据，是因为两张表中有两条重复数据，被自动去重了，如果不想去重，想全部显示呢？
SELECT cust_name
FROM customers
UNION all
SELECT cust_name
FROM customers2;
~~~

