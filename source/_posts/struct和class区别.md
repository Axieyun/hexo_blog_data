---
title: struct和class区别
tags: C++基础
abbrlink: a683
date: 2022-01-11 14:17:20
password:
---









### 两个区别

* `struct`的成员和基类默认都是`public`访问权限，而`class`是`private`。
* 当前置声明的时候，如果对于同一个类型，有的时候用`class`，有的时候用`struct`，或者前置声明跟实际不一样，会有警告。





* C 语言的`struct`有单独的命名空间，所以你甚至可以这样定义一个跟类型名字一样的变量：

~~~c
struct Student Student;
~~~

* C++ 则没有区分这些命名空间，如果你这样做的话，会给你一个警告，说你覆盖了名称，后面就用不了`Student`做类型了。

