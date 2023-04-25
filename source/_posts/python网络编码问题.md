---
title: python网络编码问题
tags:
  - 网络编程
  - python
  - 编码
abbrlink: '6834'
date: 2022-03-11 16:23:07
password:
---


#### 前言

~~~tex

最近在win 和 linux进行网络编程时，win利用python编写图形化客户端，Linux编写server。
遇到问题：网络字节流的传输

在python中，字节流与字符编码，以及老是遇到无效解码位置
~~~




### 学习

* 在最新的Python 3版本中，字符串是以Unicode编码的，也就是说，Python的字符串支持多语言。

* 对于单个字符的编码，Python提供了ord()函数获取字符的整数表示，chr()函数把编码转换为对应的字符：

~~~tex
>>> ord('a')
97
>>> ord('中')
20013
>>> chr(97)
'a'
>>> chr(20013)
'中'

~~~

* 由于Python的字符串类型是str，在内存中以Unicode表示，一个字符对应若干个字节。
* 如果要在网络上传输，或者保存到磁盘上，就需要把str变为以字节为单位的bytes。
* Python对bytes类型的数据用带b前缀的单引号或双引号表示：b'abc'



#### 理论


~~~tex
Unicode编码使得在一个文本中，各个国家的文字都可以清楚的表示出来
这点ASCII是无法做到的）。（内存中表示）

utf-8 是秉承一种节约磁盘空间或者网络带宽，它常用与在传输中，
或者是存储于磁盘中的一种编码。（硬盘或者需要传输的时候表示）

~~~



#### encode()

* 以Unicode表示的str通过encode()方法可以编码为指定的bytes


~~~python

>>> 'abc'.encode('utf-8')
b'abc'
>>> 'abc'.encode('ascii')
b'abc'

纯英文的str可以用ASCII编码为bytes，内容是一样的，
含有中文的str可以用UTF-8编码为bytes。
含有中文的str无法用ASCII编码，因为中文编码的范围超过了ASCII编码的范围，
Python会报错。

在bytes中，无法显示为ASCII字符的字节，用\x##显示。

>>> '中'.encode('utf8')
b'\xe4\xb8\xad'

>>> '中'.encode()          #python3 默认就是utf-8
b'\xe4\xb8\xad'

>>> '中'.encode('ascii')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
UnicodeEncodeError: 'ascii' codec can't encode character '\u4e2d' in
position 0: ordinal not in range(128)


~~~



#### decode()

* 如果我们从网络或磁盘上读取了字节流，那么读到的数据就是bytes。要把bytes变为str，就需要用decode()方法

~~~python
>>> b'\xe4\xb8\xad'.decode('utf-8')
'中'
>>> b'\xe4\xb8\xad'.decode()     #python3 默认就是utf-8   
'中'

如果bytes中包含无法解码的字节，decode()方法会报错：
>>>  b'\xe4\xb8\xad\xff'.decode()
  File "<stdin>", line 1
    b'\xe4\xb8\xad\xff'.decode()
    ^
IndentationError: unexpected indent


如果bytes中只有一小部分无效的字节，可以传入errors='ignore'忽略错误的字节：
>>>b'\xe4\xb8\xad\xff'.decode('utf8',errors='ignore')

~~~


### 总结


* 在操作字符串时，我们经常遇到str和bytes的互相转换。为了避免乱码问题，应当始终坚持使用UTF-8编码对str和bytes进行转换。

#### 问题1
~~~tex
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xc8 in position 0: invalid continuation byte
~~~
* 该情况是由于出现了无法进行转换的 二进制数据造成的

**解决**

* 采用decode('utf8',errors='ignore')进行解码
* 出现异常报错是由于设置了decode()方法的第二个参数errors为严格（strict）形式造成的，因为默认就是这个参数，将其更改为ignore等即可。




#### 参考博客

~~~tex
https://blog.csdn.net/cxs123678/article/details/80040360?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164698628316781685370527%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=164698628316781685370527&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-3-80040360.pc_search_insert_es_download&utm_term=%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8B%E7%BC%96%E7%A0%81&spm=1018.2226.3001.4187

~~~



~~~tex
https://blog.csdn.net/qq_18888869/article/details/82625343?ops_request_misc=&request_id=&biz_id=102&utm_term=%E7%BD%91%E7%BB%9C%E7%BC%96%E7%A8%8BUnicodeDecodeError:%20%27utf-8&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-1-82625343.nonecase&spm=1018.2226.3001.4187

~~~







