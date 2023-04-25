---
title: matlab之struct数组排序
tags: matlab
abbrlink: '1899'
date: 2022-03-25 22:04:50
password:
---

### 分配内存

~~~matlab
person = struct{'name', [], 'age', 0, 'sex', []};
persons = repmat(person, [30, 40]); % 分配内存，30*40的结构体矩阵
~~~







~~~matlab
clear;
clc;
close all;

%%
x=[7.15 8.25 3.20 10.30 6.68 12.03 16.85 17.51 9.30];
y=[11.10 15.00 6.00 16.25 9.90 18.25 20.80 24.15 15.50];
n=[568 1205 753 580 395 2104 1538 810 694]';

%{
% S=(y-x)*n
% y - x

%}
name =  ["x1", "x2", "x3", "x4", "x5", "x6", "x7", "x8", "x9"];
for i = 1:9
    st(i).name = name(i); %货号
    st(i).x = x(i); % 该商品的进价
    st(i).y = y(i); % 该商品的售价
    st(i).n = n(i); % 该商品的销量
    st(i).ny = y(i) * n(i); % 每种商品的收入
    st(i).nx = x(i) * n(i); % 每种商品的成本 
    st(i).s = st(i).ny - st(i).nx; % 每种商品的利益
end
%{
结构体排序，安装key = st.s排序 其value，返回index（排序后对应的索引）
ss为返回的排序好的对应的value
%}
[ss, index] = sort([st.s]);
ss
%index
%最小利润
fprintf("最小利润货号：%s\n", st(index(1)).name);
fprintf("最小利润：%d\n", st(index(1)).s);
%最大利润
fprintf("最大利润货号：%s\n", st(index(9)).name);
fprintf("最大利润：%d\n", st(index(9)).s);

    
[ss, index] = sort([st.ny]);
%index
%最小收入
fprintf("最小收入货号：%s\n", st(index(1)).name);
fprintf("最小收入：%d\n", st(index(1)).ny);
%最大收入
fprintf("最大收入货号：%s\n", st(index(9)).name);
fprintf("最大收入：%d\n", st(index(9)).ny);

ny = y*n; %收入
s = (y - x)*n; %利益
fprintf("收入：%d\n", ny);
fprintf("利润：%d\n", s);

~~~

