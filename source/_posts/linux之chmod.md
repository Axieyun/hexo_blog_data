---
title: linux之chmod
tags: linux
abbrlink: 458a
date: 2022-03-28 17:36:55
password:
---





### chmod

~~~shell
chomd 文件身份+操作符+权限符号 文件路径或者文件夹（目录）路径
~~~



#### 文件的身份

* u：user 文件所有者
* g：group 文件所属群组
* o：other 其他拥有者
* a：all全部身份



#### 操作符的三种类

* +：增加

* -：减少

* =：设定



#### 权限符号



* r：read可读
* w：write可写
* x：execute可执行









#### 修改文件权限



* 给当前路径文件test.html增加其它用户可读权限

~~~shell
sudo chmod o+r ./test.html
~~~

* 给当前路径文件test.html增加所属组的用户可执行权限

~~~shell
sudo chmod g+x ./test.html
~~~

* 设置当前路径文件test.html所属用户拥有者的可读可写可执行权限

~~~shell
sudo chmod u=rwx ./tset.html
~~~







#### 修改文件夹（目录）权限



* 给当前路径的文件夹（目录）test增加其它用户可读权限

~~~shell
sudo chmod -R o+r ./test
~~~

* 给当前路径的文件夹（目录）test增加所属组的用户可执行权限

~~~shell
sudo chmod -R g+x ./test
~~~

* 设置当前路径的文件夹（目录）test所属用户拥有者的可读可写可执行权限

~~~shell
sudo chmod -R u=rwx ./tset
~~~

