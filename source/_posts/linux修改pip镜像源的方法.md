---
title: linux修改pip镜像源的方法
abbrlink: a2d7
date: 2022-05-30 17:35:56
tags:
password:
---

#### 修改

~~~bash
sudo pip config set global.index-url https://pypi.doubanio.com/simple
~~~

#### 查看

~~~bash
sudo cat /root/.config/pip/pip.conf
~~~

