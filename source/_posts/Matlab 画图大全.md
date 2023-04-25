---
abbrlink: '0'
---
### figure

* 生成一张新的画布





# 二维

## plot

* matlab 绘图时 , 先绘制 Marker（标记点） , 然后再将所有的 Marker（标记点） 连接起来 ;
* 参数说明：
* * 'MarkerSize'：标记点的大小
  * 'MarkerEdgeColor'：连接标记点的线的颜色
  * 'MarkerFaceColor'：标记点的颜色
  * 'LineWidth'：连接标记点的线的大小

### 函数形式

```matlab
plot(X,Y);
plot(X,Y,LineSpec);
plot(X1,Y1,...,Xn,Yn);
plot(X1,Y1,LineSpec1,...,Xn,Yn,LineSpecn);
plot(Y);
plot(Y,LineSpec);
plot(___,Name,Value);
plot(ax,___);
h = plot(___);
plot(x,y,'--gs',...
    'LineWidth',2,...
    'MarkerSize',10,...
    'MarkerEdgeColor','b',...
    'MarkerFaceColor',[R, G, B]); % 0 <= R, G, B <= 1
```

### 具体属性

#### 线

| '-'  |  实线  |
| :--: | :----: |
| ':'  |  虚线  |
| '-.' | 点化线 |
| '--' | 双划线 |

#### 颜色

* 颜色可以这样表示：'color', [R, G, B];
* * 0 <= R, G, B <= 1

| 'k'  | 黑色   |
| ---- | ------ |
| 'b'  | 蓝色   |
| 'c'  | 蓝绿色 |
| 'g'  | 绿色   |
| 'm'  | 洋红色 |
| 'r'  | 红色   |
| 'w'  | 白色   |
| 'y'  | 黄色   |

#### 数据点

| '*'  | 星号           |
| ---- | -------------- |
| 'o'  | 圆圈           |
| 's'  | 方块           |
| 'p'  | 五角星         |
| '^'  | 朝上三角形符号 |
| 'X'  | 叉             |
| '+'  | 加号           |
| 'd'  | 菱形           |
| 'v'  | 朝下三角形符号 |
| '<'  | 朝左三角形符号 |
| '>'  | 朝右三角形符号 |
| 'H'  | 方角形         |





## fplot

#### 函数形式

```matlab
fplot(f)
fplot(f,xinterval)
fplot(funx,funy)
fplot(funx,funy,tinterval)
fplot(___,LineSpec)
fplot(___,Name,Value)
fplot(ax,___)
fp = fplot(___)
[x,y] = fplot(___)
```

## ezplot

~~~tex
ezplot即：Easy to use function plotter。它是一个易用的一元函数绘图函数 。特别是在绘制含有符号变量的函数的图像时，ezplot要比plot更方便。因为plot绘制图形时要指定自变量的范围，而ezplot无需数据准备，直接绘出图形。
~~~

* ezplot(fun)
* ezplot(fun,[xmin,xmax])
* ezplot(fun2)
* ezplot(fun2,[xymin,xymax])
* ezplot(fun2,[xmin,xmax,ymin,ymax])
* ezplot(funx,funy)
* ezplot(funx,funy,[tmin,tmax])
* ezplot(...,figure_handle)
* ezplot(axes_handle,...)
* h = ezplot(...)

### 函数说明

~~~tex
1、ezplot(fun)在默认区间-2π< x < 2π 绘制函数fun(x)的图像,其中fun(x)是x的一个显函数。fun可以是一个函数句柄或者字符。
2、ezplot(fun,[xmin,xmax])在区间 xmin <x< xmax 绘制函数fun(x)。
3、对于一个隐函数fun2(x,y)：
ezplot(fun2)在默认区间-2π <x<2π, -2π <y< 2π 绘制fun2(x,y)=0。
4、ezplot(fun2,[xymin,xymax]) 在xymin < x < xymax和xymin < y < xymax 范围内绘制fun2(x,y)=0图像。
5、ezplot(fun2,[xmin,xmax,ymin,ymax]) 在xmin < x < xmax和ymin < y < ymax 范围内绘制fun2(x,y)=0图像。
6、ezplot(funx,funy)在默认区间0 < t < 2π 绘制参数定义的平面曲线funx(t)和funy(t).
7、ezplot(funx,funy,[tmin,tmax])在默认区间tmin < t < tmax绘制参数定义的平面曲线funx(t)和funy(t).
8、ezplot(...,figure_handle)在句柄图像定义的图像窗口绘制特定区间的给定函数图像。
9、ezplot(axes_handle,...)用坐标轴句柄绘制而不是当前坐标轴句柄(gca)绘制函数图像。
10、h = ezplot(...)返回所有绘制图像的句柄。
~~~









## bar

* 画柱状图



## line

* 在画布中画一些辅助线
* line([起点横坐标，终点横坐标],[起点纵坐标，终点纵坐标])

```matlab
line(x,y)
line(x,y,z)
line
line('XData',x,'YData',y)
line('XData',x,'YData',y,'ZData',z)
line(___,Name,Value)
line(ax,___)
pl = line(___)
```









# 三维



### plot3

* 画三维曲线，类似plot

#### 函数形式

* X1、Y1、Z1：是长度相同的向量（存储对应的坐标）

~~~matlab
plot3(X1,Y1,Z1,...)
plot3(X1,Y1,Z1,LineSpec,...)
plot3(...,'PropertyName',PropertyValue,...)
plot3(ax,...)
h = plot3(...)
~~~



## fplot3

* 类似fplot

![1](http://blog.axieyun.top/img/26.png)

**代码**

~~~matlab
x = @(t) sin(t);
y = @(t) cos(t);
z = @(t) t*sin(t)*cos(t);
figure;
fplot3(x, y, z);
~~~

![1](http://blog.axieyun.top/img/27.png)



### stem3

* 常用的三维火柴杆图



#### 例子



##### 1

**代码**

~~~
clear; clc; close all;
%%
t=0:pi/30:10*pi; %设定t的范围
plot3(cos(t),sin(t),t,'-b','LineWidth',4); %绘制三维曲线，并且做修饰
grid on; %加网格
axis square; %命令坐标为方形
figure; %新建图形窗口
stem3(cos(t),sin(t),t,'-.g') %绘制三维火柴杆图
grid off;
~~~

**效果**

![题目](http://blog.axieyun.top/img/15.png)

![题目](http://blog.axieyun.top/img/16.png)



### mesh

* 常用的网线图调用格式
* [网格曲面图 - MATLAB mesh - MathWorks 中国](https://ww2.mathworks.cn/help/releases/R2020a/matlab/ref/mesh.html?searchHighlight=mesh&s_tid=doc_srchtitle#mw_521c8710-b7e5-445a-8712-cf38e30281b4)

#### 函数形式

* 参数说明：

~~~matlab
mesh(X,Y,Z)
mesh(Z)
mesh(...,C)
mesh(...,'PropertyName',PropertyValue,...)
mesh(axes_handles,...)
s = mesh(...)
~~~

#### 使用步骤

##### 1、生成二维网格矩阵

~~~matlab
x = -1:0.1:1;
y = -1:0.1:1;
[X, Y] = meshgrid(x, y);
~~~

##### 画图

~~~matlab
clear; clc; close all;
%%
x = -1:0.1:1;
y = -1:0.1:1;
[X, Y] = meshgrid(x, y);
R = sqrt(X.^2 + Y.^2) + eps;
Z = sin(R)./R;
mesh(X,Y,Z);
~~~

#### 例子

##### 1

**代码**

~~~matlab
clear; clc; close all;
%%
[X,Y] = meshgrid(-8:.5:8); % -8 到 8 之间没0.5取一个点，步长为0.5
R = sqrt(X.^2 + Y.^2) + eps;
Z = sin(R)./R;
mesh(X,Y,Z);
~~~

**效果**

![题目](http://blog.axieyun.top/img/17.png)



### surf

* 常用的曲面图调用格式
* 使用方式同mesh
* [网格曲面图 - MATLAB mesh - MathWorks 中国](https://ww2.mathworks.cn/help/releases/R2020a/matlab/ref/mesh.html?searchHighlight=mesh&s_tid=doc_srchtitle#mw_521c8710-b7e5-445a-8712-cf38e30281b4)

### contour

* 常用的的等高线调用格式

#### 函数形式

~~~matlab
contour(Z)
contour(X,Y,Z)
contour(___,levels)
contour(___,LineSpec)
contour(___,Name,Value)
contour(ax,___)
M = contour(___)
[M,c] = contour(___)
~~~





#### 例子

##### 1

**代码**

*将 `Z` 定义为 `X` 和 `Y` 的函数。在本例中，调用 `peaks` 函数以创建 `X`、`Y` 和 `Z`。然后绘制 `Z` 的 20 个等高线。*

~~~matlab
clear; clc; close all;
%%
colormap jet;
[X,Y,Z] = peaks;
contour(X,Y,Z,20);
~~~

**效果**

![题目](http://blog.axieyun.top/img/18.png)



### contourf

* 填充的二维等高线图
* 使用方式同contour

#### **linspace函数**

* ```matlab
  y = linspace(x1,x2,n) %用于在两点间下x1, x2均匀产生n个点，默认是100个点。
  ```



##### 例子

**代码**

~~~matlab
clear; clc; close all;
%%
colormap jet;
x = linspace(0,10,100); % 用于在两点间均匀产生n个点，默认是100个点。
y = linspace(0,10,100); 
[X, Y] = meshgrid(x,y);
Z = cos(5*X/pi) + sin(5*Y/pi);
subplot(1,2,1);
contour(X, Y, Z, 7); %7条等高线
subplot(1,2,2);
contourf(X, Y, Z, 14);%14条等高线
~~~

**效果**

![题目](http://blog.axieyun.top/img/19.png)

**代码**

~~~matlab
clear; clc; close all;
%%
colormap jet;
x = linspace(0,10,100);
y = linspace(0,10,100);
[X, Y] = meshgrid(x,y);
Z = cos(5*X/pi) + sin(5*Y/pi);
subplot(1,2,1)
contour(X, Y, Z, [-0.5 0.5])
subplot(1,2,2)
contourf(X, Y, Z, [-0.5 0.5])

我们可以控制显示区间，例如这里只显示[-0.5 0.5]之间的数据，大于0.5位黄色，小于-0.5白色，深蓝色为中间值。
~~~

**效果**

![题目](http://blog.axieyun.top/img/20.png)

#### 其他等高线函数

~~~tex
contour-矩阵的等高线图
contourf-填充的二维等高线图
contourc-低级等高线图计算
contour3-三维等高线图
contourslice-在三维体切片平面中绘制等高线
clabel-为等高线图添加高程标签
fcontour-绘制等高线
~~~

### surfc

* **同一图中，既表现曲面，又绘制等高线**
* 使用方式同mesh



### meshgrid

* 格点矩阵生成函数，只适用于二维、三维



#### 生成二维矩阵

~~~matlab
x = -1:0.1:1;
y = -1:0.1:1;
[X, Y] = meshgrid(x,y);
~~~

#### 生成三维

~~~matlab
x = -1:0.1:1;
y = -1:0.1:1;
z = -1:0.1:1;
[X, Y, Z] = meshgrid(x,y,z);
~~~



### ndgrid

* 格点矩阵生成函数，适用于n维









# 动画







# 其他

## 修饰图片

### legend

* 增加图例



### title

* 给图像增加标题



### xlabel

* 给横坐标轴取名字



### ylabel

* 给纵坐标轴取名字



### zlabel

* 给z坐标轴取名字



### text

* 在图像上显示文本





## 修改坐标轴显示范围

### axis

* 修改坐标轴范围



### xlim

* 修改横坐标轴显示范围



### ylim

* 修改横坐标轴显示范围

### zlim

* 修改z坐标轴显示范围

### rlim

* 修改r坐标轴显示范围



### subplot

* 一张画布画多张图



## 其他的



### view



### grid

~~~matlab
grid on; % 开启网格
grid off; %关闭网格
~~~



### hold

~~~matlab
hold on; % 在一张图片画多条曲线
hold off; % 关闭 在一张图片画多条曲线
~~~









*参考*

[(57条消息) 【matlab】函数meshgrid的用法详解(生成网格矩阵)和ndgrid的区别及用法_Treysure的博客-CSDN博客_meshgrid](https://blog.csdn.net/u013346007/article/details/54581253?ops_request_misc=&request_id=&biz_id=102&utm_term=matlab mesh&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-4-54581253.142^v9^control,157^v4^control&spm=1018.2226.3001.4187)

