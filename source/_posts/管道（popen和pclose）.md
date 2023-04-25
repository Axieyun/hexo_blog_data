---
title: 管道（popen和pclose）
tags: 系统编程
abbrlink: 3b37
date: 2022-01-28 22:16:19
password:
---





**这两个函数实现的操作：创建一个管道，fork一个子进程，关闭管道不使用端，exec一个cmd命令，等待命令终止**

~~~c
       #include <stdio.h>

       FILE *popen(const char *command, const char *type);

       int pclose(FILE *stream);

~~~







#### 过程

* 函数popen先执行fork，然后调用exec以执行command，并且返回一个标准I/O文件指针。
* 如果type是”r“，则文件指针连接到cmd的标准输出
* 如果type是”w“，则文件指针连接到cmd的标准输入
* pclose函数关闭标准I/O流，等待命令执行结束，然后返回cmd的终止状态；
* 如果cmd不能被执行，则pclose返回的终止状态与shell执行exit一样。







~~~c
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

int main() {
    FILE* f = popen("cat ./test.txt", "r"); //只读
    if (!f) {
        perror("popen");
        exit(1);
    }

    char c;
    while (~(c = fgetc(f))) {
        putchar(toupper(c)); //小写字母变大写
    }

    return 0;
}
~~~

