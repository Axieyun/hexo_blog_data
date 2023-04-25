---
title: linux的apt命令
tags: linux
abbrlink: 19be
date: 2022-03-13 19:27:31
password:
---



### apt与apt-get之间的区别

~~~tex
通过 apt 命令，用户可以在同一地方集中得到所有必要的工具，apt 的主要目的是提供一种以「让终端用户满意」的方式来处理 Linux 软件包的有效方式。

apt 具有更精减但足够的命令选项，而且参数选项的组织方式更为有效。除此之外，它默认启用的几个特性对最终用户也非常有帮助。例如，可以在使用 apt 命令安装或删除程序时看到进度条。

apt 还会在更新存储库数据库时提示用户可升级的软件包个数。

如果你使用 apt 的其它命令选项，也可以实现与使用 apt-get 时相同的操作。
~~~

#### 注意

* 虽然 apt 与 apt-get 有一些类似的命令选项，但它并不能完全向下兼容 apt-get 命令。
* 也就是说，可以用 apt 替换部分 apt-get 系列命令，但不是全部。


### 命令

| apt命令 | 取代的命令 | 命令的功能|
| :-: | :-: | :-: |
| apt install | apt-get install | 安装软件包 |
| apt remove | apt-get remove     | 移除软件包 |
| apt purge	| apt-get purge	| 移除软件包及配置文件|
| apt update	| apt-get update	| 刷新存储库索引|
| apt upgrade	| apt-get upgrade	| 升级所有可升级的软件包|
| apt autoremove	| apt-get autoremove | 	自动删除不需要的包|
| apt full-upgrade	| apt-get dist-upgrade | 	在升级软件包时自动处理依赖关系|
| apt search	| apt-cache search	| 搜索应用程序|
| apt show	| apt-cache show	| 显示安装细节|
| apt list	|| 列出包含条件的包（已安装，可升级等）|
| apt edit-sources| |	编辑源列表|