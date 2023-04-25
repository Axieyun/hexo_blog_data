---
title: BP神经网络
abbrlink: 4cf
date: 2022-07-10 14:38:05
tags:
password:
---





## 单层BP神经网络

### 概念

![](http://blog.axieyun.top/img/83.png)

BP(back propagation)神经网络是1986年由Rumelhart和McClelland为首的科学家提出的概念，是一种按照误差逆向传播算法训练的多层前馈神经网络，是应用最广泛的神经网络模型之一。

BP神经网络具有任意复杂的模式==分类==能力和优良的==多维函数映射==能力，解决了简单感知器不能解决的异或(Exclusive OR，XOR)和一些其他问题。

从结构上讲，BP网络具有输入层、隐藏层和输出层；从本质上讲，BP算法就是以网络误差平方为目标函数、采用梯度下降法来计算目标函数的最小值。





### 算法步骤

* 1、初始化网络学习参数，如设置网络初始权矩阵、学习因子等；
* 2、提供训练模式，训练网络，直到满足学习要求；
* 3、前向传播过程：对给定训练模式输入，计算网络的输出模式，并与期望模型比较，若有误差，则执行步骤4，否则返回步骤2；
* 4、反向传播过程：计算同一层单元的误差，修正权值和阈值，返回步骤2。







### 激活函数

![](http://blog.axieyun.top/img/84.png)



#### sigmod函数

* 这个函数在伯努利分布上非常好用
* 值域在0和1之间
* 函数具有非常好的对称性
* 函数对输入超过一定范围就会不敏感
* sigmoid的输出在0和1之间，我们在二分类任务中，采用sigmoid的输出的是事件概率，也就是当输出满足满足某一概率条件我们将其划分正类，不同于svm。



#### *双曲正切函数* tanh函数

* tanh函数是sigmoid函数向下平移和收缩后的结果



#### relu(修正线性单元)





#### Leaky relu

![](http://blog.axieyun.top/img/85.png)



#### 缺点

* sigmoid和tanh激活函数有共同的缺点：即在z（x）很大或很小时，梯度几乎为零，因此使用梯度下降优化算法更新网络很慢。由于sigmoid和tanh存在上述的缺点，因此relu激活函数成为了大多数神经网络的默认选择。
* relu也存在缺点：即在$z或x$小于0时，斜率即导数为0，因此引申出eaky relu函数，但是实际上leaky relu使用的并不多。











### 前向传播

![](http://blog.axieyun.top/img/86.png)

![](http://blog.axieyun.top/img/87.png)

![](http://blog.axieyun.top/img/88.png)







### 目标函数

![](http://blog.axieyun.top/img/89.png)





### 梯度下降

![](http://blog.axieyun.top/img/90.png)

![](http://blog.axieyun.top/img/91.png)

![](http://blog.axieyun.top/img/92.png)





### 反向求梯度

![](http://blog.axieyun.top/img/93.png)

![](http://blog.axieyun.top/img/94.png)

![](http://blog.axieyun.top/img/95.png)

![](http://blog.axieyun.top/img/96.png)

![](http://blog.axieyun.top/img/97.png)

![](http://blog.axieyun.top/img/98.png)

![](http://blog.axieyun.top/img/99.png)





### 综合梯度

![](http://blog.axieyun.top/img/100.png)



#### **隐藏层神经元和输出层神经元激活函数均为sigmod**

![](http://blog.axieyun.top/img/101.png)



* **输出层神经元激活函数改变**
* * ![](http://blog.axieyun.top/img/102.png)
  * ![](http://blog.axieyun.top/img/103.png)



#### **隐藏层神经元激活函数为sigmod *输出层神经元激活函数为*****tanh**

![](http://blog.axieyun.top/img/104.png)



#### **隐藏层神经元激活函数为sigmod** 输出层神经元激活函数为Purelin(线性)函数

![](http://blog.axieyun.top/img/105.png)



















### 转载或参考链接

* [机器学习之sigmoid函数 - 简书 (jianshu.com)](https://www.jianshu.com/p/506595ec4b58)
* [好玩的matlab](https://mp.weixin.qq.com/s/EbHgAZ-eU1qTvRnBZ1TSWA)
* [常见的激活函数（sigmoid、tanh、relu） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/63775557)

