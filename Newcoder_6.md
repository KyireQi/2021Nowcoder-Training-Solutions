# 2021牛客暑期多校训练营6

## 题解

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

