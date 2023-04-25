---
title: python之pandas
tags: python
abbrlink: 80fc
date: 2021-10-31 16:32:25
password:
---



*先上代码，笔记后面总结*





### 代码一



~~~python
import pandas as pd
import matplotlib.pyplot as plt
import string as st
import numpy as np
import jieba
import wordcloud

# fpath = "南京二手房.csv"
# df = pd.read_csv(fpath, skiprows = 0)
# df.to_excel('南京二手房.xlsx', sheet_name='南京二手房') # 生成xlsx文件

fpath = "南京二手房.xlsx"    # 定义文件名字符串
df = pd.read_excel(fpath, '南京二手房', skiprows=0)  # 打开文件
# skiprows = 0 : 表示从第0行开始读取
# f = open('xx.txt', mode='r', encoding='utf-8')
# ff = open('xxx.txt', mode='w', encoding='utf-8')
# for x in f:
#     ff.write('{}'.format(x))
df.set_index('id', inplace=True)

# print(df.head(5)) # 查看前面5行数据
# print(df.tail(5)) # 查看后面面5行数据
# print(df.index)     # 查看行索引
# print(df.columns)   # 查看列索引
# print(df.values)    # 查看表格数据
# print(df['总价(万元)'])  #获取列索引为'总价(万元)'的数据
# print(df[0: 3])        # 获取行索引为0， 1， 2的行数据


print(df.loc[1, '总价(万元)'])  # 获取index=1, columns = '总价(万元)'的值
print(df.loc[1, ['总价(万元)', '产权年限']])  # Series 获取index=1, columns = '总价(万元)', '产权年限' 的值
print(df.loc[[1, 2], ['总价(万元)', '产权年限']]) # 得到 DataFrame

# 行index按区间1: 5
print(df.loc[1: 5, ['总价(万元)', '产权年限']])

# 列columns按区间
print(df.loc[1, '总价(万元)': '产权年限'])

# 行和列都按区间查询
print(df.loc[1: 5, '总价(万元)': '产权年限'])

## 条件表达式查询
# print(df['产权年限'] == '未知') # 返回真假
print(df.loc[df['产权年限'] == '未知', :])
print(df.loc[df['产权年限'] == '未知']['产权年限']) # 打印未知数据


## 调用函数查询lambda

print('调用函数查询lambda')
query_my_lambda_dataf = lambda f: (f['总价(万元)'] >= 298) & (f['总价(万元)'] <= 300)
print(df.loc[query_my_lambda_dataf(df), :])

print('调用函数查询')
def query_my_data(f):
    return (f['总价(万元)'] >= 298) & (f['总价(万元)'] <= 300)
print(df.loc[query_my_data(df), :])

# apply
def get_fumao_type(df):
    # print(type(df))
    if df['总价(万元)'] > 500:
        return '富翁'
    else:
        return '平民'
# python函数是否带括号的区别
# 不带括号，调用函数自己
# 带括号，有参得传参数。返回函数执行的结果
df.loc[:, 'fumao_type'] = df.apply(get_fumao_type, axis=1)
print(df.head(10))

print(df['fumao_type'].value_counts()) # 计算该列的不同值的数量


df['111'] = '' #创建空列
print(df.head(10))

## 统计所有数字列的结果
print(df.describe())

# 查看单列统计结果
print(df['总价(万元)'].max())
print(df['总价(万元)'].std())  #标准差
print(df['总价(万元)'].count())
print(df['总价(万元)'].mean()) #均值
print(df['总价(万元)'].min())

##
print(df['所在区域'].unique())   # 打印有多少个不同的区域



## 相关系数与协方差
print(df.cov())
df.loc[df['建筑类型'].notnull(), :] # 去除缺失值的行
df.loc[df['梯户比例'].notnull(), :] # 去除缺失值的行
df.loc[df['配备电梯'].notnull(), :] # 去除缺失值的行
df.dropna(axis='columns', how='all', inplace=True) # 去除空的列
df.dropna(axis='index', how='all', inplace=True)   # 去除空的行
df.to_excel('南京二手房_temp.xlsx', sheet_name='南京二手房', index=False) # 生成xlsx文件
# 绘图
# plt.figure()
# plt.plot(df.index, df['总价(万元)'])
# plt.show()

~~~







### 代码二



~~~python
# -*- coding:utf-8 -*-
import jieba
import pandas as pd
import wordcloud

file = open("xx.txt","r",encoding="utf-8")
t = file.read()
file.close()
# df = pd.read_excel('南京二手房.xlsx')
# t = df['所在区域']
ls = jieba.lcut(t)
txt = ''.join(ls)
w = wordcloud.WordCloud(font_path = "msyh.ttc",width=1000,height = 800, background_color="white" )
w.generate(txt)
w.to_file("wordcloud.png")

~~~







### 词云





~~~python
# -*- coding:utf-8 -*-
import jieba
import pandas as pd
import wordcloud

file = open("xx.txt","r",encoding="utf-8")
t = file.read()
file.close()
# df = pd.read_excel('南京二手房.xlsx')
# t = df['所在区域']
ls = jieba.lcut(t)
txt = ''.join(ls)
w = wordcloud.WordCloud(font_path = "msyh.ttc",width=1000,height = 800, background_color="white" )
w.generate(txt)
w.to_file("wordcloud.png")

~~~

