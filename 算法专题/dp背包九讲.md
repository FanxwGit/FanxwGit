

# 01背包

## 题面

> 有 N 件物品和一个容量是V的背包。每件物品只能拿一次。
>
> 第 i 件物品的体积是 vi，价值是 wi。
>
> 求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
> 输出最大价值。

## 分析

## 代码

### 基础写法

```

#include<bits/stdc++.h>
using namespace std;
int v[1001];
int w[1001];
int f[1001][1001];
int main(){
    int n,m;
    cin>>n>>m;
    for(int i=1;i<=n;i++)
        cin>>v[i]>>w[i];
    for(int i=1;i<=n;i++){
        for(int j=0;j<=m;j++){//遍历容量
            f[i][j]=f[i-1][j];
            if(j>=v[i]){//容量够用 
                f[i][j]=max(f[i-1][j],f[i-1][j-v[i]]+w[i]);
            }
        }
    }
    int res=0;
    for(int j=0;j<=m;j++){
        res=max(res,f[n][j]);
    }
    cout<<res<<endl;
    return 0;
}
```

### 滚动数组优化写法

```

#include<bits/stdc++.h>
using namespace std;
int v[1001];
int w[1001];
int f[2][1001];
int main() {
	int n, m;
	cin >> n >> m;
	for (int i = 1; i <= n; i++)
		cin >> v[i] >> w[i];
	for (int i = 1; i <= n; i++) {
		int now = i & 1;
		int last = now ^ 1;
		for (int j = 0; j <= m; j++) {//遍历容量
			f[now][j] = f[last][j];
			if (j >= v[i]) {//容量够用 
				f[now][j] = max(f[last][j], f[last][j - v[i]] + w[i]);
			}
		}
	}
	int res = 0;
	for (int j = 0; j <= m; j++) {
		res = max(res, f[n & 1][j]);
	}
	cout << res << endl;
	return 0;
}
```



### 最终优化

```
#include<bits/stdc++.h>
using namespace std;
int v[1001];
int w[1001];
int f[1001];
int main() {
	int n, m;
	cin >> n >> m;
	for (int i = 1; i <= n; i++)cin >> v[i] >> w[i];
	for (int i = 1; i <= n; i++)
		for (int j = m; j >= v[i]; j--)//遍历容量
				f[j] = max(f[j], f[j - v[i]] + w[i]);
	cout << f[m]<< endl;
	return 0;
}
```



## 01背包练习



# 完全背包

## 题面

> 有 N 件物品和一个容量是V的背包。每件物品可以拿无数次。
>
> 第 i 件物品的体积是 vi，价值是 wi。
>
> 求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
> 输出最大价值。

## 分析

## 代码

### 基于01背包思维的朴素写法

```
#include<bits/stdc++.h>
using namespace std;
int v[1001];
int w[1001];
int f[1001][1001];
int main() {
	int n, m;
	cin >> n >> m;
	for (int i = 1; i <= n; i++)cin >> v[i] >> w[i];
	for (int i = 1; i <= n; i++) {
		for (int j = 0; j <= m; j++) { //j代表背包容量
			for (int k = 0; v[i] * k <= j; k++) { // 一直拿 直到拿爆为止
				f[i][j] = max(f[i][j], f[i - 1][j - k * v[i]] + k*w[i]); // 维护好f[i][j]
				//错误写法  利用f[i-1][j]不断去比较的话无法找到真正的最大值！
				//f[i][j] = max(f[i-1][j], f[i - 1][j - k * v] + k * w); 
			}
		}
	}
	cout << f[n][m];
	return 0;
}、、
```

### 状态转移思想改良写法

```
#include<bits/stdc++.h>
using namespace std;
int v[1001];
int w[1001];
int f[1001][1001];
int main() {
	int n, m;
	cin >> n >> m;
	for (int i = 1; i <= n; i++)cin >> v[i] >> w[i];
	for (int i = 1; i <= n; i++) {
		for (int j = 0; j <= m; j++) { //j代表背包容量
			//考虑的是三种状态的转移
			f[i][j] = f[i-1][j];
			if (j >= v[i]) {
				f[i][j] = max(f[i-1][j], max(f[i - 1][j - v[i]] + w[i], f[i][j - v[i]] + w[i]));
			}
		}
	}
	cout << f[n][m];
	return 0;
}
```

### 状态转移思想的进一步优化

多余的，因为j-v[i]已经和上一步进行比较了

```
#include<bits/stdc++.h>
using namespace std;
int v[1001];
int w[1001];
int f[1001][1001];
int main() {
	int n, m;
	cin >> n >> m;
	for (int i = 1; i <= n; i++)cin >> v[i] >> w[i];
	for (int i = 1; i <= n; i++) {
		for (int j = 0; j <= m; j++) { //j代表背包容量
			//考虑的是三种状态的转移
			f[i][j] = f[i - 1][j];
			if (j >= v[i]) {
				f[i][j] = max(f[i-1][j], f[i][j - v[i]] + w[i]);
			}
		}
	}
	cout << f[n][m];
	return 0;
}
```

### 用滚动数组优化写法

```
#include<bits/stdc++.h>
using namespace std;
int v[1001];
int w[1001];
int f[2][1001];
int main() {
	int n, m;
	cin >> n >> m;
	int now = 1, last = 0;
	for (int i = 1; i <= n; i++)cin >> v[i] >> w[i];

	for (int i = 1; i <= n; i++) {
		now = i & 1;
		last = now ^ 1;
		for (int j = 0; j <= m; j++) { //j代表背包容量
			//考虑的是三种状态的转移
			f[now][j] = f[last][j];
			if (j >= v[i]) {
				f[now][j] = max(f[last][j], f[now][j - v[i]] + w[i]);
			}
		}
	}
	cout << f[now][m];
	return 0;
}
```

### 最终优化

```c++
#include<bits/stdc++.h>
using namespace std;
int v[1001];
int w[1001];
int f[1001];
int main() {
	int n, m;
	cin >> n >> m;
	for (int i = 1; i <= n; i++)cin >> v[i] >> w[i];
	for (int i = 1; i <= n; i++) {
		for (int j = m; j >= 0; j--) {
				for (int k = 0; k*v[i] <= j; k++) {
					f[j] = max(f[j], f[j - v[i] * k] + w[i] * k);
			}
		}
	}
	cout << f[m];
	return 0;
}
```





# 多重背包

## 题面

> n个物品，每个物品有$v_i,w_i,s_i$。分别代表物品的体积、价值和数量。



## 分析

不能和完全背包一样处理，因为你永远不知道同阶段的上一个状态已经选了几次。

所以，可以把$s$个物品的数量拆分出多个物品，然后转化成01背包

* 二进制拆分法
