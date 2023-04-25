---
title: MySQL之视图
tags: MySQL
abbrlink: 7a16
date: 2021-11-02 21:43:46
password:
---



### 视图作⽤

* 视图是 虚拟的表 。与包含数据的表不⼀样，视图只包含使⽤时动态检索数据的查询。
* 在视图创建之后，可以⽤与表基本相同的⽅式利⽤它们。可以对视图执⾏ SELECT 操作，过滤和排序数据，将视图联结到其他视图或 表，甚⾄能添加和更新数据。
* 重要的是知道视图仅仅是⽤来查看存储在别处的数据的⼀种设施。
* 视图本身不包含数据 ，因此它们返回的数据是从其他表中检索出来的。
* 在添加或更改这些表中的数据时，视图将返回改变过的数据。
* 因为视图不包含数据，所以每次使⽤视图时，都必须处理查询执⾏时所需的任⼀个检索。
* 如果你⽤多个联结和过滤创建了复杂的视图或者 嵌套了视图，可能会发现性能下降得很厉害。所以 不要滥用视图 。





### 注意事项



* 与表⼀样，视图必须唯⼀命名（不能给视图取与别的视图或表相同的名字）。
* 对于可以创建的视图数⽬没有限制。
* 为了创建视图，必须具有⾜够的访问权限。这些限制通常由数据库管理⼈员授予。
* 视图可以嵌套，即可以利⽤从其他视图中检索数据的查询来构造⼀个视图。
* ORDER BY 可以⽤在视图中，但如果从该视图检索数据 SELECT 中也含有 ORDER BY，那么该视图中的 ORDER BY 将被覆盖。
* 视图不能索引，也不能有关联的触发器或默认值。
* 视图可以和表⼀起使⽤。例如，编写⼀条联结表和视图的 SELECT 语句。







### 视图创建





#### 格式1



~~~mysql
create view 视图名 (列名，列名，...)
as (select语句)
(with check option);
~~~



#### 语法格式



~~~mysql
CREATE [OR REPLACE] [ALGORITHM={UNDEFIEND | MERGE | TEMPTABLE}]
VIEW view_name [(column_list)]
AS SELECT_statement
[WITH [CASCADED | LOCAL] CHECK OPTION];
~~~

* CREATE：表示创建视图的关键字
* OR REPLACE：如果给定了此子句，表示该语句能够替换已有视图
* ALGORIGHM：可选参数，表示视图选择的算法
* UNDEFIEND：表示MySQL将自动选择，所有使用的算法
* MERGE：表示将使用视图的语句，与视图定义合并起来，使得视图定义的某一部分，取代语句的对应部分
* TEMPTABLE：表示将视图的结果存入临时表，然后使用临时表执行语句
* View_name：表示要创建的视图名称
* Column_list：可选参数，表示属性清单，指定了视图中各个属性的名称，默认情况下，与SELECT语句中查询的属性相同
* AS：表示指定视图要执行的操作
* SELECT_statement：是一个完整的查询语句，表示从某个表或视图中查出，某些满足条件的记录，将这些记录导入视图中
* LOCAL：可选参数，表示创建视图时，只要满足该视图本身定义的条件即可
* CASCADED：可选参数，表示创建视图时，需要满足跟该视图有关的，所有相关视图和表的条件，该参数为默认值
* WITH CHECK OPTION：可选参数，表示创建视图时，要保证在该视图的权限范围之内



### 查看视图



~~~mysql
SHOW CREATE VIEW view_name;
~~~



### 删除视图



~~~mysql
drop view view_name;
~~~





### 更新视图



#### 方式1



* 可以先⽤ DROP 再⽤ CREATE



#### 方式二



* ⽤ CREATE OR REPLACE VIEW ，如果要更新的视图不存在，则会创建⼀个视图；如果要更新的视图存在，则会替换原有视图。





### 实例



#### 创建视图



~~~mysql
create view bt_s(s_no, s_name, s_birth)
as
select s_no, s_name, 2014-s_age
from student;

create view s_g(s_no, g_avg)
as
select s_no, avg(g_rade)
from sc
group by s_no; # 分组
~~~





#### 实战



~~~mysql
## 查询订单编号为20005的：产品名称，供应商名称，产品价格，购买数量

# 没有视图
SELECT prod_name, vend_name, prod_price, quantity
FROM orderitems
         JOIN products ON orderitems.prod_id = products.prod_id
         JOIN vendors ON products.vend_id = vendors.vend_id
WHERE orderitems.order_num = 20005;


# 有视图
CREATE VIEW order_detail AS # 创建视图
SELECT prod_name, vend_name, prod_price, quantity, order_num
FROM orderitems
         JOIN products ON orderitems.prod_id = products.prod_id
         JOIN vendors ON products.vend_id = vendors.vend_id;

# 删除视图
DROP VIEW order_detail;
# 更新视图
CREATE OR REPLACE VIEW order_datail();

# 查看创建视图的语句
SHOW CREATE VIEW order_detail;

# 查询视图所有数据
SELECT *
FROM order_detail;

# 从视图过滤数据
SELECT prod_name, vend_name, prod_price, quantity
FROM order_detail
WHERE order_num = 20005;

~~~

