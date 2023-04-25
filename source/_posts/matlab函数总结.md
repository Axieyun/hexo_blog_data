---
title: matlab函数总结
abbrlink: b5d8
date: 2022-06-14 13:57:20
tags:
password:
---

*这是不是你😜😜😜*

![gou](http://blog.axieyun.top/img/69.png)



## 画图



### 一、特殊图形&图形绘制



#### 0、稀疏矩阵可视化

| 函数名 | 功能描述     | 函数名 | 功能描述         |
| ------ | ------------ | ------ | ---------------- |
| gplot  | 绘制图论图形 | spy    | 绘制稀疏矩阵结构 |



#### 1、特殊二维图形

![gou](http://blog.axieyun.top/img/70.png)

* hisfit：画具有分布拟合的直方图



#### 2、等高线及其他二维图形

![gou](http://blog.axieyun.top/img/71.png)



#### 3、特殊三维图形

![gou](http://blog.axieyun.top/img/72.png)



#### 5、基本三维图形

![gou](http://blog.axieyun.top/img/76.png)



#### 4、实体模型

* cylinder：圆柱体生成
* sphere：球体生成



#### 6、基本二维图形

![gou](http://blog.axieyun.top/img/77.png)



#### 7、三维颜色控制

![gou](http://blog.axieyun.top/img/78.png)



#### 8、三维光照模型

![gou](http://blog.axieyun.top/img/79.png)



#### 9、标准调色板设置

![gou](http://blog.axieyun.top/img/80.png)



#### 10、三维视点控制

| 函数名   | 功能描述         | 函数名  | 功能描述     |
| -------- | ---------------- | ------- | ------------ |
| rotate3d | 设置三维旋转开关 | viewmtx | 求视转换矩阵 |
| view     | 设置视点         |         |              |



### 二、图形处理



#### 1、图形窗口生成与控制

![gou](http://blog.axieyun.top/img/73.png)



#### 2、坐标轴建立与控制

![gou](http://blog.axieyun.top/img/74.png)



#### 3、处理图形对象

![gou](http://blog.axieyun.top/img/75.png)



#### 4、图形注解

| 函数名   | 功能描述             | 函数名 | 功能描述              |
| -------- | -------------------- | ------ | --------------------- |
| colorbar | 颜色条设置           | xlabel | 给图形的x轴加文字说明 |
| gtext    | 在鼠标位置加文字说明 | ylabel | 给图形的y轴加文字说明 |
| text     | 在图形上加文字说明   | zlabel | 给图形的z轴加文字说明 |
| title    | 给图形加标题         |        |                       |





## 字符串处理函数



#### 1、字符串处理

![gou](http://blog.axieyun.top/img/81.png)



#### 2、字符串与数值转换

| 函数名  | 功能描述       | 函数名  | 功能描述       |
| ------- | -------------- | ------- | -------------- |
| num2str | 变数值为字符串 | sprintf | 数值的格式输出 |
| str2num | 变字符串为数值 | sscanf  | 数值的格式输入 |
| int2str | 变整数为字符串 |         |                |

#### 3、进制转换

| 函数名  | 功能描述                         | 函数名  | 功能描述               |
| ------- | -------------------------------- | ------- | ---------------------- |
| hex2num | 十六进制到IEEE标准下浮点数的轮换 | hex2dec | 十六进制到十进制的轮换 |
| dec2hex | 十进制到十六进制的轮换           |         |                        |







## 文件操作



#### 1、基本文件输入输出

![gou](http://blog.axieyun.top/img/82.png)









## 概率论与数理统计





* mle：最大似然估计、区间估计

* * 最大似然估计：

  * * ~~~matlab
      phat = mle('dist',data)
      ~~~

  * 区间估计:

  * * ~~~matlab
      [phat,pci] = mle('dist',data)
      [phat,pci] = mle('dist',data,alpha)
      [phat,pci] = mle('dist',data,alpha,p1)
      ~~~

* moment：矩估计















**转载&参考**

* [(74条消息) matlab库函数大全_腾讯数据架构师的博客-CSDN博客_matlab函数大全](https://blog.csdn.net/luanpeng825485697/article/details/77510669)
* 
