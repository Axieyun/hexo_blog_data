---
abbrlink: '0'
---




## 闫氏DP分析法



### 核心

* 从集合角度分析DP问题

#### 思想

* 求有限集合中的最值







* 化零为整：状态表示：集合表示
* * 集合：表示什么
  * 属性：集合的关系：max/min/count
* 化整为零：状态计算：分成子集
* * 子集：
  * * 满足条件：不遗漏
  * 子集合划分依据：找最后一个不同点









### 选择DP

* 状态表示：f(i, j)
* * 第一个参数一般表示：只考虑前i个
  * 第二个参数一般表示：限制条件，数量限制、体积限制、重量限制、...。

#### 01背包

* 状态定义：dp(i, j) ：前i个物品，在体积为j时的最大价值
* * 属性最大值

* 状态计算：满足不重复性
* * 子集1：对于第i个物品不选
* * 子集2：对于第i个物品    选（在体积够大，即 j>=vi 才能选择）

* * 那么：dp(i, j) = max(dp(i - 1, j), dp(i - 1, j - vi) + wi);

##### 优化

* 观察状态计算公式：i只与i - 1有关，那么可以改用滚动数组
* 再观察j，j只与第 i - 1 层的 j - v 有关，即只与i - 1层的小于j的j - v有关。



##### 01背包

[01背包](https://www.acwing.com/problem/content/2/)



##### 题解

*朴素代码*

~~~c++
#include <algorithm>
#include <iostream>
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

using namespace std;

int N, V;
int v[1002], w[1002];
int dp[1002][1002];

int main() {
    cin >> N >> V;
    for (int i = 1; i <= N; ++i) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= N; ++i) {
        for (int j = 0; j <= V; ++j) {
            // if (j >= v[i]) {
            //     dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - v[i]] + w[i]);
            // } else {
            //     dp[i][j] = dp[i - 1][j];
            // }
            
            //优化1
            dp[i][j] = dp[i - 1][j];
            // 因为 dp[i][j] = dp[i - 1][j];
            if (j >= v[i]) dp[i][j] = max(dp[i][j], dp[i - 1][j - v[i]] + w[i]);
        }
    }
    cout << dp[N][V] << endl;
    
    return 0;
}
~~~



*优化*

~~~c++
#include <algorithm>
#include <iostream>
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

using namespace std;

int N, V;
int v[1002], w[1002];
int dp[1002];

int main() {
    cin >> N >> V;
    for (int i = 1; i <= N; ++i) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= N; ++i) {
        for (int j = V; ~j; --j) {
            // if (j >= v[i]) {
            //     dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - v[i]] + w[i]);
            // } else {
            //     dp[i][j] = dp[i - 1][j];
            // }
            
            //优化1
            dp[j] = dp[j]; // 恒等于 dp[i][j] = dp[i - 1][j]; 因为：赋值的 是上一层的j
            // 因为 dp[i][j] = dp[i - 1][j];
            
            //下面一行代码 恒等于 dp[i][j] = max(dp[i][j], dp[i - 1][j - v[i]] + w[i]);
            if (j >= v[i]) dp[j] = max(dp[j], dp[j - v[i]] + w[i]); //当j到过来，dp[j - v[i]]是i - 1层的，且未更新过，如果没有到过来，那么dp[j - v[i]]是i层的，且更新过的
        }
    }
    cout << dp[V] << endl;
    
    return 0;
}





#include <algorithm>
#include <iostream>
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

using namespace std;

int N, V;
int v[1002], w[1002];
int dp[1002];

int main() {
    cin >> N >> V;
    for (int i = 1; i <= N; ++i) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= N; ++i) {
        for (int j = V; j >= v[i]; --j) {
            // if (j >= v[i]) {
            //     dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - v[i]] + w[i]);
            // } else {
            //     dp[i][j] = dp[i - 1][j];
            // }
            
            //优化1
            //dp[j] = dp[j]; // 恒等于 dp[i][j] = dp[i - 1][j]; 因为：赋值的 是上一层的j
            // 因为 dp[i][j] = dp[i - 1][j];
            
            //下面一行代码 恒等于 dp[i][j] = max(dp[i][j], dp[i - 1][j - v[i]] + w[i]);
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]); //dp[j - v[i]]是i - 1层的，且未更新过且
        }
    }
    cout << dp[V] << endl;
    
    return 0;
}
~~~



##### 完全背包

[完全背包](https://www.acwing.com/problem/content/3/)



* 状态表示：f(i, j)：前i个物品体积不超过j的最大值
* * 属性：最大值
* 状态计算：满足不重复性
* 集合划分：对于每个物品可以不限制的选择
* * 集合0：对于第i个物品，不选择
  * 集合1：对于第i个物品，选择1次
  * 集合2：对于第i个物品，选择2次
  * 集合3：对于第i个物品，选择3次
  * ...
* 那么dp(i, j) = max(dp(i - 1, j), dp(i - 1, j - vi) + wi, dp(i - 1, j - 2*vi) + 2*wi, ...)，也就是dp(i, j) = max(集合0的值，集合1的值，集合2的值，...)

###### 优化:令j = j - vi

* 那么：dp(i, j - vi) = max(dp(i - 1, j - vi), dp(i - 1, j - 2vi) + wi, dp(i - 1, j - 3vi) + 2*wi, ...)

* 而：max(dp(i - 1, j - vi) + wi, dp(i - 1, j - 2*vi) + 2*wi, ...) = max(dp(i - 1, j - vi), dp(i - 1, j - 2vi) + wi, dp(i - 1, j - 3vi) + 2*wi, ...) + wi = dp(i, j - vi) + wi
* 那么：dp(i, j) = max(dp(i - 1, j), dp(i, j - vi) + wi);







##### 题解



###### 朴素

~~~c++
/*
01: dp(i, j) = max(dp(i - 1, j), dp(i - 1, j - vi) + wi)
完全：dp(i, j) = max(dp(i - 1, j), dp(i, j - vi) + wi)
*/
#include <algorithm>
#include <iostream>
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

using namespace std;

const int nn = 1005;

int N, V;
int dp[nn][nn];
int v[nn], w[nn];

int main() {
    cin >> N >> V;
    
    for (int i = 1; i <= N; ++i) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= N; ++i) {
        for (int j = 1; j <= V; ++j) {
            // if (j >= v[i]) dp[i][j] = max(dp[i - 1][j], dp[i][j - v[i]] + w[i]);
            // else dp[i][j] = dp[i - 1][j];
            
            
            dp[i][j] = dp[i - 1][j];
            if (j >= v[i]) dp[i][j] = max(dp[i - 1][j], dp[i][j - v[i]] + w[i]);
            
            
            
            
        }
    }
    
    cout << dp[N][V] << endl;
    
    return 0;
}
~~~







###### 优化空间

~~~c++

/*
01: dp(i, j) = max(dp(i - 1, j), dp(i - 1, j - vi) + wi)
完全：dp(i, j) = max(dp(i - 1, j), dp(i, j - vi) + wi)
*/
#include <algorithm>
#include <iostream>
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

using namespace std;

const int nn = 1005;

int N, V;
// int dp[nn][nn];
int dp[nn];
int v[nn], w[nn];

int main() {
    cin >> N >> V;
    
    for (int i = 1; i <= N; ++i) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= N; ++i) {
        for (int j = 1; j <= V; ++j) {
            // if (j >= v[i]) dp[i][j] = max(dp[i - 1][j], dp[i][j - v[i]] + w[i]);
            // else dp[i][j] = dp[i - 1][j];
            
            
            // dp[i][j] = dp[i - 1][j];
            // if (j >= v[i]) dp[i][j] = max(dp[i][j], dp[i][j - v[i]] + w[i]);
            
            dp[j] = dp[j];
            if (j >= v[i]) dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
            
            
        }
    }
    
    // cout << dp[N][V] << endl;
    cout << dp[V] << endl;
    
    return 0;
}



/*
01: dp(i, j) = max(dp(i - 1, j), dp(i - 1, j - vi) + wi)
完全：dp(i, j) = max(dp(i - 1, j), dp(i, j - vi) + wi)
*/
#include <algorithm>
#include <iostream>
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

using namespace std;

const int nn = 1005;

int N, V;
// int dp[nn][nn];
int dp[nn];
int v[nn], w[nn];

int main() {
    cin >> N >> V;
    
    for (int i = 1; i <= N; ++i) cin >> v[i] >> w[i];
    
    for (int i = 1; i <= N; ++i) {
        for (int j = v[i]; j <= V; ++j) {
            // if (j >= v[i]) dp[i][j] = max(dp[i - 1][j], dp[i][j - v[i]] + w[i]);
            // else dp[i][j] = dp[i - 1][j];
            
            
            // dp[i][j] = dp[i - 1][j];
            // if (j >= v[i]) dp[i][j] = max(dp[i][j], dp[i][j - v[i]] + w[i]);
            
            dp[j] = max(dp[j], dp[j - v[i]] + w[i]);
            
            
        }
    }
    
    // cout << dp[N][V] << endl;
    cout << dp[V] << endl;
    
    return 0;
}
~~~







### 区间DP



#### 题目



##### 合并石子

[合并石子](https://www.acwing.com/problem/content/284/)



* 状态表示：f(i, j)：将第[i, j]区间的石子合并为一堆石子的最小代价
* * 属性：最小代价-min
* 状态计算：f(i, j) = min(f(i, k), f(k + 1, j))







##### 题解



~~~c++
#include <algorithm>
#include <iostream>
#include <stdio.h>
#include <string.h>
#include <math.h>
#include <stdlib.h>

const int N = 305;

int n;
int s[N]; // 前缀和
int f[N][N];
#define INF 0x7fffffff

using namespace std;

int main() {
    
    cin >> n;
    for (int i = 1; i <= n; ++i) cin >> s[i], s[i] += s[i - 1];
    
    for (int len = 2; len <= n; ++len) { //枚举区间长度
        for (int l = 1; l + len - 1 <= n; ++l) { //枚举区间左端点
            int r = l + len - 1; //获得右端点
            f[l][r] = INF;
            for (int k = l; k < r; ++k) { //合并区间
                f[l][r] = min(f[l][r], f[l][k] + f[k + 1][r] + s[r] - s[l - 1]);
            }
        }
    }
    cout << f[1][n] << endl;
    return 0;
}
~~~







#### 最长重复子序列





* 状态表示：f(i, j): 在串a区间为[1, i]和串b区间为[1, j]的最长重复子序列长度

* * 属性：max

* 状态计算：f(i, j) = max(f(i - 1, j), f(i, j - 1), f(i - 1, j - 1) + 1)，ai == bj，才存在f(i -1, j - 1) + 1

* * 当计算到ai与bj时，ai与bj有4种表示 00, 01, 10, 11;分别表示ai与bj不选，ai选与bj不选，ai不选与bj选，ai与bj都选。
  * 有因为 f(i - 1, j)和f(i, j - 1)都包含了f(i - 1, j - 1)，那么我们可以忽略f(i - 1, j - 1)
  * 可以重复不漏原则

* * ~~~c++
    f(i, j) = max(f(i - 1, j), f(i, j - 1));
    if (ai == bj) f(i, j) = max(f(i, j), f(i - 1, j - 1) + 1);
    ~~~









