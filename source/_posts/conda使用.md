---
title: conda使用
abbrlink: d6d0
date: 2022-06-18 11:19:52
tags:
password:
---

* 查看源

  ~~~bash
  conda config --show-sources
  ~~~



* 添加仓库

  ~~~bash
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
  
  
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  
  # 意思是从channel中安装包时显示channel的url，这样就可以知道包的安装来源
  conda config --set show_channel_urls yes
  ~~~

  

* 添加第三方源

  ~~~bash
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/bioconda/
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/menpo/
  conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
  ~~~



* 清除索引缓存，保证用的是镜像站提供的索引

  ~~~bash
  conda clean -i
  ~~~

  



* 查看有哪些虚拟环境

  ~~~bash
  conda env list
  ~~~

  



* 创建虚拟环境

  ~~~bash
  # 创建一个名为py34的环境，指定Python版本是3.5（不用管是3.5.x，conda会为我们自动寻找3.５.x中的最新版本）
  conda create --name py35 python=3.5
  ~~~



* 复制某个虚拟环境

  ~~~bash
  conda create --name new_env_name --clone old_env_name 
  在安装前的确认[Y/N]的时候，false表示由用户再做决定，而不直接进行： 
  ~~~





* 激活虚拟环境（进入虚拟环境）

  ~~~bash
  conda activate py35
  ~~~

* * 如果第一次激活虚拟环境，可能报以下错误：

  * ~~~bash
    CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
    If using 'conda activate' from a batch script, change your
    invocation to 'CALL conda.bat activate'.
    
    To initialize your shell, run
    
        $ conda init <SHELL_NAME>
    
    Currently supported shells are:
      - bash
      - cmd.exe
      - fish
      - tcsh
      - xonsh
      - zsh
      - powershell
    
    See 'conda init --help' for more information and options.
    
    IMPORTANT: You may need to close and restart your shell after running 'conda init'.
    ~~~

  * 解决：按照提示，我们应该执行：

  * ~~~bash
    CALL conda.bat activate
    ~~~





* 退出当前环境

  ~~~bash
  conda deactivate
  ~~~

  



* 删除某个虚拟环境

~~~bash
# -n即--name
conda remove -n py35 --all
~~~







* 更新conda

~~~bash
conda update -n base -c defaults conda
~~~







* 清理没用的包

  ~~~bash
  conda clean -p
  ~~~

  

* 删除tar包

  ~~~bash
  conda clean -t  
  ~~~

  



* 删除所有的安装包及cache

  ~~~bash
  conda clean -y --all 
  ~~~

  
