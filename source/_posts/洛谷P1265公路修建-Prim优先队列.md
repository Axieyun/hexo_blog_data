---
title: 洛谷P1265公路修建-Prim优先队列
abbrlink: '843'
date: 2021-10-02 09:04:53
tags:
      - Prim算法
      - 图论
password:
---



#### 朴素Prim

~~~c

//法二：朴素版

#include <stdio.h>
#include <queue>
#include <cmath>

using namespace std;

#define min(a, b) ({\
	(a) > (b) ? (b) : (a);\
})

inline int read() {
	int x = 0, w = 0;
	char ch = getchar();
	while (ch > '9' || ch < '0') {
		w |= ch == '-';
		ch = getchar();
	}
	while (ch >= '0' && ch <= '9') {
		x = (x << 3) + (x << 1) + (ch ^ 48);
		ch = getchar();
	}
	
	return w ? -x : x;
}

#define MAX_N 5001

struct point {
	int x, y;
};

struct Node {
	int to;
	double v;
	bool operator< (const Node &a) const {
		return this->v > a.v;
	}
};

point num[MAX_N];

int n;
int vis[MAX_N];
double dis[MAX_N]; // dis[i]记录联通i点的最短边 

double d(int a, int b) {
	return sqrt(pow(num[a].x - num[b].x, 2) + pow(num[a].y - num[b].y, 2));
}

#define INF 9e15

inline double Prim(int u) {
	double sum = 0;
	dis[u] = 0;
	for (int k = 1; k <= n; k++) {
		double mi = INF;
		int pos = 0;
		for (int i = 1; i <= n; i++) {
			if (!vis[i] && dis[i] < mi) {
				mi = dis[i];
				pos = i;
			}
		}
		vis[pos] = 1;
		sum += mi;
		for (int i = 1; i <= n; i++) {
			dis[i] = min(dis[i], d(pos, i));
		}
	}
	return sum;
}

int main() {
	n = read();
	for (int i = 1; i <= n; i++) {
		num[i].x = read(), num[i].y = read();
		dis[i] = INF;
	}
	double sum = Prim(1);
	printf("%.2lf\n", sum);
	return 0;
}
~~~



#### 优先队列Prim



~~~c
#include <stdio.h>
#include <queue>
#include <cmath>

using namespace std;

inline int read() {
	int x = 0, w = 0;
	char ch = getchar();
	while (ch > '9' || ch < '0') {
		w |= ch == '-';
		ch = getchar();
	}
	while (ch >= '0' && ch <= '9') {
		x = (x << 3) + (x << 1) + (ch ^ 48);
		ch = getchar();
	}
	
	return w ? -x : x;
}

#define MAX_N 5001

struct point {
	int x, y;
};

struct Node {
	int to;
	double v;
	bool operator< (const Node &a) const {
		return this->v > a.v;
	}
};

point num[MAX_N];
double dis[MAX_N][MAX_N];

int n;
int vis[MAX_N];
double s[MAX_N]; //s[i]记录联通i点的最短边 

inline double Prim(int u) {
	double sum = 0;
	int cnt = 0;
	priority_queue <Node> q;
	q.push((Node){u, 0});
	while (!q.empty() && cnt < n) {
		Node tp = q.top();
		q.pop();
		if (vis[tp.to]) continue;
		vis[tp.to] = 1;
		sum += tp.v;
		cnt++;
		for (int i = 1; i <= n; i++) {
			if (vis[i] || i == tp.to) continue;
			long long dx = num[i].x - num[tp.to].x;
			long long dy = num[i].y - num[tp.to].y;
			double ss = sqrt(dx * dx + dy * dy);
			if (s[i] <= ss) continue;
			s[i] = ss;
			q.push((Node){i, ss});

		}
	}
	return sum;
}

int main() {
	n = read();
	for (int i = 1; i <= n; i++) {
		num[i].x = read(), num[i].y = read();
		s[i] = 9999999999999999;
	}
	double sum = Prim(1);
	printf("%.2lf\n", sum);
	return 0;
}


~~~

