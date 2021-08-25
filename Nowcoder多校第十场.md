# Nowcoder 多校第十场




## 	F.Train Wreck

涉及到栈这个数据结构，然后建了一个图，这个图是按照栈的树型表示建立的，然后在把这个图的每个分叉节点打标记，然后先把每个分叉的染好色之后，就随便给其他的节点染色（这个题数据真的很水，我们代码写的也不怎么好但是过掉了）

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e6+5;
stack<char>st;
int las[maxn];
int color[maxn];
int a[maxn];
vector<int>g[maxn];
int col[maxn];
set<int> s;
int ans[maxn];
int flag = 0;
void dfs(int x){
    if(g[x].size()==0)return ;
    else if(g[x].size()==1)dfs(g[x][0]);
    else {
        int k = 0;
        queue<int>q;
        for(auto i:s){
            if(k == g[x].size())break;
            ans[g[x][k++]] = i;
            if((--col[i])==0){
                // s.erase(i);
                q.push(i);
            }
        }
        while(!q.empty()){
            s.erase(q.front());q.pop();
        }
        if(k<g[x].size()){
            // cout<<x<<":"<<g[x].size()<<endl;
            flag = 1;
            return ;
        }
        for(int i = 0 ;i < g[x].size();i++)dfs(g[x][i]);
    }
}
int main(){
    int n;
    scanf("%d",&n);
    getchar();
    int mx = 0;
    int stn = 1;
    memset(ans,-1,sizeof(ans));
    for(int i = 0;i < 2*n ;i++){
        char ch;
        scanf("%1c",&ch);
        // cout<<ch<<" ";
        if(ch=='('){
            mx += 1;
            a[stn++] = mx;
            st.push(ch);
        }
        else {
            st.pop();
            mx--;
        }
    }
    //  for(int i = 0;i < stn;i++){
    //     cout<<a[i];
    // }
    las[1] = 0;
    for(int i = 1; i < stn ;i++){
        las[a[i]+1] = i;
        // cout<<a[i]<<" "<<las[a[i]]<<endl;
        g[las[a[i]]].push_back(i);
    }    
    // for(int i = 0 ;i < n;i++){
    //     cout<<i;
    //     for(int j  = 0;j < g[i].size();j++){
    //         cout<<" "<<g[i][j];
    //     }
    //     cout<<endl;
    // }
    int ct = 0;
    for(int i = 1 ;i <= n;i++){
        scanf("%d",&color[i]);
        col[color[i]]++;
        s.insert(color[i]);
    }
    dfs(0);
    if(flag == 1){
        printf("NO\n");return 0;
    }
    // cout<<col[1]<<"%%"<<endl;
    int k = 1;
    for(int i = 1;i <= 1e6+1;i++){
        while(col[i]){
            if(ans[k]==-1){
                ans[k++] = i;col[i]--;
                // cout<<k<<" "<<ans[k]<<endl;
            }
            else k++;  
        }
    }
    printf("YES\n");
    for(int i = 1;i<=n;i++){
        if(i!=n)printf("%d ",ans[i]);
        else printf("%d ",ans[i]);
    }

}

```

## H.War of Inazuma (Easy Version)

暴力找就好了，从0标0然后和0相连的标1，然后再把标1的相连的标二，这样类似筛素数一样筛过去就ok

```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = (5e6+5);
int ans [maxn];
int main(){
    int n;
    scanf("%d",&n);
    int cnt = 0;
    memset(ans,-1,sizeof(ans));
    ans[0] = 0;
    for(int i = 0;i < (1<<n);i++){
        if(cnt==((1<<n)-1))break;
        for(int j = 0 ;j < n;j++){
            if((i|(1<<j))!=i){
                if(ans[i|(1<<j)]!=-1)continue;
                ans[i|(1<<j)] = ans[i]^1;
                cnt++;
            }
        }
    }
    for(int i = 0 ;i < (1<<n);i++){
        printf("%d",ans[i]);
    }
}

```
