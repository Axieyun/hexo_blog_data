---
title: linux内核源码下载
tags: linux
abbrlink: 7e3f
date: 2022-03-13 17:05:14
password:
---



#### 1、查看内核版本
~~~bash
apt-cache search linux-source
~~~


**显示如下**

~~~tex
linux-source - Linux kernel source with Ubuntu patches
linux-source-5.4.0 - Linux kernel source for version 5.4.0 with Ubuntu patches
linux-gkeop-source-5.4.0 - Linux kernel source for version 5.4.0 with Ubuntu patches
~~~


#### 我的版本

* 5.4.0




#### 下载内核源码

* 1、命令
~~~bash
sudo apt install linux-source-5.4.0
~~~

* 2、解压
下载内容放在目录/usr/src下
~~~bash
cd /usr/src
sudo tar -xjvf linux-source-5.4.0.tar.bz2
~~~


#### 配置


* 不需要在 /usr/src/linux-source-5.4.0目录
~~~bash
sudo ctags -R /usr/src/linux-source-5.4.0
~~~



* 在/usr/src/linux-source-5.4.0目录
~~~bash
sudo ctags -R*
~~~

*源码获取完毕*














