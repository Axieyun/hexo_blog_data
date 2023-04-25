---
abbrlink: '0'
---
* 第一步：创建镜像

~~~bash
sudo dd if=/dev/zero of=/root/disk1.img bs=4M count=10    //4M*10 = 40M, zero 是dev下的文件，创建镜像
~~~

* 第二步：挂载硬盘为/dev/loop31

~~~bash
sudo losetup /dev/loop31 /root/disk1.img
~~~

* 第三步：格式化文件系统

~~~bash
sudo mkfs.ext3 /dev/loop31
~~~

* 第四步：在当前目录创建文件夹

~~~bash
sudo mkdir ./test1   
~~~

* 第五步：挂载硬盘，./test1目录的容量为40M

~~~bash
sudo mount -t ext3 /dev/loop31 ./test1
~~~

