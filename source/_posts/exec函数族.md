---
title: exec函数族
tags: 系统编程
abbrlink: d811
date: 2022-01-27 22:00:32
password:
---









**当进程调用一种exec函数时，该进程的用户空间代码和数据完全被新程序替换，从新的程序的启动例程开始执行。**



**这些函数如果调用成功则加载新的程序从启动代码开始执行，不再返回，如果调用出错则返回-1，所以exec函数只有出错的返回值。**



#### 头文件

~~~c
#include <unistd.h>
extern char** environ;
int execl(const char* path, const char* arg, ...);
int execlp(const char* file, const char* arg, ...);
int execle(const char* path, const char* arg, ..., char* const envp[]);
带有字母l（表示list）的exec函数要求将新程序的每个命令行参数都当作一个参数传给它，命令行参数的个数可变的，最后一个可变参数应该是NULL，起哨兵作用
    
    
int execv(const char* path, char* const argv[]);
int execvp(const char* file, char* const argv[]);
int execvpe(const char* file, char* const argv[], char* const envp[]);
对于带有v（表示vector）的函数，则应该先构造一个指向各个参数的指针数组，然后将该数组的首地址当作参数传给它，数组最后一个指针为NULL
    

不带字母p（表示path）的exec函数第一个参数必须是程序的相对路径或绝对路径，例如"/bin/ls"或"./a.out"。
    
对于带字母p的函数：如果参数中包含/，则将其视为路径名。否则视为不带路径的程序名，在PATH环境变量的目录列表搜索这个程序。
    
对于以e（表示environment）结尾的exec函数，可以把一份新的环境变量传给它，其他exec函数仍然使用当前的环境变量表执行新程序。


int execve(const char* finlename, char* const argv[], char* const envp[]);
~~~







~~~c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
    execlp("ls", "ls", "-al", "-l", NULL);
    perror("execlp");
    exit(1);
}

~~~

