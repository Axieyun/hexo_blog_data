---
title: MySQL之分组查询
tags: MySQL
abbrlink: ebf0
date: 2021-10-27 23:36:02
password:
---





~~~mysql
# 查询商品表，返回供应商1003提供的产品数目
SELECT COUNT(*) AS prod_num
FROM products
WHERE vend_id = 1003;

SELECT COUNT(prod_id) AS prod_num
FROM products
WHERE vend_id = 1003;

# 如果我想查询每个供应商提供的产品数量呢？
SELECT count(prod_id), vend_id
FROM products
GROUP BY vend_id;

SELECT vend_id, count(*) as prod_num
FROM products
GROUP BY vend_id;

# 查询每个供应商提供的产品数量，并且产品数量大于2的供应商id

SELECT vend_id, count(prod_id) as prod_num
from products
GROUP BY vend_id
HAVING prod_num > 2;

SELECT vend_id, COUNT(prod_id) AS prod_num
FROM products
GROUP BY vend_id
HAVING COUNT(prod_id) > 2;

SELECT vend_id, count(*) AS prod_num
FROM products
GROUP BY vend_id
HAVING count(*) > 2;
# 思考题：查询每个供应商提供的产品数目，以及每个供应商提供的商品中最贵的价格；--提示，在同时查询多个聚合函数
SELECT vend_id, count(prod_id), max(prod_price), min(prod_price)
FROM products
GROUP BY vend_id;

# 思考题：在供应商id大于1001的供应商中，查询每个供应商提供的产品数目，以及每个供应商提供的商品中最贵的价格；
-- 效率高，先筛选，再分组
SELECT vend_id, count(prod_id), max(prod_price), min(prod_price)
FROM products
WHERE vend_id > 1001
GROUP BY vend_id;

-- 效率低，先分组，再过滤
SELECT vend_id, count(prod_id), max(prod_price), min(prod_price)
FROM products
GROUP BY vend_id
HAVING vend_id > 1001;

# 思考题：在供应商id大于1001的供应商中，查询每个供应商提供的产品数目，以及每个供应商提供的商品中最贵的价格，并且最高价格大于10的数据
-- 分组再过滤
SELECT vend_id, count(prod_id)
FROM products
WHERE vend_id > 1001 -- 供应商id大于1001的供应商中
GROUP BY vend_id -- 对供应商进行分组
HAVING max(prod_price) > 10; -- 每个供应商提供的商品中最贵的价格，并且最高价格大于10的数据

# 思考题：在供应商id大于1001的供应商中，查询每个供应商提供的产品数目，以及每个供应商提供的商品中最贵的价格，并且最高价格大于10的数据，并按供应商id从高到低排序
SELECT vend_id, COUNT(prod_id), MAX(prod_price)
FROM products
WHERE vend_id > 1001 -- 供应商id大于1001的供应商中
GROUP BY vend_id -- 对供应商进行分组
HAVING MAX(prod_price) > 10 -- 每个供应商提供的商品中最贵的价格，并且最高价格大于10的数据
ORDER BY vend_id DESC; -- 降序

# 作业题：从订单明细表中，查询每笔订单的订单编号和订单总额，并且得到订单总额大于100的数据，将最终结果按照订单总额从大到小排序, 取前两条数据
SELECT order_num, sum(item_price * quantity) as order_todal -- 查询每笔订单的订单编号和订单总额
FROM orderitems -- 从订单明细表中
GROUP BY order_num
HAVING order_todal > 100
ORDER BY order_todal DESC
LIMIT 2;

SELECT order_num, item_price * quantity as order_todal -- 查询每笔订单的订单编号和订单总额
FROM orderitems -- 从订单明细表中
ORDER BY order_num DESC;

SELECT order_num, sum(item_price * quantity) as order_todal -- 查询每笔订单的订单编号和订单总额
FROM orderitems -- 从订单明细表中
GROUP BY order_num
ORDER BY order_todal DESC;

SELECT order_num, SUM(item_price * quantity) AS order_todal -- 查询每笔订单的订单编号和订单总额
FROM orderitems -- 从订单明细表中
GROUP BY order_num
HAVING order_todal > 100
ORDER BY order_todal DESC
LIMIT 2;
~~~

