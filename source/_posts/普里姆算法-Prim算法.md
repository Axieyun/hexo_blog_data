---
title: Prim算法
abbrlink: b483
date: 2021-09-27 12:38:55
tags:
      - 最小生成树算法
      - 树
password:
---



### 普里姆算法



**以点为主**





#### 算法流程

* 全集点集为u，最小生成树点集为v

* 随机选取起点，起点入加集合v
* （1）由集合v中选取所有的点向外扩散，找到最小边权连接的点，且该点属于（u - v），把该点加入集合v
* 重复步骤（1），直到集合v == 集合u





#### 优点



* 居然**Kruskal**适用于稀疏图，那么这个算法适用于稠密图







#### 一般代码



* 时间复杂度O（n^2 - n^3）



~~~c
#include <stdio.h>

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

#define MAX_N 1000
#define MAX_M 2000

#define min(a, b) ({\
	(a) > (b) ? (b) : (a);\
})

//链式前向星
int cnt_edge, head[MAX_N], next[MAX_M], to[MAX_M], w[MAX_M];
// 加边 
inline void add_edge(int a, int b, int c) { 
	to[++cnt_edge] = b;
	w[cnt_edge] = c;
	next[cnt_edge] = head[a];
	head[a] = cnt_edge;
	return ;
}
int n, m;       //输入点的数量、边的数量 
int vis[MAX_N]; //标记进入集合v的元素
int ans[MAX_N]; //存储集合v的元素 
int sum;        //最小生成树的边权和 
int cnt;        //记录加入集合v的元素数量 
inline void Prim(int u) { //u为起始点 
	vis[u] = 1;
	ans[cnt++] = u;
	while (cnt < n) {
		int flag = 0; //判断是否找到最小边 
		int point = 0, mi = 0x7fffffff;
		for (int i = 0; i < cnt; i++) {
			for (int j = head[ans[i]]; j; j = next[j]) {
				if (vis[to[j]]) continue;     // 在集合v中
				if (mi <= w[j]) continue;
				mi = w[j];
				point = to[j];
				flag = 1;
			}
		}
		
		if (flag) {
			vis[point] = 1;
			ans[cnt++] = point;
			sum += mi;
		} else break;
		
	} 
	
	if (cnt == n) {
		printf("%d\n", sum);
		
	} else printf("-1\n");
	
	for (int i = 0; i < cnt; i++) {
		if (i) putchar(' ');
		printf("%d", ans[i]);
	}
 	return ;
} 

int main() {
	n = read(), m = read();
	for (int i = 1; i <= m; i++) {
		int a, b, c;
		a = read(), b = read(), c = read();
		add_edge(a, b, c);
		add_edge(b, a, c);
	} 
	Prim(1); 
	return 0;
}

~~~







#### 朴素代码



~~~c
#define INF 9e8   
#define Type int
#define MAX_N 100000

#define min(a, b) ({\
	(a) > (b) ? (b) : (a);\
})

Type dis[MAX_N];
bool vis[MAX_N];
int n;
int head[MAX_N], to[MAX_N << 1], v[MAX_N << 1], nex[MAX_N << 1], cnt_edge;

inline Type Prim(int u) {
	for (int i = 1; i <= n; i++) dis[i] = INF; //对距离初始化为无穷
	Type sum = 0;
	dis[u] = 0;
	for (int t = 1; t <= n; t++) {
		Type min_s = INF; //寻找最小值
		int pos = 0;      //最小值得坐标
		for (int i = 1; i <= n; i++) {
			if (!vis[i] && dis[i] < min_s) {
				min_s = dis[i];
				pos = i;
			}
		}
		vis[pos] = true; //标记这个点被访问
        sum += min_s; //将找到的最小边加入答案里
		for (int i = head[pos]; i; i = nex[i]) { //循环更新距离数组
			dis[to[i]] = min(dis[to[i]], dis[pos] + v[i]);
		}
	}
	return sum;
}
~~~





#### 优先队列实现代码





~~~c
#include <queue>
#include <stdio.h>

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

struct Node {  // 优先队列的节点 
	int to, v; // 存储下一个节点与这条边权值 
	bool operator< (const Node &a) const {
		return this->v > a.v;
	}
};

#define MAX_M 200000
#define MAX_N 100000

int head[MAX_N], to[MAX_M], nt[MAX_M], v[MAX_M], cnt_edge;

inline void add_edge(int a, int b, int c) {
	to[++cnt_edge] = b;
	v[cnt_edge] = c;
	nt[cnt_edge] = head[a];
	head[a] = cnt_edge;
	return ;
}

int n, m; 
int vis[MAX_N]; // 标记集合v的元素 

inline int Prim(int u) {
	int sum = 0;
	int cnt = 0;
	priority_queue <Node> q;
	q.push((Node){u, 0});
	while (!q.empty()) {
		Node tp = q.top(); // 每次从队列取到边权最小的边，不用和朴素代码一样，遍历每条连接的边 
		q.pop();
		if (vis[tp.to]) continue; //下一个点在集合v，跳过 
		cnt++;
		sum += tp.v; 
		vis[tp.to] = 1; // 标记集合v的元素 
		for (int i = head[tp.to]; i; i = nt[i]) {
			if (vis[to[i]]) continue;
			q.push((Node){to[i], v[i]});
		}
	}
	printf("%d\n", sum);
	return (cnt == n ? sum : -1);
} 

int main() {
	n = read(), m = read();
	for (int i = 1; i <= m; i++) {
		int a, b, c;
		a = read(), b = read(), c = read();
		add_edge(a, b, c);
		add_edge(b, a, c);
	}
	int ans = Prim(1);
	printf("%d\n", ans);
	return 0;
}
~~~

#### 习题



* 洛谷P1265 公路修建：[https://www.luogu.com.cn/problem/P1265]()
* 题解：[[https://Axieyun.top/posts/843.html](https://axieyun.top/posts/843.html)]()
* 
