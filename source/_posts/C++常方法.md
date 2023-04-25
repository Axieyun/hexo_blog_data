---
title: C++之const
tags:
  - C++基础
abbrlink: e664
date: 2022-01-13 22:21:09
password:
---







### 常方法



* 在普通成员函数后面加const修饰 ，就是成员函数了





![](https://img-blog.csdnimg.cn/20210409144517261.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl81MDg4NjUxNA==,size_16,color_FFFFFF,t_70)





* 常方法可以访问对象中的常成员，也可以访问普通成员
* 常方法不允许修改任何数据
* 常方法可以修改mutable修饰的数据







~~~c++
#include <iostream>
#include <algorithm>
#include <string.h>
#include <vector>

#ifndef _MSC_VER
#define strcpy_s(DST,SIZE,SRC) strcpy((DST),(SRC))
#endif


using namespace std;

class People{

private:
    double height;
    double weight;
    // mutable 表示该变量易变，可以被常方法修改
    mutable int age;
    // const类型数据只能在构造函数初始化列表初始化
    const int score;
    char *name;


public:

    // 常方法
    // const修饰的方法
    char *getName() const {
        addAge();
        return name;
    }

    // void addAge(const People* const this);
    void addAge() const {
        age++;
    }
    
    People()
    : name{ new char(20) }
    , score(0)
    , age(18)
    , height(180)
    , weight(65) {
        strcpy_s(name, 20, "Axieyun");
    }

};


~~~







### 常数据

* const修饰的类型定义时必须赋值也就是初始化，在类中定义时，在参数列表初始化

* 即，不能先定义，后赋值

* * ~~~c
    const int a;
    a = 1;
    // 错误
    ~~~

* 应该这样

* * ~~~c
    const int a = 1; //正确
    ~~~





#### 使用情况（修饰指针）

* const int *p与int const *p等价：说明不能通过修改`*p`来修改p指向的值

* * 即，不能

* * ~~~c
    int a = 1;
    const int *p = &a;
    (*p)++; //不能这样
    ~~~

  * ~~~c
    int a = 1, b = 2;
    const int *p = &b;
    p = &a; // 可以这样
    ~~~

* int * const p: 说明不能修改p的值（地址）

* * 即，不能

  * ~~~c
    int a = 1, b = 2;
    int * const p = &b;
    p = &a; // 不可以这样
    ~~~

  * ~~~c
    int a = 1;
    int * const p = &a;
    (*p)++; //可以这样
    ~~~

* const int * const p ：说明两种情况都不行















