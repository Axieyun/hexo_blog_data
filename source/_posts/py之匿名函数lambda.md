---
title: py之匿名函数lambda
tags: python
abbrlink: a0ce
date: 2021-10-24 22:29:46
password:
---

### lambda



#### 优点

* 一次性函数，用的时候才加载

#### 格式



* 替代名字 = lambda 参数: 表达式





#### 实例



```python
def two_number_sum(a, b):
    return a + b

a, b = 1, 2
two_number_sum_lam = lambda a, b: a + b
print(two_number_sum(a, b))
print(two_number_sum_lam(a, b))
```
