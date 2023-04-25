---
title: dup和dup2
tags: 系统编程
abbrlink: 810c
date: 2022-01-28 11:03:11
password:
---



#### 函数原型



~~~c
       #include <unistd.h>

       int dup(int oldfd);
       int dup2(int oldfd, int newfd);

       #define _GNU_SOURCE             /* See feature_test_macros(7) */
       #include <fcntl.h>              /* Obtain O_* constant definitions */
       #include <unistd.h>

       int dup3(int oldfd, int newfd, int flags);
~~~







#### 作用

* dup：复制一份新的文件描述符（默认是没有使用的最小文件描述符），指向oldfd打开的文件。
* dup2：newfd指定文件描述符位置，且复制一份新的文件描述符，指向oldfd打开的文件。









~~~c
#include <stdio.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <stdlib.h>
#include <errno.h>


int main() {
    int fd, save_fd;
    if ((fd = open("test.txt", O_RDWR)) < 0) {
        perror("open");
        exit(1);
    }

    save_fd = dup(1);
    write(1, "hello stdout\n", 13);
    dup2(fd, 1);
    close(fd);
    write(1, "hello test.txt\n", 15);
    dup2(save_fd, 1);
    close(save_fd);
    return 0;
}

~~~

