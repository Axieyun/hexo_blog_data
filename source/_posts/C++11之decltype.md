---
title: C++11之decltype
tags:
  - C++11
abbrlink: 7fcc
date: 2022-01-10 12:17:03
password:
---





* decltype与auto关键字一样，用于进行编译时类型推导。
* decltype实际上有点像auto的反函数，auto可以让你声明一个变量，而decltype则可以从一个变量或表达式中得到类型。
* 注意，decltype 仅仅“查询”表达式的类型，并不会对表达式进行“求值”。
* 注意，auto参数类型只能用在lambda上面，不能用在普通函数上面。



* 例子1

~~~c++
int x = 1;
decltype(x) y = x;
~~~



* 例子2

~~~c++
int    i;
float  f;
double d;
 
typedef decltype(i + f) type1;  // float
typedef decltype(f + d) type2;  // double
typedef decltype(f < d) type3;  // bool
~~~





* 例子3

~~~c++
template<typename T, typename U>
auto Add(T t, U u) -> decltype(t + u) {
	return t + u;
}
~~~



