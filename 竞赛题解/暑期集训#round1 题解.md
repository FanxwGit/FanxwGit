# 1:消灭星星

## 解析

最优策略是只消灭一种颜色，最后一次性消灭另外一种颜色。据此策略，取两种情况的最小值即可。

## 标程

```
    void solve(){
        int n,m;
        scanf("%d%d",&n,&m);
        int a0=0,a1=0;
        int f=-1;
        for(int i=1;i<=n;i++){
            int x;
            scanf("%d",&x);
            if(f!=x){
                if(x==0){
                    a0++;
                    f=0;
                }
                else{
                    a1++;
                    f=1;
                }
            }
        }
        int res=min(a0,a1)+1;
        if(res<=m)printf("YES\n");
        else printf("NO\n");
    }
```



# 2: 下馆子 -3

## 解析

答案是最后出勤次数最多，且出勤次数最先**大于等于max**次的同学。

考虑用map对字符串做映射，最后算出最大值$max$。然后将出勤次数为$max$同学放入set集合。

重新遍历一次，当出现出勤次数大于$max$​，判断是否在set集合之中即可。

## 代码

```
#include<bits/stdc++.h>
#include<set>
#include<map>
typedef long long ll;
using namespace std;
pair<string,int>r[100005];
set<string>st;
map<string,int>mp;

//标程
void solve(){
    st.clear();
    mp.clear();
    int n ;
    scanf("%d",&n);
    int mx=INT_MIN;
    string res="null";
    for(int i=1;i<=n;i++){
        string s;
        int x;
        cin>>s>>x;
        r[i]={s,x};
        mp[s]+=x;
    }
    for(auto i:mp){
        mx=max(mx,i.second);
    }
    for(auto i:mp){
        if(i.second==mx){
            st.insert(i.first);
        }
    }
    mp.clear();
    for(int i=1;i<=n;i++){
        string name=r[i].first;
        int score=r[i].second;
        mp[name]+=score;
        if(mp[name]>=mx && st.count(name)){
            res=name;
            break;
        }
    }
    cout<<res;
}
int main() {
    solve();
    return 0;
}
```





# 3:光照强度-2

## 解析

根据数据量，考虑O(N)的dp解法。

* **规定：**$r[i][j]$​代表$[i,j]$​的明亮度，$g[i][j]$​代表$[i,j]$​处的灯泡亮度，$g[i][j]=0$​​代表此处没有灯泡

1. 假设方格$[i,j]$​​没有灯

![image-20210731205001067](C:\Users\37802\Desktop\新建文本文档.assets\image-20210731205001067.png)

此位置的明亮度由4个方向**递减**而来。

* 左边：能贡献$2-1=1$​的明亮度
* 上边：能贡献$4-1=3$​的明亮度
* 右边：能贡献$3-1=2$​​的明亮度
* 下边：能贡献$5-1=4 $​​的明亮度

因此此处的明亮值为$max(1,3,2,4)$​​

2. 如果方格$[i,j]$有一盏灯，$g[x][y]=5$

此处的明亮值为$max(1,3,2,4,g[x][y])=5$​​

因此我们可以推断出状态转移方程。
$$
r[i][j]=max(g[i][j],r[i+1][j],r[i][j+1],r[i-1][j],r[i][j-1],r[i][j])
$$
但是，如果只从左上角开始遍历一次，第一次遍历到某个点的时候，下方和右方没有数据，因此要从4个方向维护每个位置​的最大值。

```
#include<bits/stdc++.h>
typedef long long ll;
using namespace std;
typedef pair<int,int>pii;
const int N =1005;
int r[N][N];
int g[N][N];
void solve(){
    int a,b,m;
    scanf("%d%d%d",&a,&b,&m);
    for(int i=1;i<=m;i++){
        int x,y,c;
        scanf("%d%d%d",&x,&y,&c);
        g[x][y]=max(g[x][y],c);
    }
    //左上角
    for(int i=1;i<=a;i++){
        for(int j=1;j<=b;j++){
            int q=r[i-1][j]-1,p=r[i][j-1]-1;
            int t=max(0,max(q,p));
            r[i][j]=max(max(g[i][j],t),r[i][j]);
        }
    }
    //右下角
    for(int i=a;i>=1;i--){
        for(int j=b;j>=1;j--){
            int q=r[i+1][j]-1,p=r[i][j+1]-1;
            int t=max(0,max(q,p));
            r[i][j]=max(max(g[i][j],t),r[i][j]);
        }
    }

    //右上角
    for(int i=1;i<=a;i++){
        for(int j=b;j>=1;j--){
            int q=r[i-1][j]-1,p=r[i][j+1]-1;
            int t=max(0,max(q,p));
            r[i][j]=max(max(g[i][j],t),r[i][j]);
        }
    }
    //左下角
    for(int i=a;i>=1;i--){
        for(int j=1;j<=b;j++){
            int q=r[i+1][j]-1,p=r[i][j-1]-1;
            int t=max(0,max(q,p));
            r[i][j]=max(max(g[i][j],t),r[i][j]);
        }
    }

    for(int i=1;i<=a;i++){
        for(int j=1;j<=b;j++){
            if(j!=1)printf(" ");
            printf("%d",r[i][j]);
        }
        printf("\n");
    }
}
int main() {
    solve();
    return 0;
}   
```

