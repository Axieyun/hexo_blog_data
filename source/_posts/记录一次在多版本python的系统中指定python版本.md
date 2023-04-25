---
title: 记录一次在多版本python的系统中指定python版本
tags: 日志
abbrlink: '8122'
date: 2022-03-16 21:09:07
password:
---







*在编译cocos2d-x-3.17.2需要2.7版本的python，电脑同时包含多个python版本，我们需要指定python本版来进行编译。*

*因为在电脑中，会根据python的环境变量出现顺序执行*





**操作前的准备工作**

* 在dos命令窗口中执行以下命令

~~~dos
where python
~~~

* 出现多个版本python（默认python环境变量配置好了）

~~~tex
F:\python3.8.10\python.exe
F:\py2.7.6\python.exe
F:\anaconda3\python.exe
C:\Users\Axieyun\AppData\Local\Microsoft\WindowsApps\python.exe
~~~



*所以我们运行python的时候都是运行python3.8.10版本的*



### 操作



#### 方法一

* 进入python3.8.10的安装路径，把**python.exe**重命名为**python3.exe**





#### 方法二

* 修改python的环境变量的出现顺序。
