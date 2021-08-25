# 2021牛客暑期多校训练营8

## 题解

### A - Ares, Toilet Ares

#### 题目

Ares is the Greek god of courage and war. He is one of the Twelve Olympians and the son of Zeus and Hera. In Greek literature, he often represents the physical or violent and untamed aspect of war and is the personification of sheer brutality and bloodlust, in contrast to his sister, the armored Athena, whose functions as a goddess of intelligence include military strategy and generalship.

 A Toilet-Ares appeared at the western hub of the mysterious East. They took part in ShengJing AUPC(abnormal university programming competition). There are nnn problems in this competition. For some irresistible reason, the Toilet-Ares had got solutions to all of the problems. To ensure the fairness of AUPC, the groups of problems' authors changed the problems immediately. They changed $n-m$ problems in total and enhanced data ranges of simplest aaa of the rest mmm unchanged problems. The Toilet-Ares seemed to have a little intelligence to solve the simplest aaa of unchanged problems, while they seemed not willing to pass harder ones for fear of being exposed(or maybe because they didn't get solutions to the harder ones). They have kkk chances to go to the toilet. Every time they go to toilet, they can attain xix_ixi​ lines of code related to the problem 'k', while has pip_ipi​ probability (in the form of $\frac{y}{z}$​, $0 \leq y \leq z < 4933$​) of failure in solving the problem. Notice that the length of code to solve problem 'k' is lll. It is guaranteed that$\sum x_i = l$.

#### 分析

题目的大致意思就是整个比赛有$n$个题，改了$n-m$个题，剩下的$m$个题中的$a$个题进行了数据加强，你会做这$a$个题，你有$k$次机会，每次可以写出$x_i$行的代码，你不能写出的概率为$\frac{y}{z}$，$k$次机会能写出的代码的总和刚好可以做出这个题，求最后做出的题数的数学期望对4933取模，这道题无关信息很多，都是用来迷惑人的。看完我们可以确定需要用到逆元，先打好快速幂的板子就好了，数学期望直接算就好了，但是求数学期望有个坑，因为如果$x_i$为0，这部分概率是不能乘法进去的。还有一点就是虽然数据量只有几千，但是还是要考虑爆int，写代码要严谨。

#### 代码实现

```c++
#include<bits/stdc++.h>
const int mod=4933;
using namespace std;
int qsm(int x,int y){
	x%=mod;
	int res=1;
	while(y){
		if(y&1){
			res=(res*x)%mod;
		}
		y>>=1;
		x=(x*x)%mod;
	} 
	return res;
}
int iva(int x){
	return qsm(x,mod-2);
}
int main(){
	int n,m,k,a,l;
	scanf("%d%d%d%d%d",&n,&m,&k,&a,&l);
	int res=1;
	for(int i=1;i<=k;i++){
		int x,y,z;
		scanf("%d%d%d",&x,&y,&z);
		y=z-y;
		if(x!=0)
		res=((res*(y%mod))%mod*iva(z))%mod;
	}
	res=(a+res)%mod;
	printf("%d\n",res);
	return 0;
}
```

### D - OR

#### 题目

There are two sequences of length $n-1$​, $b=(b_2,b_3,\ldots,b_n)$​, $c=(c_2,c_3,\ldots,c_n)$​. Here, each $b_i$​​,$c_i$​ is a non-negative integer.

 Now, the sequence $a=(a_1,a_2,\ldots,a_n)$ considers beautiful if and only if for all $i$ $(2 \leq i \leq n)$, $b_i = a_{i-1} \, \text{or} \, a_{i}$, $c_i = a_{i-1} + a_{i}$ and each $a_i$​is a non-negative integer.

 Now, Toilet-Ares asks you to calculate the number of beautiful sequences.

#### 分析

题目的大致意思就是给你两个长度为$n-2$的数组$b$和$c$下标为$2-n$，请你构造数组$a$，满足$b_i = a_{i-1} \, \text{or} \, a_{i}$, $c_i = a_{i-1} + a_{i}$​ ，求所能构造的$a$的数量，这个题显然只要确定了$a_1$​，就能确定之后所有的数只要判断就行了，但是由于数据量比较大，正常的枚举会TLE，我们就需要优化这个枚举，优化的方向是按位枚举，我们惊奇的发现数组$c$与$d$的差值是$a_i|a_{i-1}$，有了这些信息我们就能对每一位进行枚举满不满足情况，这样可以大幅度的降低时间复杂度，枚举的具体操作简单但是很琐碎，具体的枚举过程参见代码。​​​

#### 代码实现

```c++
#include<bits/stdc++.h>
using namespace std;
int d[100010],b[100010],c[100010];
int main(){
//	freopen("66.txt", "r", stdin);
//	freopen("out.txt", "w", stdout);
	int n;
	scanf("%d",&n);
	for(int i=2;i<=n;i++){
		scanf("%d",&b[i]);
	}
	for(int i=2;i<=n;i++){
		scanf("%d",&c[i]);
		d[i]=c[i]-b[i];
	}
	int ans=1;
	for(int i=0;i<30;i++){
		int f1=1,f2=1,one=1,two=0;
		for(int j=2;j<=n;j++){
			int yu=d[j]>>i&1;
			int huo=b[j]>>i&1;
			if(f1){
				if(one==1){
					if(huo!=1){
						f1=0;
					}
					else{
						if(yu==1) one=1;
						else one=0;
					}
				}
				else{
					if(yu!=0){
						f1=0;
					}
					else{
						if(huo==0){
							one=0;
						}
						else one=1;
					}
				}
			}
			if(f2){
				if(two==1){
					if(huo!=1){
						f2=0;
					}
					else{
						if(yu==1) two=1;
						else two=0;
					}
				}
				else{
					if(yu!=0){
						f2=0;
					}
					else{
						if(huo==0){
							two=0;
						}
						else two=1;
					}
				}
			}
			if((f1+f2)==0){
				return puts("0"),0;
			}
		}
		ans*=(f1+f2);
	}
	printf("%d\n",ans);
	return 0;
}
```

### E - Rise of Shadows

#### 题目描述

Rumor has it that shadows rise in a prime leap year. A prime leap year is a leap year, and the year number is also a prime number.

 Toilet-Ares has recently learned the definitions of leap year and prime number. Given a specific year number, he wants to know if it is a prime leap year.

 Recall that
   · every year that is exactly divisible by four is a leap year, except for years that are exactly divisible by $100$, but these centurial years are leap years if they are exactly divisible by $400$;
   · a prime number (or a prime) is a positive integer greater than one that is not a product of two smaller positive integers.

#### 分析

如果一个年份是闰年也是质数，输出yes，否则输出no

闰年不可能是质数

#### 代码

```c++
#include <cstdio>

int main() {
    int t,a;
    scanf("%d",&t);
    while (t--){
        scanf("%d",&a);
        printf("no\n");
    return 0;
}
```

### k - Yet Another Problem About Pi
#### 题目描述
On a distant planet of dreams, Toilet-Ares is packing up the things and memories, ready to commerce a new adventurous road trip. With outstanding driving skills, the driving route may be a segment, a curve, a polyline, and it can be closed (end-to-end), even self-intersect, as long as the route is continuous. The fuel capacity, oddly enough, can support π km driving. Thus, the route has a maximum total length of πkm.

The planet's surface is expansive and flat enough to regard as a plane. A Cartesian coordinate system, formed with longitude and latitude, is used to describe each geographical position on the planet. Every $w$ km, draw a line of points with some equal longitude, named meridian. Similarly, every $d$ km, draw a line of points with some equal latitude, named parallel. Notice that innumerous meridians are perpendicular to innumerous parallels, constructing a grid called graticule, dividing the plane into infinite cells. Inhabitants there are used to defining those cells as regions, and to avoid conflict, positions on meridians or parallels belong to no region.

There are so many different kinds of landscapes to see, to admire, to experience. Toilet-Ares starts the drive at an arbitrary position on the planet. Whenever passing a region for the first time, Toilet-Ares will remember its visual feature (which is always distinguishable from any other region). So, it will be easy for Toilet-Ares to count up the number of regions visited as the road trip ends.

For example, in both situations shown below, four different regions are visited along the route.

#### 题目大意

在一个经纬度每个格子区域为$m$km和$n$km的星球上，每$n*m$面积的区域就是一个格子，给定你$π$km的燃料，试问最多能走过多少格子。

#### 题目分析0

容易发现，题目中分两种情况，判断$min(n,m)$与$\sqrt{n^2+m^2}$的大小以此来决定是走直线还是对角线。在主体走完后，判断剩余的长度是否能够通过修改已经走过的路线的方式来多走一步，如把一个斜着走变成两格直着走

#### 代码片段

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
using namespace std;
typedef long long ll;
const double PI = 3.14159265358979323846;
const double st2 = sqrt(2);
 
int main() {
    int t;
    double w,d,mind,l;
    scanf("%d",&t);
    ll ans=0;
    while (t--) {
        scanf("%lf%lf",&w,&d);
        mind = min(w,d);
        l = sqrt(w*w+d*d);
        if(PI<mind) {
            ans=4;
        } else if(l*2>mind*3) {
            ans=2;
            ll n = (ll)(PI / mind);
            if(PI-n*mind>l-mind) {
                ans++;
                if(PI-n*mind>2*(l-mind)) {
                    ans++;
                }
            }
            ans+=(n+1)*2;
        } else {
            ans=1;
            ll n =(ll )(PI / l);
     
            if(PI-n*l>2*mind-l) {
                ans++;
                if(PI-n*l>mind) {
                    ans++;
                }
            }
            ans+=(n+1)*3;
        }
        printf("%lld\n",ans);
 
    }
 
 
    return 0;
}
```
