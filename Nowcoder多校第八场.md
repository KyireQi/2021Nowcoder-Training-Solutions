# Nowcoder 多校联合第八场

可以说是打的最不好的一场，越做题越差，应该去反思一下，为什么别的队都在进步，但是我们自己一直在退步，可以说这一场是非常简单的了，从写的题解就能看出来
最后就等于没出题

原因又下

- 过于浮躁，看题不能看看透，为什么不仔细思考
- 策略问题，可以先出A题，但是我们一直再看D和K
- K题想偏了，很明显的+3和+2没有看出来，转而去考虑1*2矩形的策略了，而这种策略明显是错误的，最后没有沉下心看A题
- D可以不出，因为确实没有想到那个公式，但是K和A实在是太可惜
- 还有不要受榜的影响，哪个题有人出就盲目的跟从，我觉得应该仔细看过题后在认真考虑是否在自己能力范围，在参考题榜定夺，（当然抢签到题除外）
- 沉下心很重要，不要过分的浮躁，过分的划水
  
AC代码


### A-Ares, Toilet Ares

最水的题目，大约算个连乘，在算个逆元，就可以了，注意特判x==0


```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
ll n,m,k,a,l;
ll mod = 4933;

ll MPow(ll a ,ll b,ll c){
    ll ans = 1;
    while(b){
        if(b&1)ans = ans * a % mod ;
        a = a*a%mod;
        b>>=1;
    }
    return ans ;
}

int main(){
    cin>>n>>m>>k>>a>>l;
    ll up = 1, down = 1;
    for(int i = 0;i < k;i++){
        ll x,y,z;
        scanf("%lld%lld%lld",&x,&y,&z);
        if(x==0)continue;
        up *= (z-y);
        up %= mod;
        down *= z;
        down %= mod;
    }
    printf("%lld\n",(a + up*MPow(down,mod-2,mod))%mod);
}
```

# D-OR
有$$ c[i] = a[i-1] + a[i] = a[i-1]|a[i]+a[i-1]a[i]  $$

然后按位去算，算31次，然后结果相乘，然后复杂度大幅小于1e9，大约在$O(2e6)$左右

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 2e5+5;
typedef long long ll;
int b[maxn],c[maxn];
int d[maxn];
int a1[maxn],a2[maxn];
int main(){
    int n;
    scanf("%d",&n);
    for(int i = 2; i <= n;i ++){
        scanf("%d",&b[i]);
    }
    int flag = 0 ;
    for(int i = 2;i <=n;i++){
        scanf("%d",&c[i]);
        d[i] =  c[i] - b[i];
        if(d[i]<0){
            flag = 1;
        }
    }
    if(flag){
        printf("0");
        return 0;
    }
    ll ans = 1;
    for(int i = 0;i<31;i++){
        a1[1] = 1; a2[1] = 0;
        int num = 2;
        int flag1 = 0,flag2 = 0;
        for(int j = 2; j <= n ;j++){
            if(num == 0)break;
            if(flag1==0){
                if(a1[j-1]){
                    a1[j] = (d[j]>>i)&1;
                    if((a1[j]|a1[j-1])!=((b[j]>>i)&1)){
                        // cout<<"1:"<<j<<" "<<((b[j]>>i)&1)<<endl;
                        flag1 = 1;
                        num--;
                    }
                }
                else {
                    a1[j] = (b[j]>>i)&1;
                    if((a1[j]&a1[j-1])!=((d[j]>>i)&1)){
                        // cout<<"2:"<<j<<endl;
                        flag1 = 1;
                        num--;
                    }
                }
            }
            if(flag2 ==0){
                if(a2[j-1]){
                    a2[j] = (d[j]>>i)&1;
                    if((a2[j]|a2[j-1])!=((b[j]>>i)&1)){
                        // cout<<"3:"<<j<<endl;
                        flag2 = 1;
                        num--;
                    }
                }
                else {
                    a2[j] = (b[j]>>i)&1;
                    if((a2[j]&a2[j-1])!=((d[j]>>i)&1)){
                        // cout<<"4:"<<j<<endl;
                        flag2 = 1;
                        num--;
                    }
                }
            }
            
        }
        ans *= num;
        // cout<<num<<"#"<<endl;
    }
    printf("%lld\n",ans);    
}

```

### K-Yet Another Problem About Pi
k题是方法有问题，也是脑子不够用，大致我们是不想浪费这个我们拥有的 $pi$这个长度的，然后我们就考虑把这个$pi$一段在边上，一段在对角线上，保证得到的数量最大就好了，按照北大想的，枚举了50次大约就够了，实际上我们确实可以这么做
```cpp
#include <bits/stdc++.h>
using namespace std;
#define pi acos(-1) 

int main(){
    int t;
    scanf("%d",&t);
    while(t--){
        double w,d;
        scanf("%lf%lf",&w,&d);
        double c =  sqrt(w*w + d*d);
        double mn = min(w,d);
        double res1 = pi/mn;
        if(res1 < 1){
            printf("4\n");
        }
        else{
            int  ans = 0;
            for(int i = 0 ;i < 100;i ++){
                  if (i * mn < pi)ans = max(ans , (int)((pi - i*mn)/c)*3 + i*2 + 4);
                    if (i * c < pi)ans = max(ans , (int)((pi - i*c)/mn)*2 + i*3 + 4);
            }
            printf("%d\n",ans);
        }
            
    }
}


```
