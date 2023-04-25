---
title: 记录一次mysql日志
abbrlink: fc7d
date: 2022-02-06 20:15:47
tags:
password:
---





*2022.2.6日，因为c语言网络需要使用mysql，登录我几个月没有登录的mysql*





#### 报错如下

~~~shell
root@Axieyun ~ # mysql -uroot -p                                               
Enter password:
ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)

~~~





#### 解决

~~~shell
打开mysqld服务
service mysqld start
打开mysql服务
service mysql start
建立软连接
root@Axieyun ~ # ln -s /tmp/mysql.sock /var/run/mysqld/mysqld.sock   
~~~



**解决了**





