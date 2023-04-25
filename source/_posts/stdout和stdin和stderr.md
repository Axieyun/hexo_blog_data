---
title: stdout和stdin和stderr
tags: 系统编程
abbrlink: '9021'
date: 2022-01-25 19:09:04
password:
---









~~~tex
程序启动时会自动打开三个文件：标准输入、标准输出和标准错误输出。
在C标准中分别用FILE* 指针stdin、stdout、stderr表示。
这三个文件的文件描述符分别对应0、1、2的int型数据，保存在相应的FILE结构体中。
其中文件描述符是文件描述符表（FILE结构体）的索引。
~~~

*在头文件unistd.h中有如下宏定义来表示这三个文件描述符*



~~~c
#define STDIN_FILEND 0
#define STDOUT_FILEND 1
#define STDERR_FILEND 2
~~~

