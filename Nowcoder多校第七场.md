# 2021牛客暑期多校训练营7
[比赛链接](https://ac.nowcoder.com/acm/contest/11258)
## H xay loves count
&ensp;&ensp;乘法原理,枚举$a_k$,找到$a_k$所有的除数，用一个乘法原理即可。需要注意的是，(1,1,3),(1,3,1),为不同的方案数。
```cpp
#include<iostream>
#include<algorithm>
#include<cstdio>
#include<vector>
#include<map>
#include<string>
#include<math.h>
#include<cstring>
#include<queue>
#include<set>
#include<stack>
using namespace std;
#define debug(x) cout<<"###"<<x<<"###"<<endl;
typedef long long ll;
const double eps=1e-8;
const int INF=0x3f3f3f3f;
const int N=1e6+5;
int cnt[N];
int main(){
    int n;
    cin>>n;
    int x;
    for(int i=1;i<=n;i++){
        scanf("%d",&x);
        cnt[x]++;
    }
    ll ans=0;
    for(int i=1;i<=N-5;i++){
        if(cnt[i]){
            for(int j=1;j*j<=i;j++){
                if(i%j==0){
                    if(j*j==i){
                        ans+=cnt[j]*cnt[j]*cnt[i];
                    }
                    else{
                        ans+=2*cnt[i]*cnt[j]*cnt[i/j];
                    }
                }
            }
        }
    }
    cout<<ans<<endl;
}
```
