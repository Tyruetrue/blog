MD我急了，后面两周的全没了

套路啥的就不会几个。。数据结构先天看不懂圣体

## 待掌握，但必须掌握的东西啊啊啊啊啊啊啊啊啊

- 高维树状数组
- P3401 洛谷树，奇怪写法。。。。
- 矩阵乘法

## luogu:

### P4158
看到一次涂抹只能在一行内进行，考虑对于每个木板分层处理，三维 dp 转移： $dp_{i,j,1/0}$ 表示前 $i$ 个区块，涂抹 $j$ 次，第 $j$ 个位置颜色为 $1/0$ 所能得到的最多正确区块，动态转移对于这一位不同颜色考虑颜色涂抹正确和错误，由前一位相同颜色和不同颜色进行转移 :
$$dp_{i,j,1}=\max\{dp_{i-1,j,1},dp_{i-1,j-1,0}\}+(a_i=1)$$
$$dp_{i,j,0}=\max\{dp_{i-1,j,0},dp_{i-1,j-1,1}\}+(a_i=0)$$

最后进行分组背包即可，一块木板为一组，所用次数重量，区块正确个数为价值

$tips:$ 要注意的是 $dp[][][]$ 的第一维不能滚掉，不严谨的讲，每一次会影响 $dp[][][0]$ 的计算，也会影响下一次 $dp[][][1]$ ，但是分组背包可以滚掉一维

### P4766 
线性不会。区间 $dp$ ，分时间先考虑这样转移 ：

$$dp_{i,j}=\min_{k\in[i,j)}\{dp_{i,k-1}+dp_{k+1,j}\}+\max_{i,(i\le a_i,b_i\le j)} d_i$$

但是这样就有可能导致部分被重叠消灭，考虑怎样才能划分使两部分没有交集

以范围内 $d_i$ 最大值的节点的时间范围为划分依据，证明见题解

### P2899 & P2458

考虑当前节点放以及相邻节点，然而相邻节点包括父亲节点和儿子节点，所以分三种情况进行讨论， $dp_{i,1/2/3}$ 分别表示在他自己节点放置时，儿子，以及父亲。考虑转移：他自己放置可以从儿子的任意一个情况转移过来，**注意：这里有一点，一个节点放置了说明他可以覆盖他的儿子，但是也可以从他儿子被他儿子的儿子状态转过来，因为这两者不冲突** ，其次是他被他的父亲覆盖，可以从他儿子自己有和被他儿子的儿子覆盖转移，最复杂的情况是被他的儿子覆盖，因为可能所有儿子都被孙子节点覆盖，所以需要计算儿子节点更新时哪种情况更优，如果都是孙子节点覆盖更优，需要找到儿子节点放置的最小值进行更新

### P2704

每一次转移会需要包括当前行在内的三行支持，所以存储当前行和上一行，然后枚举前第三行进行转移，通过位运算计算是否有山丘，是否相邻两个间隔 $2$ 个区块，以及是否和上面间隔  $2$ 个区块


### P5933

考虑作差求值，用所有情况（联通和不联通的都有）减去不联通的情况，当集合为 $k$ 时，贡献如下：
$$f_k=\prod_{(u,v)\in k} (c_{u,v}+1)$$

因为也可以选择不连这条边，所以 $c_{u,v}$ 要 $+1$

然后时计算不合法的情况（不连通），考虑取集合内枚举所有子集 $V\subsetneq k$ 并且设 $V$ 和 $\complement_kV$ 之间不连通，然后计算彼此之间所产生的贡献

### P4342

因为是在环上的 $dp$ 所以，考虑直接二倍区间长度，枚举断点，第一眼认为是简单区间 $dp$ ，但是还有负数和乘法运算同时存在，两负数相乘可能值更大，所以既要维护区间最大值，也要有区间最小值，这样转移的时候分四种情况进行转移

加法时 
$$f_{i,j}=\max_{k\in[i,j)}\{f_{i,k}+f_{k+1,j}\},g_{i,j}=\min_{k\in[i,j)}\{g_{i,k}+g_{k+1,j}\}$$
乘法时
$$f_{i,j}=\max_{k\in[i,j)}\{f_{i,k}\times f_{k+1,j},g_{i,k}\times f_{k+1,j},f_{i,k}\times g_{k+1,j},g_{i,k}\times g_{k+1,j}\}$$

$$g_{i,j}=\min_{k\in[i,j)}\{f_{i,k}\times f_{k+1,j},g_{i,k}\times f_{k+1,j},f_{i,k}\times g_{k+1,j},g_{i,k}\times g_{k+1,j}\}$$

直接更新即可

### P3572

$dp$ 优化，考虑需要记录的东西有位置和最小花费

设 $dp_{i}$ 表示当前位置为 $i$ 的花费最小值，考虑转移怎么转移，有个很明显的结论是从高的地方转移过来不需要花费的可能性比从低的地方转移过来要更优，有个错误方法是直接考虑优先队列维护区间高度最高进行转移，但是明显高度高的话转移到高地也是需要更大的花费才行，所以考虑维护 $dp$ 值最小，如果 $dp$ 值相等，再运用那个结论，相等时考虑更高的进行保留

$$dp_i=\min_{k\in[i-x,i)}\{dp_k+[a_k\ge a_i]]\}$$

### P2627

考虑需要维护的信息，有当前位置和最大和，先不考虑记录分段信息，先试着用这两个信息进行动态转移，设 $dp_i$ 表示前 $i$ 个位置的最大和，因为最多有 $k$ 相邻的，所以从 $[i-k+1,i]$ 这段区间进行转移，遂枚举断点 $p\in[i-k,i)$ ，方程为：

$$dp_i=\max_{p\in[i-k,i)}\{dp_p+\sum_{j=p+1}^i val_j\}$$

求和用前缀和优化：

$$dp_i=\max_{p\in[i-k,i)}\{dp_p+sum[p+1,i]\}$$

考虑每次 $i+1$ 都会导致 $p$ 的范围变为 $[i-k+1,i+1)]$ 每次都只会往后扩展一位，所以滑动窗口单调队列优化，因为考虑到对于每个 $i$ 都存在一个断点，所以最一开始的单调队列应该有 $0$ ，否则 $1$ 位置的数值取不到

### P3957

二分 $g$ ，每次 $check$ 用 $dp$ 进行判断
？类似 $P2627$ 做法，记得判断队列非空

### [P3089] 看不懂题解

### P2365

$$t_x=\sum_{i=1}^x t_i$$
$$sum_x=\sum_{i=1}^x f_i$$
$$dp_i=\min_{k=0}^i\{dp_{k}+t_i\times sum[k+1,i]+S\times sum[k+1,n]\}$$
$$dp_i=\min_{k=0}^i\{dp_{k}+t_i\times sum_i-t_i\times sum_k+S\times sum_n-S\times sum_k)\}$$
$$dp_i=\min_{k=0}^i\{dp_{k}-(t_i+S)\times sum_k+t_i\times sum_i+S\times sum_n)\}$$
$$dp_{k}=(t_i+S)\times sum_k-t_i\times sum_i-S\times sum_n+dp_i$$
$$x=sum_k,y=dp_k,a=(t_i+S),b=-t_i\times sum_i-S\times sum_n+dp_i$$
$$dp_{i\min}\rarr b_{\min}$$

对于已知斜率的一次函数，求截距最小， $y$ 值从小到大遇到的第一个 $(sum_k,dp_k)$ 点即为解

用单调队列维护斜率单调递增，每次取队头进行转移，插入时将当前节点与队尾 $r$ ，和 $r-1$ ，连线保证斜率单调递增插入

### P5785

因为 $t_i$ 可能小于零，斜率小于零，所以需要维护一个完整的下凸壳，队列保证单调递增即可，不能将小于其斜率在队列中的弹出，因为不满足单调性，考虑在队列上二分即可

### P3195

$$sum_n=\sum_{i=1}^n (c_i+1)$$
$$dp_i=\min\{dp_j+(sum_i-sum_j-1-L)^2\}$$
$$L=L+1$$
$$dp_i=dp_j+sum_i^2-2sum_isum_j-2sum_iL+(sum_j+L)^2$$
$$dp_i=(-2sum_isum_j)+((sum_j+L)^2+dp_j)+(sum_i^2-2sum_iL)$$
$$2sum_isum_j+(dp_i-sum_i^2-2sum_iL)=(sum_j+L)^2+dp_j$$
$$k=2sum_i,x=sum_j,b=dp_i-sum_i^2-2sum_iL,y=(sum_j+L)^2+dp_j$$
因为 $k$ 单调递增，所以可以像 P2365 一样直接维护队头斜率大于当前值，每次取队头即可

$tips:$ 
>$1.$ 看了大佬的博客，这里为了更快的变成一次函数的样子，可以根据数值的类型进行分类，含 $i$ 的，含 $j$ 的分开，因为 $j$ 是变量，所以可以把同时含 $i$ 和 $j$ 的数值其中的含 $i$ 项看成一次函数的 $k$ ，把只含有 $j$ 的看做是函数值 $y$ ，只含 $i$ 的看做截距 $b$ ，这样就可以快速确定如何优化，记得要塞入 $0$ 这个决策点！( $l=r=0$ 时就是已经塞入 $0$ 决策点在队列中的情况)

>$2.$ 如果这里的 $x_i$ 不单调， $x_a=x_b$ 横坐标相同时，考虑斜率的话就需要判断 $y_a$ 与 $y_b$ 的关系了，正无穷或者是负无穷返回

### P2900

按照长排序，宽为第二关键字，这样如果长 $(x)$ 小于前一位，宽 $(y)$ 小于前一位的，就直接舍掉，因为一定能合并到大的矩形上

考虑最后所剩的为长单调递减，宽单调递增的一个序列
$$dp_i=\min\{dp_j+x_{j+1}y_i\}$$
$$-y_ix_{j+1}+dp_i=dp_j$$
$$k=y_i,b=dp_i,x=-x_{j+1},y=dp_j$$
$x$ 单调递增 ，$k$ 单调递增，维护下凸壳，维护队列斜率单调递增

### P4363 

$$dp_{0/1,S}$$
$$cnt_1=m,cnt_0=n$$
如果当前状态第 $i$ 位为 $0$ ， $i+1$ 位为 $1$ ，可以向第 $i$ 位为 $1$ ， $i+1$ 位为 $0$ 的状态转移

时间复杂度是 $O((n+m)2^{m+n})$

但是因为枚举状态顺序不确定，所以只能写记忆化搜索，由当前位置找到能转移过来的位置进行下一步搜索

### P4677
P4767 前置题目（）

考虑维护村庄数，学校数， $dp_{i,j}$ 表示前 $i$ 个村庄有  $j$ 个学校的最小距离考虑转移， $dis_{i,j}$ 表示 $[i,j]$ 这段内只有一所学校的最小距离
$$dis_{i,j}=\sum_{k=i}^j(sumd_k-sumd_{\frac{i+j}{2}})$$
$$dp_{i,j}=\min_{k\in[j-1,i]}\{dp_{k,j-1}+dis_{k+1,i}\}$$

### P1912

$tips:$
> 分多段的 $dp$ 套路是设 $dp_i$ 表示前 $i$ 个分成了若干段，所用最小代价

这题也不例外（1D/1D），考虑转移方程

$$dp_i=\min_{k\in[0,i)]}\{dp_k+sum_i-sum_k+i-k-1-L\}$$
$$sum_n=\sum_{i=1}^n|string_i|+1,L=L+1$$
$$dp_i=\min_{k\in[0,i)]}\{dp_k+sum_i-sum_k-L\}$$

考虑转移关键点的单调性问题，因为如果具有单调性，内层 $k$ 可以考虑优化到 $O(n)$

![img](https://img2023.cnblogs.com/blog/2659912/202405/2659912-20240515165716312-589173040.png)

~~观察表发现确实具有单调性~~

后面进行常规转移，每次维护单调队列，维护关键点编号，以及他所能作为最优决策点的区间端点，若对于 $i$ 节点，区间右端点无法包含此节点，则弹出队列，然后用队头更新，之后用 $i$ 节点去和队尾节点进行比较，如果用 $i$ 节点更新队尾节点区间左节点更优，则弹出队尾，这个循环停止后，因为只是比较更新左节点时的代价，最终 $i$ 更新左节点更劣，但是更新右节点可能更优，所以需要在这个节点内二分，但是这里的二分 $r$ 需要等于 $n+1$ ，因为此节点可能可以更新后缀所有节点， $n+1$ 是因为如果一个都更新不了，就是 $n+1$ ，判断直接无法更新返回即可


### P1854

简单题。。就是求方案有点意思

设 $dp_{i,j}$ 表示前 $i$ 朵花放在前 $j$ 个花瓶中的最大值，考虑不放和从 $dp_{i,k},k\in[i-1,j-1)$ 转移过来，记得初始化负无穷因为有负数

考虑记录方案，记录当前最优关键点的二元节点，只可能从 $(i,j-1)$ 和 $(i-1,k)$ 转移过来，所以倒推输出，其中需要注意由 $(i,j-1)$ 转移过来的本质是第 $i$ 朵花放在前 $j-1$ 个花瓶上时的情况，但是访问的时候是从 $(i,j)$ 访问的，只需要循环查找 $i$ 不相等去重即可

### P2687

求最大值简单，求方案数：

$$cnt_i=\sum_{k\in[1,i)}
\begin{cases}
cnt_k,(dp_k=dp_i+1 \& a_i<a_k)
\\
dp_k=0,(dp_k=dp_i \& a_i=a_k)
\end{cases}
$$
如果最大长度相同，并且最后一位相同的时候，就清空，避免了有相同情况的可能，如果他的 $dp_i$ 不止有一种关键点转移过来，那从后面还可以继续转移

### P1063 

super低级错误，应该先枚举区间长度再枚举左端点。。

### P5752 & P1436

记录矩形左上角有右下角以及分裂次数，考虑转移：分裂第 $k$ 次选取竖着或者横着一条线，上面的块一次都没分裂过，下面的分裂了 $k-1$ 次，求最小
	
### P2512

考虑环形均分纸牌

对于第 $i$ 个人，向左传递 $x_i$ 个糖果，考虑计算 $x_i$
$$1\rarr 2:\ x_1=c_1-avg$$ 
$$2\rarr 3:\ x_2=c_2+c_1-avg-avg$$
$$i\rarr i+1\ x_i=\sum_{j=1}^i(c_j-avg)$$ 
设 $s_n=\sum_{i=1}^n(c_i-avg)$ ，最终花费就是 
$$\sum_{i=1}^n\sum_{j=1}^i |s_j|$$

因为是在环上并且不可能每条边都被经过，因为考虑一个点已经符合数量，就不可能再次回到这个点了，所以考虑断环成链，在第 $k$ 个位置将环断成 $[k+1,n],[1,k]$ 这两段区间，贡献如下：

$$[k+1,n]:s_{k+1}-s_k,s_{k+2}-s_k,\cdots,s_n-s_k$$
$$[1,k]:s_1-s_k,s_2-s_k,\cdots,s_k-s_k$$
$$\sum_{i=1}^n|s_i-s_k|$$
求此值最小，易证是 $s_k$ 是中位数时最小

### P1889

纵坐标一眼中位数，横坐标不会 /ll/ll/ll/ll 哭了我咋这么菜，橙题都不会了/cf

考虑按照横坐标排序，假设最终横坐标为 $k,k+1,k+2\cdots,k+n-1$ 这样每个点移动的距离就是：

$x_i-(k+i-1)=x_i-k-i+1=x_i-i-k+1=(x_i-i)-(k+1)$ 考虑 $k+1$ 为一个整体 $x_i-i-K$ ，最终又变成了中位数的样子/cf

这里默认 $x_i$ 是有序的，所以需要先拍一遍序再加下标，然后继续排序求中位数

$tips:$
> 感觉贪心的本质是假设，雾

### P6821 & P10478

考虑先将正负相同的一段区间合并成一个数，明显如果全是正的那肯定全部都选取，如果是负的且区间长度大于 $2$ ，明显不能只选一个减小总值，要选就把一块区间内的全部选取，已获得左右正区间合并为一个区间更优这样

最后合并完成整个序列是这样的：

$$\cdots +-+-+-+-+-+\cdots$$

考虑如果不限制块的个数，就是选取所有正区间，考虑怎样削减选取块数并且保证最优：

1. 从已经选取的块中删除正块
2. 选取未选取的负块来合并两个正块变成一个正块

我们可以看到无论是删除正块还是选取负块都是选取绝对值最小的那个以保证最优

所以可以将所有块放到优先队列中按照绝对值排序，每次取出对头块判断正负：
1. 如果是正数，就将左右两块负块和选取的正块结合成 $a_{i-1}+a_{i}+a_{i+1}$ 这样能保证选取这两个负块的时候可以顺带选上已经删除的正块，可以让负块绝对值更小，并且仍然是一个块
2. 如果是负数，就将其与左右正块相结合，原理同上

毕

### P4168

$tips:$
> $1.$ 分块具有的性质是可以用来维护**不满足区间可加性**类型的信息

> $2.$ 分块重点在预处理

此题的区间众数就是这样一类的信息，所以考虑分块

先离散化，数组就可以装下了

定义 $s_{i,j}$ 表示前 $i$ 块 $j$ 出现的次数，预处理复杂度是 $O(n\sqrt n)$ 的

定义 $f_{i,j}$ 表示 $[i,j]$ 这几块的众数，预处理复杂度是 $O(n\sqrt n)$

最后求值时，左右端点在同一块内的直接暴力处理，不在同一块内的，先把两边的散块暴力处理，然后记录中间整块的众数，再用两边散块的值去更新整个区间的众数即可

### P1525

多个罪犯只有两个监狱，考虑如果有 $x,y,z$ 之间有矛盾， $x$ 和 $y$ 不在一个监狱内，那么 $z$ 必定和 $x$ 或者 $y$ 在一个监狱内，所以考虑怎么求出这个的最大值，扩展域并查集模板

按照两人怒气值降序排序即可，如果两人没有同一个敌人，那么他俩就可以分别放在不同的监狱内，如果两人有相同的敌人，那很明显这两个人只能放在相同监狱内，并且降序排序后，最后找到的放在同一监狱内的一对关系必定是最大值的最小值

### P1842

考虑套路：三头牛后两头不交换或者交换：

不交换： 
$$i-1,i,i+1:\begin{cases}
w_{i-1}-s_i
\\
w_{i-1}+w_i-s_{i+1}
\end{cases}
$$
交换：
$$i-1,i+1,i:\begin{cases}
w_{i-1}-s_{i+1}
\\
w_{i-1}+w_{i+1}-s_{i}
\end{cases}
$$
比较 $\max\{w_{i-1}-s_i,w_{i-1}+w_i-s_{i+1}\}$ 和 $\max\{w_{i-1}-s_{i+1},w_{i-1}+w_{i+1}-s_{i}\}$

都同时减掉 $w_{i-1}$ 得 $\max\{-s_i,w_i-s_{i+1}\}$ 和 $\max\{-s_{i+1},w_{i+1}-s_{i}\}$

很明显 $-s_i<w_{i+1}-s_{i}$ ， $w_i-s_{i+1}>-s_{i+1}$  ，已经可以去确定其中一组的一个值与另外一组的一个值的关系，只需要再选取一个值可以同时确定另外两组关系即可，选取 $w_{i+1}-s_i$ 与 $w_i-s_{i+1}$ 进行比较，如果 $w_{i+1}-s_i$ 更大，可以得出一串： $w_{i+1}-s_i>w_i-s_{i+1}>-s_{i+1}$ 以及 $w_{i+1}-s_{i}>-s_i$ 可以发现 $w_{i+1}-s_i$ 同时大于前一组的两个值，所以不交换会更优这种情况，继续推狮子， $w_{i+1}-s_i>w_i-s_{i+1},w_{i+1}+s_{i+1}>w_i+s_i$ ，所以只需要按照  $s_i+w_i$ 升序排列即可求出最小花费 

### P2894 

。线段树

一眼维护相同数最大长度

考虑维护三个信息 $l,r,ans$ 分别表示一段区间内包含左端点/右端点/无限制的最长连续 $1$ 的个数，两段相邻区间合并时，大区间值维护分别为：

$$ans=\max\{ls_{ans},rs_{ans},ls_{r}+rs_{l}\}$$
$$l=\max\begin{cases}
ls_l\\
ls_{len}+rs_l,(ls_{ans}=ls_{len})
\end{cases}
$$
$$r=\max\begin{cases}
rs_r\\
rs_{len}+ls_r,(rs_{ans}=rs_{len})
\end{cases}
$$
询问时先查找根区间的最大连续个数，如果不满足则不进行下面求值，如果满足，就边递归，边二分求，如果左边区间内的值满足要求，就向左区间递归，如果总区间满足，就返回总区间连续段左端点 $mid-ls_r+1$ ，否则向右区间进行递归

### P1972

扫描线典题，但是第一次做的时候太年轻没有意识到其重要性。。。。我太菜了呜呜呜呜呜呜呜呜呜呜呜呜呜呜呜呜呜呜呜呜

考虑从左到右扫描这个序列，每次遇到一个点，考虑他对哪一段区间有贡献，记录这个颜色上一次出现的点是 $pre_{a_i}$ 那么它在 $[1,pre_{a_i}]$ 这段区间已经出现过至少一次了，再加入 $a_i$ 这个颜色是不会新增颜色个数的，所以会在 $[pre_{a_i}+1,i]$ 这段区间内贡献为 $1$ ，那么把询问离线下来，边扫边记录答案即可，区间修改，单点查询求差值，树状数组可以维护

### P6225

考虑快速计算的性质，一个数字 $XOR$ 奇数次，就还是原数，如果偶数次则为 $0$ ，所以考虑这个区间内所有子区间内元素异或次数，推理后发现如果区间长度为偶数，则每个数字出现次数都是偶数，所以答案是 $0$ ，如果长度是奇数，奇数位上出现的数字出现奇数次，偶数位上的数字出现偶数次，所以只需要考虑奇数位上对答案的贡献即可，维护原序列偶数位上数字异或的树状数组和奇数位上的一个树状数组，分别考虑计算即可

### P6619

因为冰战士 $ice$ 温度是单调递增排列，火战士 $fire$ 是单调递减排列，所以随着战场温度 $k$ 的增加，冰战士的能量总和是成单调递增趋势，火战士是单调递减趋势，所以可以考虑二分，找到一个最大的 $k1$ 满足 $sum_{ice}<sum_{fire}$ ，找到一个最小的 $k2$ 满足 $sum_{ice}>=sum_{fire}$ 并且找到一个 $k'$ 满足 $k'$ 处的能量总和两者最小和 $k$ 处相等

考虑在树状数组上二分，可以优化掉一个 $\log$ ，因为树状数组的结构是 $pos=p+lowbit(p)$ 而倍增过程中的 $2^k$ 正好对应 $lowbit(p)$ ，从大到小向上累加跳跃即可

$tips:$
> $1.$ 树状数组可以通过当前第 $i$ 位 $+lowbit(i)$ 的方式 $O(n)$ 建树，还有前缀和通过 $sum_i-sum_{i-lowbit(i)}$ 

### [P3401] 烂尾了

考虑在序列上的做法，常规套路是把每一个数字都拆成二进制下的数字，然后考虑每一位在子序列中异或后为 $1$ 的位置计算贡献，记录所有一位上异或后为 $1$ 的子序列个数，可以用线段树维护，左右两个子序列中的合法序列可以直接相加合并，考虑跨过中间位置的区间，可以记录做儿子区间中包含最后一个点的有贡献的区间个数以及有儿子同理，儿子节点对应值相乘也算一部分贡献

### P4198

考虑对于房屋 $j$ 什么条件下才会被 $i$ 遮挡，只有 $slope_j<slope_i$ 时，所以需要计算序列内有多少关于斜率的前缀最大值，考虑合并时应该如何合并，一般的 $push\_up$ 都是 $O(1)$ 进行的，而这里我们可以考虑一种 $\log n$ 复杂度进行更新的方法，因为一般的方法无法正常维护，考虑对于区间 $o$ 的两个儿子 $ls,rs$ ，首先可以确定的是 $ls$ 的前缀最大值不会受到影响，可以直接累加上， $rs$ 的前缀最大值则可能受到 $ls$ 的影响，如果 $rs_{ls}$ 的最大值小于 $ls$ 的最大值，则向 $rs_{rs}$ 递归，否则向 $rs_{ls}$ 递归并且加上 $rs_{ls}$ 的贡献，因为 $rs_{ls}$ 的贡献不会受到 $ls$ 的影响，但是要注意考虑 $rs_{ls}$ 的影响，用 $rs$ 的贡献减去 $rs_{ls}$ 的贡献，这样才能保证是加上 $rs_{ls}$ 情况下的正确贡献

### P4137

好像是典题，但我现在才做

考虑主席树维护对于每位数字出现的最后一个位置，查询区间 $[l,r]$ 时，如果主席树 $root_{r}$ 中左儿子中最后出现位置的最小值小于 $l$ 就向左儿子查询，否则向右二子查询，这样就能找到最小的出现位置小于 $l$ 的数字了，考虑一共有 $n$ 个数字，如果有数字 $>n$ 那他一定对答案没有贡献，因为如果 $>n$ 则值域 $[0,n]$ 中一定有空缺对 $mex$ 有贡献，所以把 $>n$ 的数字抛弃即可

### P5787

~~太唐了哥，线段树写挂了交了三发TLE，笑死我了~~

二分图判断可以用扩展域并查集，考虑怎么处理边的添加和删除

考虑在线做法，ok不会

考虑离线做法，线段树分治，建树以时间为下标，类似于线段树优化建图那样把一段时间内的操作直接放到对应线段树节点上，这样我们按照时间顺序也就是递归顺序遍历整颗线段树，每次遇到有操作的节点 $[l,r]$ ，就将边加上，同时判断此时的图是否为二分图，如果不是，则 $[l,r]$ 这段区间都不是二分图，因为这个节点加边后图内出现奇环，而后面再怎么操作也无法改变这个奇环

回溯的时候需要撤销这个节点的操作，所以并查集需要具有可撤销功能，所以不能路径压缩，这样会找不到原先的祖先，从而无法确定对应关系，但是为了保证复杂度，需要按秩合并，总的复杂度为 $O(m\log k\log n)$

### P2633

直接类似树上前缀和，额就是~~树上前缀主席树~~

大概为 $root_x+root_y-root_{lca}-root_{fa[lca]}$ 这样，求一下元素个数然后主席树上二分就行了

---

## HDU:

### HUD - 2476

考虑 $a_i\not=b_i$ 时， $a_i$ 的值是什么就不重要了，所以先从空白字符串进行操作至 $b_i$ ，$dp_{i,j}$ 表示进行操作至 $[i,j]$ 区间时，如果 $b_i=b_j$ 此时，可以先进行 $[i,j]$ 的一次涂抹，然后再从 $[i+1,j-1]$ 进行操作，这样转移是 $dp_{i,j}=dp_{i+1,j}$ ，如果不相等就进行常规的区间 $dp$ 枚举 $k$ 的位置进行 

$$\sum_{k=i}^jdp_{i,j}=\min\{dp_{i,j},dp_{i,k}+dp_{k+1,j}\}$$

从空白到 $b$ 之后，因为只是考虑了 $a_i\not=b_i$ 的情况，还需考虑 $a_i=b_i$ 的情况，因为相等，所以只需要 $dp_{i,j}=dp_{i,j-1}$ 即可，从 $1$ 枚举到 $n$ ，因为 $dp$ 数组更新了，所以说还需进行区间 $dp$ 常规更新

### HDU - 1176

类似数字三角形的数塔模型，二维 $dp$ ，记录当前时间当前位置的馅饼最多个数，每次从上一时间左右以及不动位置进行转移，从最后时间转移到第一开始的时间，因为要求最一开始的位置在 $5$ 方便求解

---

## Codeforces:

### CF1096D

$i,0$ 表示一个都没有，$i,1$ 表示一个
...

当前不同：跳过，相同：考虑删除或者不删除
- 删除 $dp_{i,x}=dp_{i-1,x}+dis$
- 不删除 $dp_{i,x}=dp_{i-1,x-1}$ 

因为要操作次数最少，所以贪心的想也就是每次不删的最少，匹配上的最多，所以普遍从 -1 过来

### CF1552F

考虑如果蚂蚁能够经过一个传送门，那他经过时这个传送门一定是关闭的，并且经过后此门一定打开，所以如果蚂蚁经过第 $i$ 个门，那 $[1,i]$ 区间内的门一定都是打开的，这也就意味着如果被传送到门之前的位置，那他需要重新走 $[y_i,x_i]$ 这段区间内的所有传送门并且被重新传送，这是这个 $dp$ 的子任务，$dp_i$ 为经过第 $i$ 个开着的门时所消耗的行走的总路程，方程为: 
$$dp_i=x_i-y_i+\sum dp_i(x_i\in(y_i,x_i))$$
后面求和可用前缀和优化

### CF372C

$dp$ 优化，启动！

考虑需要记录的信息有：当前最大开心值，所在位置，时间

于是考虑 $dp_{i,j}$ 表示在 $t_i$ 时间时，所在位置为 $j$ ，所获得的的最大开心值，对于每个 $j$ 位置，可以从 $k\in[j-\Delta t,j+\Delta t],\Delta t=t_i-t_{i-1}$ 转移过来，所以就有了转移方程：

$$dp_{i,j}=\max_{k\in[j-\Delta t,j+\Delta t],\Delta t=t_i-t_{i-1}}\{dp_{i-1,k}+b_i-|a_i-j|\}$$
$$dp_{i,j}=\max_{k\in[j-\Delta t,j+\Delta t],\Delta t=t_i-t_{i-1}}\{dp_{i-1,k}\}+b_i-|a_i-j|$$

考虑内层循环每次 $j+1$ ， $k$ 的范围都会相应的变成 $k\in[j-\Delta t+1,j+\Delta t+1]$ 明显的滑动窗口求 $dp_{i-1,k}$ 最值为题，用单调队列维护即可做到均摊 $O(1)$ 复杂度 

### CF311B

考虑求出对于每一只小猫恰好能接上他的出发时间 $a_i=t_i-sumd_i$ ，对 $a$ 数组排序 ，$dp_{i,j}$ 表示前 $i$ 个人带 $j$ 只猫（排序后）

$tips:$
> 很难想的思路：排序后，对于一只小猫 $i$ ，要从 $a_i$ 时间出发，必然可以接到 $i\in[1,i]$ 这一段前缀上所有的小猫，并且每只小猫 $k$ 所等待的时间都是出发时间减掉恰好接上他的出发时间 $a_i-a_k$
$$dp_{i,j}=\min\{dp_{i-1,k}+\sum_{t=k+1}^j(a_j-a_t)\}$$
$$dp_{i,j}=\min\{dp_{i-1,k}+a_j(j-k)-(sum_{j}-sum_{k})\}$$
$$dp_{i,j}=\min\{dp_{i-1,k}+a_jj-a_jk-sum_{j}+sum_{k}\}$$
$$a_jk+dp_{i,j}-a_jj+sum_{j}=dp_{i-1,k}+sum_{k},k\in[0,j)$$
$$x=k,y=dp_{i-1,k}+sum_k,k=a_j,b=dp_{i,j}-a_jj+sum_{j}$$
$k$ 单增，维护下凸壳

$tips:$
>要给 $dp$ 数组赋初值，保证放进去的第一个非零转移点不会被 $pop$ 掉

### CF893F

考虑找到一个点 $v\in u_{son}$ ，并且
$\begin{cases}
dfn_v\in [dfn_u,dfn_u+si_u-1]
\\
dep_v\in [dep_u+1,dep_u+k]
\end{cases}$
考虑主席树其实就是查询一个具有双重限制的数据结构，每个 $root_x$ 是可以当做一个限制，另一层就是线段树中的下标也可以当做一个限制

考虑让 $root$ 为深度限制，每颗权值线段树的下标为 $dfn_x$ ，这样就可以查找一个符合条件的 $v$ ，并且因为 $dfn_v$ 一定已经确定范围的，且这个范围不可能出现在深度小于 $u$ 的位置，所以只需要查询 $root_{dep[u]+k}$ 这个线段树即可，不需要想平时的主席树一样作差

$tips:$
>每次插入要新建点，目前还不知道为什么直接继承不可以

---

## Atcoder:

#### AT_past202010_m

第一步考虑转换成线性操作，区间 $[x,lca]$ 到 [lca,y] 以及边权转点权经典操作

下一步是套用套路，类似白雪皑皑，考虑倒着涂颜色，记录当前路径下深度最大的空白位置，每次倒着涂时只涂空白的位置即可，复杂度是 $O(n\log n)$ 的

---

## Acwing:

### 272. 最长公共上升子序列 ：

考虑设 $dp_{i,j}$ 表示 前$i$ 个 $a$ 以 $b_j$ 为结尾的最长公共上升子序列，考虑转移 :

$$a_i\not=b_i\rarr dp_{i,j}=dp_{i-1,j}$$
$$a_i=b_i\rarr dp_{i,j}=\max_{k\in[1,j),b_k<a_i} \{dp_{i-1,k}+1\}$$

求 $\max_{k\in[1,j),b_k<a_i} dp_{i-1,k}$ 最大值的时候，因为 $a_i$ 是固定的，所以可以在枚举 $j$ 的时候进行获取

### 273. 分级 ：

可证明 $b$ 中任意一数在 $a$ 中都存在，易证

考虑 $dp_{i,j}$ 表示前 $i$ 个数字，最后一位为 $b_j$ 时，所需花费最小值

$$dp_{i,j}=\min_{k\in[1,j]}\{dp_{i-1,k}+|b_j-a_i|\}$$

同样的，这里的 内层最小值也是 $i$ 不变，可以跟 $j$ 层循环一起进行求取

### 284. 金字塔 ：

考虑串上一段区间 $[l,r]$ 考虑什么情况才能继续进行子任务操作，即$a_l=a_r$ 时，说明此时可以作为是以 $a_l$ 为根节点的子树进行相应计算，或者总的来说是往后找到一个节点$a_k$ 使得 $a_k=a_l(k\in [l+2,r])$ 从 $l+2$ 开始是因为这里枚举的是子树出口，为根节点，而要拥有子树则必须含有一个节点

$$dp_{l,r}=\sum_{k\in[l+2,r]}dp_{l+1,k-1}\times dp_{k,r}$$

### 295./296. 清理班次1/2 :

考虑需要维护当前排到哪一个位置，以及最小花费。所以设 $f_i$ 表示 $[L,i]$ 这段区间所需要的最小花费，因为需要每个位置至少有一人在工作，所以对于每个人工作时间应该这样转移 :

$$f_{r_i}=\max\{f_{r_i},\max_{j\in[l_i-1,r_i)}f_j\}$$

因为需要用前面位置去更新后面位置，而且每次取出来的位点是当前人工作范围的右端点，所以按照右端点从小到大排序进行转移即可，观察到每次取 $\max$ 的范围都是已经确定的，所以直接进行线段树区间查询最值，每次将此位置的 $f$ 数值插入线段树，求最终结果为 $f_R$

### 297. 赤壁之战

思考需要维护的值都有：当前枚举的位置，当前严格单调递增子序列的长度，这俩确定之后可以很明显得推出转移方程 $dp_{i,j}$ 表示以 $a_i$ 结尾，长度为 $j$ 的严格单调递增子序列的个数，转移方程如下：

$$dp_{i,j}=\sum_{k\in[1,i),a_{k}<a_i}dp_{k,j-1}$$

很明显这个可以用树状数组维护，求取对于每个长度的前缀和

初值是对于长度为 $1$ 的情况，个数也为 $1$

### 291. 蒙德里安的梦想

先考虑维护状态 $1$ 表示此位置放的是竖着放的上半截， $0$ 表示其他状态

考虑从上一层转移过来，如果上一层 $i$ 位是 $1$ ， 当前层 $i$ 位只能是 $0$ ，当前层 $i$ 位是 $1$ ，上一层 $i$ 位只能是 $0$ ，考虑 $S_j \& S_{j-1}\not=0$ 那就不能转移，如果当前层根据上一层为 $1$ 的位确定当前层状态哪些位置确定后，剩余的 $0$ 明显只能放横着的块，需要保证每段连续的 $0$ 都是偶数个位置

最后结果是最后一层状态为 $0$ 的情况，因为不能继续往下放竖着的块

### 298. 围栏

考虑需要维护当前几个工人，几块木板，由 $dp_{i,j}$ 表示前 $i$ 个工人， $j$ 块木板（不保证全部粉刷）所能得到的最大总报酬

$$dp_{i,j}=\max
\begin{cases}
dp_{i-1,j}\\
dp_{i,j-1}\\
dp_{i,j}=p_i\times j+\max_{k\in[j-L_i,s_i-1]}\{dp_{i-1,k}-p_ik\}
\end{cases}
$$
$tips:$ 
> $1.$ 刚开始忘了把含有 $j$ 的项提出来了，因为 $j$ 会随着循环改变，而 $i$ 不会，所以把 $i$ 看成常数项，然后维护关于 $k$ 的单调队列最大值

> $2.$ 不要忘了刚开始需要考虑 $0$ 项的转移！

> $3.$ 最开始为了保证转移时前面的已经维护好，所以需要按照 $s_i$ 大小排序

### 1055. 股票买卖 II

考虑当前持股和不持股的情况 $dp_{i,0/1}$

持股和不持股都可以从前一天相同状态转移过来，持股可以从前一天不持股和今天买入计算，不持股相反，求最后一天不持股的情况，因为不持股肯定要比最后一天持股多赚一天的钱

### 116. 飞行员兄弟

不需要考虑顺序并且操作具有独立性质，所以只需要考虑每个格子是否操作过即可

因为 $n=4$ 很小，所以枚举每一个格子的状态 $0/1$ 表示在此格子是否进行操作

$1$ 时表示这个格子操作，并且此行此列都进行操作，所以行列操作数加一，最终考虑奇偶性判断这一位置操作 $x_i+y_j$ 次是否时开着的，因为当前位置如果被操作过， $x_i,y_j$ 都会 $+1$ 所以需要 $-1$ 保证合法

输出时相同的操作次数操作顺序需要满足从上到下，从左到右，即表示操作状态的数值最大即可

### 244. 谜一样的牛 & 260. 买票

本质相同，写一起吧

考虑倒序删点，每次找一个点，前面仍然存在的点数量为 $a_i$ 个，然后选取这个点，把他删掉，~~很弱智的想法我想的是链表（）~~ 但是前缀点数，删点这两个操作抽象一下就是树状数组基本操作，而找点的过程可以用二分来实现

---

## SPOJ

### SP33

考虑维护最长公共子序列，求方案

考虑每个结尾都可能对应的不同的字母，设 $la_{i,j}$ 表示 $A$ 字符串的前 $i$ 位，字母 $j$ 所在的最后一位的下标， $lb$ 同理

这样每次枚举前 $ia,ib$ 位，然后看不同字母所在下标对应的 $dp$ 值是否合法，进行转移输出方案

---

## 杂项:

### 环形 dp
1. 考虑将序列复制两遍
2. 考虑拆开计算后，分类讨论两个 $[1,n]$ 区间合并时的情况，进行合并

### 斜率优化 dp 凸壳维护种类

这里是如何分析应该维护凸壳的类型
以 $P2900$ 为例：
$$dp_i=\min\{dp_j+x_{j+1}y_i\}$$
假设 $0\le k<j<i$ ，且 $j$ 比 $k$ 更优，则有：
$$dp_j+x_{j+1}y_i\le dp_k+x_{k+1}y_i$$
$$dp_j-dp_k\le x_{k+1}y_i-x_{j+1}y_i$$
$$\frac{dp_j-dp_k}{x_{k+1}-x_{j+1}}\le y_i,(x_{k+1}>x_{j+1})$$
$$y_i\ge\frac{dp_j-dp_k}{x_{k+1}-x_{j+1}},(x_{k+1}>x_{j+1})$$
明显为维护下凸壳
