---
title: mysql创建用户设置权限
abbrlink: bc67
date: 2022-05-28 16:13:43
tags:
password:
---



#### 1、创建数据库

~~~mysql
create database wordpress;
~~~



#### 2、创建用户

~~~mysql
create user wordpress@localhost;
~~~





#### 3、设置密码

~~~mysql
alter user 'wordpress'@'localhost' identified by '该用户的密码';
~~~



#### 4、配置权限

~~~mysql
GRANT ALL PRIVILEGES ON wordpress.* to 'wordpress'@'localhost' IDENTIFIED BY '该用户的密码';
~~~



#### 5、刷新权限配置

~~~mysql
flush PRIVILEGES;
~~~

