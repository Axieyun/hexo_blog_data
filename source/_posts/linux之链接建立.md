---
title: linux之链接建立
tags: linux基础
abbrlink: 47c5
date: 2022-01-27 14:40:23
password:
---







#### 硬连接



~~~shell
ln 原文件路径 建立的连接名
~~~



* 例如：为当前目录下test.txt建立硬链接

* * ~~~shell
    ln ./test.txt ttt
    ~~~







#### 软链接



~~~shell
ln -s 原文件路径 建立的连接名字
~~~

* 例如：为当前目录下test.txt建立软链接

* * ~~~shell
    ln -s ./test.txt ttt
    ~~~







#### 区别



* 硬链接：在inode的引用原文件的计数+1，没有创建新的文件，你修改源文件内容，访问硬链接得到的内容就是修改后的内容。
* 软链接：创建新的文件，存储源（原）文件路径，例如ttt存储了./test.txt







#### stat命令



* 打印文件inode信息



~~~shell
stat 文件名
~~~

