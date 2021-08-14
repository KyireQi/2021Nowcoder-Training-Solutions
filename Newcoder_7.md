# 2021牛客暑期多校训练营7

## 题解

### I - xay loves or

xay loves or. He gives you x and s, you need to calculate how many positive integer y satisfy xor⁡y=sx \operatorname{or} y=sxory=s .

#### 分析

或运算。$x | y = s$​。

考虑二进制位上1的位置，当且仅当x为1的所有位，s也全为1，才可能存在满足条件的y​​。

存在满足条件的y时，y在x为0且s为1的位，必为1；在x为1且s为1的位，可任意；其余位必为0。

x为1的位数记为$b_x$​，则y可能的情况有$2^{b_x}$​种。特殊的当x等于s时，y不能为0，为$2^{b_x}-1$​​种。

#### 代码

```c++
#include<iostream>
#define ll long long
using namespace std;
int getn(ll  x){
    int res=0;
    while(x){
        x-=x&(-x);
        res++;
    }
    return res;
}
int main(){
    ll x,s;
    cin>>x>>s;
    if((x|s)!=s){
        cout<<0<<endl;
    }
    else{
        int xn=getn(x);
        if (x==s) {
            cout<<(1ll<<xn)-1<<endl;
        }
        else {
            cout<<(1ll<<xn)<<endl;
        }
    }
    return 0;
}
```



