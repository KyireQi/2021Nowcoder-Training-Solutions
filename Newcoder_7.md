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



### H - xay loves count

#### 题目描述

xay has an array a of length n, he wants to know how many triples (i, j, k) satisfy ai×aj=ak​。
##题目大意
xay有一个数组，他想知道在这个数组里能找到多少个$i,j,k$满足$ai * aj = ak$（$i,j,k$可以相同）输出数组中这样的对数。
##题目分析
题目给定的数组最大值为1e6,则我们可以利用和桶排序类似的想法，对输入的数组进行桶排序，这样得到的$B$数组中第$n$个元素的值$B[n]$代表的就是n在之前数组中出现过的次数，之后，我们从大往小开始判断，设从大到小取出的值为$n$，则我们可以从数组最小值开始搜索到$\sqrt{n}$,设每次取出的值为$m$，则当$n\%m$等于$0$且$n/m$时，我们就对输出结果增加$B[m]*B[n/m]$的值。遍历完得到的结果即为答案。
代码片段如下

```c++
#include<iostream>
#include<algorithm>
#include<cmath>
using namespace std;
const int maxn =1e6+7;
int A[maxn];
int B[maxn];
long long ans=0;

int main() {
	int n;
	scanf("%d",&n);
	for(int i=0; i<n; i++) {
		scanf("%d",&A[i]);
		B[A[i]]++;
	}
	sort(A,A+n);
	
	for(int i=n-1; i>=0; i--) {
		for(int j=1; j<=sqrt(A[i]); j++) {

			if(B[j]==0||A[i]%j!=0||B[A[i]/j]==0) {
				continue;
			}
			if (j==sqrt(A[i])) {
				ans+=pow(B[j],2);
			} else if(B[A[i]]!=0) {
				ans+=B[j]*2*B[A[i]/j];
			}
		}
	}
	printf("%lld\n",ans);
}
```

