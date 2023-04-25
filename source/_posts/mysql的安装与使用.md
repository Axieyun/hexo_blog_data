---
title: mysql的安装与使用
abbrlink: 7c0b
date: 2022-05-27 19:16:24
tags:
password:
---



### 安装

#### 1、下载压缩包

~~~bash
cd /usr/local

sudo wget https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.35-linux-glibc2.12-x86_64.tar.gz
~~~



#### 2、在当前目录解压

~~~bash
tar -zxvf mysql-5.7.35-linux-glibc2.12-x86_64.tar.gz
~~~



#### 3、准备一些依赖包

* 由于 MySQL 初始化数据时依赖了 libaio，如果你的服务器没有的话可能会出错，为了保险起⻅，建议安装⼀下。

~~~bash
apt-cache search libaio # search for info
apt-get install libaio1 # install library
~~~



#### 4、安装并初始化

* 1创建一个新的工作mysql组, 新工作组的信息将被添加到系统文件中

  ~~~bash
  groupadd mysql
  useradd -r -g mysql -s /bin/false mysql
  ~~~

* 建立软链接

  ~~~bash
  ln -s /usr/local/mysql-5.7.35-linux-glibc2.12-x86_64 mysql
  ~~~

* ~~~bash
  cd mysql
  mkdir mysql-files
  chown mysql:mysql mysql-files
  chmod 750 mysql-files
  bin/mysqld --initialize --user=mysql
  bin/mysql_ssl_rsa_setup
  bin/mysqld_safe --user=mysql &
  cp support-files/mysql.server /etc/init.d/mysql.server
  ~~~



#### 5、登录

~~~bash
mysql -uroot -p
~~~



#### 6、修改密码

~~~mysql
alter user 'root'@'localhost' identified by '你的密码';
~~~

* 创建 root 账号允许远程登录，并授予所有权限。

  ~~~mysql
  CREATE USER 'root'@'%' IDENTIFIED BY '你的密码';
  GRANT ALL ON *.* TO 'root'@'%';
  ~~~



#### 7、退出

~~~bash
exit;
\quit
\q
~~~



### 使用

#### 启动

~~~bash
service mysql.server start
~~~

#### 关闭

~~~bash
service mysql.server stop
~~~







### 忘记密码？

#### 1、关闭数据库服务

~~~bash
service mysql.server stop;
~~~

#### 2、修改mysql的配置文件

* 进入到mysql文件夹(压缩包解压的文件夹)

  ~~~bash
  # ln -s /usr/local/mysql-5.7.35-linux-glibc2.12-x86_64 mysql #这是我之前建立的软链接
  cd /usr/local/mysql
  ~~~

* 查询my.cnf文件

  ~~~bash
  bin/mysqld --help --verbose | grep my.cnf
  ~~~

  * 查询结果

    ![1](http://blog.axieyun.top/img/35.png)

* 进入第一个配置文件添加 skip-grant-tables，使登录时跳过权限检查；

  ~~~bash
  sudo vim /etc/my.cnf
  ~~~

#### 3、重启数据库服务

~~~bash
service mysql.server start;
~~~

#### 4、登录mysql

~~~bash
mysql -uroot -p
~~~

#### 5、修改密码

* 1、刷新MySQL权限相关的表

  ~~~mysql
  flush privileges;
  ~~~

* 2、修改密码

  ~~~mysql
  alter user 'root'@'localhost' identified by '你的密码';
  ~~~

* 3、刷新MySQL权限相关的表

  ~~~bash
  flush privileges;
  ~~~

#### 6、退出mysql

~~~mysql
exit;
~~~

#### 7、最后

* 1、关闭数据库；
* 2、把加入配置文件的skip-grant-tables删除或者注释掉；
* 3、重启数据库；
* 4、登录；
