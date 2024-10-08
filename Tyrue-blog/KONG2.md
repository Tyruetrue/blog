$$2024.07.09\to 2024.08.24$$

$\dag$ 未做

$\checkmark$ 已实现

## luogu:

### $\dag$ P5482

树状数组单点查询区间修改

将式子 $ax+b>c$ 转化为 $x>\frac{c-b}{a}$

观察到 $a$ 可以取负数，所以我们需要分类讨论

考虑当 $a>0$ 时，更改值域树状数组上一段后缀 $(\frac{c-b}{a},V]$ 加一

当 $a<0$ 时，更改一段前缀 $[0,\frac{c-b}{a})$

如果 $\frac{c-b}{a}$ 不是整数的话，需要考虑最靠近 $\frac{c-b}{a}$ 的整数，并把它当做区间端点，如果是整数的话，因为不能取等，所以还需要缩小一次范围

考虑查询就是值域上一点的累计和，删除是当时对应的区间 $-1$

### $\checkmark$ P1471

$$\frac{1}{n}\sum_{i=1}^n(a_i-\bar a)^2$$
$$\frac{1}{n}\sum_{i=1}^n(a_i^2-2a_i\bar a+\bar a^2)$$
$$\frac{1}{n}\Big(\sum_{i=1}^na_i^2-2\bar a\sum_{i=1}^na_i\Big)+\bar a^2\$$
$$\frac{\sum_{i=1}^na_i^2}{n}-\bar a^2\$$

维护区间平方和，区间和即

### $\checkmark$ P7706

线段树维护信息

考虑维护几个值 : 

1. $lx$ 表示最大的 $a_i-b_j(i<j)$
2. $rx$ 表示最大的 $a_k-a_j(k>j)$
3. $ans$ 表示当前节点的最大答案
4. $maxa$ 表示区间内 $a_i$ 最大值
5. $minb$ 表示区间内 $b_i$ 最小值

转移时通过儿子的 $ls_{ans},rs_{ans},ls_{lx}+rs_{maxa},rs_{rx}+ls_{maxa}$ 更新，但是考虑的到 $ls_{lx}$ 对应的 $b$ 不一定是 $rs$ 中 $maxa$ 左边的最小值，但是我们发现取的这个 $b$ 如果不是最小值，那当前这个节点的 $ans$ 取最大时一定会换掉这个方案，也就是说如果 $b$ 不是最小值，那我们可以覆盖掉

$tips:$
> $1.$ 这种根据线段树形态来合并状态的线段树，在询问时应该分为全部在左子树，全部在右子树，以及左右都有，而左右都有的时候需要进行一步对于左边和右边的合并操作才是最后的答案

### $\dag$ P4344

考虑线段树

1.区间推平

3.就是维护区间最长子串，典

2.考虑先求出 $l_1,r_1$ 这段中为 $1$ 的个数，如果 $l_0,r_0$ 这段区间中脑洞数量小于这个数，就可以直接全部推平，如果大于，那就需要考虑求出需要推平的区间 $l_0,r_2$ ，可以二分求出 $r_2$ ，考虑 `check` 时查询线段树上区间 $1$ 的个数即可

### $\checkmark$ P2680

就应该在套路上多想想！

先转化题意 : 将树上任意一边权值设为 $0$ ，求取给定点对中两点间路径最大值最小

这个最大值最小就很二分，然而我一开始完全脑瘫，以为是二分边长没有单调性，神告诉我我要二分总路径长，于是我就开始二分总路径长

考虑二分到一个最长路径长 $mid$ 以及对于一些点对之间的距离 $len_i$ ，若 $len_i<=mid$ ，那我们不需要对这个点对进行改边操作，若存在一个路径集合 $U=\{path_i|len_i>mid,(i\in [x,n])\}$ 这里的 $len_i$ 默认为从小到大排序后的，我们考虑寻找 $U$ 路径集合中 $path_i(i\in[x,n])$ 的公共边，并且求出公共边中权值最大者，将这条边进行改边，如果这些 $len_i$ 仍然 $>mid$ ，那这个 $mid$ 就不成立，或者如果有些路径中的路径和其他路径无公共边，那也不成立

一开始这是的一个直觉想法，但是为什么是对的呢？有以下分析
1. 选取这些路径的公共边是因为如果我们只对其中一些路径进行更改，那没有被更改到的路径一定仍然 $>mid$ ，所以需要全部更改
2. 选取最大边进行更改很直觉，因为更改最大边一定能最大程度的减小路径全职最大值

$tips:$
> 考虑怎样求取公共边，我们可以树上差分，然后求树上前缀和，如果有边经过的次数等于 $U$ 集合的大小，那他一定是公共边

### $\checkmark$ P7518

啊，脑子不转

考虑在链上的情况，假设当前点为 $i$ ，颜色为 $val_i$ ，那我们就需要找到 $i$ 右侧的第一个颜色为 $p_1$ 的点，然后不断向右找 $p_2,p_3,p_4\dots p_k$ ，这个操作可以记一个数组维护，每次往右找可以倍增处理

考虑在树上时，我们是否也可以这样做

一堆点对，起点是 $x$ ，终点是 $y$ ，最近公共祖先是 $lca$ ，考虑我们可以先从 $x\to lca$ 这条路径上进行匹配，也就是上面链的情况，找到 $x$ 上方第一个颜色为 $p_1$ 的点，然后一直跳到 $lca$ 处，这样我们就已经做完了一半了，考虑右侧怎么做，从 $lca\to y$ 的路径不好确定，所以我们可以从 $y\to lca$ 来做，但是这样我们的终点颜色就不确定了，仔细思考可以发现匹配的长度是具有单调性的，如果长度越长，我们能匹配上的就越少，越短就越容易匹配，所以更具这点我们可以二分终点颜色，然后和左边一样匹配即可

但是对于 $y$ 点，如果他的颜色不是我们二分的终点颜色，那他就需要跳到上方第一个颜色为二分终点颜色处，但是我们要是全局处理这个数组，明显是 $O(nc)$ 的

我们考虑怎样才能快速处理，我们可以离线下来，然后每次到终点时进行计算答案，这样一定能保证用 $last$ 数组可以处理到，因为起点不需要用这个数组，并且枚举到 $y$ 时 $last$ 数组所更新的也一定可以满足计算

就这样
 
### $\dag$ P4216

### $\checkmark$ P1641

考虑数字 $1$ 是向上走，数字 $0$ 是向右走，能走到坐标为 $(m,n)$ 的位置

考虑 $num_0$ 恰好 $>num_1$ 的时候，这个点是在直线 $y=x+1$ 上的，然后我们将此交点后方的折线都关于直线对称到上方，如图

![img](https://img2023.cnblogs.com/blog/2659912/202407/2659912-20240712125237922-991612194.png)

考虑后方终点的变换？由 $(n,m)$ 变为 $(m-1,n+1)$ 也就是说 $num_1=m-1,num_0=n+1$ 

最终答案就是 $\dbinom{n+m}{n}-\dbinom{n+m}{m-1}$

### $\checkmark$ P2495

虚树！

直接 `dp` 有 $50pts$ ，读题发现每次询问只跟关键点和其他某些和关键点相关的点有关，所以我们考虑不需要将整棵树都进行 `dp` ，这里就要关于关键点建出虚树

每次找到虚树上一条链，然后将这条链建出来，然后找下一条链，考虑一下下一条链和当前链的链交情况，用栈维护即可

具体的转移方程为 $dp_{u,i}=\sum_{v\in son_u}\min\{dp_{v},w_{u\rarr v}\}$

### $\checkmark$ P4359

先形式化一下 : 

对于一个大于 $1$ 的整数 $M$ ，我们考虑质因数分解， $M=\sum_{i=1}^k p_i^{q_i}$ ， $p_i$ 为两两不同的质数

因为有 $K$ 项，所以 $K=\sum_{i=1}^k q_i$ 

我们先考虑 $N$ 伪光滑数的上界是什么，如果质数指数和是一定的，那我们肯定取大的质数所得到的伪光滑数更大，也就是说 $M_{max}=p_k^{q_k}$ 

我们考虑怎样从最大的伪光滑数向下查找较小的伪光滑数，我们可以每次从最大的质数中取出来一个，然后将其删掉，并且用多出来的这次加数机会加入一个比他小的质数，这样就可以得到严格小于他的伪光滑数了

把这些都放到大根堆里，然后每次取出来最大的进行操作即可

细节看代码

### $\checkmark$ P4211

先考虑两点间的 `lca` 的深度，我们可以发现， `lca` 深度就是两点到根的路径交集长度

我们拓展到 $n$ 个点与 $z$ 点的 `lca` ，可以发现也是 $z$ 点和这些点，每个点和 $z$ 到根路径的交集的长度，所以我们考虑怎样解决这个转化后的问题

路径长度，可以转化为路径上每一个点加一，求树前缀和，于是我们就得到了一个接近正解的做法，对于 $[l,r]$ 的点，我们分别对区间内的每个点向根的路径上的每个点加一，然后考虑对于 $z$ 点进行向根的前缀和

但是这样做是 $O(qn\log^2n)$ 的，我们考虑将 $q$ 消掉

我们发现这段答案，等价于 $[1,r]$ 对于 $z$ 的贡献减掉 $[1,l-1]$ 对于 $z$ 的贡献，这样将区间询问拆成两个单点询问，按照右端点排序之后从 $[1,n]$ 扫一遍，每次扫到关键点就将他计入答案贡献即可

总复杂度是 $O(n\log^2n)$ 

### $\checkmark$ P1712

### $\checkmark$ P6378

考虑 `2-sat` ，我们考虑这两条关系

1. 一条边的两个点中必须有一个关键点 : $u_0\rarr v_1,v_0\rarr u_0$
2. $k$ 部分内只能由一个关键点 : $u_1\rarr  (\complement_{S}u)_0$ 也就是向集合内除了当前节点的任意点连边

但是这样做空间复杂度是 $O(n^2)$ 的，并且时间复杂度同样

我们考虑怎样优化这个东西，我们发现他每次连接的都是一大片点，并且这一片点只不包含很少部分的点，所以我们考虑可以将一大部分看成一个点，考虑前缀优化建图，我们对于每个 $u$ 的前缀吗，建一个点称为 $pre_{u}$ ，考虑怎样连边

$u_1\rarr pre_{u,1},pre_{u,0}\rarr u_0,pre_{u-1,1}\rarr pre_{u,1},pre_{u,0}\rarr pre_{u-1,0},pre_{u-1,1}\rarr u_{0},u_{1}\rarr pre_{u-1,0}$

但是这样看起来只会包含当前节点之前的节点和当前节点连边的情况，那后缀的点被吃了吗？我们仔细观察一下，考虑存在一个点 $v>u$ ，我们需要 $u_1$ 可能需要指向 $u_0$ ，我们发现 $u_1\rarr pre_{u,1}\rarr pre_{u+...=v-1,1}\rarr v_0$

所以是完备的，跑一边强连通分量，判断相同点的两个虚点颜色是否相同即可

### $\checkmark$ P4565

屎

先转化一下式子 : 
$$S=dep_x+dep_y-(dep_{lca(x,y)}+dep'(lca'(x,y)))$$
$$S=\frac{1}{2}(2dep_x+2dep_y-2dep_{lca(x,y)}-2dep'(lca'(x,y)))$$
$$S=\frac{1}{2}(dep_x+dep_y+w_{x\rarr y}-2dep'(lca'(x,y)))$$
$$W_x=dep_x+w_x$$
$$S=\frac{1}{2}(W_x+W_y-2dep'(lca'(x,y)))$$

观察到要计算任意两点间距离，我们考虑点分治，记 $w_u$ 为点 $u$ 到分治中心的距离，我们对于单个分治中心就可以处理出来 $W_x+W_y$ ，考虑怎样计算 $dep'(lca'(x,y))$ 对于一个分治中心，我们对答案的贡献只有 $Tree_1$ 中的一些点，所以在 $Tree_2$ 上我们需要找出这些关键点，然后建虚树，跑 `dp` 

但是要排除同一颗子树内的贡献，所以我们点分治的时候需要对于一个分治中心对他的不同子树染色，然后在 $Tree_2$ 上 `dp` 的时候需要从不同颜色的节点计算贡献

最后还要计算 $x=y$ 的情况

### $\checkmark$ P8867

考虑先缩边双，因为边双内两点一定有两条不交的路径，所以摧毁一条也总能互通

考虑缩点之后的边双树上进行 `dp`

设计状态，设 $dp_{u,1/0}$ 分别表示 $u$ 子树内不选，和 $u$ 子树内选了的所有情况

初始值设为 $dp_{u,0}=2^{E},dp_{u,1}=2^{E}\times(2^V-1)$ 

考虑转移，我们对于不选点的，边就可以随便选 : $dp_{u,0}\prod_{v\in son_u} 2dp_{v,0}$

选点的就是 $dp_{u,1}=dp_{u,1}\times dp_{v,1}+dp_{u,0}\times dp_{v,1}+2dp_{u,1}\times dp_{v,0}$

为什么这里的 $dp_{0}\times dp_{v,1}$ 不乘二，因为需要保证 $v$ 选了之后到上方节点的路一定是连着的，这样才能保证两个军营中间一定有路

计算答案的时候可以考虑除了当前子树其他的边随便选，根节点单独考虑

### $\checkmark$ P3225

感觉比 `P8867` 简单，为啥呀

发现救援出口不能放在割点上，所以考虑找出割点然后通过割点划分联通块

考虑联通块内怎么放救援出口，如果一个连通块周围只有一个割点，如果那个救援出口放在割点处，他就没法逃离了，所以周围只有一个割点的连通块我们需要放两个救援出口，考虑这是啥，这其实就是缩点后树上的叶子节点

如果只有一个连通块的话，他必须放两个，考虑组合数 $\binom{size}{2}$ 

多个的话就是乘法原理

### $\checkmark$ P2272

~~因为输出换行写成空格，被硬控 10 mins~~

考虑两点之间都有一条路径连接，发现一个强连通分量一定是满足的

缩点之后，对于这个 `DAG` ，满足条件的一定是一条链，他要求最大的点集和个数，直接在 `DAG` 上跑 `dp` 即可，求点权最大以及个数

### $\checkmark$ P4562

容易发现一些点没有约数在范围 $[l,r]$ 中，一定要单独开他们自己，下面称为关键点

先考虑对于单个排列的情况，所花费的时间就是最后一个关键点的位置，单个排列最后一个关键点的位置是平凡的，对于所有 $n!$ 个排列，需要计算最后一个关键点位置的期望值，转化为，计算最后一个关键点后方所有非关键点的期望个数

考虑单个非关键点出现在最后一个关键点后方的概率是 $\frac{1}{k+1}$ ， $k$ 是关键点的个数，这些关键点将整个 $[l,r]$ 区间分为了 $k+1$ 段区间，所以概率是这个

所有那非关键点出现在后方的期望值就是 $\frac{n+1}{k+1}$ ，最后一个关键点的期望位置就是 $n-\frac{n+1}{k+1}=\frac{n(n+1)}{k+1}$

共有 $n!$ 个排列，总共的取值是 $\frac{n(n+1)}{k+1}n!=\frac{n}{k+1}(n+1)!$

### $\checkmark$ P6192 & P4784

最小斯坦纳树模板

首先，最小斯坦纳树对应的图一定是棵树，明显可以证明从非树图上删边不会影响关键点并且边权更小

考虑怎样求，可以枚举子图然后跑最小生成树，复杂度是 $O(m\log m+2^nm\alpha(n))$

这样太慢了，并且一些枚举到的子图显然是多余的，可以考虑直接枚举树，设 $dp_{i,S}$ 表示以 $i$ 为根，包含 $S$ 点集的树的最小边权和

初始是每个点为一个状态，从 $dp_{p_i,2^{i-1}}$ 开始转移

因为他从单个点转移到全集，所以转移过程中就会有子集合并过程

考虑如果一个点 $v\not \in S$ ，那 $dp_{v,S}$ 怎样才能被更新呢？

可以从 $S$ 中的根结点向外连边，这样就可以更新了

最后答案是 $dp_{p,U},p\in U$ ，全集中任意一点为根都可以

### $\checkmark$ P2120

还是那句话，先从暴力入手。。

设 $dp_{i}$ 表示，考虑了前 $i$ 个仓库，并且在第 $i$ 个位置建仓库，因为第 $n$ 个位置一定要建仓库，所以答案是 $dp_{n}$ ，但是这样如果最后几个一个物品都没有，他就不需要建仓库，所以最后需要特殊处理一下，答案是最后一个有物品的地方到第 $n$ 个位置内建一个仓库的最小代价

先考虑朴素转移 :

$$dp_{i}=\min_{j\in[0,i)}dp_{j}+c_{i}+\sum_{k\in[j+1,i-1]}p_k(x_{i}-x_{k})$$

这样做是 $O(n^2)$ 的

考虑把式子拆开

$$dp_{i}=\min_{j\in[0,i)}dp_{j}+c_{i}+\sum_{k\in[j+1,i-1]}(p_kx_{i}-p_{k}x_{k})$$
$$dp_{i}=\min_{j\in[0,i)}dp_{j}+c_{i}+x_{i}\sum_{k\in[j+1,i-1]}p_k-\sum_{k\in[j+1,i-1]}p_{k}x_{k}$$
$$d_{i}=\sum_{k=1}^ip_ix_i,s_{i}=\sum_{k=1}^ip_k$$
$$dp_{i}=\min_{j\in[0,i)}dp_{j}+c_{i}+x_{i}(s_{i-1}-s_{j+1})-(d_{i-1}-d_{j+1})$$

设 $u<v$ ，并且 $u\rarr i<v\rarr i$

推出式子，最后是 

$$\frac{dp_{u}+d_{u}-(dp_{v}+d_v)}{s_u-s_v}>x_i$$

斜率恒正，且 $x_i$ 单调递增，斜率增大，是下凸壳

计算斜率的时候需要注意 $\Delta x=0$ 还有 $\Delta y=0$ 的情况

### $\checkmark$ P2474

按照题意形式写出式子 $A+B>/=/<a+b$

先拿一种情况考虑，移项后发现 $A-a>b-B$ ，这种情况确定满足只有 $\min\{A-a\}>\max\{b-B\}$

预处理两个数值初始差的最大最小，两个值的差值可能性最大最小可以用 `floyd` 解出，初始化就分类讨论

最后计算种类的时候分类讨论即可

### $\checkmark$ P4926

啊啊啊？乘法的约束系统怎么做啊？？

数学大法好！

$$A\ge B(k-t)\rarr \lg(A)\ge \lg(B(k-t))\rarr \lg(A)\ge lg(B)+lg(k-t)$$

这下看懂了，因为他只需要小数点后四位即可，所以另一个 $\lg(A)>\lg(B)-\lg(k+t)$ 虽然没有等号，但是也可以随便写了

一开始把所有点都入队，有准确值得直接把 `dis` 更改为准确值，如果有环或者最后有准确值的点值变了说明有可以女装的，可以继续扩大 $t$ 

如果因为 $t$ 越小，大家就越难达成条件，就越容易女装，所以如果 $check(0)$ 都没人女装，那就不存在 $t$ 使得一定有人女装了

### $\checkmark$ P5590

设从 $dis_i$ 表示从 $1$ 节点到 $i$ 节点的路程

因为每条路都相等，最后都等于 $dis_n$ 

怎样才能保证边权值在 $[1,9]$ 的范围内呢？

一条有向边 $u\rarr v$ 的长度可以用 $dis_v-dis_u$ 表示

所以 $1\le dis_v-dis_u\le 9$ ，上差分约束

### $\checkmark$ P5058

只有必经点是，那就是割点，两点间所有路径上的割点不好求，但是圆方树和上两点间经过的所有点一定有两点间割点，所以直接建出圆方树然后以其中一点为根，然后 $O(n)$ 更新这棵树上所有的点从 $s$ 出发，到他们的路径上的割点最小值

### $\checkmark$ P4630

这种三个点，并且有一定相对顺序的，一般都是枚举中间那个点，考虑剩下两个点的位置情况

因为是在图上，并且相对关系不好判断，就转成圆方树， $c$ 点一定是在 $s\rarr t$ 的路径上，所以可以枚举 $c$ 点，然后两边分别计算起点终点情况

考虑 $c$ 在圆点上的时候，可以直接计算 :

$$dp_{c}=\sum_{v\in son} size_v\times(size_c-size_v-1)$$

因为还要算上 $c$ 父亲上面的大小，所以维护一个 $up$ 数组，用来算上面的大小，总得就是 :

$$dp_{c}=up_c\times si_c+\sum_{v\in son} size_v\times(up_c+size_c-size_v-1)$$

在方点的时候要分情况讨论一下怎么算 :

1. 一个点在点双内，另一个在点双外，中间连边 $c$ 在点双内只可能在一个割点上，那个割点之前算过，所以 $c$ 的所有情况只有 $deg_c-2$ 种
2. 起点终点都在点双内，随便选， $c$ 还是有 $deg_c-2$ 种选法

细节看代码

### $\checkmark$ P4602

因为要求符合条件的混合果汁的美味值的最大值，考虑二分，多次询问，离线下来然后整体二分

先按照 $d_i$ 从大到小排序，对于每个 $mid$ ，我们找到所有 $d_i$ 大于 $d_{mid}$ 的集合，并且计算当前询问集合索要的满足 $l_i$ 的最小价值是多少，贪心的想，肯定是选单价越低的越优，所以我们从小往大找，这样做是 $O(n)$ 的，如果用上树状数组上二分，就可以做到单 `log` 的

### $\checkmark$ P3899

转化形式 :

$$s_1=\min\{dep_{p}-1,k\}\times (size_p-1)$$
$$s_2=\sum_{v\in son\wedge dep_{v}-dep_p\le k\wedge v\not=p}(size_v-1)$$
$$ans=s_1+s_2$$

答案就是这个

需要查询深度在区间 $[dep_p+1,dep_p+k]$ 并且在 $p$ 的子树内的节点

对于每个点都建一棵线段树，向上线段树合并即可

### $\checkmark$ P2839

转化问题，这里有个 `trick` 是，枚举 $x$，假设中位数 $\ge x$ ，并且将 $\ge$ 中位数的权值设为 $1$ ，否则设为 $-1$ ，查询序列总和是否 $\ge 0$ ，如果成立，那说明这个数字对应的中位数范围成立，可以进一步向下枚举

顺着这个套路，我们二分 $mid$ ，假设中位数 $x\ge mid$ ，想一下怎么判定，根据上述 `trick` 处理区间，能得到的区间最大值是 $sufmax_{[a,b-1]}+sum_{[b,c]}+premax_{[c+1,d]}$

如果按照 $a_i$ 递增加入主席树， $-1$ 的个数也是递增的，所以可以直接排序后一个一个加入，维护区间和，前缀后缀可空最大值即可

$tips:$
> 这里二分的时候因为当前 $=mid$ 的数值也算为 $-1$ ，所以实际上中位数是 $>mid$ 的，最后计入答案要考虑一下

### $\checkmark$ P4735

可持久化字典树

类似主席树的实现，转化一下题目的式子

$$x\oplus\bigoplus_{p\in{[l,r]}}a_{p}$$

还可以继续转化成 :

设 $sum_{n}=\bigoplus_{i=1}^n a_p$

$$x\oplus sum_{n}\oplus sum_{p-1},p\in[l,r]$$

所以每次把前缀异或值插入到可持久化字典树中，按 $sum_{n}\oplus x$ 每一位查找即可

$tips:$
> 记得要插入一个 $0$

## Codeforces

### $\checkmark$ CF383C

啊写了两种错误写法，然后对拍才意识到问题的严重性。。

先考虑怎样将这个问题转化一下，我们先将这棵树按照深度奇偶性分层，如果一个更改操作时作用在奇数层上的，那他所有的儿子节点中位于奇数层的点都可以加上这个贡献，同时所有偶层点都要减掉这个贡献

那我们很自然的想到线段树和 `dfs` 序，考虑如何将这个问题放到线段树上

我们对于每个更改操作都打上一个 `tag` ，对于奇数层的记为 $tag_1$ ，偶数层的记为 $tag_2$ ，然后每次询问的时候，如果是奇数层，那就是 $tag_1-tag_2+val[x]$ ，否则就是反过来加

这里一开始错误的认为只能写标记永久化，后来发现自己完全想错了，是写挂了，又写了一遍正常下传标记，很对！

### $\checkmark$ CF1204E

考虑我们设 $f_k$ 表示最长前缀 $<=k$ 的方案数，然后考虑这个怎么求

没有限制的时候方案数是 $\dbinom{n+m}{m}$ ，不符合方案的是 $\dbinom{n+m}{m+k+1}$ 所以 $f_k=\dbinom{n+m}{m}-\dbinom{n+m}{m+k+1}$

考虑计算答案 $S$ :
$$S=\sum_{k=\max\{0,n-m\}}^{n}k\times \Bigg(\binom{n+m}{m}-\binom{n+m}{m+k+1}\Bigg)$$

$tips:$
> 反射容斥的满足条件的形式是不接触条件线，所以如果是 $k$ 的限制，限制线应该是 $y=x+k+1$ 

### $\checkmark$ CF757D

观察到性质，要求划分后的数字必须覆盖 $[1,MAX]$ ，所以我们可以针对此来进行状态压缩

考虑 $state$ 表示当前已经存在的数字的集合，然后考虑怎样新加数字进去

我们发现他需要按照划分次数进行答案统计，我们需要把这个加到状态里， $dp_{k,state}$ 表示划分了 $k$ 次，当前数字集合为 $state$ ，所以可以枚举断点然后将新的断点之间的数字放入集合，转到 $dp_{k+1,state|num}$ 

$num$ 可以预处理

### $\checkmark$ CF906C

每次将一个人的所有朋友都互相认识，这一次操作的代价为 $1$

设 $state$ 表示一个两两互相认识的人的集合，设 $dp_{state}$ 表示达成这种互相认识局面的最小代价， $a_i$ 表示一个集合，第 $i$ 个人能够直接到达的所有朋友

可以找到一个 $i\in state$ 通过 $a_i$ 将 $state|a_i$ 通过 $dp_{state}+1$ 进行转移

记录方案只需要记录当前 $state$ 的上一个转移状态即可

### $\checkmark$ CF965E

~~只能想到一点。。~~

观察到与字符串的前缀有关，我们可以尝试将这些都插到 `trie` 上进行操作

考虑转化问题为 : 给你一棵树，树上有 $n$ 个特殊点，特殊点可以移动到他的祖先上，要求祖先不能为特殊点，求最后所有特殊点的深度最小值

我们考虑贪心，对于每个非特殊点，我们都找到以他为祖先的深度最深的节点，并且将它移动到当前非特殊点，？结束了

实现这个操作可以将所有的深度都压入一个大根堆，然后从下往上找非特殊点，从大根堆中取出深度最大的点，然后计算减小的贡献

### $\checkmark$ CF653E

~~我以为是乱搞（）~~

先从最简单的特判开始想 : 如果删掉的边使得 $1$ 的度数小于 $k$ ，那明显不行

再考虑还有什么限制，首先我们需要保证他是一棵树，其次他的 $1$ 节点的度数为 $k$ 

我们按照老师的思考方式，我们先考虑 $1$ 节点的度数的上下界是什么，下界明显就是一个点都不连，上界是什么呢？

考虑这个题的题意，实际上是从一张 $n$ 个节点的完全图中扣去了 $m$ 条边，我们先不考虑复杂度，想到 $1$ 节点的度数最大值其实就是能连向 $1$ 节点的所有极大连通块的个数，我们需要计算出这些连通块的个数即可，如果极大连通块的个数大于 $k$ ，那明显也是不合法的，为什么？因为每个连通块都已经是极大的了，所以不可能再向其他连通块连边，所以我们只能通过向 $1$ 号节点连边来形成树结构，所以不合法

这咋做，我们肯定不能将边都连出来遍历

我们考虑每次从除 $1$ 的一个完全点集中枚举点，然后判断是否有边，不断循环这个操作直到所有点都被遍历过一遍，将这些点从点集中删掉

考虑维护一个这样的数据结构，支持查询，删除，可以用 `set` 维护

因为每个点都只会被遍历 $1$ 次，所以复杂度是 $O(n\log n)$ 的

### $\checkmark$ CF1260E

考虑我们最后一个人肯定无法打败，所以只能贿赂

最后一个人可以帮我们打败 $\frac{n}{2}$ 个人，但是如果我们需要打败的人 $>\frac{n}{2}$ 个，那我们就无法通过这个人打败，考虑找到一个人，能打败 $\frac{n}{2^2}$ 个人，同理可得，我们需要找到我们后面的 $2^k$ 的位置的一段后缀代价最小的人，且不重复

考虑维护一个数据结构，支持插入，查询最小值，删除最小值，小根堆即可

### $\checkmark$ CF1088E

~~好简单一题~~

我们考虑这个式子的实际含义？

$$\frac{\sum_{u\in s}a_u}{k}$$ 

其实就是求多个不交的树上联通块的平均值，因为要求他的最大值，所以我们再来考虑他的上界！！发现平均值一定是 $\le a_{max}$ ，所以我们只需要找到树上的一个权值最大联通块就能完成一半问题了，我们只需要找到树上有多少联通块的值和这个最大值相等就好了

但是我们考虑到还要保证这些联通快不交，怎么做？如果我们每次都是把已经满足条件的联通快取出来，然后从补集树中继续查找就不会产生交的影响了

所以我们可以先进行一次 `dfs` ，找出最大连通块，然后再进行一遍，查找个数，如果相等，我们就将他的 `dp` 值设为负无穷，这样他的父亲就不会选取他，也就不会产生交集

### $\checkmark$ CF623B

很奇怪的 `WA` 了一整页 `codeforces` 才过。。

考虑最大公约数 $d$ 确定时怎么做？

啊，好多人说典，那可能确实，对于每个数字进行操作一的代价分别为 $a_i\equiv\begin{cases}-1/1\\0\\rest\end{cases}\mod d,c_i= \begin{cases} B \\ 0 \\ \infty\end{cases}$ 

我们先考虑没有 $\infty$ 的情况进行操作二

我们画下，发现具体的可以把区间变成这样 : $[1,l)[l,r](r,n]$ 这样三段区间我们对于左右两边的贡献就是 $\sum_{i=1}^{l-1}c_i+\sum_{i=r+1}^{n} c_i$ ，中间的贡献就是 $A\times(r-l+1)=\sum_{i=l}^r A$ 

这样形式很不好做，因为要考虑三段，我们想办法把需要考虑的区间减少，我们发现上面的和等价于 $\sum_{i=1}^n c_i+\sum_{i=l}^r(A-c_i)$ ，这样我们就只需要考虑中间一段区间就好了

发现没有 $\infty$ 的情况只需要考虑找到中间 $[l,r]$ 一段最小字段和就好了

考虑有 $\infty$ 的情况怎么办啊？中间 $[l,r]$ 的区间一定要删掉，所以我们其他考虑要删的区间只能是 $[l_0,r_0],l_0\in[1,l],r_0\in[r,n]$ 并且不能同时满足 $l_0=1,r_0=n$ 

这样我们可以先删掉中间一段区间，然后找到前缀最小和后缀最小就好了

现在就需要考虑 $d$ 的问题了，我们发现最后剩的序列中一定存在 $a_1$ 或者 $a_n$ ，所以所选质因子一定是从 $a_1+1,a_1,a_1-1,a_n+1,a_n,a_n-1$ 的全部质因子中选取

到此为止，这道题就已经解决了，需要注意一些边界问题，细节。调了两天，什么实力（

### $\checkmark$ CF79D

调这个题的时候很难受

我们观察到 $k\le 10$ ，比较自然的想到状压

但是题目看起来和状压完全没有关系

我们考虑先观察性质，选取一段区间操作，实际上就是在查分数祖上进行两次单点操作，并且异或也是具有差分性质的，所以我们可以直接差分然后考虑对于单点考虑，考虑最终状态就是

我们一次更改，会影响两个点，所以可以考虑连边？进一步考虑发现如果这样连下去，一条链的两个端点就会通过这样一系列操作直接更改，你会说中间的节点呢？我们对于一条链，中间的节点一定是度数为 $2$ ，异或两次还是原先的数值，所以其实只有两端点的值改变了，相当于这段区间被更改了

那我们可以通过这个性质，算出两个 $0$ 点，同时被变成 $1$ 点的最小代价，对于每个 $1$ 跑一遍 `bfs` 求出到每个点的最短路即可

然后我们就可以状压算出最终答案，每次枚举一个集合，并且找到两个不在集合中的点，进行转移

### $\checkmark$ CF1696D

考虑我们一定会经过全局最大值和最小值，我们针对最大值将左右分为两个区间，从左边找到区间最小值，再在右边找到区间最小值，然后分别转移到两边的最小值，再去找下一个小区间的区间最大值，不断跳下去，相当于一个分治的过程，总复杂度是 $O(n\log n+n)$ ？

### $\checkmark$ CF1009F

考虑朴素的转移 $dp_{u,k}=\sum_{v\in son_u} dp_{v,k-1}$

这样转移是 $O(n^2)$ 的，考虑长链剖分优化

我们对于一个节点 $u$ ，每次先从他的长儿子转移过来，对于轻儿子则暴力转移，这样就能做到 $O(n)$ 的，为什么？

考虑对于每个轻儿子的 `dp` 数组长度和这个儿子对应的链的长度相等，并且对于每个儿子数组我们只会暴力更新一次，所有链长和等于点数，所以是 $O(n)$ 的

### $\checkmark$ CF293E

两个限制的点分治，考虑先按正常边权进行点分治，考虑找到两个点之间边权距离满足条件时，计算边数贡献，我们可以在计算边数时使用树状数组来维护复杂度是 $O(n\log^2n)$ 的？

### $\checkmark$ CF1316E

我们观察到 $p$ 很小，并且 $s_{i,j}$ 跟在队伍中位置也有关，所以我们考虑可能需要对 $p$ 这一维进行状压

我们考虑需要记录当前位置，当前队伍中的人的位置，考虑 $k$ 的限制怎么做，我们发现 $a_i$ 选取后跟位置无关，所以说，如果我们做完了所有流程，最后选了集合 $S$ 内的人在队伍中，那我们就可以从全集 $U$ 中， $\complement_{U}S $ 中的前 $k$ 大选取

所以我们先考虑对选观众这一子任务来贪心的做，我们就是按照 $a_i$ 从大到小排序，然后每次从一段前缀中选出队伍中的人，同时统计前面一段最大值即可

考虑设计 `dp` ，设 $dp_{i,S}$ 表示当前选了前 $i$ 个人，同时 $state$ 表示当前队伍的占位状态

枚举位置 $j$ ，满足 $S \& 2^j=1$ ， 设 $T=S\oplus 2^j$ ，则对于一种情况，转移方程为 $dp_{i-1,T}+s_{i,j}\rarr dp_{i,S}$

考虑当前如果 $i-1-popcount(S)$ ，也就是没在队伍中的人数小于 $k$ ，我们可以考虑转移方程 $dp_{i-1,S}+a_i\rarr dp_{i,S}$ 否则就是 $dp_{i-1,S}\rarr dp_{i,S}$ 

细节看代码

### $\checkmark$ CF1234F

先转化题意，我们先快速找出这个字符串中所有符合每个字符都不相同的字串，考虑字符集为前 $20$ 个小写字母，所以针对每一个位置往后找 $20$ 位一定会有字符出现两次，所以我们能在 $O(20n)$ 的复杂度下处理出所有符合条件的字串，考虑我们反转串，实际上反转的这部分字串内的字符信息没有变，因为要求的跟字符位置无关，所以我们考虑我们反转一次，一定能将两个字串拼接到一起，所以转化题意为 : 找到两个字符串 $a,b$ 满足条件 $a\cap b=\varnothing$ 并且 $a\cup b$ 大小最大

考虑我们对于每个每个符合条件的字符换，用个二进制下位数为 $20$ 的状态表示当前字符串中所有字符的出现情况

刚刚的转化题意也就是找到两个二进制数 $A,B$ 满足 $A\cap B=\varnothing$ 并且求 $popcount(A\cup B)$ 最大值

我们发现针对每个 $A$ 我们满足条件的 $B$ 一定满足 $B\sube T,T=\complement_UA$ 所以我们需要针对每个 $T$ 集合，找出所有他的子集最大值，考虑这个怎么求

考虑 $B=\{0,1,1,0,1...1\},T=\{1,1,1,0,1...1\}$ ，也就是说我们需要针对 $T$ 的每一位，找到位值小于他的所有集合

是一个前缀和的形式，考虑使用高维前缀和来做这个就好了

### $\checkmark$ CF383E

考虑将求和当前字符串有交集的集合，转化为，和当前字符串的补集有交集的字符串个数，然后减掉

考虑高维前缀和即可

$tips:$
> 高维前缀和实际上是从 $dp_{i-1,state}+dp_{i-1,state\oplus2^j}\rarr dp_{i,state}$ ，但是卡空间的写法需要滚掉 $i$ 的那一维，所以我们需要固定顺序，先枚举 $i$ ，再枚举 $state$ 

### $\checkmark$ CF1709E

考虑对于一段 $u\rarr v$ 的路径，我们可以将他分成 $u\rarr lca\rarr v$ 但是考虑异或值，我们可以考虑将他继续拆成 $\bigoplus_{root\rarr u}\ \oplus\ \bigoplus_{root\rarr v}$ ，前缀异或值直接抵消掉

所以对于全部的路径异或值我们可以拆成对于点的询问

看起来很像启发式合并，我们考虑合并的过程，每次找到 $x$ 最小的子树 $v$ 中的 $dis_y$ ，对于他查找以 $x$ 子树为全集， $v$ 子树的补集中是否存在 $dis_y\oplus val_x$ ，如果存在，那肯定异或值为 $0$ 

考虑这样做正确性，我们发现如果异或值为 $0$ 其实很困难，所以只要更改当前父节点，就能抹除当前节点和其所有字数内节点对后面的贡献

$tips:$
> 为了保证时间复杂度正确，我们肯定要枚举小的集合，但是为了保证空间复杂度正确，我们需要让父节点每次继承大集合，这样剩下的集合大小才会为 $n\log n$ 

### $\checkmark$ CF425E

考虑 `dp` ，直接设计状态，设 $dp_{i,j}$ 表示当前考虑结尾为 $i$ 坐标，能最多选出 $j$ 条不交的线段

先考虑什么情况下一定能选出最多的线段，一定是选右端点靠左的线段先选不劣，所以可以先选出合法的不交线段，然后加上更劣 (对答案没有贡献) 的线段，计入方案数

所以转移方程就可以这样写 : 

$$dp_{i,r}\larr dp_{i,j}\times 2^{j-r}$$

因为还要加上右端点靠后计入答案的，所以方程式这样 :

$$dp_{i,r}\larr dp_{i,j}\times 2^{j-r}\times 2^{n-r}$$

其中初始时 $dp_{0,0}=1$

### $\checkmark$ CF93E

先考虑容斥原理 : 

$$U=\{\{a_1\},\{a_2\}\dots,\{a_1,a_2,\dots a_n\}\},\sum_{S\in U}(-1)^{|S|+1}\frac{n}{\prod_{a_i\in S}a_i}$$

这样肯定过不去啊

考虑 `dp` ，设 $dp_{i,j}$ 表示，前 $i$ 个数，用前 $j$ 个 $a_j$ 能整除的个数

答案是 $n-dp_{n,k}$ 

转移有 : $dp_{i,j}\larr dp_{i,j-1}+\lfloor \frac{i}{a_j}\rfloor-dp_{\lfloor\frac{i}{a_j}\rfloor,j-1}$ 

为什么这样转移，感觉题解写的都不清楚，我认为可以这样理解 : 右侧应该是 $j-1$ 个 $a_j$ 和第 $j$ 个 $a_j$ 能整除的数组成的集合的并集，所以可以表示为 $j-1$ 个 $a_j$ 能整除的数组成的集合，和第 $j$ 个 $a_j$ 整除的数的集合加起来再减去他们的交集，符合容斥原理，最右侧的那一项其实表示的就是他们的并集，先抛去 $a_j$ 的倍数，然后再抛去剩下 $j-1$ 个数的倍数，因为互质，所以显然成立，不重不漏

$tips:$
> $|S|$ 为奇数时，容斥系数是 $1$ ，否则为 $-1$

### $\checkmark$ CF19D

~~KDT~~

对于每个横坐标，维护他这一维最大的纵坐标即可

发现对于一个询问点，所有大于他的横坐标拥有的纵坐标只需要存在一个大于他的就可以满足条件

所以我们对于离散化后每个横坐标建一个 `set` ，然后将最大值插入线段树，删除和添加在 `set` 上操作完，把当前位置最大值更新，询问找到右边最靠左并且纵坐标大于当前点的一个横坐标，输出，线段树上二分

### $\checkmark$ CF140E

先考虑单行相邻颜色不同怎么求，考虑 `dp` ，设 $f_{i,j}$ 表示，前 $i$ 个位置，放了 $j$ 个颜色，转移方程 :

$$f_{i,j}\larr f{i-1,j-1}+f_{i-1,j}\times(j-1)$$

那相邻两行颜色方案种类不同也考虑 `dp` ，设 $g_{i,j}$ 表示前 $i$ 行，第 $i$ 行颜色种类有 $j$ 个

$$g_{i,k}=\sum_{j=1}^{l_{i-1}}\begin{cases}g_{i-1,j}\Big(\binom{m}{j}-1\Big)j!f_{i,j},(k=j)\\g_{i-1,j}\binom{m}{j}j!f_{i,j},(k\not =j)\end{cases}$$

这样是没法优化的，但是看起来只有 $(k=j)$ 这一个特例，所以考虑先不管他，看看能不能优化 $g_{i,j}\larr \sum_{j=1}^{l_{i-1}}g_{i-1,j}\binom{m}{j}j!f_{i,j}=j!f_{i,j}\binom{m}{j}\sum_{j=1}^{l_{i-1}}g_{i-1,j}$ 可以求和优化这一过程 
$$S=\sum_{j=1}^{l_{i-1}}g_{i-1,j}$$
$$g_{i,k}\larr j!Sf_{i,j}\binom{m}{j}$$
$$g_{i,k}\larr j!Sf_{i,j}\frac{m!}{j!(m-j)!}=Sf_{i,j}\frac{m!}{(m-j)!}$$

最后一项是一个下降幂

$$m^{\underline{j}}=\prod_{i=m-j+1}^m i$$
所以原先的式子可以化成

$$g_{i,k}\larr j!Sf_{i,j}\frac{m!}{j!(m-j)!}=Sf_{i,j}m^{\underline{j}}$$

还有一个地方需要减去，就是刚刚不管的那部分 $j=k$ 的部分

$$g_{i,j}=(Sm^{\underline{j}}-g_{i-1,j}j!)f_{i,j}$$

看一眼空间 `250MB` ，最后的 $g$ 数组因为关系到 $n$ 层，空间复杂度是 $O(nl)$ 的，开不下，需要滚动数组把第一维滚掉，总的时间复杂度是 $O(\sum l)$

好像是写的算是很长的一篇题解了

### $\checkmark$ CF794D

思考半天，觉得无法用什么算法来做他，所以需要考虑性质

考虑如果两个点所相邻的点集相同 (包括他们两个) ，那他们的值会有什么特殊性？考虑一种情况 $u,v$ 相邻的点集相同，设 $x_u=i,x_v=i+1$ ，$u$ 的相邻节点就需要是 $x_u-1,x_u+1,x_u$ 但是他同时是 $v$ 的相邻节点，所以说他只能是 $x_u,x_u+1$ ，那这样，将 $v$ 的值改为 $i$ ，也就可以行的通，所以我们将所有相邻节点点集相同的节点缩成一个节点，这样就得到了一张新图，对于新图上的一个节点 $t$ ，他的相邻节点一定不能和 $t$ 的 $x$ 相同了，只能是 $x_t+1,x_t-1$ ，并且值只能是这两个，不能重复

所以一个新图节点的度数只能是 $2/1$ ，所以只能是环或者是链，环一定无解，会重复，不如建新图的时候就考虑进去，所以链一定有解

用异或哈希计算相邻节点

### $\checkmark$ CF241E

同 `P5590`

### $\dag$ CF208E

随便写，然后就挂了
 
## Gym

### $\checkmark$ Gym104611E

场切

根据 `CF383C` 类似的思想，可以维护两类值，一类是奇数层的值，一类是偶数层的值

考虑怎样转移，计入的值里面还有个 $d$ ，所以需要考虑节点之间的距离，那线段树上也就需要维护当前线段树节点所包含的树上集合中点的最小深度

每次转移，如果和子树根节点的距离为奇数，那他的值就需要错位下传，否则正常下传即可

## Atcoder

### $\checkmark$ AT_arc171_d

先转化给出的 `hash` 值

$$A=a_1,a_2\dots a_n$$
$$hash(A_{l,r})=\Big(\sum_{i=l}^ra_iB^{r-i}\Big)\mod P$$
$$hash(A_{l,r})=\Big(\sum_{i=l}^ra_iB^{r-n+n-i}\Big)\mod P$$
$$hash(A_{l,r})=B^{r-n}\Big(\sum_{i=l}^ra_iB^{n-i}\Big)\mod P$$

考虑一段后缀 `hash` 值， $suf_i=\sum_{j=i}^n a_iB^{n-i}$

所以题目定义的区间 `hash` 就是 $B^{r-n}(suf_l-suf_{r+1})\mod P$

题目所求的所有所给区间 `hash` 值不为 $0$ ，也就是 $suf_l\not=suf_{r+1}$

考虑这个的实际含义

我们可以将点 $l,r+1$ 连边，这样之后我们会得到一张图，不相等的含义可以转化为给这张图赋权，任意一条边的两端点权值不同

怎么做？听说是经典题，但我不会

设状态为 $state$ 是一个点集，我们枚举其中的一个子集 $T$ ，保证 $\complement_{state}T$ 是 $state$ 的一个独立集，这样 $T$ 的补集因为是独立集所以就可以全部染成一个颜色，也就是 $dp_{state}\larr dp_{\complement_{state}T}+1$ ，这样就做完了

### $\checkmark$ AT_cf17_final_j

考虑点分治，我们先考虑不在同一子树内的贡献，我们对于每个分治中心，可以求出每个点到分治中心的距离 $dis_i$ ，然后考虑连边，找出每个分治中心下 $val_i+dis_i$ 最小的那个点，然后从他向其他节点依次连边， $W=dis_i+dis_j+val_i+val_j$ ，考虑我们一共会找到 $n$ 个分治中心，然后连边总数是 $n\log n$ 条，外面再套个 `kruskal` 复杂度是 $O(n\log n+n\log^2n)$

### $\checkmark$ AT_arc100_c

考虑对于单个 $K$ ，我们要求 $i\ or\ j\le K,\max\{a_i+a_j\}$

我们转化问题，发现 $i\ or\ j\le K$ 所包含的 $(i,j)$ 集合，和 $i\in K,j\in K$ 所包含的点对集合是等价的

这里的 $\in$ 指的是二进制下每一位都小于等于

这就转化成了一个子集问题

我们考虑求出 $i,j\in K,i\not=j,\max\{a_i+a_j\}$

用高维前缀和做即可，每次合并当前 $K$ 的最大值和次大值，最后求一边前缀最大值即可，因为显然有 $i,j\in K_1,K_1\le K_2,i\ or\ j\le K_2$

### $\checkmark$ AT_abc219_f

先考虑进行一次的情况，按照题意模拟即可，需要维护一个集合 $S_1=\{(x_1,y_1)\dots(x_n,y_n)\}$ ，然后我们考虑进行下一次时对应的点是什么，我们记第一次的终点，也就是第二次的起点为 $(a,b)$ ，那第二次就可以记为 $S_2=\{(x_1+a,y_1+b)\dots(x_n+a,y_n+b)\}$

我们考虑第 $d$ 次时，发现找到了一个集合 $S_d$ 中存在一点 $(x_1+(d-1)a,y_1+(d-1)b)\in S_{1\dots d-1}$ 那我们这时这个点就会重复计算，所以接下来考虑这类点的特征，因为他是重复的 $(x_1,y_1)$ 所以，考虑和他的特征，其实就是 $(x_1+ka,y_1+kb),k<d-1$ 时都成立，考虑求出 $d$ 即可，就是一类点 $x\mod a=x_1,y\mod b=y_1$ ，求出 $d=\frac{x}{a}$ 

需要分类讨论一下 $a,b$ 的取值

1. 如果 $a=0,b=0$ 直接求一遍即可
2. 如果 $a=0$ 我们可以坐标反转，也是对的，然后按照 $a=0$ 处理，用 $d$ 算出 $y$ 所属的 $S_1$ 中的坐标，因为无法直接除
3. 如果有负数，取反即可

### $\checkmark$ AT_abc246_g

转化一下，发现 `B` 在最优策略下会尽量拿多的分，而 `A` 需要让这个最大值最小，也就是可以考虑二分

二分 `B` 能拿到的最大值 $mid$ ，那就需要把 $val_i>mid$ 的点都删掉，考虑树上 `dp` ，因为 `A` 不知道 `B` 会怎么走，所以需要将某个节点所对应的子树内所有节点符合条件的都删掉，但是 `A` 是先手，那 `A` 就可以比 `B` 少一步操作次数，这样转移 : 

$$dp_{u}=\min\{\sum_{v\in son_u}dp_v-1,0\}+[val_u>mid]$$

因为中途已经算上了 `B` 操作的情况，所以如果 $dp_1>0$ 的话，就不能先手胜了

### $\checkmark$ AT_abc225_f

字符串需要排序，排序的比较方式可以按照字符串 $a+b<b+a$ 这样比较，原因是 : 将 $a,b$ 都看成 $27$ 进制数，这样就表示成 $a\times size_b+b<b\times size_a+a\rarr \frac{a}{size_a-1}<\frac{b}{size_b-1}$ 两边都只跟各自的长度信息有关，所以具有比较传递性

按照这个排完序之后，只能保证相同长度的时候拼成的字符串字典序最小，但是如果正着做，可能会一直选当前最优的，但是忽略了如果前面都相同，长度比较长的情况比如 
```
4 3
bbaba
bba
b
b
```
正着做就会是 `bbababbab` 但是如果反着做，就是 `bbababb` 从后面选最优的，同时也能保证长度最优

### $\checkmark$ AT_abc364_g

最小斯坦纳树板子

### $\checkmark$ AT_abc366_f

是 `dp` ，但是 `dp` 顺序怎么搞？一开始考虑区间 `dp` ，不可行，无法记录两边区间能拿到最大的一次函数，并且复杂度也不正确

我们联想上面那个字符串拼接的题目，发现通过贪心的排序，可以让 `dp` 需要使用的原始数组有顺序，从而可以直接进行转移，这类贪心之前做过，假设，然后推式子，假设 $f_1$ 在内部比 $f_2$ 在内部更优

$$f_2(f_1(1))>f_1(f_2(1))$$
$$k_2(k_1+b_1)+b_2>k_1(k_2+b_2)+b_1$$
$$k_2\times k_1+k_2\times b_1+b_2>k_1\times k_2+k_1\times b_2+b_1$$
$$k_2\times b_1+b_2>k_1\times b_2+b_1$$
$$k_2\times b_1-b_1>k_1\times b_2-b_2$$
$$b_1\times(k_2-1)>b_2\times(k_1-1)$$
$$\frac{b_1}{k_1-1}>\frac{b_2}{k_2-1}$$

两边都只跟独自的信息有关，具有传递性，直接按照这个排序即可

然后就是简单的二维 `dp` ，复杂度是 $O(nk)$

### $\checkmark$ AT_abc359_g

两点间距离 $=dep_{u}+dep_{v}-2\times dep_{lca(u,v)}$ 

按照颜色分类，前面点的深度是好算的，后面 $lca$ 深度可以类似于 `P4211` 做法，直接处理掉

复杂度是 $O(n\log n)$

### $\checkmark$ AT_abc365_g

根号分治

怎么想到这块的？因为 $\tt{polylog}$ 做法看起来不太可行，所以转战根号

先考虑暴力怎么做？每次 $O(m)$ 双指针扫一遍两个人出入情况即可

直接做复杂度是 $O(qm)$ 的

我们发现复杂度主要跟操作次数有关，所以设一个阈值 $B
$ ，如果两个人的操作次数都小于 $B$ ，那就暴力处理，复杂度是 $O(qB)$ 的

大于他的怎么做啊？考虑到大于他的最多不会超过 $\frac{m}{B}$ 个，大于他的就可以维护前缀和，对于每个人都提前预处理出答案，复杂度是 $O(\frac{m}{B}n)$

如果 $B=\sqrt{m}$ 那我们的总复杂度就是 $O(n\sqrt{m}+q\sqrt{m})$

### $\checkmark$ AT_abc361_g

感觉要想有思路，还是要从暴力往外优化！

考虑这题的暴力怎么做？把所有点都四联通判断连边，然后判断连通性

复杂度巨大，是 $V^2\alpha(V^2)$ 的

考虑怎么优化这个算法啊？画画图，感觉会有很多点容易判断是联通的，那不妨用一种简便的方法减少点数，我们对于障碍点进行分析，一共最多有 $2e5$ 个障碍点，划分到图上会有很多非障碍点连在一起，对于一行内，我们把相邻的非障碍点直接连到一起，这个点就代表一段区间

每一行由多个非障碍点组成的区间组成，行与行之间的区间也是需要联通的，怎样快速连到一起？

双指针，两行内不同的区间最多有 $4$ 中可能性，如下

![img](https://img2023.cnblogs.com/blog/2659912/202408/2659912-20240820195545136-992254384.png)

这四种情况可以分为两种，一种是下面的右端点被上面区间包含，一种是上面的右端点被下面的区间包含

这样就可以涵盖所有可能，但是大前提都是上方的右端点一定不能下面的左端点，否则就无交了

总复杂度是 $(V+n)\alpha(V+n)$

### $\checkmark$ At_abc367_f

##  BZOJ

### $\checkmark$ #3306

考虑除去第二个操作，我们所要做的就是线段树区间查询和单点修改加 `dfs` 序，好做的

考虑第二个操作怎么做，我们先分类讨论一下，称 $root$ 为新根， $u$ 是要求的节点
 
1. 如果 $root$ 不在 $u$ 的子树内，那答案无影响
2. 如果 $root$ 是 $u$ ，那答案是全局所有节点的最小值
3. 如果 $root$ 在 $u$ 的子树内，考虑此时我们有一个节点在 $u\to root$ 路径上，并且 $dep_v=dep_u+1$ ，也就是这条路径上最靠近 $u$ 的一个节点 $v$ ，我们会发现 $v$ 子树内的所有点都在 $u$ 的上方，所以我们需要将这一块刨掉，也就是查询 $[1,dfn_v-1]\cup[dfn_v+si_v,n]$ 这些区间内的最小值

$tips:$
> $1.$ 考虑寻找 $v$ 的过程，我们可以用倍增去找
> $2.$ 上面判断是否在子树内等判定都可以使用 `dfs` 序进行

完。

## Spoj

### $\checkmark$ SP5271