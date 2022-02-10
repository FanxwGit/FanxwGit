# 1:局长抓卷王

## 解析

* 算法标签：递归，暴力，对标cf 1500

我们遇到max的时候 后面必须有两个元素，当其中一个元素是max的时候，又要再向后找两个元素，因此考虑递归解决。

```
#include<bits/stdc++.h>
#include<vector>
typedef long long ll;
using namespace std;
vector<string>v;
string res;
int le;
int idx;
bool f(){
    if(idx>=le)return false;
    if(v[idx]=="max"){
        res+="max(";
        idx++;
        if(!f())return false;
        res+=",";
        idx++;
        if(!f())return false;
        res+=")";
    }
    else{
        res+=v[idx];
    }
    return true;
}

//标程
void solve(){
    v.clear();
    res="";
    idx=0;
    int n;
    cin>>n;
    string s;
    while(n--){
        cin>>s;
        v.push_back(s);
    }
    le=v.size();
    if(f() && idx==le-1)cout<<res<<";"<<endl;
    else cout<<"NO"<<endl;
}

//标程
void get_ans(){
    int t ;
    cin>>t;
    while(t--){
        solve();
    }
}

int main() {
    get_ans();
    return 0;
}
```

# 2:CCPC

## 解析

* 算法标签：二分+贪心，对标cf 1800

由于直接求最大值的话我们考虑的情况太多，因此不好直接求解。

假设我们已知“CCPC”个数的值为$mid$​，我们能不能**证明**这个字符串至少有$mid$​个“CCPC”？

我们设置4个变量$a1,a2,a3,a4$​​，这个变量用于记录4个状态。

$mid$​代表假设的"CCPC"的个数。

$a1$代表的是子序列**C**的个数，这个**C**作为子序列**CCPC**的前缀

$a2$​代表的是子序列**CC**的个数，这个**CC**作为子序列**CCPC**的前缀

$a3$​代表的子序列是**CCP**的个数，这个**CCP**作为子序列**CCPC**的前缀

$a4$​​​代表的子序列是**CCPC**的个数，代表子序列**CCPC**

我们可以发现，上面5个变量状态按顺序转移$mid->a1->a2->a3->a4$​​​​

我们现在探讨一下优先权的问题，先给出结论。

1. 当前字符为C时，以下状态的转移优先权依次递减。

   $a1->a2$​​​

   $mid->a1$

   $a_3->a_4$​

   

   -----分割线------

   * $a1->a2$

   如果$a1$​​​​有值，当前字符为"C"，优先转移到$a2$​​​​因为如果$a2$​​​状态出现晚了，遇到"P"就会错过​。

   * $mid->a1$

   如果没有第一个C,即$a1$没值，优先去拓展最开头的那个C，即mid向$a1$转移。因为如果影响到$a1$值的增加就会影响到$a2$值得增加，从而错过$p$​。

   * $a_3->a_4$

   添加最后一个C的优先权最低。

2. 当前字符为P时，直接讨论下类状态的转移。

   $a2->a3$

   

所以，我们已知“CCPC”个数的值为$mid$，我们可以**证明**这个字符串至少有$mid$个“CCPC”。

假设字符串s里，”CCPC"最多出现的次数为m，那么我们check的mid<=m时，结果都是成立的，故出现次数具有单调性。因此考虑对答案进行二分。



## 标程

```
#include<bits/stdc++.h>
typedef long long ll;
using namespace std;
bool check(char s[],int mid,int n){// check mid ccpc
    int x=mid;
    int a1=0,a2=0,a3=0,a4=0;
    for(int i=0;s[i];i++){
        if(s[i]=='C'){
            if(a1)a1--,a2++;
            else if(mid)a1++,mid--;
            else if(a3)a3--,a4++;

        }
        else if(s[i]=='P'){
            if(a2)a2--,a3++;
        }
    }
    return a4>=x;
}
//标程
void solve(){
    char s[100005];
    scanf("%s",s);
    int n=strlen(s);
    int l=0,r=n/4;
    while(l<r){
        int mid=(l+r+1)>>1;
        check(s,mid,n)?l=mid:r=mid-1;
    }
    printf("%d\n",l);
}

//标程
void get_ans(){
    int t ;
    cin>>t;
    while(t--){
        solve();
    }
}

int main() {
    get_ans();
    return 0;
}   
```

# 3：消灭珠子

## 解析

算法标签：数学

结论：最大值为$\frac{3n}{2}$​​​,证明如下。​

## 证明

设：$n$​为双方珠子的个数，$k$​为先手第一次消灭的珠子的个数。

第一回合：先手第一次消灭了后手$k$个珠子

第二回合：后手只剩下$n-k$个珠子，因此最大只能消灭$n-k$个珠子。

第三回合：此时先手只剩下$k$​个珠子，最大能消灭对方k个珠子，但是如果后手的珠子$n-k<=k$​个，我们就只能消灭$n-k$​个。

故分类讨论

1. 当$n-k<=k$，​​​​​即$\frac{n}{2}<=k$​​​​。

   最大得分为$res$，$res=k+n-k+n-k=2n-k(\frac{n}{2}<=k)$​

   最大值当且仅当$k=\frac{n}{2}$，$res=2n-\frac{n}{2}$

2. 当$n-k>=k$，即$\frac{n}{2}>=k$​。

   最大得分为$res$​​​，$res=k+n-k+k=n+k(\frac{n}{2}>=k)$​​​​

   最大值当且仅当$k=\frac{n}{2}$​​，$res=n+\frac{n}{2}$

考虑n为奇数，可分别带入n+1,n-1，由于珠子个数不能为半个，向下取整，最大值不变，证毕。

## 标程

```
    void solve(){
        int x;
        scanf("%d",&x);
        printf("%d\n",x+(x/2));
    }
```



