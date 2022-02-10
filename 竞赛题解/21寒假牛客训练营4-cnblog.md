# 邬澄瑶的公约数

> 题目$gcd(x_1^{p_1},⋯,*x_n^{p_n}) $

## 解析

1. 每个数都可以分解乘若干个质因数，gcd就是取分解后**共同**质因数的最小次幂。
2. 因此可以对每一项的数字进行**质因数分解**，如果其**未分解到某个质因**数，那么这个质因数就置为0，表示不是每一项都有的。
3. 可以**维护每一个质数prime**，好处是：
   * 如果没有共有的，那么就将其最小次幂变为0，如果发现某个质数$prime[i]$并不是这个数字的质因数，就可以将其最小次幂变为0。
   * 方便最小质因数的筛选，并且筛的同时。

4. 对于每一项都有的质数，我们将这个数字的最小质因数其乘起来，就是$gcd$的答案。

```c++

#include<bits/stdc++.h>
typedef long long ll;
using namespace std;
const int N = 1e4 + 5;
const int mod = 1e9 + 7;
int cnt = 0;
int prime[N];
bool isprime[N];
int x[N], p[N];
int num[N];//记录第i个质因数的最低次幂
void table() {
	memset(num, 127, sizeof(num));//127表示很大的一个数
	for (int i = 2; i < N; i++) {
		if (isprime[i] == 0) {
			prime[++cnt] = i;
		}
		for (int j = 1; j <= cnt && prime[j] * i < N; j++) {
			isprime[prime[j] * i] = 1;
			if (i%prime[j]== 0)break;
		}
	}
}
ll qp(ll a, ll b) {
	ll ans = 1;
	while (b) {
		if (b & 1) { ans = ans * a%mod; }
		b >>= 1;
		a = a * a%mod;
	}
	return ans;
}
int main()
{
	table();
	int n;
	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> x[i];
	}
	for (int i = 1; i <= n; i++) {
		cin >> p[i];
	}
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <=cnt; j++) {//维护每一个质数，如果某一项不存在某个质数，那么就顺便将其置为0。
			int cnt = 0;
			while (x[i] % prime[j] == 0) {//不断分解质因数
				x[i] /= prime[j];
				cnt++;
			}
			num[j] = min(num[j], cnt*p[i]);
		}
	}
	ll ans = 1;
	for (int i = 1; i <= cnt; i++) {
		if (num[i] != 0) {//0表示不是共有的
			ans = ans * qp(prime[i], num[i]) % mod;
		}
	}
	cout << ans;
}

```





