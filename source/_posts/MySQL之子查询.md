---
title: MySQL之子查询
tags: MySQL
abbrlink: e0e6
date: 2021-10-28 00:50:25
password:
---



~~~mysql
# 假设现在需要列出订购物品 id 为 60005 的所有客户 id、客户名称，应该怎样检索？下⾯列出了具体的步骤。

SELECT cust_id, cust_name
FROM customers;

# (1) 从明细表中查出包含物品 60005 的所有订单的编号。
SELECT prod_id, order_num
FROM orderitems;

SELECT order_num
FROM orderitems
WHERE prod_id = 60005;

# (2) 根据前⼀步骤查询出的订单编号，从订单表中查出所有客户的 ID。
SELECT cust_id
FROM orders
WHERE order_num IN (20005, 20007, 20009);
# (3) 根据前⼀步骤查询出的的所有客户 ID，从客户表中查出对应的客户信息。
SELECT cust_id
FROM orders
WHERE order_num IN (
    SELECT order_num
    FROM orderitems
    WHERE prod_id = 60005
);

SELECT cust_id, cust_name
FROM customers
WHERE cust_id IN (
    SELECT cust_id
    FROM orders
    WHERE order_num IN (
        SELECT order_num
        FROM orderitems
        WHERE prod_id = 60005
    )
);

SELECT cust_id, cust_name
FROM customers
WHERE cust_id IN (
    SELECT cust_id
    FROM orders
    WHERE order_num IN (
        SELECT order_num
        FROM orderitems
        WHERE prod_id = 60005
    )
);

#使⽤⼦查询，返回购买单价为 10 元或以上产品的顾客 id、顾客名称。
#（思路提示：使⽤ orderitems 表查找匹配的订单号（order_num）,然后使⽤ order 表检索这些匹配订单的顾客 id（cust_id））
SELECT cust_id, cust_name
FROM customers
WHERE cust_id IN (
    SELECT cust_id
    FROM orders
    WHERE order_num IN (
        SELECT order_num
        FROM orderitems
        WHERE item_price >= 10
    )
);
# 假设现在你需要查询购买 id 为 60005 的产品的所有下单⽇期，以及下单的顾客 id，SQL 该如何写？
# 未排序
SELECT order_date, cust_id
FROM orders
WHERE order_num IN (
    SELECT order_num
    FROM orderitems
    WHERE prod_id = 60005
);

# 日期升序
SELECT order_date, cust_id
FROM orders
WHERE order_num IN (
    SELECT order_num
    FROM orderitems
    WHERE prod_id = 60005
)
ORDER BY order_date;

# 查询购买 id 为 60005 的产品的所有顾客的邮箱，该如何写 SQL？

# 思路：顾客表

SELECT cust_email # 顾客的邮箱
FROM customers # 顾客表
WHERE cust_id IN ( # 查询顾客id
    SELECT cust_id
    FROM orders # 用户-订单表（中间表）
    WHERE order_num IN ( # 查询订单id
        SELECT order_num
        FROM orderitems # 订单明细表
        WHERE prod_id = 60005 # 查询商品id
    )
);




~~~

