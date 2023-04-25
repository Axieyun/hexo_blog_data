---
title: 系统调用open函数
tags: 系统编程
abbrlink: ffb4
date: 2022-01-26 13:05:42
password:
---





### open()





#### 头文件



~~~c
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

~~~



#### 函数原型



~~~c
int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode);
~~~







* pathname:需要打开或者创建的文件的相对路径或者绝对路径

* flags参数有一系列的常数值可以选择，可以同时选择多个常数用按位与运算连接起来，所以这些常数的宏定义都以o_开头，表示or

* * 必选项：以下三个常数必须指定一个，切仅能指定一个

    ~~~tex
    O_RDONLY 只读打开
    O_WRONLY 只写打开
    O_RDWR 可读可写打开
    ~~~

  * 可选项：以下常数可以同时指定0个或多个，和必选项按位与起来作为flags参数

  * ~~~tex
    O_CREAT 若此文件不存在则创建它，必须提供第三个参数mode，表示文件的访问权限
    O_EXCL 如果同时指定了O_CREAT，并且文件存在，则返回出错
    O_TRUNC 如果文件存在，并且以只写或可读可写方式打开，则将其长度截断为0字节（清空）
    O_APPEND 表示追加。如果文件已有内容，这次打开文件所写的数据附加到文件的末尾
    O_NONBLOCK 对于设备文件，以O_NONBLOCKF方式打开可以做非阻塞I/O。
    ~~~

* mode：指定文件权限，可以用八进制数表示，比如0644表示-rw-r--r--，也可以用宏定义按位与表示起来。





#### 返回值

* 该进程尚未使用的最小的文件描述符







