---
title: zsh插件
tags: linux
abbrlink: '8569'
date: 2022-03-13 16:49:27
password:
---


#### Zsh命令自动补全插件 zsh-autosuggestions

* 1、安装命令
* github的gitee的两个挑一个
~~~bash
git clone https://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
~~~
~~~shell
git clone https://gitee.com/phpxxo/zsh-autosuggestions.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
~~~

* 2、配置
* * 1、打开文件

~~~bash
vim ~/.zshrc
~~~
* * 2、在~/.zshrc文件中找到plugins数组，加入zsh-autosuggestions


#### Zsh命令语法高亮插件 zsh-syntax-highlighting


* 1、命令
~~~bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
~~~
* 2、配置
* * 1、打开文件
~~~bash
vim ~/.zshrc
~~~
* * 2、在~/.zshrc文件中找到plugins数组，在 zsh-autosuggestions 后面加入zsh-syntax-highlighting（必须是后面）

