# Nowcoder 多校联合第四场

## C.LCS

**题解：**

题意为，我们给出三个等长字符串$s_1$,$s_2$,$s_3$，定义$a = LCS(s_1,s_2)$，$b = LCS(s_2,s_3)$，$c = LCS(s_1,s_3)$。要求构造出满足条件的三个字符串，如果不存在输出$NO$。

先构造公共前缀，取$a$,$b$,$c$中最小的，记为$min$作为三个字符串公共前缀，为了便于分析我们把公共前缀写为：$aaa....a$

此后我们令$a - min;b - min;c- min$，这样得到的三个数中必定至少有一个是$0$，剩余的数我们可以认为是从他对应的字符串身上继承下来的。比如$a = 0$，那就相当于是$s_1 = a ... a$，$s_2 = a .. a b...b$除了前缀外都不一样，$s_3$的后缀来源于两方面：一方面是根据$b$的值，从$s_2$中后的非公共部分拿出$b$个，另一方面是根据$c$的值，从$s_3$后的非公共部分拿出$c$个，最后补充到要求长度即可。

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
string A,B,C;
int a,b,c,n;

int main()
{
    ios;
    cin >> a >> b >> c >> n;
    int minn = min(min(a,b),c);
    a -= minn;b -= minn;c -= minn;
    char t = 'a';
    for (int i = 1;i <= minn;i ++) {
        A += t;
        B += t;
        C += t;
    }
    if (a == 0) {
        if (minn + b + c > n) {
            cout << "NO" << endl;
            return 0;
        }
        for (int i = 1;i <= n - minn;i ++) A += (t);
        for (int i = 1;i <= n - minn;i ++) B += (t + 1);
        for (int i = 1;i <= b;i ++) C += (t + 1);
        for (int i = 1;i <= c;i ++) C += t;
        for (int i = 1;i <= n - minn - b - c;i ++) C += (t + 2);
        cout << A << endl << B << endl << C << endl;
    }
    else if (b == 0) {
        if (minn + a + c > n) {
            cout << "NO" << endl;
            return 0;
        }
        for (int i = 1;i <= n - minn;i ++) C += (t);
        for (int i = 1;i <= n - minn;i ++) B += (t + 1);
        for (int i = 1;i <= a;i ++) A += (t + 1);
        for (int i = 1;i <= c;i ++) A += t;
        for (int i = 1;i <= n - minn - a - c;i ++) A += (t + 2);
        cout << A << endl << B << endl << C << endl;
    }
    else {
        if (minn + a + b > n) {
            cout << "NO" << endl;
            return 0;
        }
        for (int i = 1;i <= n - minn;i ++) C += (t);
        for (int i = 1;i <= n - minn;i ++) A += (t + 1);
        for (int i = 1;i <= a;i ++) B += (t + 1);
        for (int i = 1;i <= b;i ++) B += t;
        for (int i = 1;i <= n - minn - a - b;i ++) B += (t + 2);
        cout << A << endl << B << endl << C << endl;
    }
    return 0;
}
```

## F.Just a Joke

**题解：**

每一次有两种选择的删除方法，第一种是我们直接删除一整个连通分量（**不能含有环**），第二种是只能删一条边，但是注意，**删除这个边后还是有点存在的！**这个点也是需要单独删除的。我们其实可以直接统计点和边的个数，不用考虑连边的问题，因为不管执行哪一种操作，最后得到的一定是边点和为奇数时`Alice`胜，否则`Bob`胜。

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

int main()
{
    int n,m;
    cin >> n >> m;
    int x,y;
    for(int i = 0;i < m;i++) cin >> x >> y;
    if ((n + m) & 1) puts("Alice");
    else puts("Bob");
    return 0;
}
```

## I.Inverse Pair

**题解：**

其实很水的一个题，算出逆序对，然后因为本身序列是一个排列，所以我们可以扫描序列，假设当前的值为$P$，则我们只需要判断$P -1$是否在$P$的前面就可以了。如果在前面就直接加一，逆序对就少了一对。

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
 
const int maxn = 2e5 + 10;
int s1[maxn] = {},n,s2[maxn] = {},flag[maxn] = {},a[maxn] = {};
ll cnt = 0;
map<int,int > mp;
 
void merge(int L, int R, int Mid)
{
    int i = L;int j = Mid + 1;int k = L;
    while(i <= Mid && j <= R){
        if(s1[i] <= s1[j])s2[k ++] = s1[i ++];
        else{
            cnt += Mid - i + 1;
            s2[k ++] = s1[j ++];
        }
    }
    while(i <= Mid) s2[k ++] = s1[i ++];
    while(j <= R) s2[k ++] = s1[j ++];
    for(i = L; i <= R; i ++) s1[i] = s2[i];
}
 
void mergesort(int L, int R)
{
    if(L < R){
        int Mid = (L + R) / 2;
        mergesort(L, Mid),mergesort(Mid + 1, R);
        merge(L, R, Mid);
    }
}
 
int main()
{
    scanf ("%d", &n);
    for (int i = 1; i <= n; i ++) {
        scanf ("%d",&s1[i]);
        a[i] = s1[i];
        mp[a[i]] = i;
    }
    mergesort (1,n);
    for (int i = 1;i <= n;i ++) {
        int now = mp[a[i]];
        if (flag[mp[a[i]]] == 1) continue;
        if (a[i] == 1) continue;
        else {
            int last = mp[a[i] - 1];
            if (last > now) {
                cnt --;
                flag[mp[a[i] - 1]] = 1;
            }
        }
    }
    cout << cnt << endl;
    return 0;
}
```

## J.Average

**题解：**

二分+尺取

核心思想就是把一个二维数组的求和优化到两个一维数组上，这样我们就把问题转换成了，对于两个一维数组，分别求出他们的子数组最大平均值即可，这就用到了尺取和二分的思想。

**AC代码：**

```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXN = 1e5;
int arr[MAXN + 5];
double sum[MAXN + 5];
int arr2[MAXN + 5];
double sum2[MAXN + 5];
int n, m;
int x,y;
bool check(double avg)
{
    for(int i = 1; i <= n; ++i) sum[i] = sum[i - 1] + arr[i] - avg;
    double minv = 0;
    for(int i = x; i <= n; ++i) {
        minv = min(minv, sum[i - x]);
        if(sum[i] >= minv) return true;
    }
    return false;
}
bool check2(double avg)
{
    for(int i = 1; i <= m; ++i) sum[i] = sum[i - 1] + arr2[i] - avg;
    double minv = 0;
    for(int i = y; i <= m; ++i) {
        minv = min(minv, sum[i - y]);
        if(sum[i] >= minv) return true;
    }
    return false;
}
int main()
{
    
    scanf("%d%d%d%d", &n,&m,&x,&y);  // cin  cout
    double l = 0, r = 0;
    for(int i = 1; i <= n; ++i) {
        scanf("%d", arr + i);
        r = max(r, double(arr[i]));
    }
    double r2 = 0;
    double l2 = 0 ;
    for(int i = 1; i <= m; ++i) {
        scanf("%d", arr2 + i);
        r2 = max(r2, double(arr2[i]));
    }
    while(r - l > 1e-7) {//二分法
        double mid = (l + r) / 2;
        if(check(mid)) l = mid;
        else r = mid;
    }
    while(r2 - l2 > 1e-7) {//二分法
        double mid = (l2 + r2) / 2;
        if(check2(mid)) l2 = mid;
        else r2 = mid;
    }

    printf("%.7lf\n",r2+r);
    return 0;
}
```

