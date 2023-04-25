---
title: Linux端口
abbrlink: db44
date: 2022-02-05 21:30:27
tags:
password:
---







~~~tex
ufw是一个主机端的iptables类防火墙配置工具
~~~

### 安装

~~~shell
apt-get install update
apt-get install ufw
~~~









### 查看端口打开情况

~~~shell
sudo ufw status
~~~





### 打开端口



~~~shell
sudo ufw allow 端口号
~~~



### 关闭端口

~~~shell
sudo ufw delete allow 端口号
~~~

 

### 允许特定ip访问

~~~shell
ufw allow from ip地址 to any port 端口号
~~~







### 防火墙开启(ubuntu下执行)

~~~shell
sudo ufw enable
~~~



### 防火墙重启(ubuntu下执行)

~~~shell
sudo ufw reload
~~~



