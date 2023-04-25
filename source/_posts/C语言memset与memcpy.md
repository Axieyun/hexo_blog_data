---
title: C语言memset与memcpy
tags: C语言
abbrlink: '3643'
date: 2022-01-25 16:41:46
password:
---





### memset

~~~c
void * memset ( void * ptr, int value, size_t num );
~~~

~~~tex
memset函数将从ptr指针所指的位置开始的、大小为num字节的内存中的每个字节设为你所指定的数值value。这个函数在将一段不用的内存全部归零时非常实用。你也许会问，用这个函数和用循环语句把某一段位置都设为零有什么区别吗？一般来讲，memset在系统自带的库文件中被优化过了（因为很多人使用），因此用它的效率往往比用循环简单实现的更高。当你想要把一大段内存设为一个数值的时候，不妨考虑使用这个函数，而不是一般的循环。
~~~





### memcpy



~~~c
void * memcpy ( void * destination, const void * source, size_t num );
~~~



~~~tex
memcpy与memset类似，也是修改一段连续的内存的函数。它将由source开始的长度为num字节的内存复制到由destination开始的长度为num字节的内存中。与memset类似，它的实现也在编译器中被优化了，因此速度会比较快。
~~~







