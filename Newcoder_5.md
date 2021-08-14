# 2021牛客暑期多校训练营5

## 题解

### B - Boxes

#### 题目

There're $n$​ boxes in front of you. You know that each box contains a ball either in white or in black. The probability for a ball to be white is $\frac{1}{2}$, and the colors of balls are independent of each other. The PJ King invites you to guess the colors of all balls. PJ King has assigned some costs to the boxes. If we number the boxes from $1_{}$​ to $n_{}$​, the cost to open the box $i_{}$ is $w_i$, and after a box is opened you can see the ball inside this box.

 For sure, there's no way to know all the colors except by opening all boxes. However, Gromah wants to give you some hints. Gromah can tell you secretly the number of black balls among all boxes that have not been opened yet, but you have to pay $C_{}$ cost to get one such hint from Gromah. Anyway, if you're superpowered, you can do it without any hint. What's the mathematical expectation of the minimum cost to figure out all colors of balls?

#### 分析


题目的大致意思就是有$n$个盒子，每个盒子开出白球和黑球的概率是$\frac {1}{2}$，每个盒子都有一个特定的权值，表示打开它需要的代价$W_i$，有一个特殊的操作可以知道剩下的盒子中的黑球的总数，需要的代价为$C$。求在最优策略下的数学期望。首先可以想到的是，这个特殊操作后面用的话肯定是不优的，因为最开始用就等于后面用了。所以最后的答案肯定是一个一个开完，和使用特殊操作的最小值。这个题看起来像数学题，我们队就是把它当数学题做的，公式过于复杂导致最后没有做出来，实际上这道题是一个思维题。这$n$个盒子可以开出的结果可以看成是长度为$n$的$01$串，使用特殊操作的话，我们开到第$i$个盒子时，可以知道所有盒子的颜色的条件是剩余的$n-i$个盒子全部为黑色或者白色，对应着剩余的$01$串全为$0$或者$1$和第$i$个盒子的颜色与后面异色，它出现的概率是$\frac{1}{2}^{n-i}$（网上有很多题解没有说明第$i$个盒子的颜色与后面异色这个前提，直接说后面$n-i$个盒子同色的概率是$\frac{1}{2}^{n-i}$，这是错的，因为有两种颜色需要乘以$2$​）。还有一点，题目中需要求的最小值，开盒子的必须按照盒子的权值从小往大开，只需要加一个排序即可。

#### 代码实现

代码没有啥好解释，算法核心：把数组中每个数都减去二分的值，问题就转化为“是否存在一个长度不小于f的字段，字段和非负”。转化之后使用前缀和就能轻松实现。

```c++
#include<bits/stdc++.h>
const int N=1e5+5;
using namespace std;
int n,m,x,y;
int a[N],b[N];
double sum[N];
double slove(int a[],int n,int f){
	double l=0,r=1e5;
	sum[0]=0;
	while(r-l>1e-7){
		double mid=(l+r)/2;
		for(int i=1;i<=n;i++)
		sum[i]=sum[i-1]+a[i]-mid;
		int flag=0;
		double minv=0;
		for(int i=f,j=0;i<=n;i++,j++){
			minv=min(minv,sum[j]);
			if(sum[i]>minv){
				flag=1;
			}
			if(flag==1) break;
		}
		if(flag){
			l=mid;
		} 
		else r=mid;
	}
	return r;
}
int main(){
	scanf("%d%d%d%d",&n,&m,&x,&y);
	for(int i=1;i<=n;i++)
	scanf("%d",&a[i]);
	for(int j=1;j<=m;j++)
	scanf("%d",&b[j]);
	double ans=slove(a,n,x)+slove(b,m,y);
	printf("%.10lf\n",ans);
}
```

### H - Holding Two

#### 题目描述

Given $n,m_{}$, construct a matrix $A_{}$ of size $n\times m$, whose entries are all either 0 or 1, and no three distinct entries $A_{i_1,j_1}, A_{i_2,j_2}, A_{i_3,j_3}$​satisfying that $A_{i_1,j_1} = A_{i_2,j_2} = A_{i_3,j_3}, -1\le i_1-i_2=i_2-i_3\le 1, -1\le j_1-j_2=j_2-j_3\le 1$. If multiple solutions exist, print any one of them. If no solution, print "-1" in one line.

#### 题目大意

给定n和m两个数，要求建立一个nm的方形区域，区域中的每个格子可以填1或0，但是不能有三个1或0连续在一条线上，即横向，纵向或者对角线相连都是不被容许的

       0 0 0（x）
       1 0 1      
       1 1 0
       1 1 0（x）
     （x）

从这个性质中容易得到当方形区域如下时便可完美符合题目要求:

       0 0 1 1 0 0 1 1
       1 1 0 0 1 1 0 0
       0 0 1 1 0 0 1 1
       1 1 0 0 1 1 0 0
       0 0 1 1 0 0 1 1
       1 1 0 0 1 1 0 0

此时无论是横竖亦或是斜的对角线都不会有连续三个的情况
代码片段如下

```c++
#include <cstdio>
 
int main() {
    int n,m,x;
    scanf("%d%d",&n,&m);
    for (int i=1;i<=n;i++){
        x=i&1;
        for (int j=1;j<=m;j++){
            printf("%d",x^=(j&1));
        }
        printf("\n");
    }
    return 0;
}
```

### J - Jewels

#### 题目

There are $n_{}$​ jewels under the sea, and you want to salvage all the jewels. Image that the sea is a 3D coordinate system, and that you are at $(0,0,0)_{}$​ while the ii_{}i-th jewel is at $(x_i, y_i, z_i)~(z_i \ge 0)$​​initially. You can salvage one jewel immediately(regardless of other jewels wherever they are) with strength $d^2$ at every non-negative integer moment where d denotes the distance between you and the jewel you salvage at that moment. However, the jewels sink. Specifically, for the ii_{}i-th jewel, it will be at $(x_i, y_i, z_i + t\times v_i)$ after $t_{}$ seconds. So you want to know the minimum possible total strength to salvage all the jewels.

#### 分析

题目的大致意思就是你的位置在$(0,0,0)$需要打捞$n$个珠宝，每个珠宝的初始位置为$(x_i,y_i,z_i)$，并且这个珠宝会按照速度$v_i$往下沉，每一秒可以打捞一个珠宝，打捞一个珠宝所需要的权值为原点到珠宝距离的平方，并且你能在一秒内返回原点，从第$0$​秒开始计时，求打捞完所有珠宝需要最小的权值。首先这道题的数据量比较小，只有300，应该是需要一个暴力的方法来做。如何暴力的求解呢？我们可以确定的是能在$[0,n-1]$内打捞完全部的珠宝，且每一秒钟需要打捞一个珠宝。这不就是一个最小匹配问题吗？（虽然我不会写~( ´•︵•` )~），赛后经过两天的学习，我终于学会了如何求最小匹配问题。首先取它的相反数转化为求匹配的最大权值，在使用KM算法进行相应的处理。不得不说网上看KM算法得找对板子，网上有些板子的最坏时间复杂度是$O(n^4)$​会导致TLE。

#### 代码实现

```c++
#include<bits/stdc++.h>
#define int long long
#define N 505
using namespace std;
const int INF=1e18;
int n,m;
int la[N],ra[N],w[N][N],vis[N],slack[N],match[N],pre[N]; 
void bfs(int u){
	int x,y=0,yy=0,delta;
	memset(pre,0,sizeof(pre));
	for(int i=1;i<=n;i++) slack[i]=INF;
	match[y]=u;
	while(1){
		x=match[y],delta=INF,vis[y]=1;
		for(int i=1;i<=n;i++){
			if(vis[i]){
				continue;
			}
			if(slack[i]>la[x]+ra[i]-w[x][i]){
				slack[i]=la[x]+ra[i]-w[x][i];
				pre[i]=y;
			}
			if(slack[i]<delta) delta=slack[i],yy=i; 
		}
		for(int i=0;i<=n;i++){
			if(vis[i]){
				la[match[i]]-=delta;
				ra[i]+=delta;
			}else{
				slack[i]-=delta;
			}
		} 
		y=yy;
		if(match[y]==-1){
			break;
		}
	}
	while(y){
		match[y]=match[pre[y]];
		y=pre[y];
	}
}
int KM(){
	memset(match,-1,sizeof(match));
	memset(la,0,sizeof(la));
	memset(ra,0,sizeof(ra));
	for(int i=1;i<=n;i++){
		memset(vis,0,sizeof(vis));
		bfs(i);
	}
	int ans=0;
	for(int i=1;i<=n;i++){
		if(match[i]!=-1){
			ans+=w[match[i]][i];
		}
	}
	return ans;
}
signed main(){//与int main()一样，但是由于使用了#define int long long,觉得这点非常nice
	long long ans=0;
	cin>>n;
	int x,y,z,d;
	for(int i=1;i<=n;++i){
		cin>>x>>y>>z>>d;
		ans+=x*x+y*y;
		for(int j=1;j<=n;++j){
			x=z+d*(j-1);
			w[i][j]=-x*x;
		}
	}
	cout<<ans-KM()<<endl;
	return 0;
}
```



### K - King of Range

Given nn_{}n integers a1,a2,⋯ ,ana_1,a_2,\cdots,a_na1,a2,⋯,an and mm_{}m queries. For each query, you are given a const kk_{}k and you should determine how many different pairs (l,r)(l,r)_{}(l,r) are there meeting the condition that the range of the subsequence al,al+1,⋯ ,ara_l,a_{l+1},\cdots,a_ral,al+1,⋯,ar is strictly greater than kk_{}k.
 
 Note: the range of a sequence equals the difference between the maximum and the minimum of the sequence.

#### 分析

问有多少个子序列满足：最大值和最小值之差严格大于k。

如果子序列$[i,j]$​满足该条件，则所有包含该子序列的所有子序列，都能满足。

#### 代码

分别维护最小值的递增队列和最大值的递减队列。找到一个满足条件的区间$[i,j]$​，则向后所有的$n-j+1$个右区间都满足条件。

```c++
#include<bits/stdc++.h>
using namespace std;
int m,n,k;
long long ans;
int mm,mx,mi,xi;
int a[100005],maxi[100005],mini[100005];//mini为维护最小值得递增队列，maxi为维护最大值的递减队列
int main()
{
    cin>>n>>m;
    for(int i=1;i<=n;i++)cin>>a[i];
    while(m--){
        ans=0;
        cin>>k;
        mm=mx=mi=xi=1;//mm，mx，mi，xi分别是maxi队首队尾和mini队首队尾
        maxi[1]=mini[1]=1;
        int j=1;
        for(int i=1;i<=n;i++){
            if(maxi[mx]<i)mx++;
            if(mini[mm]<i)mm++;
            while(j<i||a[maxi[mx]]-a[mini[mm]]<=k){
                if(j==n)break;
                j++;
                while(xi>=mx&&a[maxi[xi]]<a[j])--xi;//维护单调递减队列
                while(mi>=mm&&a[mini[mi]]>a[j])--mi;//维护单调递增队列

                maxi[++xi]=j;
                mini[++mi]=j;
            }
            if(a[maxi[mx]]-a[mini[mm]]<=k)break;
            ans+=n-j+1;//j后面的所有右区间都满足条件        
        }
         
        cout<<ans<<endl;
    }
    return 0;
}
```

