---
title: sunday与kmp总结
abbrlink: '7663'
date: 2022-02-25 16:31:51
tags: 
  - 字符串匹配算法
  - 高级数据结构
password:
---


### kmp算法

#### 时间复杂度
* O(n + m)


#### next数组

* next数组 代表当前字符之前的字符串中，有多大长度的相同前缀后缀

* 有两种表示方法：
* * next[i] = k：表示第i位字符之前的字符串最长前缀的长度（这里的最长前缀满足next数组定义）
* * next[i] = k：表示第i位字符之前的字符串最长前缀的最后一个字符的下标



#### next数组的两种求法

~~~ c
//代表前缀长度 
void init_next(int *next, const char *t) {
	next[0] = next[1] = 0; // 第一个字符前面没有字符
	//k = next[j] 
//	for (int j = 1, k = 0; t[j]; ++j) {
//		while ( k > 0 && t[k] != t[j]) k = next[k]; //这不就是回溯么，啥情况回溯捏？while括号写了 
//		// 退出回溯 
//		// 如果退出回溯满足这个条件 
//		if (t[k] == t[j]) ++k;
//		next[j + 1] = k; //为什么是j+1，因为经过上面运算，k是第j+1位字符的最长前缀长度， 
//	}
//	
	for (int i = 1; t[i]; ++i) {
		int j = next[i];
		while (j && t[i] != t[j]) j = next[j];
		next[i + 1] = (t[i] == t[j]) ? j + 1 : 0;
	} 
	
}

~~~



~~~ c

//代表前缀下标
 
void init_next(int *next, const char *t) {
	// init next;初始化next数组 
	int next[maxn]; //next数组存储 最长前缀的下标位置 
	next[0] = -1; // 哨兵
	// k = next[j-1]，表示 前一个字符的最长前缀下标的位置 
	for (int j = 1, k = -1; t[j]; ++j) {
		
		/*
		k != -1
			：处理的当前字符 不是 第一个字符 
		t[k + 1] != t[j] 
			：（当前字符的前一个字符的最长前缀下标位置的下一个字符） 
				不等于 （当前字符） 
		*/ 
		while (k != -1 && t[k + 1] != t[j]) { 
			/*
			如果条件成立，那么最长前缀缩短，往回找最长前缀 
			*/ 
			k = next[k]; 
		}
		
		if (t[j] == t[k + 1]) { ++k; }
		next[j] = k;
	}
}
~~~



#### kmp代码

~~~ c

/*
s：文本串
t：模式串
从s中找t 
*/


/*
next数组 代表当前字符之前的字符串中，有多大长度的相同前缀后缀

*/ 


//代表前缀长度 
void init_next(int *next, const char *t) {
	next[0] = next[1] = 0; // 第一个字符前面没有字符
	//k = next[j] 
//	for (int j = 1, k = 0; t[j]; ++j) {
//		while ( k > 0 && t[k] != t[j]) k = next[k]; //这不就是回溯么，啥情况回溯捏？while括号写了 
//		// 退出回溯 
//		// 如果退出回溯满足这个条件 
//		if (t[k] == t[j]) ++k;
//		next[j + 1] = k; //为什么是j+1，因为经过上面运算，k是第j+1位字符的最长前缀长度， 
//	}
//	
	for (int i = 1; t[i]; ++i) {
		int j = next[i];
		while (j && t[i] != t[j]) j = next[j];
		next[i + 1] = (t[i] == t[j]) ? j + 1 : 0;
	} 
	
}

int kmp1(const char *s, const char *t) {
	
	// init next;初始化next数组 
	int next[maxn]; //next存储 当前字符之前 的 字符串的最长前缀的长度
	init_next(next, t);
	//匹配
	for (int i = 0, j = 0; s[i]; ++i) {
		while ( j > 0 && s[i] != t[j]) { j = next[j]; }
		if (s[i] == t[j]) ++j;
		if (t[j] == '\0') return i - j + 1;
	}
	return -1;
}



int kmp(const char *s, const char *t) {
	
	// init next;初始化next数组 
	int next[maxn]; //next数组存储 最长前缀的下标位置 
	next[0] = -1; // 哨兵
	// k = next[j-1]，表示 前一个字符的最长前缀下标的位置 
	for (int j = 1, k = -1; t[j]; ++j) {
		
		/*
		k != -1
			：处理的当前字符 不是 第一个字符 
		t[k + 1] != t[j] 
			：（当前字符的前一个字符的最长前缀下标位置的下一个字符） 
				不等于 （当前字符） 
		*/ 
		while (k != -1 && t[k + 1] != t[j]) { 
			/*
			如果条件成立，那么最长前缀缩短，往回找最长前缀 
			*/ 
			k = next[k]; 
		}
		
		if (t[j] == t[k + 1]) { ++k; }
		next[j] = k;
	}

	
	//匹配
	for (int i = 0, j = -1; s[i]; ++i) {
		while (j != -1 && s[i] != t[j + 1]) { j = next[j]; }
		if (s[i] == t[j + 1]) ++j;
		if (t[j + 1] == '\0') return i - j;
	}
	
	return -1;
}


~~~



### sunday算法


* 比BM算法更快


#### 时间复杂度分析

* Sunday预处理阶段的时间为：O(|∑| + m)
* 最坏情况下时间复杂度为：O(nm)
* 平均时间复杂度：O(n)

#### 算法分析

* Sunday算法是从前往后匹配，在匹配失败时，关注的是 文本串中参加匹配的最后一个字符（不是匹配失败的那个字符） 的 下一位字符
* * 如果该字符没有在模式串中出现则直接跳过，即移动位数 = 模式串长度 + 1；
* * 否则，其移动位数 = 模式串长度 - 该字符最右出现的位置(以0开始) = 模式串中该字符最右出现的位置到尾部的距离 + 1。

~~~ tex
例如

文本串s = assjdghsdgh
模式串t = asshdfs

对于第一次匹配
j != h
那么我们关注的字符是文本串中的第8个字符s


~~~



#### sunday代码

~~~c
int sunday(const char *s, const char *t) {
	int len_s = strlen(s);
	int len_t = strlen(t);
	int ind[256];
	
	//初始化每个字符的出现位置，没有在模式串t出现的字符初始化为len(t) + 1
	for (int i = 0; i < 256; ++i) ind[i] = len_t + 1;
	//每个字符从后往前在t出现的第一个位置距离t最后一个字符的距离 + 1
	for (int i = 0; t[i]; ++i) ind[ (int)t[i] ] = len_t - i; 


	int i = 0, flag;
	while (i + len_t <= len_s) {
		flag = 1;
		for (int j = 0; t[j]; ++j) {
			if (s[i + j] == t[j]) continue;
			flag = 0;
			break;
		}
		if (flag == 1) return i;
		i += ind[ (int)s[i + len_t] ]; //找出文本串头部的下一个位置（也就是模式串下一次对齐文本串的位置）
	}
	
	
	return -1;
}
~~~








