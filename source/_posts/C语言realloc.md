---
title: C语言realloc
tags: C语言基础
abbrlink: 64c9
date: 2022-01-25 15:55:42
password:
---







* 当`realloc`第一个参数所指向的内存空间大小不足够扩大为第二个参数所指定的的大小时，`realloc`将新分配一段足够大的内存空间，将旧的那段内存空间里的内容拷贝过去然后释放旧的内存空间。
