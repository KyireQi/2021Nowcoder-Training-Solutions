# 2021牛客暑期多校训练营6

## 题解

### H - Hamburger Steak

#### 题目描述

Riko is ready to cook hamburger steaks. There are m pans and n hamburger steaks that need to be fried. The i-th hamburger steak needs to be fried for ti (which is a positive integer) minutes. Riko can fry it in a certain pan for ti​ minutes, or in two different pans for ai​ and bi​ minutes respectively, where ai and bi​ are both positive integers and ai+bi=ti. Riko will start cooking at time 0 and she wants to finish cooking as soon as possible. Please help Riko make a plan to minimize the time spent cooking all the hamburger steaks. 

In this problem, we assume that a pan can fry at most one hamburger steak at the same time, and a hamburger steak can be put in at most one pan at the same time. Different pans can fry different hamburger steaks at the same time. We also assume that it takes no time to put a hamburger steak in a pan or take it out.
##题目大意
Riko 是一个厨师，他有$m$个锅和$n$块肉饼，一个锅一次只能烹饪一块肉饼，每块肉饼烹饪时需要的时间各不相同，同时，每块肉饼也可以被分成两次烹饪，被分成两次烹饪时，满足两次烹饪的总时间和之前一次烹饪的时间相同。求在最短时间内烹饪全部肉饼的一种方式。
##题目分析
由于每块肉饼都可以最多被分成两次烹饪，因此对于任意的样例来说，设$n$为样例的肉饼总数，设$maxn$为这$n$个肉饼中烹饪要求时间最长的一个肉饼，$sum$为这$n$个肉饼全部烹饪完成的时间，则烹饪要求的最小值为$max(maxn，\frac{sum}{n})$。找到符合题意的烹饪时间后只需要从第一个锅开始往里面安排肉饼被烹饪，当肉饼总烹饪时间超过我们设定好的烹饪时间后，把超出烹饪时间的部分肉饼放入下一个锅中。

``` c++
#include<bits/stdc++.h>
using namespace std;
long long int ans=0,A[100010],m,n,maxn=0,base=0,guo=1;
int main() {
	scanf("%lld%lld",&m,&n);
	for(int i=0; i<m; i++)scanf("%lld",&A[i]),ans+=A[i],maxn=maxn>A[i]?maxn:A[i];
	maxn=(ans+n-1)/n>maxn?(ans+n-1)/n:maxn;
	for(int i=0; i<m; i++) {
		if(base+A[i]<=maxn) {
			printf("1 %lld %lld %lld\n",guo,base,base+A[i]);
			if(base+A[i]==maxn)guo++,base=0;
			else base+=A[i];
		} else {
			printf("2 %lld 0 %lld %lld %lld %lld\n",guo+1,base+A[i]-maxn,guo,base,maxn);
			base=base+A[i]-maxn,guo++;
		}
	}
}
```

### I - Intervals on the Ring

There is a ring of numbers consisting of 111 to nnn sequentially. For every number iii (1≤i≤n−1)(1 \leq i \leq n - 1)(1≤i≤n−1), iii and i+1i+1i+1 are adjacent to each other. Particularly, nnn and 111 are adjacent. We use [l,r][l,r][l,r] to describe an interval on the ring. Formally, if l≤rl \leq rl≤r, then the interval [l,r][l,r][l,r] is equivalent to the set {l,l+1,…,r−1,r}\{ l, l + 1, \ldots, r - 1, r \}{l,l+1,…,r−1,r}. Otherwise, the interval [l,r][l,r][l,r] is equivalent to {l,l+1,…,n,1,…,r−1,r}\{l,l+1,\ldots,n,1,\ldots,r-1,r\}{l,l+1,…,n,1,…,r−1,r}.  

   Yukikaze has mmm non-intersecting intervals. She wants you to construct a set of intervals such that the intersection of them is the union of the mmm intervals that Yukikaze has. The intersection of the intervals is the set of integers that the intervals have in common.

#### 分析

一个环，要求构造一组区间，使得所有区间的交集为给定的区间集合。

先对给定的区间以左端点从小到大排序。

显然，每一个构造的区间要覆盖全部的给定区间。构造$n$个区间，第$i$个构造区间：左端为第$i$个给定区间的左端，右端为第$i-1$个给定区间的右端，即可符合题意。

```c++
#include <cstdio>
#include <algorithm>
using namespace std;
 
const int maxn=2005;
 
struct c {
    int l,r;
}a[maxn<<1];
 
bool cmp(struct c a,struct c b) {
    return a.l<b.l;
}
 
int main() {
 
    int t,n,m;
    scanf("%d",&t);
    while (t--) {
        scanf("%d%d",&n,&m);
        for (int i=1;i<=m;i++){
            scanf("%d%d",&a[i].l,&a[i].r);
        }
        sort(a+1,a+m+1,cmp);
 
        printf("%d\n",m);
        printf("%d %d\n",a[1].l,a[m].r);
 
        for (int i=2;i<=m;i++){
            printf("%d %d\n",a[i].l,a[i-1].r);
        }
    }
    return 0;
}
```

