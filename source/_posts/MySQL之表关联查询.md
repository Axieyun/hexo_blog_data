---
title: MySQL之表关联查询
tags: MySQL
abbrlink: cb91
date: 2021-11-02 14:21:22
password:
---



### 内连接



#### 习题1



~~~mysql
-- 假设现在我想查询给我们供货的供应商的名称，以及商品名称和商品价格。此时我们发现，要查询的字段不在同⼀张表⾥。供应商名称
-- 在 vendors 表⾥，⽽商品名称和商品价格在 products 表⾥，这时可以使⽤内联查询，将两张表进⾏关联之后进⾏查询。

## 大概输出
SELECT *
FROM vendors,
     products
WHERE vendors.vend_id = products.vend_id;

## 隐性内连接 == 隐式内联
SELECT vend_name, prod_name, prod_price
FROM vendors,
     products
WHERE vendors.vend_id = products.vend_id; #供应商id相同

#起别名
SELECT vend_name, prod_name, prod_price
FROM vendors as v,
     products as p
WHERE v.vend_id = p.vend_id; #供应商id相同

SELECT vend_name, prod_name, prod_price
FROM vendors v,
     products p
WHERE v.vend_id = p.vend_id; #供应商id相同

## 显示内联
SELECT vendors.vend_name, products.prod_name, products.prod_price
FROM vendors
         INNER JOIN products ON products.vend_id = vendors.vend_id;

SELECT vendors.vend_name, products.prod_name, products.prod_price
FROM products
         INNER JOIN vendors ON products.vend_id = vendors.vend_id;

# inner可以省略
SELECT vendors.vend_name, products.prod_name, products.prod_price
FROM products
         JOIN vendors ON products.vend_id = vendors.vend_id;


~~~



#### 习题2



~~~mysql
-- 编写 SQL 语句，返回 customers 表中的顾客名称 cust_name 和 orders 表中的相关订单号 order_num，并按顾客名称再按订单号对结果进⾏排序。

## 大概输出
SELECT *
FROM orders o,
     customers c
WHERE c.cust_id = o.cust_id;

## 隐性内连接
SELECT c.cust_name, o.order_num
FROM orders o,
     customers c
WHERE c.cust_id = o.cust_id
ORDER BY cust_name, order_num;

SELECT c.cust_name, o.order_num
FROM orders AS o,
     customers AS c
WHERE c.cust_id = o.cust_id
ORDER BY cust_name DESC, order_num;


SELECT cust_name, order_num
FROM orders,
     customers
WHERE orders.cust_id = customers.cust_id
ORDER BY cust_name ASC, order_num DESC;

## 内连接

# 别名+内连接
SELECT c.cust_name, o.order_num
FROM customers c
         JOIN orders o ON c.cust_id = o.cust_id;

~~~





### 笛卡尔积



* 笛卡⼉积（cartesian product） 由没有联结条件的表关系返回的结果为笛卡⼉积。检索出的⾏的数⽬将是第⼀个表中的⾏数乘以第⼆个 表中的⾏数。

~~~mysql
SELECT v.vend_name, p.prod_name, p.prod_price
FROM products p, vendors v ORDER BY vend_name,prod_name;

~~~



#### 用处



* 当然笛卡尔积也不是丝毫没有⽤处，⽐如商品型号和颜⾊的组合就可以⽤笛卡尔积。





### 多表关联



#### 习题1



~~~mysql
# 假设现在要查询订单编号为 20005 的产品名称、产品价格、产品订购数量、供应商名称。该如何查询呢？

# 隐式连接
SELECT prod_name, prod_price, quantity, vend_name
FROM products p,
     vendors v,
     orderitems o
WHERE p.prod_id = o.prod_id
  AND p.vend_id = v.vend_id
  AND o.order_num = 20005;

# 显示连接
SELECT p.prod_name, p.prod_price, o.quantity, v.vend_name
FROM products p
         INNER JOIN orderitems o ON p.prod_id = o.prod_id
         INNER JOIN vendors v ON p.vend_id = v.vend_id
WHERE o.order_num = 20005;

SELECT SUM(o.quantity), v.vend_name
FROM products AS p #内连接
         JOIN orderitems AS o ON p.prod_id = o.prod_id
         JOIN vendors AS v ON p.vend_id = v.vend_id
WHERE order_num = 20005 #筛选
GROUP BY v.vend_name # 分组条件
HAVING sum(o.quantity) > 4 # 过滤
ORDER BY v.vend_name ASC; #排序


SELECT p.prod_name, p.prod_price, o.quantity, v.vend_name
FROM products AS p
         JOIN orderitems AS o ON p.prod_id = o.prod_id
         JOIN vendors AS v ON p.vend_id = v.vend_id
WHERE o.order_num = 20005
ORDER BY p.prod_price DESC ;

SELECT p.prod_name, p.prod_price, o.quantity, v.vend_name
FROM products p
         JOIN orderitems o ON p.prod_id = o.prod_id
         JOIN vendors v ON p.vend_id = v.vend_id
WHERE o.order_num = 20005;
~~~





### 外连接





#### 习题1

* 查询所有客户的订单情况 cust_id order_num，包括没有订单的客户



##### 左外连接



~~~mysql
# 查询所有客户的订单情况 cust_id order_num，包括没有订单的客户
# 左外连接
SELECT c.cust_id, o.order_num
FROM customers c #使⽤ LEFT OUTER JOIN 从 FROM ⼦句的左边表（customers 表）中选择所有⾏
         LEFT OUTER JOIN orders o ON c.cust_id = o.cust_id;

# 右外连接:
SELECT c.cust_id, o.order_num
FROM orders o # 从右边的表中选择所有⾏，应该使⽤ RIGHT OUTER JOIN
         RIGHT OUTER JOIN customers c ON c.cust_id = o.cust_id;
~~~

##### 右外连接

~~~mysql
# 右外连接:
SELECT c.cust_id, o.order_num
FROM orders o # 从右边的表中选择所有⾏，应该使⽤ RIGHT OUTER JOIN
         RIGHT OUTER JOIN customers c ON c.cust_id = o.cust_id;
~~~







### 自连接





#### 习题1

* 假设你发现商品 id 为 60001 的商品存在质量缺陷，现在需要查找，60001 供应商所提供的所有商品的名称和商品 id。请问如何编写 SQL 语句？



~~~mysql
SELECT prod_name, prod_id, vend_id
FROM products;

# 先查询供应商id
SELECT prod_name, prod_id, vend_id
FROM products
WHERE prod_id = 60001;

# 多表查询
SELECT prod_name, prod_id, vend_id
FROM products
WHERE vend_id IN (
    SELECT vend_id
    FROM products
    WHERE prod_id = 60001
);

# 自连接
SELECT p1.vend_id, p1.prod_name, p1.prod_id
# 假设我有两张表
FROM products p1,
     products p2
WHERE p1.vend_id = p2.vend_id
AND p2.prod_id = 60001;
~~~







### 作业1



~~~mysql
# 1.⽤ innor join 编写 SQL，查询每个下单顾客的名称，和所有订单号；
SELECT c.cust_name, o.order_num
FROM customers c
         INNER JOIN orders o ON c.cust_id = o.cust_id;

SELECT c.cust_name, o.order_num
FROM customers c
         JOIN orders o ON c.cust_id = o.cust_id;
# 2.修改上⼀题的 SQL 语句，列出所有顾客，即使他们没有下过订单

# 左连接
SELECT c.cust_name, o.order_num
FROM customers c
LEFT OUTER JOIN orders o ON c.cust_id = o.cust_id;

SELECT c.cust_name, o.order_num
FROM customers c
LEFT JOIN orders o ON c.cust_id = o.cust_id;

# 右连接
SELECT c.cust_name, o.order_num
FROM orders o
         RIGHT OUTER JOIN customers c ON c.cust_id = o.cust_id;

SELECT c.cust_name, o.order_num
FROM orders o
         RIGHT JOIN customers c ON c.cust_id = o.cust_id;

# 3. 使⽤ outer join 连接 products 表和 orderitems 表，返回产品名称 prod_name 和与之相关的订单号 order_num 的列表，并按商品名称排序。
SELECT p.prod_name, o.order_num
FROM products p
LEFT JOIN orderitems o ON p.prod_id = o.prod_id
ORDER BY p.prod_name DESC ;

# 4. 修改上⼀题中创建的 SQL 语句，使其返回每⼀项产品的总订单数（不是订单号）。
SELECT p.prod_name, COUNT(o.order_num)
FROM products AS p #左连接
         LEFT JOIN orderitems AS o ON p.prod_id = o.prod_id
GROUP BY p.prod_name # 分组
ORDER BY COUNT(o.order_num) DESC;

# 5. 编写 SQL 语句，列出供应商 id（vend_id）及其可供产品的数量，包括没有产品的供应商。

SELECT vendors.vend_id, count(prod_id)
FROM vendors
LEFT JOIN products p ON vendors.vend_id = p.vend_id
GROUP BY vendors.vend_id;


SELECT products.vend_id, orderitems.prod_id, COUNT(quantity), SUM(quantity)
FROM orderitems,
     products
WHERE products.prod_id = orderitems.prod_id
GROUP BY vend_id
ORDER BY vend_id;

~~~



### 作业2



~~~mysql
# 1. 使⽤innor join 编写SQL，查询每个下单顾客的名称，和所有订单号。

SELECT cust_name, order_num
FROM orders,
     customers
WHERE orders.cust_id = customers.cust_id;



SELECT c.cust_name, o.order_num
FROM customers c
         INNER JOIN orders o ON c.cust_id = o.cust_id;


# 2. 修改上⼀题的SQL语句，列出所有顾客，即使他们没有下过订单。
SELECT c.cust_name, o.order_num
FROM customers c
         LEFT JOIN orders o ON c.cust_id = o.cust_id;

#3. 使⽤outer join 连接products表和orderitems表，返回产品名称
# prod_name 和与之相关的订单号order_num的列表，并按商品名称排序。

SELECT prod_name, order_num
FROM orderitems o
INNER JOIN products p ON o.prod_id = p.prod_id
ORDER BY prod_name;

# 4. 修改上⼀题中创建的SQL语句，使其返回每⼀项产品的总订单数（不是
# 订单号）。

SELECT prod_name, COUNT(order_num)
FROM products p
         JOIN orderitems o ON p.prod_id = o.prod_id
GROUP BY prod_name;

# 5. 编写SQL语句，列出供应商id（vend_id）及其可供产品的数量，包括
# 没有产品的供应商。提⽰：使⽤外连接和分组。
SELECT v.vend_id, COUNT(p.prod_id)
FROM vendors v
         LEFT JOIN products p ON v.vend_id = p.vend_id
GROUP BY v.vend_id;

~~~

