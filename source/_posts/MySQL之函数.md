---
title: MySQL之函数
tags: MySQL
abbrlink: 20ee
date: 2021-10-27 00:05:55
password:
---



~~~mysql

SELECT prod_id, prod_name, prod_price FROM products ORDER BY prod_price DESC LIMIT 0, 3;
SELECT products.prod_id, products.prod_name FROM mydb.products;
SELECT DISTINCT vend_id from products;
SELECT prod_name, prod_price FROM products LIMIT 1, 3;

# 先按供应商id降序，再按产品价格升序
SELECT vend_id, prod_price FROM products ORDER BY vend_id DESC , prod_price ASC ;
# 先按供应商id升序，再按产品价格升序
SELECT vend_id, prod_price FROM products ORDER BY vend_id ASC , prod_price ASC ;

SELECT prod_name, prod_price FROM products WHERE prod_price = 55.00;
SELECT prod_price, prod_name FROM products WHERE prod_price != 55;

SELECT prod_price, prod_name FROM products WHERE prod_price <> 55.00;

SELECT prod_price, prod_name FROM products WHERE prod_price >= 5 and prod_price <= 10;
SELECT prod_price, prod_name FROM products WHERE prod_price between 5 and 10;

SELECT prod_name, prod_desc FROM products WHERE prod_desc IS NULL;
SELECT prod_name, prod_desc FROM products WHERE prod_desc is NOT NULL;


SELECT prod_name, vend_id, prod_price FROM products WHERE vend_id = 1001 OR vend_id = 1002;
SELECT prod_name, vend_id, prod_price FROM products WHERE (vend_id = 1001 OR vend_id = 1002) AND prod_price > 10;

SELECT prod_name, vend_id, prod_price FROM products WHERE vend_id IN (1001, 1002);
SELECT prod_name, vend_id, prod_price FROM products WHERE vend_id NOT IN (1001, 1002);



# 查询产品名称中 i 开头的产品名称
SELECT prod_name FROM products WHERE prod_name LIKE 'i%';
# 注意：MySQL默认是不区分大小写的，所以查询小写 i , 大写 I 也会被查询出来
# 如果想区分大小写呢？
# 有两种方式，一种是修改表结构，字段后面加 binary 关键字，但修改表结构一般不推荐
# 第二种是，在SQL语句中添加 BINARY 格式：SELECT 列名 FROM 表名 WHERE BINARY 列名 LIKE '字符‘;
SELECT prod_name FROM products WHERE BINARY prod_name LIKE 'I%';
# % 可以匹配0到多个字符，但不能匹配 null
# 查询产品名称中第二个字是’王‘的产品
SELECT prod_name FROM products WHERE prod_name LIKE '_王%';
# 注意：通配符虽然好用，但不要滥用，尽量不要把%放在最前面，比如：where like '%王'。因为这也搜索效率会很慢，具体原因，进阶阶段再深入探

#查询商品表，返回一列：商品名称（价格）。
SELECT concat(prod_name, ' ( ', prod_price, ' ) ') FROM products WHERE prod_price <= 10;

SELECT CONCAT(prod_name, '(', prod_price, ')')
FROM products
ORDER BY prod_price;

#查询商品表，返回一列：商品名称（价格），并将该列命名为prod_name_price。
SELECT CONCAT(prod_name, '(', prod_price, ')') AS prod_name_price
FROM products
ORDER BY prod_name;

SELECT CONCAT(prod_name, '(', prod_price, ')') AS prod_name_price
FROM products
ORDER BY prod_price DESC
LIMIT 0, 7;

# 查询 orderitems 表中，订单编号为 20006的产品id（prod_id），物品单价(item_price)，物品数量(quantity)。并计算每个产品的总价(total
SELECT prod_id, item_price, quantity
FROM orderitems
WHERE order_num = 20006;

SELECT prod_name
FROM products
ORDER BY prod_price DESC
LIMIT 0, 5;

SELECT order_num, prod_id, item_price, quantity, item_price * quantity total
FROM orderitems
ORDER BY order_num DESC;

SELECT order_num, prod_id, item_price, quantity, item_price * quantity
FROM orderitems
ORDER BY order_num DESC;

SELECT order_num, prod_id, item_price, quantity, item_price * quantity AS total
FROM orderitems
ORDER BY order_num DESC;


#查询产品表，将产品名称中所有字母转换为大写、小写
SELECT UPPER(prod_name)
FROM products;

SELECT lower(prod_name)
FROM products;

# 查询产品表，返回产品名称的前三个字符/后三个字符
SELECT left(prod_name, 3)
FROM products;

SELECT right(prod_name, 3)
FROM products;


# substring() 有三种用法，下面直接看示例吧
# substring(字符串,n) 从第n个索引位置开始截取字符串。索引从1开始。
SELECT substring(prod_name, 2)
FROM products;

SELECT substring(prod_name, 2, 3)
FROM products;

SELECT substring(prod_name, 1, 4)
FROM products
ORDER BY prod_name DESC;

SELECT substring(prod_name FROM 1 FOR 4)
FROM products
ORDER BY prod_name DESC ;

SELECT substring_index('www.kaikeba.com','.',2) as abstract;

SELECT prod_name
FROM products
ORDER BY CONVERT(prod_name USING gbk) DESC;

SELECT prod_name, prod_price, vend_id
FROM products
ORDER BY vend_id DESC, CONVERT(prod_name USING gbk) asc;

SELECT prod_name, prod_price
FROM products
WHERE prod_price >= 5
  AND prod_price <= 10;

SELECT prod_name
FROM products
WHERE prod_name LIKE 'i%';


SELECT CONCAT(prod_name, '(', prod_price, ')'), prod_id
FROM products
ORDER BY prod_price DESC
LIMIT 0, 5;

SELECT CONCAT(prod_name, '(', prod_price, ')') AS prod_name_price
FROM products
WHERE prod_price >= 5;

SELECT trim(prod_name)
FROM products;

SELECT ltrim(prod_name)
FROM products;

SELECT rtrim(prod_name)
FROM products;

# 增加日期 语法格式：DATE_ADD(date,INTERVAL expr type) 注意：DATE_ADD和adddate是同义词
SELECT date_add(order_date, INTERVAL 5 DAY ) FROM orders WHERE order_num = 20005;
# 查询当前时间
SELECT curdate();
SELECT curtime();
SELECT curdate(), curtime();
# 查询当前日期和时间
SELECT curdate(), curtime();
# 查询订单列表，返回下单时间的日期部分
SELECT DATE(order_date) FROM orders;
SELECT DATE(order_date), TIME(order_date) FROM orders;
SELECT order_date FROM orders;
# DATEDIFF() 函数返回两个日期之间的天数。格式：DATEDIFF(date1,date2) 只有日期部分参与运算
SELECT datediff('2007-09-12', '2000-09-12');
SELECT order_date FROM orders;
SELECT now(); -- 返回当前日期和时间；
SELECT DATE (order_date) FROM orders;

# 查询订单表中的下单时间，将下单时间格式化为：年/月/日。格式
SELECT DATE_FORMAT(order_date, '%Y/%m/%d')
FROM orders;

SELECT date_format(order_date, '%y/%m/%d')
FROM orders;

# 查询订单表的下单时间，只返回年份
SELECT year(order_date)
FROM orders;
# 查询今天是一周中的第几天（西方周日为第一天）
SELECT dayofweek(now());
# 查询今天是周几
SELECT dayname(now());

SELECT COUNT(order_date)
FROM orders;

SELECT count(prod_desc)
FROM products;
# 查询商品表中单价最高的商品
SELECT max(prod_price)
FROM products;
# 查询商品表中单价最低的商品
SELECT min(prod_price)
FROM products;
# 查询商品表中所有商品单价的平均值
SELECT avg(prod_price)
FROM products;
# 查询商品表中一共有多少条数据 -- 注意和count（1）的区别，阿里手册推荐用count（*）
SELECT count(*)
FROM products;
SELECT count(1)
FROM products;
# 查询商品表中商品描述列，不为null 的数据有多少条
SELECT count(prod_desc)
FROM products;
# 查询商品表中供应商有几家？
SELECT count(DISTINCT vend_id)
FROM products;
# 查询订单明细表中，商品数量（quantity）的总和
SELECT sum(quantity)
FROM orderitems;
# 查询订单明细表中，订单编号为20005的订单的总订单金额
SELECT sum(item_price * orderitems.quantity)
FROM orderitems
WHERE order_num = 20005;
# 查询商品表中，一共多少条商品、最低价格、最高价格、平均价格；
SELECT count(prod_name), min(prod_price), max(prod_price), avg(prod_price)
FROM products;

SELECT count(*), min(prod_price), max(prod_price), avg(prod_price)
FROM products;

-- md5 加密函数
SELECT md5(prod_name) FROM products;
SELECT md5('Axieyun');
select md5('1993539830');

-- 查询当前⽤户
select user();
-- 查询当前数据库的版本号
select version();

-- IFNULL() 函数⽤于判断第⼀个表达式是否为 NULL，如果为 NULL 则返回第⼆个参数的值，如果不为 NULL 则返回第⼀个参数的值。
-- IFNULL() 函数语法格式为： IFNULL(expression, alt_value)

SELECT ifnull(prod_desc, '我是默认值null') FROM products;


~~~

