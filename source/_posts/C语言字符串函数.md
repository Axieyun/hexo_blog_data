---
title: C语言字符串函数
abbrlink: 63a6
date: 2022-01-25 15:58:41
tags:
password:
---





### strcpy



~~~c
char* strcpy(char * dest, char* src);
~~~

~~~tex
是一个将存放在src里的字符串复制到dest的字符串里的函数；它的返回值是dest。需要注意的是，strcpy在复制字符串时会自动在末尾加上\0，
因此dest的长度必须比src多至少 1 个字符；如果dest的长度小于这个长度，你就会在运行时碰到 段错误（Segmentation Fault） 。这个看似很小的问题可能会导致你 debug 到凌晨三点，所以绝不要忘记！
~~~







### strlen



~~~c
size_t strlen(const char* str);
~~~

~~~tex
这个函数在输入的字符串str中寻找\0，返回从第一个字符到第一个\0的长度（不包括\0）。注意，如果你用一个长度为 \ 100 100 的数组存储了一个实际长度只有 \ 5 5 的字符串，strlen会返回 \ 5 5，而不是 \ 100 100。
~~~







### strcat



~~~c
char* strcat(char* dest, char* src);
~~~



~~~tex
它可以将src里存储的字符串接到dest的后面，它的返回值也是dest。
~~~





### strcmp

~~~c
int strcmp(char* str1, char* str2);
~~~



~~~tex
这个函数可以比较两个字符串是否完全一致。如果函数返回 0，则表示两个输入的字符串完全一致；如果函数返回值大于 0，则str1与str2第一个分歧的字符处，str1的字符比str2的同位置字符对应更大的值（你可以参考 ASCII 表格来找到每个字符唯一对应的整数）；否则str2的第一个分歧字符大于str1的第一个分歧字符。
~~~





### strtok

~~~c
char* strtok(char* str, const char* delimiters);
~~~



~~~~tex
这个函数将输入的字符串str用输入的分隔符delimiters分为更短的字符串。delimiters是一个含有多个字符的字符串，其中每一个字符都是一个独立的分隔符。如\n\t中\n和\t分别可以作为独立的分隔符。需要注意的是，strtok会修改输入的字符串str；因此如果你想保留原有的字符串，最好先用strcpy将原有的字符串复制到另一个字符串里，然后再将字符串输入到strtok里分割。当我们将一个字符串str输入到strtok里后，strtok会返回一个指向第一个由非分隔符字符的指针的分割片段；之后的每一次调用，我们都会把NULL作为第一个参数，如果调用成功就会返回下一个分割片段，如果已经到达str的末尾则会返回NULL。
~~~~







### 题目



小开喜欢上了同一个办公室的小虹，于是给小虹写了一封情书，但他有点担心自己的情书不能充分表达自己的爱意。请你写一个程序帮小开数一数他的情书里出现了几次单词`love`（不忽略大小写，仅统计小写）。为了找出单词`love`，我们需要把情书分割为一个个单词，然后统计单词`love`出现的次数，最后为了方便小开检查，我们还要附上情书原文。读取情书和输出原文部分已经写好，你需要编写拆分单词和统计出现次数的部分。

#### 输入格式

一个长度不超过 256 个字符的字符串，字符串中的单词由且仅由空格、`\t`、`\n`、`\r\n`、`"`、`'`、`,`或`.`分隔开。

#### 输出格式

先按照顺序输出情书中出现的所有单词（不排除重复），每个占一行，然后输出一个整数（占一行），代表输入字符串中`love`出现的次数。最后输出情书的内容。





#### 样例输入

~~~tex
I love you so much, Ms.Ginger.
My love is like a flame burning in my heart, making it difficult for me to think about anyone but you.
Please accept my love.

With love,
Mr.Happy
~~~





#### 样例输出

~~~tex
I
love
you
so
much
Ms
Ginger
My
love
is
like
a
flame
burning
in
my
heart
making
it
difficult
for
me
to
think
about
anyone
but
you
Please
accept
my
love
With
love
Mr
Happy
4
I love you so much, Ms.Ginger.
My love is like a flame burning in my heart, making it difficult for me to think about anyone but you.
Please accept my love.

With love,
Mr.Happy
~~~





#### 代码



~~~c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <malloc.h>
const int MAX_LENGTH = 256;

char* get_letter(void) {
    static char letter[1000000];
    letter[999999] = ' ';
    char *p = letter;
    int size = MAX_LENGTH;
    while (feof(stdin) == 0) {
        if (size == 0) {
            break;
        }
        fgets(p, size + 1, stdin);
        while (*p != '\0') {
            p++;
            size--;
        }
    }
    return letter;
}

int main() {
    char *str = get_letter();
    char *tp_str = (char*)malloc(strlen(str) + 1);
    int cnt = 0;
    tp_str = strcpy(tp_str, str);
    //char* dim = malloc(9);
    char* dim = " \t\n\r\"\',.";
    // dim[0] = ' ';
    // dim[1] = '\t';
    // dim[2] = '\n';
    // dim[3] = '\r';
    // dim[4] = '\"';
    // dim[5] = '\'';
    // dim[6] = ',';
    // dim[7] = '.';
    // dim[8] = '\0';
    char* s = strtok(tp_str, dim);
    while (s != NULL) {
        printf("%s\n", s);
        if (strcmp(s, "love") == 0) ++cnt;
        s = strtok(NULL, dim);
    }
    printf("%d\n", cnt);
    free(tp_str);
    // free(dim);    
    puts(str);

    return 0;
}

#undef MAX_LENGTH
~~~

