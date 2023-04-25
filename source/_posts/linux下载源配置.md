---
title: linux的apt源配置
tags: linux
abbrlink: b2a1
date: 2022-03-13 17:22:45
password:
---

#### 查看源类型

* 命令
~~~bash
lsb_release -a
~~~

* 显示
~~~tex
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.3 LTS
Release:        20.04
Codename:       focal
~~~


#### 切换apt源

* 打开文件/etc/apt/sources.list

~~~bash
sudo vim /etc/apt/sources.list
~~~


* 切换[清华源](https://mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

~~~vim
#清华源
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse



#阿里源
deb http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main restricted universe multiverse

# deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse
# deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main restricted universe multiverse

deb http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main restricted universe multiverse

~~~

* 最后执行 刷新 存储库索引
~~~bash
sudo apt update
~~~

#### 学习


~~~tex
ubuntu各个版本的源如下：

Ubuntu 12.04 (LTS)代号为precise。

Ubuntu 14.04 (LTS)代号为trusty。

Ubuntu 15.04 代号为vivid。

Ubuntu 15.10 代号为wily。

Ubuntu 16.04 (LTS)代号为xenial。

Ubuntu 18.04 (LTS)代号为bionic。

Ubuntu 20.04 (LTS)代号为focal。
~~~




*参考*
[链接](https://blog.csdn.net/woshiheweigui/article/details/115557020?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522164715562716780271932529%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=164715562716780271932529&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~rank_v31_ecpm-5-115557020.pc_search_insert_es_download&utm_term=E%3A+Unable+to+correct+problems%2C+you+have+held+broken+packages.&spm=1018.2226.3001.4187)