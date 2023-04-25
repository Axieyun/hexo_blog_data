---
title: dfs分书
tags:
  - dfs
abbrlink: b832
date: 2021-09-08 12:56:26
password:
---





#### 题目描述

 有 n 本书（编号为 1∼n）和 n 个人（编号为 1∼n），每个人都有一个自己喜爱的书的列表。现需要设计一种分书方案，使得每个人都能获得一本书，且这本书一定要在它的喜爱列表中。

------

#### 输入

 第一行一个正整数 n。（n≤20）

 接下来一个 n∗n的二维 01 矩阵，a[i,j]=1 说明第 i 人喜爱第 j 本书，=0 说明不喜欢。

#### 输出

 输出一个整数，表示分书方案总数。

------

#### 样例输入

```
5
00110
11001
01100
00010
01001
```

#### 样例输出

```
1
```

------

#### 数据规模与约定

 时间限制：1 s

 内存限制：256 M

 100% 的数据保证 n≤20





















#### 思路

* 按照题目的想法，本来想要用贪心思想写的；
* 按照升序解决某人喜爱的书的数量的该人的需求
* 由于本文章是dfs，故用dfs；
* 由题意可建立人与书的邻接矩阵v;
* 利用数组vis标记某本书是否被分配了；
* 不考虑每个人得到书的顺序，我按照从1号人到n号人分配书籍；





#### 代码如下





~~~c
#include <stdio.h>
#include <vector>

using namespace std;

vector <int> v[21]; //邻接矩阵 
int n, vis[21], ans; //vis标记书本, ans记录答案 

void dfs(const int& x) { //表示当前为第x号人分配符合条件的书本 
	if (x == n + 1) {
		++ans;
		return ;
	}
	for (int i = 0; i < (int)v[x].size(); i++) {
		if (vis[v[x][i]]) continue;
		vis[v[x][i]] = 1; //标记 
		dfs(x + 1);
		vis[v[x][i]] = 0; //回溯 
	}
}

int main() {
	scanf("%d", &n);
	
	//建立邻接矩阵 
	for (int i = 1; i <= n; i++) {
		getchar();
		for (int j = 1; j <= n; j++) {
			char op;
			scanf("%c", &op);
			if (op == '1') { //op == 1  说明第 i 人喜爱第 j 本书 
				v[i].push_back(j);
			} 
		}
	}
	
	
	for (int i = 0; i < (int)v[1].size(); i++) {
		vis[v[1][i]] = 1;
		dfs(2);
		vis[v[1][i]] = 0;
	} 
	printf("%d\n", ans);
	
	return 0;
}
~~~

