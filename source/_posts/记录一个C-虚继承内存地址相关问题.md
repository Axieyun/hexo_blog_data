---
title: 记录一个C++虚继承内存地址相关问题
tags:
  - 问题
abbrlink: adb5
date: 2022-01-17 20:00:49
password:
---





*运行环境：64位win电脑*



#### 贴出代码



~~~c
// 解决菱形继承问题


#include <iostream>

using std::cout;
using std::endl;

class A {
public:
    int a; 
public:
    A() {
        cout << "A constructor" << endl;
    }  
};

class A2 : virtual public A{ //在类的首地址生成一个虚基表指针，占8字节
public:
    A2() {
        cout << "A2 constructor" << endl;
    }
};


class A1 : virtual public A{ //在类的首地址生成一个虚基表指针，占8字节
public:
    A1() {
        cout << "A1 constructor" << endl;
    }
};


// 调用父类构造函数顺序？
// 调用父类构造函数与父类继承顺序有关，析构顺序相反
class Derived : virtual public A1, virtual public A2 {
public:
    Derived() {
        cout << "Derived constructor" << endl;
    }
};

int main() {
    
    cout << sizeof(A) << endl;
    cout << sizeof(A1) << endl;
    cout << sizeof(A2) << endl;
    cout << sizeof(Derived) << endl;

    A a;
    cout << "Address a = " << &a << endl;
    cout << "Address a.a = " << &a.a << endl;


    A1 a1;
    cout << "Address a1 = " << &a1 << endl;
    cout << "Address a = " << &a1.a << endl;

    A2 a2;
    cout << "Address a2 = " << &a2 << endl;
    cout << "Address a = " << &a2.a << endl;

    Derived d;
    // 问题一：为啥65行于68行输出地址相差8字节
    cout << "Address d = " << &d << endl;

    //问题二：为啥下面3行输出地址一样
    cout << "Address a = " << &d.a << endl;
    cout << &(d.A1::a) << endl;
    cout << &(d.A2::a) << endl;

    return 0;

}

~~~









#### 问题一



**当代码中class Derived : virtual public A1, virtual public A2{}改成class Derived : public A1, public A2{}的时候，会出现代码中的问题一，字节相差数量由8变成16，那么相差16我知道是存储了2个8字节的虚基表指针，相差8字节的话可以理解成存储1个8字节的虚基表指针吗？**





~~~tex
但是sizeof(Derived)在代码改变前后一直不变为24.
如果理解成存储1个8字节的虚基表指针，那么按照8字节对齐，为啥sizeof(Derived) = 24
字节相差数量 = 16，可以理解存储了2个8字节的虚基表指针，最后8字节存储两个int
~~~







#### 问题二：对于代码中，为啥出现下面问题





~~~tex
    问题二：为啥下面3行输出地址一样
    cout << "Address a = " << &d.a << endl;
    cout << &(d.A1::a) << endl;
    cout << &(d.A2::a) << endl;
~~~















*有待解决*

