---
title: C++之string的使用
tags:
  - 字符串
  - C++基础
abbrlink: 52b9
date: 2021-08-17 18:24:05
password:
---









#### string类



* string 类重载了 + ， +=， =, [] ；







#### 结构定义



~~~c++
string s;

~~~









#### 获取长度

**时间复杂度为O(1)**

* s.size()
* s.length()





#### 查找



**s.find( const basic_string& str, size_type pos = 0 )**

 

* 第一个参数是查找的字符串，第二个参数是开始查找的位置，第二个可以不写，默认为0，查找不到返回string::npos
* 返回值：找到的子串的首字符位置，或若找不到这种子串则为string::npos



















