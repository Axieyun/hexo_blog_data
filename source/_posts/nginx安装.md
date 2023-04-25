---
abbrlink: '0'
---
### 环境准备

#### 1、安装[openssl](https://www.openssl.org/source/old/1.1.1/)

* OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，
* 并提供丰富的应用程序供测试或其它目的使用。
* nginx不仅支持http协议，还支持https（即在ssl协议上传输http），所以需要在linux安装openssl库。

~~~bash
cd /usr/local/src

sudo wget https://www.openssl.org/source/openssl-1.1.1l.tar.gz

sudo tar -zxvf openssl-1.1.1l.tar.gz

cd openssl-1.1.1l

sudo ./config

sudo make && make install
~~~



#### 2、安装

* **pcre-devel是使用pcre开发的一个二次开发库。nginx也需要此库**。

~~~bash
cd /usr/local/src

sudo wget https://jaist.dl.sourceforge.net/project/pcre/pcre/8.45/pcre-8.45.tar.gz

cd pcre-8.45

sudo ./configure

sudo make && make install
~~~



#### 3、安装zlib

* zlib库提供了很多种压缩和解压缩的方式，nginx使用zlib对http包的内容进行gzip，所以需要在linux上安装zlib库。

~~~bash
cd /usr/local/src


sudo wget https://jaist.dl.sourceforge.net/project/libpng/zlib/1.2.11/zlib-1.2.11.tar.gz


sudo tar -zxvf zlib-1.2.11.tar.gz

cd zlib-1.2.11/

sudo ./configure

sudo make

sudo make install
~~~





### 安装



#### 源码下载

~~~bash
wget http://nginx.org/download/nginx-1.18.0.tar.gz
~~~

#### 解压

~~~bash
tar -zxvf nginx-1.18.0.tar.gz
~~~

#### 编译安装

~~~bash
cd nginx-1.18.0/

sudo ./configure --prefix=/usr/local/nginx --with-pcre --with-threads --with-file-aio --with-http_ssl_module --with-http_stub_status_module --with-pcre --with-http_gzip_static_module --add-module=/home/axieyun_aly_kkb/ngx_brotli --with-http_v2_module

# 安装了两次，两次都不同，原理一样
sudo ./configure \                                                 
--prefix=/usr/local/nginx \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi \
--with-http_stub_status_module \
--with-http_ssl_module \
--with-file-aio \
--with-http_realip_module \
--with-openssl=/usr/local/src/openssl-1.1.1l


make
make install
~~~

* --prefix：安装目录
* threads：激活线程池
* file-aio：文件io
* http_ssl_module：ssl模块   
* http_stub_status_module：状态监控模块

* --add-module=/home/axieyun_aly_kkb/ngx_brotli：增加brotli模块（自己安装的）



#### 查看nginx编译配置

~~~bash
/usr/local/nginx/sbin/nginx -V
~~~







### 卸载

#### 1、停止nginx

~~~bash
/usr/local/nginx/sbin/nginx -s stop
~~~

#### 2、查找根下所有名字包含nginx的文件

~~~bash
find / -name nginx
~~~

#### 3、执行rm -rf删除所有相关文件

#### 4、设置开机自启动的需要执行

~~~bash
chkconfig nginx off
rm -rf /etc/init.d/nginx
~~~







### nginx服务器名称修改

*修改为Axieyun*

~~~bash
#执行更名操作
sed -i "s#\"NGINX\"#\"Axieyun\"#" src/core/nginx.h
sed -i "s#\"nginx/\"#\"Axieyun/\"#" src/core/nginx.h
sed -i "s#Server: nginx#Server: Axieyun#" src/http/ngx_http_header_filter_module.c
sed -i "s#\"<hr><center>nginx<\/center>\"#\"<hr><center>Axieyun<\/center>\"#"src/http/ngx_http_special_response.c
~~~

#### 隐藏版本号

* 在http模块加上

~~~tex
server_token off;
~~~



#### 参考文献

* [Nginx修改服务名称任意名字 - 云+社区 - 腾讯云 (tencent.com)](https://cloud.tencent.com/developer/article/1696133)





### 错误日志

#### 问题1

~~~tex
./configure: error: the HTTP rewrite module requires the PCRE library.
You can either disable the module by using --without-http_rewrite_module
option, or install the PCRE library into the system, or build the PCRE library
statically from the source with nginx by using --with-pcre=<path> option.
~~~

##### 原因：
* 缺少pcre和pcre-devel

##### 解决：

~~~bash
./configure --prefix=/usr/local/nginx --with-pcre=/usr/local/pcre
~~~

#### 问题2
~~~tex
./configure: error: the HTTP XSLT module requires the libxml2/libxslt
libraries. You can either do not enable the module or install the libraries.
~~~

##### 解决：
~~~bash
apt-get install libxml2 libxml2-dev libxslt-dev
~~~



#### 问题3：

~~~tex
./configure: error: the HTTP image filter module requires the GD library.
You can either do not enable the module or install the libraries.
~~~

##### 解决：

~~~bash
sudo apt update
sudo apt upgrade
sudo apt full-upgrade -y libgd-dev
~~~







### nginx操作

#### 启动服务

* /usr/local/nginx/sbin/nginx 

#### 停止服务

* /usr/local/nginx/sbin/nginx  -s stop

#### 重新加载配置

*  /usr/local/nginx/sbin/nginx  -s reload

#### 测试配置文件是否正确
* /usr/local/nginx/sbin/nginx -t

#### 查看端口占用情况

~~~bash
netstat -ntlp|grep 80
~~~

