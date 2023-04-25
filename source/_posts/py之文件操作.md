---
title: py之文件操作
tags: python
abbrlink: 2e4e
date: 2021-10-25 19:06:22
password:
---



### 注意



* write写文件都是字符串，不是字符串，先转换为字符串读入



~~~python
f = open('text.txt', mode='w', encoding='utf-8')
num = f.write('Axieyun:Python是一门十分优雅的语言。\n某老头：我也觉得是的。\nAxieyun：桂林电子科技大学是一个监狱!\n某老头：我也觉得是。\n')
f.close()
print(num)

print('&'*30 + '1')
f = open('text.txt', mode='r', encoding='utf-8')
data = f.read()
print(data)
f.close()

print('&'*30 + '2')
f = open('text.txt', mode='r', encoding='utf-8')
data = f.readlines()
for x in data:
    print(x, end='')
f.close()
~~~

