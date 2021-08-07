# Nowcoder 多校联合第六场

## K.King of Range

**题解：**

大概的意思就是，我们有一个环形的序列（可以想象成钟表）然后给了你$m$​个不相交的区间，现在让我们找$k$​个序列，能满足：这$k$个序列的交集就是$m$个区间的并集。

给的思路就是：我们每一次取当前区间的左端点，然后以当前的左端点为构造区间的左端点，选取他的上一个区间的右端点为构造区间的右端点（如果是第一个区间则取第$m$个区间对应的位置），最后我们发现这$m$个构造区间的交集就是所有给定区间的并集。

```cpp
#include <bits/stdc++.h>

#define ill __int128
#define ll long long
#define PII pair <ll,ll>
#define ull unsigned long long
#define me(a,b) memset (a,b,sizeof(a))
#define rep(i,a,b) for (int i = a;i <= b;i ++)
#define req(i,a,b) for (int i = a;i >= b;i --)
#define ios std :: ios :: sync_with_stdio(false)

const double Exp = 1e-9;
const int INF = 0x3f3f3f3f;
const int inf = -0x3f3f3f3f;
const ll mode = 1000000007;
const double pi = 3.141592653589793;

using namespace std;

const int maxn = 1e5 + 10;
ll a[maxn] = {}, k;
ll Q1[maxn], Q2[maxn];
int head1, head2, point1, point2, l;
int n ,m;

void init()
{
    me(Q1, 0);me(Q2, 0);
    head1 = head2 = 0;
    point1 = point2 = -1;
    l = 1;
}

void solve()
{
    ll ans = 0;
    scanf ("%lld", &k);
    for (int i = 1;i <= n;i ++) {
        while (head1 <= point1 && a[Q1[point1]] <= a[i]) point1 --;
        Q1[++ point1] = i;
        while (head2 <= point2 && a[Q2[point2]] >= a[i]) point2 --;
        Q2[++ point2] = i;
        while (l <= i && a[Q1[head1]] - a[Q2[head2]] > k) {
            ans += (n - i + 1);
            l ++;
            if (l > Q1[head1]) head1 ++;
            if (l > Q2[head2]) head2 ++;
        }
    }
    printf ("%lld\n", ans);
    return ;
}

int main()
{
    scanf ("%d%d", &n, &m);
    for (int i = 1;i <= n;i ++) scanf ("%lld", &a[i]);
    while ( m -- ) {
        init();
        solve();
    }
    return 0;
}
```

## F.Hamburger Steak

**题解：**

没错果然电子科技大学死性不改......

又是个二分答案，又是绵阳站的噩梦...

其实二分答案都不用，我们选取所有$t_i$​​中的最大值和$\frac{\sum_{i = 1}^{n}t_i}{m}$​​之间的最大值，然后就以他作为我们最小的花费，我们可以把这个问题理解为：我们有$m$​​个空间，每一个空间的大小我们确定为$t$​​，然后我们需要把$n$​​个物品塞进这些空间里，而且允许分割（注意这里的分割**必须遵守时间流**）那这个问题就转换为了，我们求解这$n$个物品在塞进空间里得时候的时间和位置。

**AC代码：**

```cpp
#include <bits/stdc++.h>

#define ill __int128
#define ll long long
#define PII pair <ll,ll>
#define ull unsigned long long
#define me(a,b) memset (a,b,sizeof(a))
#define rep(i,a,b) for (int i = a;i <= b;i ++)
#define req(i,a,b) for (int i = a;i >= b;i --)
#define ios std :: ios :: sync_with_stdio(false)

const double Exp = 1e-9;
const int INF = 0x3f3f3f3f;
const int inf = -0x3f3f3f3f;
const ll mode = 1000000007;
const double pi = 3.141592653589793;

using namespace std;

const int maxn = 1e5 + 10;
int n, m;
int flas[maxn] = {};
ll t[maxn] = {};

int main()
{
    ll sum = 0, maxx = 0;
    scanf ("%d%d",&n, &m);
    for (int i = 1;i <= n;i ++) {
        scanf ("%lld",&t[i]);
        sum += t[i];
        maxx = max(maxx, t[i]);
    }
    maxx = max(maxx, (sum + m - 1) / m);
    ll now = 0;int p = 1;
    for (int i = 1;i <= n;i ++) {
        if (t[i] + now <= maxx) {  //一个空间够放
            printf ("1 %d %lld %lld\n",p, now, now + t[i]);   
            now += t[i];
            if (now == maxx) {
                p ++;
                now = 0;
            }
        } 
        else {//需要放到两个空间里
            printf ("2 %d %lld %lld %d %lld %lld\n",p + 1, 0, (now + t[i]) % maxx, p, now, maxx);
            p ++;
            now = (now + t[i]) % maxx;
        }
    }
    return 0;
}
```

## H.Holding Two

先咕咕了......后面补上QAQ