---
title: C++lambda表达式动态执行
tags: C++
abbrlink: 6dc3
date: 2022-02-26 13:56:49
password:
---

#### 作用

* 先于main函数之前执行


~~~ c

#include <stdio.h>
#include <stdlib.h>
#include <vector>
#include <iostream>

#include <time.h>

using namespace std;

int main() {
	printf("df\n");
	return 0;
}

//在main函数之前执行
//lambda的动态创建执行
int _p = [](int&& a) { //万能引用传递
	printf("hello %d!!!\n", forward<int&&>(a) );
	return 0;
}(0);

~~~



~~~c

int _IO = []() {
	std::ios::sync_with_stdio(0);
	cin.tie(0); 
  	cout.tie(0);
	return 0;
}();

~~~

