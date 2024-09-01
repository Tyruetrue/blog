# 题解 [[ARC149B] Two LIS Sum](https://www.luogu.com.cn/remoteJudgeRedirect/atcoder/arc149_b)

大胆猜结论，按照 $a$ 数组为关键字进行排序，求更改后 $b$ 的 $LIS$ 。

证明：每次移动，都有 $a$ 中增加一个长度， $b$ 中贡献可能为 $\{-1,0,1\}$ ， 总体贡献为 $\{0,1,2\}$ ，具体为：
1. $b$ 中排序后大数变到小数前方 。
2. $b$ 中排序后小数仍然在大数前方 。
3. $b$ 中排序后小数变到大数前方 。

总体是不劣的，所以正确性显然。

树状数组 $\mathcal{O(n\log n)}$ 求 $LIS$ 。

```cpp
#include<cstdio>
#define max(x,y) x>y?x:y
const int N=1e6+10;

int n;
int c[N],a[N],b[N];

void add(int x,int v){
	for(int i=x;i<=n;i+=(i&-i)) c[i]=max(c[i],v);
	return ;
}
int query(int x){
	int res=0;
	for(int i=x;i;i-=(i&-i)) res=max(res,c[i]);
	return res;
}

int main(){
	scanf("%d",&n);
	for(int i=1;i<=n;i++) scanf("%d",&a[i]);
	for(int i=1;i<=n;i++) scanf("%d",&b[a[i]]);
	for(int i=1;i<=n;i++)
		add(b[i],query(b[i]-1)+1);
	int ans=query(n);
	printf("%d",(ans?ans:1)+n);
	return 0;
}
```