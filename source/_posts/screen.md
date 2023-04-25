---
title: screen
abbrlink: d06e
date: 2022-05-30 13:30:17
tags:
password:
---



### screen命令简介

~~~tex
Screen是一个全屏窗口管理器，它在多个进程（通常是交互式shell）之间多路传输物理终端。
每个虚拟终端提供DEC VT100终端的功能，以及ANSI X3的几个控制功能。
64（ISO 6429）和ISO 2022标准（例如，插入/删除行和支持多个字符集）。
每个虚拟终端都有一个回滚历史缓冲区和一个复制粘贴机制，允许用户在窗口之间移动文本区域。
当调用screen时，它会创建一个包含shell（或指定命令）的窗口，然后避开您的方式，以便您可以正常使用该程序。
然后，您可以随时创建包含其他程序（包括更多shell）的新（全屏）窗口、关闭当前窗口、查看活动窗口列表、打开和关闭输出日志、在窗口之间复制文本、查看滚动历史记录、在窗口之间切换，等等。
所有窗口都完全独立运行其程序。当窗口当前不可见时，甚至当整个屏幕会话与用户终端分离时，程序仍继续运行。
~~~

***简而言之***

~~~tex
远程服务器的时候，断网或者手误关掉了远程终端，会导致会话中断，程序终止。
而Screen连接的终端，会话独立运行，程序会一直进行。
而且会话可以恢复，还可以自行删除。
~~~



#### 安装

~~~bash
sudo apt-get install screen
~~~







### 操作

* id：为会话的id
* session_name：为会话的名字



#### 创建一个新的窗口（会话）

* 创建 名为 test 新的会话，并进入新的会话窗口

  ~~~bash
  screen -S test
  ~~~

  

#### 退出当前窗口（会话）

* 可以使用ctrl+a, 然后输入d，退出当前窗口；
* 也可以使用screen -d 退出当前窗口；





#### 结束窗口

* 结束当前窗口

  ~~~bash
  exit
  ~~~

* 在一个窗口结束别的 screen 创建的窗口，id为会话的id

  ~~~bash
  screen -S id.session_name -X quit
  ~~~

* 没有自己命名的，session_name为 screen -ls 出来的名字

  ~~~bash
  screen -S session_name -X quit
  ~~~

  



#### **查看有多少会话**

~~~bash
screen -ls
~~~





#### 进入别的窗口（会话）

* 重新连接会话前要求会话的状态为Detached。

~~~bash
screen -r id
screen -r session_name
~~~





#### 清除dead状态窗口

* 如果会话窗口被kill，状态转为dead无法连接，可以使用screen -wipe命令清除会话窗口。

~~~bash
screen -wipe
~~~





#### 会话锁定与解锁

* 虽然屏幕锁定的时候无反应但是会接受输入的命令，解锁后会全部执行，切勿输入危险命令。

* 锁定当前会话：ctl+a s，锁定之后输入任何内容屏幕都无反应；
* * 输入ctl+a x锁定会话，需要输入用户密码后才可以解锁。

* 解锁当前会话：输入ctl+a q之后解锁







### Screen命令中用到的快捷键

* Ctrl+a c ：创建窗口
* Ctrl+a w ：窗口列表
* Ctrl+a n ：下一个窗口
* Ctrl+a p ：上一个窗口
* Ctrl+a 0-9 ：在第0个窗口和第9个窗口之间切换
* Ctrl+a K(大写) ：关闭当前窗口，并且切换到下一个窗口（当退出最后一个窗口时，该终端自动终止，并且退回到原始shell状态）
* exit ：关闭当前窗口，并且切换到下一个窗口（当退出最后一个窗口时，该终端自动终止，并且退回到原始shell状态）
* Ctrl+a d ：退出当前终端，返回加载screen前的shell命令状态









#### 参考

* [(67条消息) Linux命令之screen命令_浪子吴天的博客-CSDN博客_screen命令](https://blog.csdn.net/carefree2005/article/details/122415714)
* [(67条消息) linux中screen的使用_我是天才很好的博客-CSDN博客_linux screen](https://blog.csdn.net/weixin_43593330/article/details/120591323)

