---
title: abd使用
abbrlink: c786
date: 2022-07-24 13:21:44
tags:
password:
---





### 开启服务

~~~cmd
adb nodaemon server #不要关闭窗口
~~~



### 安装软件

~~~cmd
adb install
~~~







### 显示目前成功连接的设备

~~~cmd
adb devices
~~~









## 案例

* 手机电源键坏了

* adb.exe、AdbWinApi.dll需要复制到windows/system32 文件夹里

* 执行以下命令

* * 登录设备

  * ~~~cmd
    adb shell
    ~~~

  * ~~~cmd
    su
    adb reboot1
    ~~~







### 参考链接

* [adb.exe adb.exe进程是什么 有什么用-太平洋IT百科 (pconline.com.cn)](https://product.pconline.com.cn/itbk/software/dnyw/1704/9090747.html)
* [Win10 配置安装ADB教程总结20200514 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/140828682)、
* adb安装包：[知乎 - 安全中心 (zhihu.com)](https://link.zhihu.com/?target=https%3A//dl.google.com/android/repository/platform-tools-latest-windows.zip)

