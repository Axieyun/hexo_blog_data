---
title: epoll
tags: 网络编程
abbrlink: f84b
date: 2022-02-05 13:53:23
password:
---

~~~c
 #include <sys/epoll.h>

~~~











### epoll_create()



~~~c
int epoll_create(int size);
~~~

* 创建一个epoll对象，返回调用该对象的句柄

* size ：告诉内核这个监听的数目一共有多大。实际是size + 1个，本身占一个。
* 执行epoll_create()时，创建了红黑树和就绪链表；



### epoll_ctl()

* 

~~~c
int epoll_ctl(int epfd, int op, int fd, struct epoll_event *event);

typedef union epoll_data {
    void *ptr;
    int fd;
    __uint32_t u32;
    __uint64_t u64;
} epoll_data_t;

struct epoll_event {
    __uint32_t events; /* Epoll events */
    epoll_data_t data; /* User data variable */
};
~~~

* epfd : epoll对象的句柄
* op：有三种选择：
* * EPOLL_CTL_ADD：注册新的fd到epfd中
  * EPOLL_CTL_MOD：修改已经注册的fd的监听事件；
  * EPOLL_CTL_DEL：从epfd中删除一个fd；
* fd：op操作的fd
* event：事件结构体的指针，需要干啥事，回调函数等待
* * events ：结构体epoll_event的参数，events可以是以下几个宏的集合：
  * * EPOLLIN ：表示对应的文件描述符可以读（包括对端SOCKET正常关闭）；
    * EPOLLOUT：表示对应的文件描述符可以写；
    * EPOLLPRI：表示对应的文件描述符有紧急的数据可读（这里应该表示有带外数据到来）；
    * EPOLLERR：表示对应的文件描述符发生错误；
    * EPOLLHUP：表示对应的文件描述符被挂断；
    * EPOLLET： 将EPOLL设为边缘触发(Edge Triggered)模式，这是相对于水平触发(Level Triggered)来说的。
    * EPOLLONESHOT：只监听一次事件，当监听完这次事件之后，如果还需要继续监听这个socket的话，需要再次把这个socket加入到EPOLL队列里
* 执行epoll_ctl()时，如果增加socket句柄，则检查在红黑树中是否存在，存在立即返回，不存在则添加到树干上，然后向内核注册回调函数，用于当中断事件来临时向准备就绪链表中插入数据；

* ET模式（边缘触发）**只有数据到来才触发**，**不管缓存区中是否还有数据**，缓冲区剩余未读尽的数据不会导致epoll_wait返回；



### epoll_wait()

* 返回需要处理事件的集合数量，0表示超时

~~~c
int epoll_wait(int epfd, struct epoll_event * events, int maxevents, int timeout);
~~~



* events：接收需要处理事件的集合的指针，指向一个数组
* maxevents：告之内核这个events有多大，小于等于 epoll_create函数的参数size
* timeout ：超时时间，（单位ms），0会立即返回，-1将不确定，也有说法说是永久阻塞。





