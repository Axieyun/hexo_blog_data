---
title: py之输入输出
tags:
  - python
abbrlink: 6aa2
date: 2021-10-25 01:03:20
password:
---





### 原型



### print函数



#### 原型



~~~python
print(*objects, sep='', end='\n', file=sys.stdout, flush=False)
~~~

* *objects：一个或多个待打印对象
* sep与end看实例
* file：指定文本流，表示打印的地方，默认控制台，可以接收文件（可写），被打印的参数转换为文本，不支持二进制文件。
* flush：从内存中刷新到磁盘中





#### 实例

~~~python


# sep参数

data = 'hello world!'
print(data, data, sep='xxx')

# end参数
print('asdasd', end='*******')
print('sdfsd')
~~~



### 格式化字符



~~~python
s = 'hello world'

print(str(s))

print(repr(s))
~~~



### 老式格式化字符



~~~python
print('%d + %d = %d\n'%(1, 2, 1 + 2))
print('%s + %c = %f\n'%('2342', 'f', 1.5 + 2))
print('%f + %d = %f\n'%(1.5, 2, 1.5 + 2))
~~~







### 格式化字串format





~~~python
import math
name = 'Axieyun'
age = 20
print('常量PI的值近视为：{0:.12f}'.format(math.pi))
# 大家好，我是Axieyun，今年20岁了

print('大家好，我是', name, '今年',age, '岁了')

info = '大家好，我是{}，今年{}岁了'.format(name, age)
print(info)
~~~



#### format根据关键字进行格式化字符



~~~python
name = 'Axieyun'
age = 20

print('大家好，我是{name}，今年{age}岁了'.format(age = age, name = name))
print('大家好，我是{name}，今年{age}岁了'.format(name = name, age = 18))
~~~



#### format根据索引进行格式化字符



~~~python
name = 'Axieyun'
age = 20
print('大家好，我是{0}，今年{1}岁了'.format(name, age))
print('大家好，我是{1}，今年{0}岁了'.format(age, name))
~~~





#### format根据字典进行格式化字符



~~~python
table = {'Google': 1, 'Baidu': 2, 'Taobao': 3}
for name, number in table.items():
    print('{0:7}==>{1:2d}'.format(name, number))

print('Baidu: {0[Baidu]: 2d}\nTaobao: {0[Taobao]: 2d}\nGoogle: {0[Google]: 2d}'.format(table))
~~~











### input函数



~~~python



s = input()
print(type(s))
print(s)

name = input('请输入你的名字：')
print('{}欢迎你'.format(name))



info = input().split()
print(info)
print(info[1], info[0])


~~~

