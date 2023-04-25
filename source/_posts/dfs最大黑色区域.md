---
title: dfs最大黑色区域
abbrlink: 7f73
date: 2021-09-08 13:05:26
tags:
password:
---

#### 题目描述

 现在给出一个 n∗m 的二维矩阵，矩阵上的每个点只可能是 0 （代表白色）或 1 （代表黑色）。现规定某一点的颜色与它的上下左右某点的颜色相同，则它们为同一区域，现求最大黑色区域的大小。

------

#### 输入

 第一行两个正整数 n,m。（1≤n,m≤100）

 接下来输入一个二维字符矩阵，每个字符为 0 或 1。

#### 输出

 输出一个整数，表示可以最大黑色区域面积。

------

#### 样例输入

```
5 6
011001
110101
010010
000111
101110
```

#### 样例输出

```
7
```

------

#### 数据规模与约定

 时间限制：1 s

 内存限制：256 M

 100% 的数据保证 1≤n,m≤100

















#### 思路

* 求最大面积，本题有两种做法，分别是dfs与bfs
* 本文演示dfs
* 该题无非就是联通性问题，求最大连通块；







#### 代码演示





~~~c
#include <stdio.h>
int n, m, vis[101][101]; //标志 
char mmap[101][101]; //地图 
int dis[4][2] = {1, 0, -1, 0, 0, 1, 0, -1}; //方向 
int dfs(const int x, const int y) { 
	int ans = 1;
	for (int i = 0; i < 4; i++) {
		int dx = x + dis[i][0];
		int dy = y + dis[i][1];
		if (mmap[dx][dy] != '1') continue;  
		if (vis[dx][dy]) continue;  
		vis[dx][dy] = 1;
		ans += dfs(dx, dy);
	}
	return ans;
}

int main() {
	scanf("%d %d", &n, &m);
	for (int i = 1; i <= n; i++) {
		getchar();
		for (int j = 1; j <= m; j++) {
			scanf("%c", &mmap[i][j]);
		}
	}
	int mmax = -1;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= m; j++) {
			if (mmap[i][j] != '1') continue;
			if (vis[i][j]) continue;
			vis[i][j] = 1;
			int tp = dfs(i, j);
			mmax = mmax > tp ? mmax : tp;
		}
	}
	printf("%d\n", mmax);
	return 0;
}




~~~





~~~~c

#include <stdio.h>

int n, m, vis[101][101]; //标志 
char mmap[101][101]; //地图 
int dis[4][2] = {1, 0, -1, 0, 0, 1, 0, -1}; //方向 
int dfs(int *xx, int *yy) {
	int ans = 1, x = *xx, y = *yy;
	for (int i = 0; i < 4; i++) {
		int dx = x + dis[i][0];
		int dy = y + dis[i][1];
		if (mmap[dx][dy] != '1') continue;
		if (vis[dx][dy]) continue;
		vis[dx][dy] = 1;
		ans += dfs(&dx, &dy);
	}
	return ans;
}

int main() {
	scanf("%d %d", &n, &m);
	for (int i = 1; i <= n; i++) {
		getchar();
		for (int j = 1; j <= m; j++) {
			scanf("%c", &mmap[i][j]);
		}
	}
	int mmax = -1;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= m; j++) {
			if (mmap[i][j] != '1') continue;
			if (vis[i][j]) continue;
			vis[i][j] = 1;
			int tp = dfs(&i, &j);
			mmax = mmax > tp ? mmax : tp;
		}
	}
	printf("%d\n", mmax);
	return 0;
}
~~~~





~~~c
#include <stdio.h>

int n, m, vis[101][101]; //标志 
char mmap[101][101]; //地图 
int dis[4][2] = {1, 0, -1, 0, 0, 1, 0, -1}; //方向 
int dfs(const int& x, const int& y) { 
	int ans = 1;
	for (int i = 0; i < 4; i++) {
		int dx = x + dis[i][0];
		int dy = y + dis[i][1];
		if (mmap[dx][dy] != '1') continue;
		if (vis[dx][dy]) continue;
		vis[dx][dy] = 1;
		ans += dfs(dx, dy);
	}
	return ans;
}

int main() {
	scanf("%d %d", &n, &m);
	for (int i = 1; i <= n; i++) {
		getchar();
		for (int j = 1; j <= m; j++) {
			scanf("%c", &mmap[i][j]);
		}
	}
	int mmax = -1;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= m; j++) {
			if (mmap[i][j] != '1') continue;
			if (vis[i][j]) continue;
			vis[i][j] = 1;
			int tp = dfs(i, j);
			mmax = mmax > tp ? mmax : tp;
		}
	}
	printf("%d\n", mmax);
	return 0;
}

~~~



