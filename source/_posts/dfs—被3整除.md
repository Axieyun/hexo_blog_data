---
title: dfs—被3整除
tags:
  - dfs
abbrlink: fa86
date: 2021-09-18 18:51:47
password:
---





#### 题目



题目描述

给定 n 个数字，要在其中选出 k 个数，组成一个整数，现输出所有组合当中，能被 3 整除的整数。

输入

第一行输入两个整数 n，k，表示待选数字个数和需要选出的数字个数 （n <= 15，k <= 6）。
接下来一行输入 n 个一位数。

输出

输出共两行，第一行为组合后能被 3 整除的数字个数，保证至少有一个。
第二行为上述数字，按照数字从小到大的顺序输出，每两个数之间用空格隔开。

题目限制

**时间限制：**C/C++语言 1000MS；其他语言 3000 MS
**内存限制：**C/C++语言 65536KB；其他语言 589824KB

样例输入

```
5 2
4 4 2 3 3
```

样例输出

```
3
24 33 42
```







#### 思路



* 排列组合问题
* 简单暴力dfs
* 利用递归，层次遍历，枚举答案
* 每递归一层，判断一下是否符合条件





#### 代码演示





~~~c
#include <stdio.h>
#include <set>
#include <algorithm>

using namespace std;

int n, k, num[16], cnt, sum = 0, ans[100000];

int vis[16];
int s[10000000];
void dfs(int h) {
	if (h > k + 1) return ;
	if (h == k + 1 && sum % 3 == 0 && s[sum] == 0) {
		ans[cnt++] = sum;
		s[sum] = 1;
		
	}
	for (int i = 1; i <= n; i++) {
		if (vis[i]) continue;
		sum = sum * 10 + num[i];
		vis[i] = 1;
		dfs(h + 1);
		vis[i] = 0;
		sum /= 10;
	}
}

int main() {
	scanf("%d %d", &n, &k);
	for (int i = 1; i <= n; i++) scanf("%d", &num[i]);
	dfs(1);
	sort(ans, ans + cnt);
	printf("%d\n", cnt);
	for (int i = 0; i < cnt; i++) {
		i && putchar(' ');
		printf("%d", ans[i]);
	}
	return 0;
}
~~~

