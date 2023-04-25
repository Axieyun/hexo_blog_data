---
title: wait与waitpid
tags: 系统编程
abbrlink: bc2f
date: 2022-01-28 12:29:15
password:
---



~~~c
       #include <sys/types.h>
       #include <sys/wait.h>

       pid_t wait(int *wstatus);

       pid_t waitpid(pid_t pid, int *wstatus, int options);

~~~





#### 注意

* 父进程调用wait或者waitpid时，可能会：
* * 1、阻塞（如果它所有的子进程都在运行）。
  * 2、带子进程的终止信息立即返回（如果一个子进程已经终结，正等待父进程读取其终止信息）。
  * 3、出错立即返回（它没有任何子进程）









#### 两个函数区别



~~~tex
如果父进程的所有子进程都在运行，调用wait将使父进程阻塞，而调用waitpid时，如果在options参数中指定WNOHANG（wnohang）可以时父进程不阻塞而立即返回0。
wait等待一个终止的子进程。waitpid可以通过pid参数指定等待哪一个子进程。
~~~

