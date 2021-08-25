# Nowcoder 多校联合第六场

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

## H.Hopping Rabbit

扫描线问题

首先这个题这个兔子只能挖一次洞，换句话说如果确定了兔子的起始位置，我们就一定可以知道它可以到达的洞口：$(x + d,y),(x - d ,y),(x, y + d),(x, y - d)$。我们可以把所有陷阱全部放到一个$d \times d$的矩形里，每一个陷阱在这个区域里的位置就是他们**相对于兔子可达点**的位置。所以我们可以把问题直接简化到一个$(1,1)$到$(d,d)$的矩形上。最后需要做的就是维护每一列有没有空隙，相当于维护了一个区间最小值，如果有的话就直接输出坐标即可，这里就是扫描线的问题了。

**AC代码：**

```cpp
#include <bits/stdc++.h>

#define ill __int128
#define ll long long
#define PII pair <int,int>
#define ull unsigned long long
#define me(a,b) memset (a,b,sizeof(a))
#define rep(i,a,b) for (int i = a;i <= b;i ++)
#define req(i,a,b) for (int i = a;i >= b;i --)
#define ios std :: ios :: sync_with_stdio(false)

const double Exp = 1e-9;
const int INF = 0x3f3f3f3f;
const int inf = -0x3f3f3f3f;
const int mode = 1000000007;
const double pi = 3.141592653589793;

using namespace std;

const int maxn = 2e5 + 10;
int n, d;
vector<PII> In[maxn], Out[maxn];

struct SegmenTree
{
    int l, r;
    int val, add;
}t[maxn << 2];

inline void pushdown(int p)
{
    if (t[p].add) {
        t[2 * p].add += t[p].add;t[2 * p + 1].add += t[p].add;
        t[2 * p].val += t[p].add;t[2 * p + 1].val += t[p].add;
        t[p].add = 0;
    }
    return ;
}

inline void pushup(int p)
{
    t[p].val = min(t[2 * p].val, t[2 * p + 1].val);
    return ;
}

inline void buildTree(int p, int l, int r)
{
    t[p].l = l;t[p].r = r;
    t[p].val = t[p].add = 0;
    if (l == r) return ;
    int mid = (l + r) >> 1;
    buildTree(2 * p, l, mid);
    buildTree(2 * p + 1, mid + 1, r);
    pushup(p);
    return ;
}

inline void solve(int x1, int y1, int x2, int y2)
{   
    int xl, yl, xr, yr;
    if (x2 - x1 >= d) {
        xl = 1;
        xr = d;
    }
    else {
        xl = (x1 % d + d) % d + 1;
        xr = ((x2 - 1) % d + d) % d + 1;
    }
    if (y2 - y1 >= d) {
        yl = 1;
        yr = d;
    }
    else {
        yl = (y1 % d + d) % d + 1;
        yr = ((y2 - 1) % d + d) % d + 1;
    }
    if (xl <= xr) {
        if (yl <= yr) {
            In[xl].push_back({yl, yr});
            Out[xr + 1].push_back({yl, yr});
        }
        else {
            In[xl].push_back({yl, d});
            Out[xr + 1].push_back({yl, d});
            In[xl].push_back({1, yr});
            Out[xr + 1].push_back({1, yr});
        }
    }
    else {
        if (yl <= yr) {
            In[xl].push_back({yl, yr});
            Out[d + 1].push_back({yl, yr});
            In[1].push_back({yl, yr});
            Out[xr + 1].push_back({yl, yr});
        }
        else {
            In[xl].push_back({1, yr});
            Out[d + 1].push_back({1, yr});
            In[xl].push_back({yl, d});
            Out[d + 1].push_back({yl, d});
            In[1].push_back({1, yr});
            Out[xr + 1].push_back({1, yr});
            In[1].push_back({yl, d});
            Out[xr + 1].push_back({yl, d});
        }
    }
    return ;
}

inline int ask(int p, int l, int r)
{
    if (l <= t[p].l && t[p].r <= r) return t[p].val;
    pushdown(p);
    int mid = (t[p].l + t[p].r) >> 1;
    int ans = INF;
    if (l <= mid) ans = min(ans, ask(2 * p, l, r));
    if (r > mid) ans = min(ans, ask(2 * p + 1, l, r));
    return ans;
}

inline void update(int p, int l, int r, int d)
{
    if (t[p].l >= l && t[p].r <= r) {
        t[p].val += d;
        t[p].add += d;
        return ;
    }
    pushdown(p);
    int mid = (t[p].l + t[p].r) >> 1;
    if (l <= mid) update (2 * p, l, r, d);
    if (r > mid) update (2 * p + 1, l, r, d);
    pushup(p);
    return ;
}

int main()
{
    scanf ("%d%d", &n, &d);
    for (int i = 1;i <= n;i ++) {
        int x1, x2, y1, y2;
        scanf ("%d%d%d%d", &x1, &y1, &x2, &y2);
        solve(x1, y1, x2, y2);
    }
    buildTree(1, 1, d);
    int ansx, ansy, flag = 0;
    for (int i = 1;i <= d;i ++) {
        for (auto k : In[i]) update(1, k.first, k.second, 1);
        for (auto k : Out[i]) update(1, k.first, k.second, -1);
        if (ask(1, 1, d) == 0) {
            for (int j = 1;j <= d;j ++) {
                if (ask(1, j, j) == 0) {
                    ansx = i;
                    ansy = j;
                    flag = 1;
                    break;
                }
            }
            break;
        }
    }
    if (flag) {
        printf ("YES\n");
        printf ("%d %d\n", ansx - 1 + d, ansy - 1 + d);
    }
    else printf ("NO\n");
    return 0;
}
```

