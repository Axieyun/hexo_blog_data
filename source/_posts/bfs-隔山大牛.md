---
title: bfs-隔山大牛
tags:
  - bfs
abbrlink: de1b
date: 2021-09-19 00:34:36
password:
---







#### 题目



题目描述

相信大家都会下中国象棋：在中国象棋中，有一种棋子“炮”，它的行走规则较为特殊，在没有障碍的情况下可以向四个方向走任意距离，特殊走法为：若和其他某个棋子 y 之间的直线上，有且只有另一棋子 z 时，则可以越过 z 直接跳到 y 处。现在给定 n 行 m 列的棋盘，在棋盘上有一枚“炮”棋子（用 x 表示），及若干其他棋子（用 o 表示），其他棋盘的空白处用 . 填充。现给定一终点，按照象棋规则，“炮”棋子走到终点至少需要多少次移动。注意：炮可以按照象棋中的规则“隔山打牛”，但在“隔山打牛”时，落点处之前必须有棋子，在落点后，该位置的原棋子暂时消失，当“炮”重新移走后，该点原棋子会恢复。

输入

输入第一行两个整数 n,m，表示棋盘的大小。（1 <= n,m <= 2000）

接下来 n 行，每行输入 m 个字符，表示棋盘上的信息。（字符只可能为 x、o、. 三种之一，x 只有一个）

再接下来一行输入两个整数 x1, y1，为终点坐标（棋盘左上角为 1 1 点），坐标一定在棋盘范围之内，终点不保证一定为空（可能需要绕一圈通过“隔山打牛”才能到达该点）。



输出

输出共一个数字，为“炮”棋子走到终点的最少移动次数；若无法走到终点，则输出该“炮”棋子最多能走到多少个点（本身也算一个点）。

题目限制

**时间限制：**C/C++语言 2000MS；其他语言 4000 MS
**内存限制：**C/C++语言 262144KB；其他语言 786432KB

样例输入

```
5 6
x.o..o
......
.....o
....oo
...oo.
5 6
```

样例输出

```
3
```





#### 代码演示





~~~c
#include <stdio.h>
using namespace std;

struct Node {
	int x, y, setp;
};
Node q[100000];
char mmap[2002][2002], vis[2002][2002];
int dis[4][2] = {1, 0, 0, 1, 0, -1, -1, 0}, cnt;
int main() {
	int n, m, x1, y1;
	scanf("%d %d", &n, &m);
	 
	for (int i = 1; i <= n; i++) {
		getchar(); //刷新缓冲区 
		for (int j = 1; j <= m; j++) {
			scanf("%c", &mmap[i][j]);
			if (mmap[i][j] == 'x') {
				q[0] = (Node){i, j, 0};
				vis[i][j] = '1';
				cnt++;
			}
		}
	}
	scanf("%d %d", &x1, &y1); //终点坐标 
	int l = 0, r = 1;
	while (l < r) {
		Node tp = q[l++];
		if (tp.x == x1 && tp.y == y1) {
			printf("%d\n", tp.setp);
			return 0;
		}
		for (int i = 0; i < 4; i++) {
			for (int j = 1; j; j++) {
				int dx = tp.x + j * dis[i][0];
				int dy = tp.y + j * dis[i][1];
				if (mmap[dx][dy] == 'o') { //遇到障碍物 
					do {
						dx += dis[i][0];
						dy += dis[i][1];
					} while (dx > 0 && dy > 0 && dx < n && dy < m && mmap[dx][dy] != 'o'); //寻找下一个障碍物 
					if (mmap[dx][dy] == 'o' && vis[dx][dy] != '1') { //判断退出循环的点是否为障碍物 
						vis[dx][dy] = '1';
						q[r++] = (Node){dx, dy, tp.setp + 1};
						cnt++;
					}
					break; // 不管有没有障碍物，都得结束 
				}
				if (dx < 1 || dy < 1 || dx > n || dy > m) break; //出界了 
				if (mmap[dx][dy] == '.' && vis[dx][dy] != '1') { //没有遇到障碍物之前，正常每个点都走 
					cnt++;
					vis[dx][dy] = '1';
					q[r++] = (Node){dx, dy, tp.setp + 1};
				}
			}
		}
	}
	printf("%d\n", cnt); //输出走过的点的数量 
	return 0; 
}
~~~

