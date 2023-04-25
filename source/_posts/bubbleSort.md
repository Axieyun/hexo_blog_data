---
title: bubbleSort
abbrlink: 6b7c
date: 2022-06-09 13:53:35
tags:
password:
---

## 冒泡排序







### 思想

* 对于n个元素，至多需要==n - 1==趟排序







### 代码演示

* 外层循环是趟数
* 内层循环是待排序元素集合



#### 外层循环从1开始

~~~cpp
//降序 
void bubbleSort4(dataType *data, int n) {
	for (int i = 1; i <= n - 1; ++i) {
		int noSwap = true;
		for (int j = 1; j <= n - i; ++j) {
			if (data[j] > data[j - 1]) {
				int temp = data[j - 1];
				data[j - 1] = data[j];
				data[j] = temp;
				noSwap = false;
			}
		}
		if (noSwap) break;
	}
	return ;
}
~~~

| 趟数 | 待排序元素集合下标 |
| ---- | ------------------ |
| 1    | 0 ~ n - 1          |
| 2    | 0 ~ n - 2          |
| ...  | ...                |
| i    | 0 ~ n - i          |

* 得到：0 <= j <= n - i  && 0 <= j - 1 <= n - i
* 推出：1 <= j <= n - i





~~~cpp
//降序
void bubbleSort2(dataType *data, int n) {
	for (int i = 1; i <= n - 1; ++i) {
		int noSwap = true;
		for (int j = n - 1; j >= i; --j) {
			if (data[j] > data[j - 1]) {
				int temp = data[j];
				data[j] = data[j - 1];
				data[j - 1] = temp;
				noSwap = false;
			}
		}
		if (noSwap) break;
	}
	return ;
}
~~~

| 趟数 | 待排序元素集合下标 |
| ---- | ------------------ |
| 1    | 0 ~ n - 1          |
| 2    | 1 ~ n - 1          |
| ...  | ...                |
| i    | i -1 ~ n - 1       |

* 得到：i - 1 <= j <= n - 1  &&  j - 1 <= j - 1 <= n - 1
* 推出：i <= j <= n - 1







#### 外层循环从0开始

~~~cpp
//升序
void bubbleSort5(dataType *data, int n) {
	for (int i = 0; i < n - 1; ++i) {
		int noSwap = true;
		for (int j = n - 2; j >= i; --j) {
			if (data[j] > data[j + 1]) {
				int temp = data[j + 1];
				data[j + 1] = data[j];
				data[j] = temp;
				noSwap = false;
			}
		}
		if (noSwap) break;
	}
	return ;
}
~~~

| 趟数 | 待排序元素集合下标 |
| ---- | ------------------ |
| 0    | 0 ~ n - 1          |
| 1    | 1 ~ n - 1          |
| ...  | ...                |
| i    | i  ~ n - 1         |

* 得到：i <= j <= n - 1    &&   i <= j + 1 <= n - 1
* 推出：i <= j <= n - 2







~~~cpp
//升序
void bubbleSort3(dataType *data, int n) {
	for (int i = 0; i < n - 1; ++i) {
		int noSwap = true;
		for (int j = 0; j <= n - i - 2; ++j) {
			if (data[j] > data[j + 1]) {
				int temp = data[j];
				data[j] = data[j + 1];
				data[j + 1] = temp;
				noSwap = false;
			}
		}
		if (noSwap) break;
	}
	return ;
}
~~~

| 趟数 | 待排序元素集合下标 |
| ---- | ------------------ |
| 0    | 0 ~ n - 1          |
| 1    | 0 ~ n - 2          |
| ...  | ...                |
| i    | 0  ~ n - i - 1     |

* 得到：0 <= j <= n - i - 1    &&   0 <= j + 1 <= n - i - 1
* 推出：0 <= j <= n - i - 2







### 测试

~~~cpp
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

typedef int dataType; //排序的数据类型 

//升序
void bubbleSort5(dataType *data, int n) {
	for (int i = 0; i < n - 1; ++i) {
		int noSwap = true;
		for (int j = n - 2; j >= i; --j) {
			if (data[j] > data[j + 1]) {
				dataType temp = data[j + 1];
				data[j + 1] = data[j];
				data[j] = temp;
				noSwap = false;
			}
		}
		if (noSwap) break;
	}
	return ;
}
//升序
void bubbleSort3(dataType *data, int n) {
	for (int i = 0; i < n - 1; ++i) {
		int noSwap = true;
		for (int j = 0; j <= n - i - 2; ++j) {
			if (data[j] > data[j + 1]) {
				dataType temp = data[j];
				data[j] = data[j + 1];
				data[j + 1] = temp;
				noSwap = false;
			}
		}
		if (noSwap) break;
	}
	return ;
}

//降序
void bubbleSort2(dataType *data, int n) {
	for (int i = 1; i <= n - 1; ++i) {
		int noSwap = true;
		for (int j = n - 1; j >= i; --j) {
			if (data[j] > data[j - 1]) {
				dataType temp = data[j];
				data[j] = data[j - 1];
				data[j - 1] = temp;
				noSwap = false;
			}
		}
		if (noSwap) break;
	}
	return ;
}

//降序 
void bubbleSort4(dataType *data, int n) {
	for (int i = 1; i <= n - 1; ++i) {
		int noSwap = true;
		for (int j = 1; j <= n - i; ++j) {
			if (data[j] > data[j - 1]) {
				dataType temp = data[j - 1];
				data[j - 1] = data[j];
				data[j] = temp;
				noSwap = false;
			}
		}
		if (noSwap) break;
	}
	return ;
}

// 元素个数
#define N 30


int main() {
	srand(time(NULL));
	dataType data[N];
	//生成数据 
	for (int i = 0; i < N; ++i) data[i] = rand() % 1000;
	
	//输出排序前的数据 
	puts("排序前：");
	for (int i = 0; i < N; ++i) printf("%d ", data[i]);
	
	//降序 
	bubbleSort2(data, N);
	//输出排序后的数据 
	puts("\n降序后："); 
	for (int i = 0; i < N; ++i) printf("%d ", data[i]);	
	
	//升序 
	bubbleSort3(data, N);
	//输出排序后的数据 
	puts("\n升序后："); 
	for (int i = 0; i < N; ++i) printf("%d ", data[i]);
	
	//降序 
	bubbleSort4(data, N);
	//输出排序后的数据 
	puts("\n降序后："); 
	for (int i = 0; i < N; ++i) printf("%d ", data[i]);	
	
	//升序 
	bubbleSort5(data, N);
	//输出排序后的数据 
	puts("\n升序 后："); 
	for (int i = 0; i < N; ++i) printf("%d ", data[i]);	
	return 0;
}
~~~

