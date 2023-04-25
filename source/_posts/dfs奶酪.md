---
title: dfs奶酪
abbrlink: '7811'
date: 2021-09-08 13:11:44
tags:
password:
---

#### 题目描述

 现有一块大奶酪，它的高度为 h，它的长度和宽度我们可以认为是无限大的，奶酪中间有许多半径相同的球形空洞。我们可以在这块奶酪中建立空间坐标系，在坐标系中， 奶酪的下表面为 z=0，奶酪的上表面为 z=h。

 现在，奶酪的下表面有一只小老鼠 Jerry，它知道奶酪中所有空洞的球心所在的坐标。如果两个空洞相切或是相交，则 Jerry 可以从其中一个空洞跑到另一个空洞，特别地，如果一个空洞与下表面相切或是相交，Jerry 则可以从奶酪下表面跑进空洞；如果一个空洞与上表面相切或是相交，Jerry 则可以从空洞跑到奶酪上表面。

 位于奶酪下表面的 Jerry 想知道，在不破坏奶酪的情况下，能否利用已有的空洞跑到奶酪的上表面去?



------

#### 输入

 每个输入包含多组数据。

 数据的第一行，包含一个正整数 T，代表该输入文件中所含的数据组数。

 接下来是 T 组数据，每组数据的格式如下：

 第一行包含三个正整数n,h 和 r，两个数之间以一个空格分开，分别代表奶酪中空洞的数量，奶酪的高度和空洞的半径。

 接下来的 n 行，每行包含三个整数 x,y,z，两个数之间以一个空格分开，表示空洞球心坐标为(x,y,z)。

#### 输出

 输出共 T 行，分别对应 T 组数据的答案，如果在第 i 组数据中，Jerry 能从下 表面跑到上表面，则输出Yes，如果不能，则输出 No。

------

#### 样例输入

```
3 
2 4 1 
0 0 1 
0 0 3 
2 5 1 
0 0 1 
0 0 4 
2 5 2 
0 0 2 
2 0 4
```

#### 样例输出

```
Yes
No
Yes
```

#### 样例说明

 第一组数据，两个空洞分别和上(0,0,0) 下 (0,0,4) 表面相切，两个空洞相切(0,0,2)，输出Yes。

 第二组数据，两个空洞既不相切也不相交，输出 No。

 第三组数据，两个空洞相交，且分别于上下表面相切或相交，输出Yes。

------

#### 数据规模与约定

 时间限制：1 s

 内存限制：256 M

 100% 的数据保证 1≤n≤1000 , 1≤h,r≤109 , T≤201≤n≤1000 , 1≤h,r≤109 , T≤20 坐标的绝对值不超过 109





#### 思路

* 老鼠入口很多，我们可以直接遍历得到，也可以存储入口位置，本文采取存储入口
* 为了减少过多的搜索，本文提前处理每个空洞，利用邻接矩阵存储联通的空洞；









#### 代码如下



~~~~
#include <stdio.h>
#include <iostream>
#include <cmath>
#include <vector>
#include <cstring>

using namespace std;
struct Node {
	int x, y, z;
};

int x, y, z, n, h, r, t, vis[1005];
double dist(const Node& a, const Node& b) { // 计算距离
	return sqrt(pow(a.x - b.x, 2) + pow(a.y - b.y, 2) + pow(a.z - b.z, 2));
}

vector <int> merge[1005], start; //merge为邻接矩阵，start存储入口位置
Node num[1005];  // 全部空洞，下标表示其空洞位置

bool dfs(const int& ind) {
	if (num[ind].z + r >= h) return true;
	for (int i = 0; i < (int)merge[ind].size(); i++) {
		if (vis[merge[ind][i]]) continue;
		vis[merge[ind][i]] = 1;
		if (dfs(merge[ind][i])) return true; // 剪枝
	}
	return false;
}
int main() {
	cin >> t;
	while (t--) {
		cin >> n >> h >> r;
		for (int i = 1; i <= n; i++) {
			cin >> x >> y >> z;
			if (z <= r) {
				start.push_back(i);
			}
			for (int j = 1; j < i; j++) { //邻接矩阵建立
				double dis = dist(num[j], (Node){x, y, z});
				if (dis  > r + r) continue;
				merge[i].push_back(j);
				merge[j].push_back(i);
			}
			num[i] = (Node){x, y, z};
		}
		bool boo = false;
		for (int i = 0; i < (int)start.size(); i++) {
			if (vis[start[i]]) continue;
			vis[start[i]] = 1;
			boo = dfs(start[i]);
			if (boo) break;
			
		}
		if (boo) cout << "Yes" << endl;
		else cout << "No" << endl;
		start.clear();
		for (int i = 0; i < 1005; i++) merge[i].clear();
		memset(num, 0, sizeof(num));
		memset(vis, 0, sizeof(vis));
	}
	return 0;
}
~~~~

