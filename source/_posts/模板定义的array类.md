---
title: 模板定义的array类
abbrlink: ca4c
date: 2022-01-11 13:41:18
tags:
      - C++基础
password:
---





### 题目描述

* 定义一一个可以保存数组，并且支持多种类型的 `array`
* 定义的 `array` 需要支持以下功能：
* * 构造的时候，给予一个整数参数 `n`，确定对象的容量（即内部要开多大的数组）
  * 通过insert成员函数在数组内的末尾插入一个元素，如果容量已经满了则输出 `array full`
  * 通过showAll成员函数，输出所有元素，每个占一行













~~~c++
#include <iostream>
#include <string>
#include <cstdlib>
using namespace std;

// 模板定义
template <class T>
class Array {
private:
    T* p;
    int size;
    int i;
public:
    // 申请堆内存
    Array(int size) {
        p = new T[size];
        this->size = size;
        i = 0;
    }
    // 释放堆内存
    ~Array() {
        delete[] p;
    }
    // 插入元素
    void insert(const int& val) {
        if (i < size) {
            p[i++] = val;
        } else {
            cout << "array full" << endl;
        }
    }

    // 按行输入所有元素
    void showAll() {
        for (int j = 0; j < i; j++) {
            cout << p[j] << endl;
        }
    }
};


int main() {
    string str1="yangzhou301";
    Array<char> arr1(str1.length());
    for(auto c:str1)
    {
        arr1.insert(c);
    }
    arr1.showAll();
    int num[]={1,9,2,6,0,8,1,7};
    Array<int>arr2(sizeof(num)/sizeof(int));
    for(auto c:num)
    {
        arr2.insert(c);
    }
	arr2.showAll();
    arr2.insert(301);
    return 0;
}

~~~



