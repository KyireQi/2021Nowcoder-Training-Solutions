# 2021牛客暑期多校训练营8

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

