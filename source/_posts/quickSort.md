---
title: quickSort
abbrlink: cb7f
date: 2022-06-09 13:36:53
tags:
password:
---



## 快速排序



### 定义

* data：需要排序的元素向量，元素下标从0开始
* l_ptr：data的左边界
* r_ptr：data的有边界
* flag：基准值、基准
* * 基准值左边：小于基准值的元素
  * 基准值右边： 大于等于基准值的元素





### 基本思想

* 1、在当前无序区data[l]到data[r]中任取一个记录作为比较的基准值flag
* 2、用此基准值将当前无序区划分为左右两个较小的无序区：
* * 即：data[l] 到 data[k - 1] 和 data[k + 1] 到 data[r]
  * k为flag最后插入的位置：即data[l] 到 data[k - 1] <= flag <= data[k + 1] 到 data[r]
  * data[l] 到 data[k - 1] ：为小于flag的无序区
  * data[k + 1] 到 data[r] ：为大于等于flag的无序区
* 3、data[l] 到 data[k - 1] 和 data[k + 1] 到 data[r] 非空时，将重复上述1、2步骤。





### 代码实现



~~~c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

typedef int dataType; //排序的数据类型 

void quickSort(dataType *head, int l_ptr, int r_ptr) {
	if (l_ptr >= r_ptr) return ;
    
	dataType flag = *(head + ((l_ptr + r_ptr) >> 1)); //保存基准值，以防丢死
	
	//dataType flag = head[l_ptr];
	//flag：基数、基准值
	//基准值左边：小于基准值的元素
	//基准值右边： 大于等于基准值的元素

	int l = l_ptr, r = r_ptr;
	while (l < r) {
		
		while (l < r && head[r] >= flag) --r; //找到一个小于flag的元素，如果存在
		if (l < r) { head[l] = head[r]; ++l; } //存在，则替换
		
		while (l < r && head[l] < flag) ++l; //找到一个大于flag的元素，如果存在
		if (l < r) { head[r] = head[l]; --r; } //存在，则替换
		
	}
	//r == l 
	head[r] = flag; //基准值最终的位置
	
	quickSort(head, l_ptr, r - 1);
	quickSort(head, r + 1, r_ptr);
	
	return ;
}

// 元素个数 
#define N 40

int main() {
    
	srand(time(NULL)); //生成随机种子
	dataType data[N];
    
	//生成数据 
	for (int i = 0; i < N; ++i) data[i] = rand() % 1000;
	
	//输出排序前的数据 
	puts("排序前：");
	for (int i = 0; i < N; ++i) printf("%d ", data[i]);
	
	//排序 
	quickSort(data, 0, N - 1);
	
	//输出排序后的数据 
	puts("\n排序后："); 
	for (int i = 0; i < N; ++i) printf("%d ", data[i]);
	
	return 0;
}
~~~



