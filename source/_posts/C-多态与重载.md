---
title: C++多态与重载
tags:
  - C++基础
abbrlink: aade
date: 2022-01-16 13:25:19
password:
---







### 重载





#### 语法



~~~tex
<返回值类型> operator<重载的运算符> (<参数列表>) {
// 函数体
}
~~~



#### 不能重载的运算符



* ? : 条件运算符
* .(成员访问运算符)
* *(成员指针访问运算符)
* ::(域运算符)
* sizeof(sizeof是运算符，不是函数)





### 多态

* 多态按字面的意思就是多种形态，相同的方法调用，但是有不同的实现方式。
* 多态性可以简单地概括为“一个接口，多种方法”。
* C++有两种多态形式：
* * 静态多态
  * 动态多态





#### 静态多态

* 也称为编译期间的多态，编译器在编译期间完成的，编译器根据函数实参的类型(可能会进行隐式类型转换)，可推断出要调用那个函数，如果有对应的函数就调用该函数，否则出现编译错误。
* 静态多态有两种实现方式
* * 函数重载：包括普通函数的重载和成员函数的重载
  * 函数模板的使用
  * 宏的使用



#### 动态多态

* 动态多态（动态绑定）：即运行时的多态，在程序执行期间(非编译期)判断所引用对象的实际类型，根据其实际类型调用相应的方法。









### 代码实现



~~~c++
#include <iostream>



using namespace std;


class Point {


private:
    float _x, _y;

public:

    Point() : _x(0), _y(0) {

    }

    Point(float x, float y) : _x(x), _y(y) {

    }

	// 多态实现
    // 成员函数重载
    void print(int n) { //5printi
        cout << "int :" << "_x = " << (int)_x << ", _y = " << (int)_y << endl;
    }

    void print(float n) { //5printf
        cout << "float :" << "_x = " << _x << ", _y = " << _y << endl;
    }

    void print(float, int) { // 5printfi

    }

    void print(float, float) { // 5printff

    }

    void print(int, int) { // 5printii

    }

    void print(int, float) { // 5printif

    }

    // +
    Point operator+ (const Point& p) {
        return Point{_x + p._x, _y + p._y};
    }
    // +
    Point operator- (const Point& p) {
        return Point{_x - p._x, _y - p._y};
    }

    // p++
    Point operator++ (int) {
        Point p{_x, _y};
        _x += 1.0f;
        _y += 1.0f;
        return p;
    }
    // ++p
    Point& operator++ () {
        _x += 1.0f;
        _y += 1.0f;
        return *this;
    }

    // p--
    Point operator-- (int) {
        Point p{_x, _y};
        _x -= 1.0f;
        _y -= 1.0f;
        return p;
    }
    // --p
    Point& operator-- () {
        _x -= 1.0f;
        _y -= 1.0f;
        return *this;
    }

    // +=
    Point& operator+= (const Point& p) {
        _x += p._x;
        _y += p._y;
        return *this;
    }

    // -=
    Point& operator-= (const Point& p) {
        _x -= p._x;
        _y -= p._y;
        return *this;
    }

    // *=
    Point& operator*= (const Point& p) {
        _x *= p._x;
        _y *= p._y;
        return *this;
    }

    // /=
    Point& operator/= (const Point& p) {
        _x /= p._x;
        _y /= p._y;
        return *this;
    }

    float operator*(const Point& p) {
        return _x * p._x + _y * p._y;
    }




    friend ostream & operator<<(ostream& os, const Point& p) {
        os << "_x = " << p._x << ", _y = " << p._y;
        return os;
    }

    friend void print(const Point& p, int n);
    friend void print(const Point& p, float n);

};

// 普通函数重载

void print(const Point& p, int n) { //5printi
    cout << "_x = " << (int)p._x << ", _y = " << (int)p._y << endl;
}

void print(const Point& p, float n) { //5printf
    cout << "_x = " << p._x << ", _y = " << p._y << endl;
}

int main() {
    Point p1{1.2f, 2.3f}, p2{2.4f, 5.2f};
    
    p1.print(1);
    p2.print(1.0f);

    print(p1, 1);
    print(p2, 1.0f);
    // p1.print(1.0); //报错 
    // 1.0是double类型，编译器隐式转换为int类型或者float类型，但是转换为int与float相同，报错

    cout << p1 << p2 << endl;
    cout << typeid(p1).name() << endl;

    cout << p1 + p2 << endl; //(3.6, 7.5)
    cout << p1 - p2 << endl; //(-1.3, -2.9)
    cout << ++p1 << endl; // (1.2, 2.3) => (2.2, 3.3)
    cout << p1++ << endl; // (2.2, 3.3) => (3.2, 4.3)
    cout << --p2 << endl; // (2.4, 5.2) => (1.4, 4.2)
    cout << p2-- << endl; // (1.4, 4.2) => (0.2, 3.2)
    cout << p1 * p2 << endl; // (3.2, 4.3) * (0.2, 3.2) = 15.04

    return 0;
}
~~~



#### 问题









#### 静态多态与动态多态的区别？



