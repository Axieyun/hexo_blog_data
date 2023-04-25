---
title: C++之new和delete
tags:
  - C++基础
abbrlink: 35c4
date: 2022-01-10 13:02:34
password:
---



### new



* 申请内存，调用构造函数

* 语法

* ~~~c++
  new 数据类型(初始化参数列表);
  // 内存申请成功，那么new运算符就会返回一个指向新分配内存区域首地址的指针;
  ~~~

* 例子

* ~~~c++
  double *point;
  point=new double(2);
  
  
  sizeof(p) = 4; 指针占8字节
  sizeof(*p) = 8; 指向首地址，存储double类型数据对象，占8字节。
      
      
  int *point = new int;//没有初值
  int *point = new int();//初始值为0
  
  ~~~







### delete

* 删除一个用`new`建立的对象，回收堆内存，调用析构函数

* 使用

* ~~~c++
  delete line;//对于基本类型或者对象的指针
  delete[] a;//对于指向数组的指针
  ~~~

* 对nullptr调用delete或者delete[]都不会发生异常。









### 注意

* **所有用`new`分配的内存，都必须使用`delete`进行回收，否则会导致动态分配的内存无法回收，造成内存泄露**。







### 实例





~~~c++
#include <iostream>
#include <cmath>
using namespace std;

class Point{
public:
    Point(int newX=0,int newY=0) {
        cout << "calling constructor" << endl;
        x = newX;
        y = newY;
    }
    Point(Point &p) {
        x = p.x;
        y = p.y;
    }
    ~Point() {
        cout << "calling destructor " << endl;
    }
    void move(int x,int y) {
        this->x += x;
        this->y += y;
    }
    int getX() {
        return x;
    }
    int getY() {
        return y;
    }
private:
    int x,y;
};

class PointsArray {
public:
    PointsArray(int size):size(size) {
        array = new Point[size];
    }
    Point &getElement(int index) {
        if(index >= 0 && index < size) {
            return array[index];
        } else {
            cout << "invalid index!" << endl;
        }
    }
    ~PointsArray() {
        delete[] array;
    }
private:
    Point * array;
    int size;
};

int main(){
    Point * p = new Point(1, 2);
    delete p;
    int n;
    cin >> n;
    PointsArray pa(n);
    cout << pa.getElement(0).getX() << " " << pa.getElement(0).getY() << endl;
    pa.getElement(1).move(1, 1);
    cout << pa.getElement(1).getX() << " " << pa.getElement(1).getY() << endl;
    return 0;
}
~~~

