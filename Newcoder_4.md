# 2021牛客暑期多校训练营4

## 题解

### LCS

#### 题目描述

Let $LCS(s1,s2)$ denote the length of the longest common subsequence (not necessary continuity) of string $s1$ and string $s2$.

Now give you four integers a,b,c,na,b,c,na,b,c,n, you need to find three lowercase character strings $s1, s2 , s3$ satisfy that $∣s1∣=∣s2∣=∣s3∣= n  $

and  $LCS(s1,s2)=a,LCS(s2,s3)=b,LCS(s1,s3)=c$。

##题目大意
定义$LCS(s1,s2)=a$的含义为两个字符串$s1，s2$中有$a$个相同的字符,如

$s1:$ aabbcx
$s2:$ aacyyy
$s3:$ aabbzz

其中 $LCS(s1,s2)=3, LCS(s2,s3)=2, LCS(s1,s3)=4$,
给定 $a,b,c,n $ 四个数字，要求按照规则输出三个字符串，字符串的长度都为n，并且$a,b,c$的值分别为$LCS（s1，s2），LCS（s2，s3），LCS（s3,s1)$的值，求三个符合条件的字符串。若无法符合题目条件则输出“NO”
##题目分析
在这道题目中，通过概念可知，输出的三个字符串应该在满足$LCS$为$a,b,c$的情况下同时使得字符串长度最小，才能最大化满足$n$的条件。同时若要满足最短的输出条件就应该找出$a,b,c$中最少的一个数字，并且用一个相同的字符填充这个最小值的次数，之后对每个字符串来说，填充字母只需要让$a,b,c$每个字母减去他们的最小值后进行填充，例如在$a,b,c,n$分别为$3,4,5,7$的情况下，最小的值为$3$，则每个字符串在填充完$3$个a后，第一个字符串再填充$(3-3)$个b和$(5-3)$个d，最后填充$(7-5)$个x，按着这样的规律最后得到的字符串如下：


$s1:$ aaaddxx
$s2:$ aaacyyy
$s3:$ aaacddz

同时我们也可以发现，假设$a$为$a,b,c$中最小的数值，则$n$的值不能小于$(b+c-a)$,若$n$小于$(b+c-a)$则三个数组无法全部构建。即输出NO。代码片段如下：

```c++
    
#include<bits/stdc++.h>
using namespace std;

int MIN(int a,int b,int c){
    return min(a,min(b,c));
}

int main(){
    int a,b,c,n;
    while(cin>>a>>b>>c>>n){
    int m = MIN(a,b,c);
    a -= m,b -= m,c -= m;
    if(a + b + c + m > n){
        puts("NO");
        return 0;
    }

    string s1(m,'a'),s2(m,'a'),s3(m,'a');
    while(a--)s1+='b',s2+='b';
    while(b--)s2+='c',s3+='c';
    while(c--)s3+='d',s1+='d';
    while(s1.size() < n)s1 += 'x';
    while(s2.size() < n)s2 += 'y';
    while(s3.size() < n)s3 += 'z';
    cout<<s1<<"\n"<<s2<<"\n"<<s3;
    }
}

```

### F - Just a joke

#### 题目

Alice and Bob are playing a game.

 At the beginning, there is an undirected graph $G$ with $n$ nodes.

 Alice and Bob take turns to operate, Alice will play first. The player who can't operate will lose the game.

 Each turn, the player should do one of the following operations.

1. Select an edge of $G$ and delete it from $G$.

2. Select a connected component of $G$ which doesn't have any loop, then delete it from $G$.

 Alice and Bob are smart enough, you need to find who will win this game.

 A connected component of an undirected graph is a set of nodes such that each pair of nodes is connected by a path, and other nodes in the graph are not connected to the nodes in this set.

 For example, for graph with 333 nodes and edge set $\{(1,2),(2,3),(1,3)\}. $ is a connected component but $\{1,2\},\{1,3\}$ are not. 

#### 分析

玩家可以做两个操作：（1）删除一条边（2）删除一整个不成环的连通分量。两人依次操作，当可操作步数为奇数时，先手胜；反之后手胜。

由于单个点也算作一个连通分量，所以当删除一个连通分量中的某条边时，不改变剩余步数的奇偶性（删除之后把一个连通分量断开变成两个连通分量，对方再操作后，奇偶性不变）。

对于成环的连通分量，第一步首先要删掉其中一条边，把环断开（每多一个环就要多一步操作），该操作影响奇偶性。

所以只要统计连通分量的个数+环的个数，判断奇偶，即可得出答案。

#### 代码实现

用并查集求出连通分量的个数，当进行Merge操作时如果发现合并的两个节点已经在同一连通分量中，则成环。最后把环数和连通分量数加起来，奇数Alice胜。

```c++
#include<iostream>
using namespace std;
int f[105];
int get(int x){
    if(f[x]!=x){
        return f[x]=get(f[x]);
    }
    return x;
}
void merge(int x,int y){
    if(get(x)!=get(y)){
        f[get(x)]=y;
    }
}
int main(){
    for(int i=0;i<105;i++){
        f[i]=i;
    }
    int n,m;
    cin>>n>>m;
    int ans=0;
    for(int i=0;i<m;i++){
        int x,y;
        cin>>x>>y;
        if(get(x)!=get(y)){
            merge(x,y);
        }
        else{
            ans++;
        }
    }
    int k;
    for(int i=1;i<=n;i++){
        if(f[i]==i){
            k++;   
        }
    }
    ans+=k;
    if(ans&1){
        cout<<"Alice"<<endl;
    }
    else cout<<"Bob"<<endl;
    return 0;
}
```

#### 大佬的题解

看了大佬的分析，发现有更简单的做法。

有两种操作：
（1）删除一条边（边的数目-1）
（2）删除个连通分量（点的数目-k，边的数目-(k-1) ）
所以每次操作边与点的和的奇偶性都会变化，所以只需要判断边加点和的奇偶性即可。

```c++
#include<cstdio>

int main() {
    int n,m,x,y;
    scanf("%d%d",&n,&m);
    for(int i=0;i<m;i++) 
        scanf("%d%d",&x,&y);
    if((n+m)&1) puts("Alice");
    else puts("Bob");
}
```

### I - Inverse Pair

#### 题目

For a sequence $t_{1...n}$​ ,we define the weight of it is the number of pairs $(i,j)$​satisfy $i<j$​ and $t_i>t_j$​.

Now give you a permutation $a_{1...n}$​​​, you need to choose a sequence $b_{1...n}$​​​ satisfies $b_i\in \{0,1\}$​ to minimize the weight of sequence $c_{1...n}$​ which satisfies $c_i=a_i+b_i$​.

#### 分析

题目大意是：一个长度为$n$​的数列$\{a_n\}$​，其中$a_i \in \{1,2...n\}$​ 。通过给序列$a$​的若干数加$1$​，使序列$a$​的逆序对减少，问操作后最少的逆序对数量是多少。

可以看出，每次加一的操作，最多可以减掉一个逆序对，且只有当逆序对满足$i< j$​且$a_i+1=a_j$​​时，对$a_j$​​加一才能减掉该逆序对。只要计算原数组的逆序数，从中减去可以减掉的所有逆序对数量，就是题目的答案。

对于数列`5 4 3 2 1`我们可以给他们两两分组后者加一，变为`5 5 3 3 1`。可以证明，对于这样差为1的递减数列，可通过上述操作减去的逆序对数量为$\lfloor \frac n 2 \rfloor$​。我们可以从题目给的数列$\{a_n\}$​​​中，按照这个方式，对每一个差为1的单调递减子序列进行该操作。

#### 代码实现

在输入过程中用一个`is[]`数组记录该数字是否出现过，用一个`add[]`数组判断该数字是否执行加一操作。判断当前数字$a_i$​的前面，如果存在$a_i+1$​，且不执行加一操作，那么将$a_i$​​执行加一操作，并计一次可减逆序对数。最后用归并排序求得原数组的逆序数，减去之前的计数，就是题目的答案。

```c++
#include <cstdio>
#include <cstring>
using namespace std;

const int maxn = 2e5+5;

int a[maxn];
bool is[maxn]={0},add[maxn]={0};

long long ans=0;

void Merge(int* A,int l,int mid,int r,int* C){
    int i=l;
    int j=mid+1;
    int k=l;
    while(i<=mid && j<=r) {
        if(A[i]<=A[j]){
            C[k++]=A[i++];
        } else {
            C[k++]=A[j++];
            ans += mid-i+1;
        }
    }
    while(i<=mid)
        C[k++]=A[i++];
    while(j<=r)
        C[k++]=A[j++];
    for(int i=l;i<=r;++i)
        A[i]=C[i];
}

void MergeSort(int* A,int l,int r,int* C) {
    if(l < r) {
        int mid=(l+r)/2;
        MergeSort(A,l,mid,C);
        MergeSort(A,mid+1,r,C);
        Merge(A,l,mid,r,C);
    }
}

int main(){
    int n;
    scanf("%d",&n);
    long long cnt=0;
    for (int i=1;i<=n;i++){
        scanf("%d",a+i);
        is[a[i]]=1;
        if (is[a[i]+1]==1 && add[a[i]+1]==0) {
            add[a[i]]=1;
            cnt++;
        }
    }
    int *C=new int[n];
    MergeSort(a,1,n,C);
    printf("%lld",ans-cnt);
    return 0;
}
```

### J - Average

#### 题目

Bob has an $n×m$​ matrix $W$ . 

 This matrix is very special, It's calculated by two sequences $a_{1...n}$​​,$b_{1...m}$​​,$\forall i \in[1,n]$​​,$\forall j \in[1,m]$​​,$W_{i,j}=a_i+b_j$​

 Now Bob wants to find a submatrix of WWW with the largest average value.

 Bob doesn't want the size of submatrix to be too small, so the submatrix you find must satisfy that the height(the first dimension of matrix) of it is at least $x$​ and the width(the second dimension of matrix) of it is at least $y$.

 Now you need to calculate the largest average value.

#### 分析

题目大意是：两个长度分别为$n,m$的序列 $a_{1...n}$,$b_{1...m}$，一个 $n$行$m$列的矩阵$W$，矩阵中的点的权值由这两个序列所决定，$W_{i,j}=a_i+b_j$​。求当子矩阵的行大于等于$x$，列大于等于$y$​时,子矩阵的平均数的最大值。

首先我们考虑一般的情况，任意一个子矩阵，假设它为$n$​行$m$​列，它的左上角的下标为$k,l$​。
$$
\frac {\sum_{i=k}^{k+n-1}\sum_{j=l}^{l+m-1}(a_i+b_j)}{n\times m}​​
$$

$$
\frac {m \times\sum_{i=k}^{k+n-1}a_i+n\times \sum_{j=l}^{l+m-1}b_j}{n\times m}​
$$

$$
\frac {\sum_{j=l}^{l+m-1}b_j}{m}+\frac {\sum_{i=k}^{k+n-1}a_i}{n}
$$


经过计算，我们神奇的发现子矩阵的平均值是由两个序列独立决定的，所以原题所求等价于在序列$a_{1...n}$​​中求出长度大于$x$​​的子区间的平均数的最大值以及在序列$b_{1...m}$​​中求长度大于$y$​​​的子区间的平均数的最大值，所求即为它俩的和。这一点在打比赛的时候我们就已经推出来了，但是没有想到一个足够优越的算法来实现这个操作，赛后看过题解才知道计算序列中子区间的平均数的最大值是可以通过二分+前缀和实现的。

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

