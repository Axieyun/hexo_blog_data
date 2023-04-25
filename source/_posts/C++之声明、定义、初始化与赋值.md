---
title: C++之声明、定义、初始化与赋值
tags:
  - C++基础
abbrlink: 8fdc
date: 2022-01-14 15:50:04
password:
---



### 前言



* 区分声明和定义可以让C++支持分开编译
* 声明常常见于头文件中









### 声明(Declaration)



* 声明是向编译器介绍名字－－标识符。它告诉编译器“**这个函数或变量在某处可找到，它的模样象什么”。**



#### 作用

* 只是规定了变量的类型和名字，而**没有进行内存分配**。



#### 语法



~~~c++
extern int i; // 声明declaration
int j; // 声明并定义j
extern double pi = 3.1416; // 定义definition
~~~







### 定义definition



* 定义是说：“**在这里建立变量”或“在这里建立函数”。它为名字分配存储空间**。无论定义的是函数还是变量，编译器都要为它们在定义点分配存储空间。

* 不仅规定了变量的类型和名字，而且**进行了内存分配**，也**可能**会对量进行初始化。







### 初始化initialization



* 初始化是在对象创建期获得一个默认值，并不是赋值。
* 在函数外部定义的全局变量(global variable)，函数内部定义的局部静态变量(local static object)全部初始化为0。
* 函数内部定义的局部变量，以及类中不在初始化成员列表和构造函数里体的成员变量都是未初始化的

~~~c++
#include <iostream>
 
using namespace std;
 
class Box {
   friend int main();
public:

   // 默认构造函数
   Box() : _i(1), _j(20) {
      // 函数体内部赋值为二次赋值
   }

   // 不能给i和j配默认值，即不能这样写：Box(int i = 0, j = 0)，会与默认构造函数冲突
   Box(int i, int j) : _i(i), _j(j) {

   }

private:
   int _i;
   int _j;
   // 声明
   static int count1; // 非const类型的静态类成员必须类外初始化
   static int count2; // 非const类型的静态类成员必须类外初始化
   static int count3; // 非const类型的静态类成员必须类外初始化
   const static int avg = 0; // const静态类成员可以直接初始化
};


int Box::count1{5}; //初始化
int Box::count2(2); //初始化
int Box::count3; //默认初始化为0

int i; // 初始化为0


int main(void) {

   Box b; // 调用默认构造初始化了
   int j; //没有初始化

   cout << i << " " << j << endl;
   cout << b._i << " " << b._j << endl;
   cout << "count1 = " << Box::count1 << endl;
   cout << "count2 = " << Box::count2 << endl;
   cout << "count3 = " << Box::count3 << endl;
   cout << "avg = " << Box::avg << endl;
   return 0;
}
~~~









### 赋值assignment



* **擦除**原有的值，并赋予**新值**。
* = 即为赋值







### 注意





* **内联函数的声明和定义需要在同一个文件里，否则无法通过编译。**
* 类的定义可以放到头文件中
* 值在编译时就已知的const 变量的定义可以放到头文件中
  如：const int num（10）;

* C++中变量的定义必须有且仅有一次，而变量的声明可以多次。
* 类的静态函数成员可以在类内部或者外部定义，而静态数据成员(const除外)则只能在外部定义以及初始化
