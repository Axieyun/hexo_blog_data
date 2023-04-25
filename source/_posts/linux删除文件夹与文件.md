---
title: linux删除文件夹与文件
tags:
  - linux
  - 云主机
abbrlink: '1302'
date: 2021-10-09 13:28:44
password:
---



*每天学习一个linux小操作*





## rm



* rm（英文全拼：remove）,作用: 删除文件或目录



### -r

* 向下递归删除所有东西（文件与文件夹）



#### 实例



* ~~~bash
  rm -rf /var/log/httpd/access
  ~~~

* 删除/var/log/httpd/access目录以及其下所有文件、文件夹





### -f

* 就是直接强行删除，不作任何提示的意思



#### 实例

* ~~~bash
  rm -f /var/log/httpd/access.log
  ~~~

* 将会强制删除/var/log/httpd/access.log这个文件



