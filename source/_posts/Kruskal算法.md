---
title: Kruskal算法
abbrlink: d923
date: 2021-09-28 19:18:25
tags:
      - 树
      - 图论
password:
---





### 克鲁斯卡算法



#### 算法思想



**以边为主**



* （1）将图中的所有边都去掉。
* （2）将边按权值（排序，升序）从小到大的顺序添加到图中，保证添加的过程中不会形成环
* （3）重复上一步直到连接所有顶点，此时就生成了最小生成树。这是一种贪心策略。



**Kruskal算法就是基于并查集的贪心算法**





#### 使用地方



* **用于边数较少的稀疏图**





#### 时间复杂度O(|Elog|E|)

* E为边的数量







#### 代码演示





~~~c
#include <stdio.h>
#include <stdlib.h>

using namespace std;

const int M = 100010;
const int N = 100000;

typedef struct Node { //边的集合 
	int a, b, v;
} Node;

int fa[N]; //集合 
Node edges[M];  //存储边的数组 

int n, m; //n个点，m条边 

int comp(const void *a, const void *b) {
	return ((Node *)a)->v - ((Node *)b)->v;
}

inline void init(int n) { //集合元素初始化 
	for (int i = 1; i <= n; i++) {
		fa[i] = i;
	}
	return ;
}

inline int find(int x) {
	return x == fa[x] ? x : fa[x] = find(fa[x]);
}

void merge(int a, int b) { // 可能退化为链表 
	int f_a = find(a), f_b = find(b);
	if (f_a == f_b) return ;
	fa[f_a] = f_b;
	return ;
}

void Kruskal() { //最小生成树算法 
	init(n);
	qsort(edges + 1, m, sizeof(Node), comp);
	int t = 0, sum = 0, cnt_point = 1;
	while (++t <= m && cnt_point < n) {
		int f_a = find(edges[t].a);
		int f_b = find(edges[t].b);
		if (f_a == f_b) continue; //a与b在一个联通环内，如果连接，则a与b形成环 
		merge(edges[t].a, edges[t].b);
		cnt_point++; //点的数量加一 
		sum += edges[t].v;
		//printf("%d - %d = %d, %d\n", edges[t].a, edges[t].b, edges[t].v, sum);
	}
	printf("%d\n", cnt_point == n ? sum : -1);
	return ;
}


int main() {
	
	scanf("%d %d", &n, &m);
	for (int i = 1; i <= m; i++) {
		scanf("%d %d %d", &edges[i].a, &edges[i].b, &edges[i].v);
	}
	Kruskal();	
	return 0;
}

/*
7 11
1 2 7
1 4 5
2 3 8
2 4 9
2 5 7
3 5 5
5 7 9
6 7 11
4 6 6
5 6 8
4 5 15
*/
~~~







