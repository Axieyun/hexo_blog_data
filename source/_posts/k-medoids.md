---
title: k-medoids
abbrlink: 657a
date: 2022-06-07 22:28:57
tags:
password:
---







**k-medoids又称PAM（**Partitioning Around Medoids**）**



### 优点

* 优点：当存在噪音和孤立点时, K-medoids 比 K-means 更健壮。





### 缺点

* K-medoids 对于小数据集工作得很好, 但不能很好地用于大数据集，计算质心的步骤时间复杂度是O(n^2)，运行速度较慢
* 对于大样本，使用[**CLARA**](http:blog.axieyun.top/html/CLARA.html)







### 具体描述

* 初始随机选择k个对象作为中心点，并代表初始簇，然后根据欧式距离划分其余所有对象到各个中心点所代表的簇，得到初始簇划分。
* 该算法反复利用数据中所有非代表对象替换当前代表对象，试图找出更好的中心点，以改进聚类质量。==中心点==也叫代表对象，其他对象叫非代表对象。
* 在每次迭代中，所有可能的对象都被分析，每一对替换中的一个对象是中心点，而另一个是非对象。如果一个当前的中心点被一个非对象所替换，代价函数将计算平方误差值所产生的差别；替换的总代价是所有非对象去替换所产生的代价之和。如果总代价为负值，则实际的平方误差将会减少，则代表对象可被非代表对象替换。
* ==中心点==：簇中某点的平均差异性在这一簇中所有点中最小。







### 算法描述



#### 输入

* k：划分簇（类）的数目
* N：包含N个对象的数据



#### 输出

* k个簇，使得所有对象与其距离最近的中心点的差异度总和最小。
* 1、初始化：随机选择N个点中的k个点作为初始中心点；
* 2、将其余各个点根据欧式距离划分到这k个类中；
* 3、当损失值减少时：
* * 对于每个中心点o，对于每个非中心点m：
  * * 交换o和m，重新计算损失（损失值为：所有点到中心点的距离之和）；
    * 如果总的损失增加则不进行交换









**kMedoids.m代码：**

~~~matlab
function [cx,cost] = kMedoids(K,data,num)
%   生成将data聚成K类的最佳聚类
%   K为聚类数目，data为数据集,num为随机初始化次数
    [cx,cost] = kMedoids1(K,data);
    for i = 2:num
        [cx1,min] = kMedoids1(K,data);
        if min<cost
            cost = min;
            cx = cx1;
        end
    end
end

function [cx,cost] = kMedoids1(K,data)
%   把分类数据集data聚成K类
%   [cx,cost] = kmeans(K,data)
%   K为聚类数目，data为数据集
%   cx为样本所属聚类，cost为此聚类的代价值
% 选择需要聚类的数目

% 随机选择聚类中心
    centroids = data(randperm(size(data,1),K),:);
% 迭代聚类 
    centroids_temp = zeros(size(centroids));
    num = 0;
    while (~isequal(centroids_temp,centroids)&&num<20) 
        centroids_temp = centroids;
        [cx,cost] = findClosest(data,centroids,K);
        centroids = compueCentroids(data,cx,K);
        num = num+1;
    end
%     cost = cost/size(data,1);

end


function [cx,cost] = findClosest(data,centroids,K)
% 将样本划分到最近的聚类中心
    cost = 0;
    n = size(data,1);
    cx = zeros(n,1);
    for i = 1:n
%       曼哈顿距离
        [M,I] = min(sum(abs((data(i,:)-centroids))'));
        cx(i) = I;
        cost = cost+M;
    end
end


function centroids = compueCentroids(data,cx,K)
% 计算新的聚类中心
    centroids = zeros(K,size(data,2));
    for i = 1:K
%       寻找代价值最小的当前聚类中心
        temp = data((cx==i),:);
        [~,I] = min(sum(squareform(pdist(temp))));
        centroids(i,:) = temp(I,:);
    end
end
~~~

**main**

~~~matlab
% 主函数

% 生成符合高斯分布的数据
mu = [5,5];
sigma = [16,0;0,16];
sigma1 = [0.5,0;0,0.5];
data =  gaussianSample(8,50,mu,sigma,sigma1);

% 聚类
K = 6;
[cx,cost] = kMedoids(K,data,10);
plotMedoids(data,cx,K);
~~~





























*参考*

* [K-medodis聚类算法MATLAB - 走看看 (zoukankan.com)](http://t.zoukankan.com/lolybj-p-10165417.html)
* 
