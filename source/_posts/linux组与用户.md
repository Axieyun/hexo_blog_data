---
abbrlink: '0'
---
### 组操作

#### 创建组

* 增加一个test组

~~~bash
groupadd test
~~~

#### 修改组

* 将test组的名子改成test2

~~~bash 
groupmod -n test2 test
~~~

#### 删除组

* 删除 组test2，注意：不能移除用户test2的主组test2

~~~bash
groupdel test2
~~~

#### 查看组

* 查看当前登录用户所在的组 groups，查看用户：apacheuser所在的组

  ~~~bash
  groups apacheuser
  ~~~

* 查看所有组 

  ~~~bash
  bashcat /etc/group
  ~~~

* 有的linux系统没有/etc/group文件的，这个时候看下面的这个方法

  ~~~bash
  cat /etc/passwd |awk -F [:] '{print $4}' |sort|uniq | getent group |awk -F [:] '{print $1}'
  ~~~

* * 这里用到一个命令是getent,可以通过组ID来查找组信息,如果这个命令没有的话,那就很难查找,系统中所有的组了.



### 用户操作

#### 增加用户

* 增加用户test，有一点要注意的，useradd增加一个用户后，不要忘了给他设置密码，不然不能登录的。

~~~bash
useradd test
passwd test
~~~





#### 修改用户

* 将test用户的登录目录改成/home/test，并加入test2组，注意这里是大G。

  ~~~bash
  usermod -d /home/test -G test2 test
  ~~~

  

* 将用户test加入到test2组

  ~~~bash
  gpasswd -a test test2
  ~~~

  

* 将用户test从test2组中移出

  ~~~bash
  gpasswd -d test test2
  ~~~

  

#### 删除用户

* 将test用户删除

  ~~~bash
  userdel test
  ~~~



#### 查看用户



* 查看当前登录用户

  ~~~bash
  w
  who
  ~~~

* 查看自己的用户名

  ~~~bash
  whoami
  ~~~

* 查看用户: apacheuser信息

  ~~~bash
  id apacheuser
  ~~~

* 查看用户登录记录

* * 查看登录成功的用户记录

    ~~~bash
    last
    ~~~

  * 查看登录不成功的用户记录

    ~~~bash
    lastb
    ~~~

* 查看所有用户

  ~~~bash
  cut -d : -f 1 /etc/passwd
  cat /etc/passwd |awk -F \: '{print $1}'
  ~~~

  

