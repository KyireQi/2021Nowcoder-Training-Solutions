# 2021牛客暑期多校训练营9

## 题解

### H - Happy Number

Digits 2, 3 and 6 are happy, while all others are unhappy. An integer is happy if it contains only happy digits in its decimal notation. For example, 2, 3, 263 are happy numbers, while 231 is not.

 Now Cuber QQ wants to know the n-th happy positive integer.

#### 分析

观察发现，$n$位的happy number共有$3^n$​个。可以先找出第$i$个数的位数$n$，减去$\sum_{i=1}^{n-1} 3^i$，即为当前数在所有n位的happy number中的序号，转换为三进制后，012分别对应236。

#### 代码

```c++
#include <iostream>
#include <cstdio>
using namespace std;

typedef long long ll;

int bi[4]={2,3,6};
int b[100]={};
int cnt=0;

int main(){
    ll n;
    scanf("%lld",&n);

    int bit=0,r=0,k=1;
    while (r<n) {
        k*=3;
        r+=k;
        bit++;
    }

    n-=r-k+1;

    while (n) {
        b[++cnt]=n%3;
        n/=3;
    }
    for (int i=bit;i>cnt;i--) {
        printf("%d",bi[0]);
    }
    for (int i=cnt;i;i--) {
        printf("%d",bi[b[i]]);
    }
    
    return 0;
}

```

### E - Eyjafjalla

#### 题目描述

There are n cities in the volcano country, numbered from 1 to n. The city 1 is the capital of the country, called Eyjafjalla. A large volcano is located in the capital, so the temperature there is quite high. The temperature of city i is denoted by tit_iti.

 The n cities are connected by n-1 undirected roads. The i-th road connects city uiu_iui​ and city viv_ivi​. If uiu_iui​ is closer to the capital than viv_ivi​, then tui>tvit_{u_i}>t_{v_i}tui​​>tvi​​. The capital has the highest temperature.

 The coronavirus is now spreading across the whole world. Although the volcano country is far away from the mess, Cuber QQ, the prime minister of the country, is still designing an emergency plan in case some city getting infected. Now he wants to know if a virus whose survival temperature range is [l, r] breaks out in city x, how many cities will be infected. The assumption is that, if the temperature of one city is between l and r inclusive, and it is connected to another infected city, it is infected too.

#### 分析

n个城市由n-1条道路相连，首都温度最高，并且其他城市的温度随着远离首都而下降，现在在某一城市爆发病毒，病毒存活的温度区间是$[l,r]$​，如果两个城市相连并且温度适宜，病毒就会传播。

回答m次询问：在第i个城市爆发病毒，温度区间$[l，r]$​,病毒会传染几个城市

由于距离首都越近温度越高，可以发现，从第$i$个城市顺着树向上找，找到小于$r$的最高温度城市$j$，然后求$j$为根的这棵树上有多少节点温度大于$l$。



