---
title: C语言中#的使用
abbrlink: 2c9a
date: 2021-05-13 15:54:12
tags: 随手笔记
---



## 一：使用#在宏的作用



~~~c

#define P(func) {\
	printf("%s\n", #func);\
}
相当于传入abc,就输出abc;
即P(abc) = abc;
~~~















