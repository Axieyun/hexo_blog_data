---
title: C++类方法与类属性
tags:
  - C++基础
abbrlink: f9b1
date: 2022-01-13 22:21:34
password:
---





### 类属性（静态变量成员）

* static修饰的变量
* 非const修饰static类型的变量只能类外定义初始化，只能在类内声明
* const修饰的数据必须初始化









### 类方法（静态函数成员）



* static修饰的方法
* 类方法只能访问修改类属性



### **为什么要用得静态成员变量和静态成员函数**



* 为了实现共享。因为静态成员函数和静态成员变量属于类，不属于类的实体，这样可以被多个对象所共享





### **静态成员的作用、优点**



* 静态成员函数主要为了调用方便，不需要生成对象就能调用





### **静态成员函数与非静态成员函数的区别**



* 类的静态成员(数据成员和函数成员)为类本身所有，在类加载的时候就会分配内存，可以通过类名直接访问；非静态成员(数据成员和函数成员)属于类的实例所有，所以只有在创建类的实例的时候才会分配内存，并通过实例去访问。





### 总结



* static修饰的变量存储在静态区，只属于类，
* 类的静态数据成员是静态存储，它是静态生存周期，必须进行初始化。
* 静态数据成员的初始化在类体外进行，前面不加static以免与一般静态变量或者对象混淆。
* 要访问类的非静态成员可以通过对象来实现。
* 静态成员函数不能访问非静态成员
* 类对象可以调用静态成员，且可以修改静态变量成员





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
   static int getCount() {
   	  // _i += _j; //报错
      return count1;
   }
   
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
   cout << b.count1 << endl;
   b.count1 += 1;
   cout << b.count1 << endl;
   return 0;
}
~~~

