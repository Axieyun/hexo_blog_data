---
title: 进程创建fork函数与收尸wait函数
tags: 系统编程
abbrlink: 6a9b
date: 2022-01-25 20:56:12
password:
---



*一个进程，包括代码、数据和分配给进程的资源。*



### fork()





**头文件：unistd.h**



#### 原型



~~~c
pid_t fork(void);
~~~





#### 返回值



* 在父进程中，fork返回新创建子进程的进程标识符（process ID）；

* 在子进程中，fork返回0；

* 如果出现错误，fork返回一个负值；

* * 出错原因

  * ~~~tex
    1、当前的进程数已经达到了系统规定的上限，这时errno的值被设置为EAGAIN。
    2、系统内存不足，这时errno的值被设置为ENOMEM。
    ~~~





**也就是说，我们可以通过返回值判断当前处于那个进程中。**





* fork()函数通过系统调用创建一个与原来进程几乎完全相同的进程，也就是两个进程可以做完全相同的事，但如果初始参数或者传入的变量不同，两个进程也可以做不同的事。
* 一个进程调用fork()函数后，系统先给新的进程分配资源，将现在运行的进程的处理器状态、地址空间直接拷贝到新线程。例如存储数据和代码的空间。然后把原来的进程的所有值都复制到新的新进程中，只有少数值与原来的进程的值不同。相当于克隆了一个自己。







### exit()

~~~c
void exit(int status);
~~~

~~~tex
参数status表示当前进程退出时的结束状态。
当用这个函数结束一个子进程时，status的值会作为子进程的结束状态被提供给wait的第一个参数。
~~~



### wait()

~~~c
pid_t wait(int* stat_loc);
~~~

~~~tex
参数stst_loc存储等待的进程的结束状态值，也就是上面的exit(status)的status的值
返回值：结束状态值的子进程的进程标识id；如果等待过程中出现错误（如没有可以等待的子进程），那么返回-1。
~~~







### 代码实践



~~~c
#include <unistd.h>
#include <sys/wait.h>
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("%s\n", "我是鸣人！");
    
    pid_t pid = fork(); //创建子进程
    
    if (pid != 0) { //说明我们没有在子进程中
        int status;
        pid_t result = wait(&status);
        if (result == -1 || status != 0) {
            printf("%s\n", "可恶，又失败了，再来一次！");
            return -1;
        } else {
            printf("%s\n", "我负责性质变化！");
        }
        
    } else { //说明我们在子进程中
        pid_t second_pid = fork();
        
        if (second_pid != 0) {//说明我们在子进程中
            int new_status;
            pid_t new_result = wait(&new_status);
            //没有可以等待的子进程或者子进程没有退出
            if (new_result == -1 || new_status != 0) {
                exit(-1); //子进程退出返回-1
            } else {
                printf("%s\n", "我负责形态变化！");
                exit(0); //子进程退出返回0
            }
        } else {//说明我们在孙进程中
            printf("%s\n", "我负责产生查克拉！");
            exit(0);     
        }
    }
    return 0;
}
~~~



