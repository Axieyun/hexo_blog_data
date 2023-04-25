---
title: 高阶线性微分方程组matlab求解
abbrlink: a1b6
date: 2022-05-24 19:18:39
tags:
password:
---





![1](http://blog.axieyun.top/img/30.png)

![1](http://blog.axieyun.top/img/31.png)

![1](http://blog.axieyun.top/img/32.png)







~~~matlab
clear; clc; close all;
%%
f = @(x, y) [
    y(2);
    (1 - y(1)^2)*y(2) - y(1)
    ];
[x, y] = ode45(f, [0 10], [2 0]);
plot(x, y, '-*')
~~~

**效果**

![1](http://blog.axieyun.top/img/33.png)

