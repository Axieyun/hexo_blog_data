---
title: 体积dfs
tags:
  - dfs
abbrlink: 60ba
date: 2021-08-23 15:24:42
password:
---





#### 题目描述

 给出 n 件物品，每个物品有一个体积 Vi，从中取出若干件物品能够组成的不同的体积和有多少种可能。例如，n=3 , Vi={1,3,4}，那么输出 6，6 种不同的体积和为 1,3,4,5,7,8。

------

#### 输入

 第一行一个正整数 n。（n≤20）

 第二行 n 个整数，表示 Vi。（1≤Vi≤50）

#### 输出

 输出一个整数，表示不同体积的组合数。

------

#### 样例输入

```
3
1 3 4
```

#### 样例输出

```
6
```

------

#### 数据规模与约定

 时间限制：1 s

 内存限制：256 M

 100% 的数据保证 n≤20





#### 代码演示



**60分**



~~~c
#include <stdio.h>

int n, volume[21], vis[1001], ans, vis1[31];

void dfs() { //60分 
	for (int i = 1; i <= n; i++) {
		if (vis1[i] == 1) continue;
		ans += volume[i];
		vis1[i] = 1;
		vis[ans] = 1;
		dfs();
		ans -= volume[i];
		vis1[i] = 0;
	}
}

int main() {
	
	scanf("%d", &n);
	
	for (int i = 1; i <= n; i++) {
		scanf("%d", &volume[i]);
		vis[volume[i]] = 1;
	}
	dfs();
	int cnt = 0;
	for (int i = 1; i <= 1000; i++) {
		if (vis[i] == 1) {
			++cnt;
		//	printf("%d ", i);
		}
	}
	printf("%d\n", cnt);
	return 0;
}
~~~



**80分**





~~~c
#include <stdio.h>
int n, volume[21], vis[1001], ans, vis1[31];
void dfs(int h) { 
	for (int i = h; i <= n; i++) {
		if (vis1[i] == 1) continue;
		ans += volume[i];
		vis1[i] = 1;
		vis[ans] = 1;
		dfs(h + 1);
		ans -= volume[i];
		vis1[i] = 0;
	}
}
int main() {
	scanf("%d", &n);
	for (int i = 1; i <= n; i++) {
		scanf("%d", &volume[i]);
		vis[volume[i]] = 1;
	}
	dfs(1);
	int cnt = 0;
	for (int i = 1; i <= 1000; i++) {
		if (vis[i] == 1) {
			++cnt;
		//	printf("%d ", i);
		}
	}
	printf("%d\n", cnt);
	return 0;
}
~~~



**100分**

~~~c
#include <stdio.h> // 100分 
int n, volume[21], vis[1001] = {1}, cnt;
void dfs(int h, int s) {
	(vis[s] == 0) && (vis[s] = 1) && (++cnt);
	for (int i = h; i <= n; i++) {
		dfs(i + 1, s + volume[i]);
	}
}
int main() {
	scanf("%d", &n);
	for (int i = 1; i <= n; i++) {
		scanf("%d", &volume[i]);
		//vis[volume[i]] = 1;
	}
	dfs(1, 0);
//	for (int i = 1; i <= 1000; i++) {
//		if (vis[i] == 1) {
//		//	printf("%d ", i);
//			cnt++;
//		}
//	}
	//putchar('\n');
	printf("%d\n", cnt);
	return 0;
}
~~~

