---
title: C语言函数scanf的认识
tags:
  - c语言基础
abbrlink: 48a8
date: 2021-10-22 16:00:33
password:
---



#### 读入字母，陷入死循环？？？？？？？？？



~~~
#include <stdio.h>

int main() {
	// ~0 = -1;
	int a;
	while (~scanf("%d", &a)) {
		//陷入死循环
		/*原因：
		scanf返回值==按照占位符正确输入的数量。 
		当没有按照占位符输入的数据类型时，scanf返回0，
		而那个没有按照占位符输入的数据类型存在缓冲区没有被读取，
		while循环使得scanf一直从缓冲区读取那个字符，从而导致while陷入死循环。 
		解决方法：刷新缓冲区即可
		1、getchar();
		 
		*/ 
	}
	printf("%d\n", a); 
	return 0;
}
~~~





### 究其原因



~~~
#include <stdio.h>

int main() {
	// ~0 = -1;
	int a;
	while (~scanf("%d", &a)) {

		getchar();
		 
	}
	printf("%d\n", a); 
	return 0;
}
~~~

