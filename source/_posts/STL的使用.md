---
title: STL的使用
abbrlink: '9331'
date: 2021-04-05 00:39:27
tags: 算法入门与进阶

---


# **STL的使用**

****

## 顺序式容器 ：vector, list, deque, priority_queue, stack;

### **一、vector**

##### **头文件：#include < vector >**

##### **优点 ：**

1、是动态数组, 是模板类也称向量类, 可以存放任何类型的对象；

2、从末尾快速插入与删除元素，可直接访问任何元素，支持[]重载；

##### **缺点 ：**

1、vector 插入与删除中间某一元素需要线性时间，时间复杂度为O(n), 若频繁移动元素，效率很低；

*****

##### **定义 ：**

功能|例子|说明
:---:|:--:|:---:
定义int型数组|vector < int > a;|默认初始化，a为空
定义int型数组|vector < int > b(a);| 用a定义b
定义int型数组|vector < int > a(100);| 初始化a有100个值为0的元素
定义int型数组|vector < int > a(100, 6);|初始化a有100个值为6的元素
定义int型数组|vector < int > a[n]; | 定义二维容器a, 含有n个一维容器
定义int型数组|vector < vector <int> > a;|定义二维容器a, 含有n个一维容器
定义string型数组| vector < string > ch(10, "hello");| 初始化10个值为hello的元素
定义string型数组|vector < string > ch(10, "null"); | 初始化10个值为null的元素
定义string型数组|vector < string > ch(a.begin(), a.end());| ch是a的复制
定义结构体型数组|struct point {int x, y;}; vector < point > node;| node存放坐标

*****

##### **常用操作：**

以下操作均以vector < int > a 来使用

功能|例子|说明
:---:|:--:|:---:
赋值|a.push_back(100);|在a的尾部压入100
提取a的第一个元素| int t = a.front();|返回a的第一个元素值
元素个数|int size = a.size();| 求a的元素个数
判断是否为空| bool flag = a.empty();|a为空返回值为true, 反之为false
打印|cout << a[0] << endl; | 打印a的第一个元素
中间插入|a.insert(a.begin() + i, k);|在第i个元素前插入k
中间插入|a.insert(a.begin() + 1, 3, 5); | 在a的第2个元素的位置插入3个5
尾部插入|a.insert(a.end(), 10, 5)； | 尾部插入10个值为5的元素
删除尾部|a.pop_back();|删除尾部元素
删除区间|a.erase(a.brgin() + i, a.begin() + j);|删除区间[i, j - 1] 的元素
删除元素|a.erase(a.begin() + 2); | 删除第3个元素
调整大小|a.resize(n);|调整容器大小为n
清空|a.clear();|清空容器a
翻转|reverse(a.begin(), a.end());|用函数reverse()翻转容器a
提取a的尾部元素|int t = a.back();|返回a的最后一个元素值

*****

##### **迭代器 iterator**

```c

vector < int > iterator it = a.begin();
while (it != a.end()) {
	cout << *it << endl;
    ++it;
    
}


```

**习题** hdu4841 [http://acm.hdu.edu.cn/showproblem.php?pid=4841](http://)

```c
#include <iostream>
#include <vector>

using namespace std;

int main() {
	vector < int > table;
	int n, m;
	while (cin >> n >> m) {
		table.clear();//清空向量table;
		for (int i = 0; i < 2 * n; i++) table.push_back(i); //初始化table;
		int pos = 0;
		for (int i = 0; i < n; i++) {
			pos = (pos + m - 1) % table.size();
			table.erase(table.begin() + pos);
		}
		int j = 0;
		for (int i = 0; i < 2 * n; i++) {
			if (!(i % 50) && i) cout << endl;
			if (j < (int)table.size() && !(i ^ table[j])) {
				j++;
				cout << "G";
			} else cout << "B";
		}
		cout << endl << endl;
	}
	return 0;
}


```
*****
### **二、栈stack**

##### **头文件 #include < stack >**

##### **基本操作**
****

stack < Type > s; //定义栈，Type为数据类型，形如 int, float, cahr等

函数|说明
:---:|:--:|
s.push(item);| 把 item 放到栈顶
s.top();| 返回栈顶的元素，但不删除
s.pop()|删除栈顶元素
s.size()| 返回栈中元素的个数
s.empty()| 检查栈是否为空，空则返回true,非空返回fales

*****

习题1062 [http://acm.hdu.edu.cn/showproblem.php?pid=1062](http://)

```c

#include <bits/stdc++.h>

using namespace std;

int main() {
	int n;
	char ch;
	scanf("%d", &n);
	getchar(); //刷新缓冲区
	while (n--) {
		stack < char > s;
		while (true) {
			ch = getchar();
			if (ch == ' ' || ch == '\n' || ch == EOF) {
				while (!s.empty()) {
					printf("%c", s.top());
					s.pop();
				}
				if (ch == EOF || ch == '\n') break;
				printf(" ");
			} else s.push(ch);
		}
		cout << endl;
	}
	return 0;
}

```
*****
### **三、队列queue**

##### **头文件 #include < queue >**

##### **基本操作**

*****

queue < Type > q;

函数|说明
:---:|:--:|
q.push(item);| 把item 压入队列
q.front();| 返回队列首元素，但不删除
q.empty(); | 检查队列是否为空，空则返回true,非空返回fales
q.pop(); | 删除队首元素
q.back(); | 返回队尾元素
q.size();| 返回队列元素个数 

*****

习题1702 [http://acm.hdu.edu.cn/showproblem.php?pid=1702](http://)
```c

#include <iostream>
#include <queue>
#include <stack>

using namespace std;

int main() {
	int t, n, temp;
	cin >> t;
	while (t--) {
		string str, str1;
		queue < int > q;
		stack < int > s;
		cin >> n >> str;
		for (int i = 0; i < n; i++) {
			if (str == "FIFO") {
				cin >> str1;
				if (str1 == "IN") {
					cin >> temp;
					q.push(temp);
				}
				if (str1 == "OUT") {
					if (q.empty()) cout << "None" << endl;
					else {
						cout << q.front() << endl;
						q.pop();
					}
				}
			} else {
				cin >> str1;
				if (str1 == "IN") {
					cin >> temp;
					s.push(temp);
				}
				if (str1 == "OUT") {
					if (s.empty()) {
						cout << "None" << endl;
					} else {
						cout << s.top() << endl;
						s.pop();
					}
				}
			}
		}
	}
	return 0;
}

```
*****
### **四、优先队列priority_queue**

##### **头文件 #include < queue >**

##### my理解：
在 STL 中 优先队列用二叉堆实现，在队列中push一个数或pop一个数时间复杂度都是O(logn);
priority_queue默认优先级最高在前面

##### 定义:
**原型**

priority_queue < Type, Container, Functional >
Type : 数据类型
Container : 容器类型，不能用list
Functional : 比较方式

##### **基本操作**

****

greater和less是std实现的两个仿函数（就是使一个类的使用看上去像一个函数。其实现就是类中实现一个operator()，这个类就有了类似函数的行为，就是一个仿函数类了）, 头文件为 #include < functional > ;

priority_queue < int, vector < int >, less < int > > q; // 大顶堆
priority_queue < int, vector < int >, greater < int > > q; // 小顶堆


以下默认大顶堆

函数|说明
:---:|:--:|
q.pop(); | 删除最高优先级元素
q.top(); | 返回最高优先级元素，但不删除该元素
q.push(item);| 插入新元素
q.size(); | 返回队列元素个数
q.empty() | 检查队列是否为空，空则返回true,非空返回fales


*****

习题hdu1873 [http://acm.hdu.edu.cn/showproblem.php?pid=1873](http://)

```c

//网上抄的 // hdu提交50ms
#include <cstdio>
#include <queue>
#include <cstring>
using namespace std;
/*
   优先队列： q.top() --> 在队列中查找优先权最大的元素 有限队列插入和删除都是 O（lgn)
   优先级最高的在最前面
   */

struct Node    //此题的坑在于————————> 排序的稳定性，如果两个数一样，不能交换
{
        int priority;
        int index;
        bool operator < (const Node &b) const
        {
                if ( priority != b.priority)//不等于就倒退 
                        return priority < b.priority;
                return index > b.index;
        }
};
 
 
int main()
{
        int n;
        while( ~scanf("%d", &n) )   
        {
                priority_queue < Node > que[4];//系统默认元素值大的优先级高
                int ccccc = 1;
                for(int i = 0; i < n; ++i)
                {
                        char str[10];
                        scanf("%s", str);
 
                        if( !strcmp(str, "IN"))
                        {
                                Node temp;  
                                temp.index = ccccc++;//记录这个病人的索引  
 
                                int doctor_index;//病人要看的医生的索引  
                                int priority;//这个病人的优先级  
                                scanf("%d%d",&doctor_index,&priority);  
                                temp.priority = priority;  
 
                                que[doctor_index].push(temp); 
                        }
 
                        else if( !strcmp(str, "OUT"))
                        {
                                int doctor_index;
                                scanf("%d", &doctor_index);
                                if( !que[doctor_index].empty())
                                {
                                        printf("%d\n", que[doctor_index].top().index);
                                        que[doctor_index].pop();
                                }
                                else
                                        printf("EMPTY\n");
                        }
                }
        }
        return  0;
}


//自己写的暂时不过，未能处理好级别相同先输出先到的病人
#include <iostream>
#include <queue>
#include <cstdio>
#include <cstring>
using namespace std;
int main() {
	int n, a, b;
	string str;
	while (scanf("%d", &n) != EOF) {
		priority_queue < pair < int, int > > a1; //一号医生
		priority_queue < pair < int, int > > a2; //二号医生
		priority_queue < pair < int, int > > a3; //三号医生
		int t = 1; // 记录第几个病人
		while (n--) {
			cin >> str;
			if (str == "IN") {
				scanf("%d %d", &a, &b);
				pair <int, int> ab(b, t);
				switch (a) {
					case 1 :
						a1.push(ab);
						break;
					case 2 :
						a2.push(ab);
						break;
					case 3 :
						a3.push(ab);
						break;
				}
				++t;
			} else if (str == "OUT") {
				scanf("%d", &a);
				switch (a) {
					case 1 :
						if (a1.empty()) printf("EMPTY\n");
						else {
							printf("%d\n", a1.top().second);
							a1.pop();
						}
						break;
					case 2 :
						if (a2.empty()) printf("EMPTY\n");
						else {
							printf("%d\n", a2.top().second);
							a2.pop();
						}
						break;
					case 3 :
						if (a3.empty()) printf("EMPTY\n");
						else {
							printf("%d\n", a3.top().second);
							a3.pop();
						}
						break;
				}
			}
		}
	}
	return 0;
}


```


历时一晚上终于搞清楚了，咨询了大佬，需要重新写个仿函数my_greater,下面来看代码；

```c

// hdu 提交 110ms,但还是过了
#include <iostream>
#include <queue>
#include <cstdio>
#include <cstring>

using namespace std;

class my_less { //重载的大顶堆仿函数；
public:
	bool operator ( ) (const pair < int, int > & a, const pair < int, int > & b) const
	{
		if(a.first == b.first) return a.second > b.second;
		else return a.first < b.first;
	}
};
int main() {
	int n, a, b;
	string str;
	while (scanf("%d", &n) != EOF) {
		priority_queue < pair < int, int >, vector < pair <int, int > >, my_less> a1; // 一号医生
		priority_queue < pair < int, int >, vector < pair <int, int > >, my_less> a2;
		priority_queue < pair < int, int >, vector < pair <int, int > >, my_less> a3;
		int t = 1; // 病人的1病号
		while (n--) {
			cin >> str;
			if (str == "IN") {
				scanf("%d %d", &a, &b);
				pair <int, int> ab(b, t);
				switch (a) {
					case 1 :
						a1.push(ab);
						break;
					case 2 :
						a2.push(ab);
						break;
					case 3 :
						a3.push(ab);
						break;
				}
				++t;
			} else if (str == "OUT") {
				scanf("%d", &a);
				switch (a) {
					case 1 :
						if (a1.empty()) printf("EMPTY\n");
						else {
							printf("%d\n", a1.top().second);
							a1.pop();
						}
						break;
					case 2 :
						if (a2.empty()) printf("EMPTY\n");
						else {
							printf("%d\n", a2.top().second);
							a2.pop();
						}
						break;
					case 3 :
						if (a3.empty()) printf("EMPTY\n");
						else {
							printf("%d\n", a3.top().second);
							a3.pop();
						}
						break;
				}
			}
		}
	}
	return 0;
}

```

偷偷的把代码缩短，但是运行时间还是没多大变化；
以后还是少用pair吧，改用结构体还好点；

````c

#include <iostream>
#include <queue>
#include <cstdio>
#include <cstring>
using namespace std;
class my_less {
public:
	bool operator ( ) (const pair < int, int > & a, const pair < int, int > & b) const
	{
		if(a.first == b.first) return a.second > b.second;
		else return a.first < b.first;
	}
};
int main() {
	int n, a, b;
	string str;
	while (scanf("%d", &n) != EOF) {
		priority_queue < pair < int, int >, vector < pair < int, int > >, my_less > dector[4]; // 医生序号 
		int t = 1; //病人的病号 
		while (n--) {
			cin >> str;
			if (str == "IN") {
				scanf("%d %d", &a, &b);
				pair <int, int> ab(b, t);
				dector[a].push(ab);
				++t;
			} else if (str == "OUT") {
				scanf("%d", &a);
				if (dector[a].empty()) printf("EMPTY\n");
				else {
					printf("%d\n", dector[a].top().second);
					dector[a].pop();
				}
			}c
		}
	}
	return 0;
}

````

**总结**
题目不难，但有个难点，需要重载<运算符；或者直接重新写过greator仿函数；

```c
// 重载pair，其实pair是结构体实现的；而不是class;

typedeof pair < int, int > pll;

// 实现 first相等时，second小的优先级高
class my_greater {
public:
	bool operator ( ) (const pll &a, const pll &b) const {
    return a.first == b.first ? a.second < b.second : a.first > b.first;
    }
};

//其实greater 本来是这样的原理

class my_greater {
public:
// 实现 first相等时，second大的优先级高
	bool operator ( ) (const pll &a, const pll &b) const {
    return a.first == b.first ? a.second > b.second : a.first > b.first;
    }
};

//同理  less也可以实现 first相等时，second小的优先级高
class my_less {
public:
// 实现 first相等时，second 小的优先级高
	bool operator ( ) (const pll &a, const pll &b) const {
    return a.first == b.first ? a.second > b.second : a.first < b.first;
    }
};


当然   除了用上述方法外，还可以用下列方法(我懂的)

// 自定义类型如下


// 结构体小于号重载
struct node {
	int x, y; 
	bool operator < (const node &b) const { //应用于本题
    return x == b.x ? y < b.y : x > b.x;
    }
};


```

*****















### **五、链表list**



##### **头文件 #include < list >**







### **六、集合set**

##### **头文件 #include < set >**



#### 主要操作





### **七、映射map**



##### **头文件 #include  < map >**



**unordered_map内部原理是散列表，查找操作O(1)，而map内部原理为一颗红黑树**

#### 主要操作



* 