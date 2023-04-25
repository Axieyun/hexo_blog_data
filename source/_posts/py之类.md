---
title: py之类
tags: python
abbrlink: 2cca
date: 2021-10-25 23:44:00
password:
---



### 类的定义



* 类（class）：**用来描述具有相同的属性和方法的对象的集合，对象是类的实例**
* 方法：类中的函数
* 类变量：在整个实例化的对象是公有的。类变量定义在类中且在函数体之外。类变量通常不作为实例变量使用
* 数据成员：类变量或者实例变量用于处理类及其实例对象的相关的数据
* 方法重写：如果从父类继承的方法不能满足子类的需求，可以对其进行改写，这个过程叫方法的覆盖，也称为方法的重写。
* 局部变量：定义在方法中的变量，只作用于当前实例的类。
* 实例变量：在类的声明中，属性是用变量来表示的，这种变量称为实例变量，实例变量就是一个用self修饰的变量
* 继承：一个派生类继承基类的字段和方法。
* 实例化：创建一个类的实例，类的具体对象
* 对象：通过类定义的数据结构实例。对象包含两个数据成员（类变量与实例变量）和方法



#### 格式

* ~~~python
  class 类名:
      ...
      ...
  ~~~



#### 类的实例self

**self代表类的实例，是一个对象**



* 对于类中的每一个方法，没有参数时，记得加self
* 类的第一个参数为self









### 继承与重写





~~~python
## 继承与重写

class People:
    def __init__(self, name, age, wight):
        print(self)
        self.name = name
        self.age = age
        self.wight = wight
        return
    def sayhello(self):
        print('大家好，我是{}，体重为{}Kg，今年{}岁了！！！'.format(self.name, self.wight, self.age))
        return

class student(People):
    def __init__(self, name, age, wight, grade):
        People.__init__(self, name, age, wight)
        self.grade = grade
        return
    def sayhello(self):
        print('大家好，我是{}，体重为{}Kg，今年{}岁了，在读{}年级'.format(self.name, self.wight, self.age, self.grade))
        return

s = People('Axieyun', 20, 56)
s.sayhello()
p = student('Axieyun', 20, 56, '大一')
p.sayhello()
~~~





### 类的私有属性与私有方法



#### __private_attrs私有属性

* 变量格式：变量名前加两个下划线__
* 函数格式：函数名前加两个下划线__





~~~python
class JustCounter:
    publicCount = 0
    __privateCount = 0
    def count(self):
        self.publicCount += 1
        self.__privateCount += 1
        return

counter = JustCounter()

counter.count()
counter.count()

print(counter.publicCount)
# print(counter.__privateCount) # 报错
~~~



~~~python
class Site():
    def __init__(self, name, url):
        self.name = name
        self.url = url

    def who(self):
        print('{}：{}'.format(self.name, self.url))

    def __private(self):
        print('私有的方法')
    def public(self):
        """

        :rtype: object
        """
        print('共同的方法')
        self.who()
        print('&&&&&&&&&')
        print('私有的方法')

s = Site('KKB', 'kaikeba.com')
s1 = Site('TaoBao', 'taobao.com')
s.who()
s.public()
# s.__private()
~~~





### 类的专用方法

* 1.init()：构造函数，在生成对象时调用
* 2.del()：构造函数，释放对象时使用 

* 3.repr()：打印，转换 

* 4.setitem():按照索引赋值==》a[idx] 

* 5.getitem():按照索引获取值==》a[idx] 

* 6.len():能够获得长度==》len(obj) 

* 7.cmp():比较运算 

* 8.call():函数调用

* 9.add():加运算 

* 10.sub():减运算 

* 11.mul():乘运算 

* 12.truediv():除运算 

* 13.mod():求余运算 

* 14.pow():乘方





#### add函数使用



~~~python
class Site():
    def __init__(self, name, url):
        self.name = name
        self.url = url

    def who(self):
        print('{}：{}'.format(self.name, self.url))

    def __private(self):
        print('私有的方法')
    def public(self):
        """

        :rtype: object
        """
        print('共同的方法')
        self.who()
        print('&&&&&&&&&')
        print('私有的方法')
    def __add__(self, other):
        return Site(self.name + other.name, self.url + other.url)

s = Site('KKB', 'kaikeba.com')
s1 = Site('TaoBao', 'taobao.com')
s.who()
s.public()
# s.__private()


print('%' * 40)
tp = s + s1
tp.who()
tp.public()

~~~



