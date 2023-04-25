---
title: py之模块
tags:
  - python
abbrlink: 3c1f
date: 2021-10-24 22:33:16
password:
---





### 定义



* 模块是一个包含所有你自己定义的函数和变量的文件，后缀名为.py





### 使用



* import 文件名（没有后缀名）
* 或者：import 文件名（没有后缀名）as 别名



#### 实例



~~~python
# -*- coding:utf-8 -*-
'''
---
作者: Axieyun
日期: 2021年10月24日
---
'''

import sys
import support as st
import fibonaciil as fl

st.welcome('Axieyun')

for i in sys.argv:
    print(i)
'''
>> python 1\测试.py 123 abc
<<< 
1\测试.py
123
abc
'''

print('\npython的路径：', sys.path)

print('\npython的路径：', sys.path[0])


print(fl.fibo_list(10))
fl.fibo(10)
~~~





### __name__属性



#### 定义



* 一个模块被另一个程序第一次引用时，其主程序将运行。

* 如果我们想再模块引入时，模块中的某一程序块不执行，

* ~~~python
  我们可以利用__name__属性来使该程序仅在该模块自身执行时执行
  ~~~



#### 实例



~~~python

# 直接打印前n个斐波那契数列的数
def fibo(n):
    a, b = 0, 1
    while n:
        a, b = b, a + b
        print(a)
        n = n - 1;
    return

# 以列表形式返回前n个斐波那契数列的数
def fibo_list(n):
    a, b = 0, 1
    result = []
    while n:
        a, b = b, a + b
        result.append(a)
        n = n - 1
    return result


if __name__ == '__main__':
    fibo(10)
    print('fibo_list调试：', fibo_list(10))
~~~







### 实例



#### 执行文件测试.py



~~~python
# -*- coding:utf-8 -*-
'''
---
作者: Axieyun
日期: 2021年10月24日
---
'''

import sys
import support as st
import fibonaciil as fl

st.welcome('Axieyun')

for i in sys.argv:
    print(i)
'''
>> python 1\测试.py 123 abc
<<< 
1\测试.py
123
abc
'''

print('\npython的路径：', sys.path)

print('\npython的路径：', sys.path[0])


print(fl.fibo_list(10))
fl.fibo(10)
~~~



#### 调用模块文件fibonaciil.py



~~~python

# 直接打印前n个斐波那契数列的数
def fibo(n):
    a, b = 0, 1
    while n:
        a, b = b, a + b
        print(a)
        n = n - 1;
    return

# 以列表形式返回前n个斐波那契数列的数
def fibo_list(n):
    a, b = 0, 1
    result = []
    while n:
        a, b = b, a + b
        result.append(a)
        n = n - 1
    return result


if __name__ == '__main__':
    fibo(10)
    print('fibo_list调试：', fibo_list(10))

~~~

