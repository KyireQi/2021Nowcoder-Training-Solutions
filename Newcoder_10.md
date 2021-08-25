# 2021牛客暑期多校训练营10

## 题解

### F-Train Wreck

链接：https://ac.nowcoder.com/acm/contest/11261/F
来源：牛客网

You are now the coordinator for the Nonexistent Old Railway Station! The Nonexistent Old Railway Station only has one track with a dead end, which acts as a stack: everytime you either push a train into the station, or pop the most recently added train out of the station. 

 Everyday, n trains gets push into and pop out of the station. The inspector has already decided today's sequence of ``pushing'' and ``popping''. You now have a list of the 
 colored trains and have to assign each train to one ``pushing'' in inspector's sequence. 

 Meanwhile, the inspector also requires you to make the sequence of trains remaining on the track unique every time you push a train onto it. Two sequence of trains are considered different if the length is different or the i-th train in the two sequences have a different color. 

 Output a solution or decide that it is impossible.

#### 分析

题目的大致意思是，在一个”磕碜“的火车站，它只有一个死胡同用来停火车，火车的进出类似一个栈，每个火车有一个颜色，你的任务是在每一次火车进入死胡同时，轨道上的火车的颜色序列不一样，序列不一样包括长度和颜色排列。这道题的正解应该是需要建树+贪心，火车的进战可以序列可以建一棵树，只要每一个节点下的所有子节点颜色不一样即可满足条件，而我采用的是统计每一层的节点数量，让每一层的颜色均不一样，涂色时优先使用种类数较多的，这个可以用一个优先队列来维护，这个题的输出比较恶心，维护输出需要耐心，再提一句，这题的数据比较水，这题我们队在比赛时看错题了，然后直接人麻了，看懂题意之后这道题的思路还是比较好想的。

#### 代码实现

```c++
#include<bits/stdc++.h>
using namespace std;
int n,color[1000010];
int layer[1000010],ans[1000010];
char s[2000020];
typedef pair<int ,int> P;
priority_queue <P> q;
struct train{
	int id,c,lay;
	bool operator<(train &A) const{
		return lay<A.lay;
	}
}t[1000010];
int main(){
	scanf("%d",&n);
	scanf("%s",&s);
	int zuo=0,cnt=1;
	for(int i=0;i<2*n;i++){
		if(s[i]=='('){
			zuo++;
			layer[zuo]++;
			t[cnt].id=cnt;
			t[cnt].lay=zuo;
			cnt++;
		}
		else{
			zuo--;
		}
	}
	sort(t+1,t+1+n);
	for(int i=1;i<=n;i++){
		int x;
		scanf("%d",&x);
		color[x]++;
	} 
	for(int i=1;i<=n;i++){
		if(color[i]!=0) q.push(make_pair(color[i],i));
	}
	int i=1;
	cnt=1;
	while(1){
		if(q.size()<layer[i]){
			printf("NO\n");
			return 0;
		}
		int a[layer[i]][2],k=0;
		for(int j=1;j<=layer[i];j++){
			a[k][0]=q.top().first;
			if(a[k][0]==0){
				printf("NO\n");
				return 0;
			}
			a[k][0]--;
			a[k][1]=q.top().second;
			ans[t[cnt].id]=q.top().second;
			q.pop();
			k++,cnt++;
		}
		for(int j=1;j<=layer[i];j++){
			k--;
			q.push(make_pair(a[k][0],a[k][1]));
		}
		i++;
		if(layer[i]==0) break;
	}
	printf("YES\n");
	for(int i=1;i<=n;i++){
		if(i==n) printf("%d\n",ans[i]);
		else printf("%d ",ans[i]);
	}
	return 0;
} 
```

### H - War of Inazuma (Easy Version)

#### 题目描述

作为一个旅行者，你曾经目睹过闪电之战，其中双方是抵抗军和幕府将军的军队。

闪电之地图可以作为一个单位查看$n$维超立方体，它正好有 $2^n$​顶点。每个顶点都标有一个从 0 到$2^n-1$. 两个顶点相邻当且仅当它们的两个顶点中恰好存在一个不同的位$n$-位 二进制表示。

在闪电之战中，一些顶点被抵抗军占领，另一些则被将军军占领。您还注意到，对于每个顶点u你你, 相邻的顶点数 u你你 并在同一侧占据 u你你 不超过 $ \sqrt n $. 

多年以后，你已经忘记了战争的细节。你能构造一个满足以上所有要求的超立方体吗？

#### 分析

考虑2维空间中的情况，可以在2维超立方体（正方形）中构建一个正二面体（某一条对角线连接）0110可以满足。

考虑3维空间中的情况，将二维的情况先复制到第三个维度，然后在原来的空间中做一个镜像翻转。可以在3维超立方体（正方体）中构建一个正四面体01101001可以满足。

对于$n$​维空间中的情况，则构建一个$n$​维正$2^{n-1}$​面体，即把$n-1$​维中的正$2^{n-2}$​面体复制后翻转。所以写一个递推式，复杂度为线性。

#### 代码实现

```c++
#include <cstdio>

const int maxn = 4e6+2e5;
bool p[maxn];
 
int main(){
    p[0]=0;
    int n;
    scanf("%d",&n); 
    for (int b=1;b<=n;b++){
        for (int i=(1<<(b-1));i<1<<(b);i++){
            p[i]=!p[i-(1<<(b-1))];
        }
    }
    for (int i=0;i<(1<<(n));i++) printf("%d",p[i]);
    return 0;
}
```

