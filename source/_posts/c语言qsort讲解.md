---
title: c语言qsort讲解
abbrlink: '461'
date: 2021-08-13 14:58:14
tags: c语言
password:
---





#### 原型



**void qsort(void *Base, int size_t_NumOfElements, size_t_SizeOfElements, int (*_PtFuncCompare)(const void *, const void *))**



**其中**



* Base：要排序数组的第一个元素地址
* size_t_NumOfElements ：排序元素的个数
* size_t_SizeOfElements ：该类型元素的字节个数
*  (*_PtFuncCompare)(const void *, const void *) ：排序规则函数







#### 排序规则函数解析



**int类型元素排序**

~~~c

//*((int *)a) == 将无类型指针转换为int型指针，在取该地址元素值

int comp(const void *a, const void *b) {
	return *((int *)a) - *((int *)b); //a - b表示升序
}

int comp(const void *a, const void *b) {
	return *((int *)b) - *((int *)a); //b - a表示降序
}



~~~





**结构体类型元素排序**



~~~
typedef struct Node {
	int time, key;
} Node;

//a - b  表示升序
//b - a  表示降序
//排序time;
int comp(const void *a,const void *b) { //无类型指针 
	return ((Node *)a)->time - ((Node *)b)->time;//强制转换为该类型指针 
}
//排序key
int comp(const void *a,const void *b) { //无类型指针 
	return ((Node *)a)->key - ((Node *)b)->key;//强制转换为该类型指针 
}

~~~







#### 结构体排序代码演示



~~~
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
	int time, key;
} Node;


//a - b  表示升序
//b - a  表示降序
int comp(const void *a,const void *b) { //无类型指针 
	return ((Node *)a)->time - ((Node *)b)->time;//强制转换为该类型指针 
}

Node num[32];

int main() {
	int n;
	scanf("%d", &n);
	for (int i = 1; i <= n; i++) {
		num[i].key = i;
		scanf("%d", &num[i].time);
	}
	qsort(num + 1, n, sizeof(Node), comp);
	int sum = 0, temp = 0;
	for (int i = 1; i <= n; i++) {
		(i > 1) && printf(" ");
		printf("%d", num[i].key);
		if (i > 1) temp += num[i - 1].time;
		sum += temp;
	}
	printf("\n%.2lf\n", (double)sum / n);
	return 0;
}
~~~



