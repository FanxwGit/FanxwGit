# 试题 A: 门牌制作

>  小蓝要为一条街的住户制作门牌号。 这条街一共有 2020 位住户，门牌号从 1 到 2020 编号。 小蓝制作门牌的方法是先制作 0 到 9 这几个数字字符，最后根据需要将字 符粘贴到门牌上，例如门牌 1017 需要依次粘贴字符 1、0、1、7，即需要 1 个 字符 0，2 个字符 1，1 个字符 7。 请问要制作所有的 1 到 2020 号门牌，总共需要多少个字符 2？

## 分析：

主要考察数字的分解

```
int x=12345;
while(x){
	x%10获取个位数字
	x/=10;
}
```



# 既约分数



## 分析

两重循环遍历分子分母

主要考察辗转相除法

```
int gcd(int a,int b){
	return a%b==0? b:gcd(b,a%b);
}
```

```
int gcd(int a, int b) {//最大公约数
	return a % b == 0 ? b : gcd(b, a%b);
}

int main() {
	int ans = 0;
	for (int i = 1; i <= 2020; i++) {
		for(int j=1;j<=2020;j++){
			if (gcd(i, j) == 1) {
				ans++;
			}
		}
	}
}
```

# 蛇形填数

## 分析

规律：

1.第几行就几个数规定每一斜线为一行

* 首先观察规律，每一行的末尾为$1,3,6,10...$ 即 $1,1+2,1+2+3,1+2+3+4....$为等差数列前n项之和。
* 其次我们需要求的是第$20$行,$20$列，当我们求$1$行,$1$列发现是第一行的中间，$2$行,$2$列为第三行的中间，所以我们要求的是第$$行的中间一个数字，可以编程模拟或者直接计算。



### 直接计算

第$38$行最后一个数字带入等差数列求和公式$\frac{n*(n+1)}{2}$，带入得到741，第$39$行有$39$个数字，其中间一个数字为第(39/2)+1=20个，所以741+20=761

### 编程验证

```
#include <iostream>
#include <sstream>
#include <string>
#include <algorithm>
using namespace std;
typedef long long LL;
int main() {
    int a[45][45];
    for (int i = 1, cnt = 1; i <= 45; i++) { //i枚举的是反对角线
        if (i & 1) { //奇数对角线从左向右，偶数对角线从右向左
            for (int x = i, y = 1; x >= 1 && y <= i; x--, y++) {
                a[x][y] = cnt++;
            } 
        } else {
            for (int x = 1, y = i; x <= i && y >= 1; x++, y--) {
                a[x][y] = cnt++;
            }
        }
    }
    cout << a[20][20] << endl;
    return 0;
}
```





### EXCEL

![img](https://img2020.cnblogs.com/blog/1971335/202011/1971335-20201101100654044-1666396574.png)





# 跑步锻炼

> 小蓝每天都锻炼身体。 正常情况下，小蓝每天跑 1 千米。如果某天是周一或者月初（1 日），为了 激励自己，小蓝要跑 2 千米。如果同时是周一或月初，小蓝也是跑 2 千米。 小蓝跑步已经坚持了很长时间，从 2000 年 1 月 1 日周六（含）到 2020 年 10 月 1 日周四（含）。请问这段时间小蓝总共跑步多少千米？ 



## 分析

日期模拟题，需要用到的知识：

* 闰年判断

模拟日期的而增加即可，注意要维护好日期，并且判断周一和月初

### 编程模拟

```
bool isrun(int i) {
	return ((i % 4 == 0 && i % 100 != 0) || i % 400 == 0)? true : false;
}
int day[] = { 0,31,28,31,30,31,30,31,31,30,31,30,31 };
int main()
{
	int y, m, d;
	int week;
	int cnt = 0;
	y = 2000;
	m = 1;
	d = 1;
	week = 6;
	int ans = 0;
	while (!(y == 2020 && m == 10 && d == 2)) {//这一天的后一天
		int k = 0;//km

		if (week == 1 || d == 1) {
			k = 2;
		}
		else {
			k = 1;
		}

		cnt++;
		if (isrun(y))
			day[2] = 29;
		else
			day[2] = 28;

		d++;
		week++;
		if (d > day[m]) {
			d = 1;
			m++;
			if (m > 12) {
				m = 1;
				y++;
			}
		}
		if (week > 7) {
			week = 1;
		}
	}
	cout << cnt<<" "<<ans;
}
```

答案：8879

# 七段码

* 前缀知识点：枚举+连通分量

  枚举：1.二进制枚举 2.dfs枚举

  连通分量经典求法：1.bfs  2.dfs  3.并查集



## 解析一

7个灯光，每个灯光有亮和暗两种，所有的情况是$2^7=128$，我们可以用excel去模拟所有的情况

## 解析二

首先，要枚举出所有灯光亮暗的情况，我们用数字1表示灯光亮，数字0表示灯光暗，所以可以模拟出所有的开关情况，并且求其**连通分量**,如果连通分量的个数为1，那么表示此情况成立，累计答案中即可。

### 枚举方式

##### dfs枚举

我们用大小为7的数组$r$表示开关的情况，$r[0]$代表$a$,$r[1]$代表$b$......$r[6]$代表$g$

```
int r[7];
void select(int step) { // 0-6 7结束
	if (step == 7) {//最后我们获取了一种情况 
		for(int i=0;i<7;i++){
			cout<<r[i];//枚举的情况放到r[]数组里面
		}
		cout<<end;
		return;
	}
	for (int i=0; i <= 1; i++) {
		r[step] = i;
		select(step+1);
	}
}
```

##### 二进制枚举 

不熟悉二进制枚举的请看：[班长请客解析二](https://www.cnblogs.com/Fanxw/p/14415387.html)

枚举的情况放temp数组里面

```
int n=(1<<7);// 1 0000000 
int ans = 0;
for (int i = 1; i < n; i++) {
		int x = i;//表示此选择
		int cnt = 0;
		int temp[7] = {};
		while (x != 0) {
			temp[cnt++] = x & 1;
			x >>= 1;
		}
}
```

### 求解连通分量

获取了所有情况，我们就要开始判断某一情况是否可行，很经典的求解连通分量。

#### 1.dfs遍历

#### dfs枚举 + dfs遍历

对于某个未被标记过的亮灯点，将其作为起点进行深度优先搜索，标记其相邻的亮灯点，将一个亮灯点作为起点，连通分量的个数就增加一个。

```
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
/*
不推荐手动录入，推荐下标
*/
int link[10][10] = {
	{1,1,0,0,0,1,0},//a
	{1,1,1,0,0,0,1},//b
	{0,1,1,1,0,0,1},//c
	{0,0,1,1,1,0,0},//d
	{0,0,0,1,1,1,1},//e
	{1,0,0,0,1,1,1},//f
	{0,1,1,0,1,1,1} //g
};

int r[7];//选择的可能r[i]代表i灯是否是亮的
int ans = 0;//最后答案

void dfs(int visit[], int x) {
	visit[x] = 1;
	for (int i = 0; i < 7; i++) {//x所链接的点
		if (link[x][i] == 1 && visit[i] == 0 && r[i] == 1) {
			dfs(visit, i);
;		}
	}
}
void select(int step) { // 0-6 7结束
	if (step == 7) {//最后我们获取了一种情况 
		int visit[7] = {};//表示是否已经被标记过
		int cnt = 0;
		for (int i = 0; i < 7; i++) {
			if (r[i] == 1 && visit[i]==0) {
				//遍历其所有联通的点
				cnt++;
				dfs(visit, i);
			}
		}
		if (cnt == 1) {
			ans++;
		}
		for (int j = 0; j < 7; j++) {
			cout << r[j];
		}
		//cout << endl;
		return;
	}
	for (int i=0; i <= 1; i++) {
		r[step] = i;
		select(step+1);
	}
}
int main() {
	int n=(1<<7);// 10000000 
	select(0);
	cout << ans;
}
```

##### 二进制枚举+dfs遍历

```
#include<iostream>
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;

int link[10][10] = {
	{1,1,0,0,0,1,0},//a
	{1,1,1,0,0,0,1},//b
	{0,1,1,1,0,0,1},//c
	{0,0,1,1,1,0,0},//d
	{0,0,0,1,1,1,1},//e
	{1,0,0,0,1,1,1},//f
	{0,1,1,0,1,1,1} //g
};

void dfs(int temp[],int visit[],int x) {
	visit[x] = 1;
	for (int i = 0; i < 7; i++) {
		if (temp[i]==1 && link[x][i] && visit[i] == 0) {
			dfs(temp,visit,i);
		}
	}
}
int main() {
	int n=(1<<7);// 1 0000000 
	int ans = 0;
	for (int i = 1; i < n; i++) {
		int x = i;//表示此选择
		int cnt = 0;
		int temp[7] = {};
		while (x != 0) {
			temp[cnt++] = x & 1;
			x >>= 1;
		}
		int visit[10] = {};
		int nub = 0;//联通分量的个数
		for (int j = 0; j < cnt; j++) {
			if (temp[j] == 1 && visit[j]==0) {//进行深搜
				dfs(temp,visit, j);
				nub++;
			}
		}
		if (nub == 1) {
			pt(i);
			cout << endl;
			ans++;
		}
	}
	cout << ans;
}
```



#### 并查集

我们对每一个点的可通点进行合并，最后用set统计根节点，size的大小即是集合的个数。

```
#include<iostream>
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
/*
不推荐手动录入，推荐下标
*/
int link[10][10] = {
	{1,1,0,0,0,1,0},//a
	{1,1,1,0,0,0,1},//b
	{0,1,1,1,0,0,1},//c
	{0,0,1,1,1,0,0},//d
	{0,0,0,1,1,1,1},//e
	{1,0,0,0,1,1,1},//f
	{0,1,1,0,1,1,1} //g
};

int r[7];//选择的可能r[i]代表i灯是否是亮的
int ans = 0;//最后答案
int find(int par[],int i) {
	return par[i] == i ? i : par[i] = find(par,par[i]);
}

void select(int step) {
	if (step == 7) {
		int par[7];
		for (int i = 0; i < 7; i++)par[i] = i;
		for (int i = 0; i < 7; i++) {
			if (r[i] == 1) {//向这个点的其他可连接点拓展
				for (int j = 0; j < 7; j++) {//表示i到ｊ
					if (link[i][j] == 1 && r[j] == 1){
						int fi = find(par,i);
						int fj = find(par,j);
						par[fi] = fj;
					}
				}
			}
		}
		set<int>s;
		for (int i = 0; i < 7; i++) {
			if (r[i]==1) {
				s.insert(find(par,i));//判断根节点
			}
		}
		if (s.size() == 1) {
			ans++;
		}
		return;
	}
	for (int i=0; i <= 1; i++) {
		r[step] = i;
		select(step+1);
	}
}
int main() {
	select(0);
	cout << ans;
}
```



#### bfs判断连通块

bfs选择一个点对其拓展，思路和dfs相像，本文不多做赘述



# 编程题类型

**编程题**主要有几种常有的类型

1. 白给题
2. 大模拟题
3. dfs  /  bfs 
4. 贪心思维题
5. 前缀和
6. 数论（找规律或证明....）
7. dp
8. 数据结构

# 成绩统计

> 小蓝给学生们组织了一场考试，卷面总分为 100 分，每个学生的得分都是 一个 0 到 100 的整数。 如果得分至少是 60 分，则称为及格。如果得分至少为 85 分，则称为优秀。 请计算及格率和优秀率，用百分数表示，百分号前的部分四舍五入保留整数。
>
> 【输入格式】 输入的第一行包含一个整数 n，表示考试人数。 接下来 n 行，每行包含一个 0 至 100 的整数，表示一个学生的得分。 
>
> 【输出格式】 输出两行，每行一个百分数，分别表示及格率和优秀率。百分号前的部分 四舍五入保留整数。

## 解析

知识点：四舍五入

* 手写

```
void getRound(int x){
	double x;	
if (x-(int)x>=0.5)
	return x+1
else
	return x;
}
```

* 内置函数

```
round(x)
```

* 代码

```
#include<bits/stdc++.h>
using namespace std;
int main() {
	double n;
	cin >> n;
	double jg = 0, yx = 0;
	for (int i = 1; i <= n; i++) {
		int x;
		cin >> x;
		if (x >= 85) {
			yx++;
		}
		if (x >= 60) {
			jg++;
		}
	}
	double jgRate = jg / n * 100;
	double yxRate = yx / n * 100;
	cout << round(jgRate) <<"%"<< endl;
	cout << round(yxRate) << "%" << endl;
}

```





# 回文日期

>
>
>【问题描述】 2020 年春节期间，有一个特殊的日期引起了大家的注意：2020 年 2 月 2 日。因为如果将这个日期按 “yyyymmdd” 的格式写成一个 8 位数是 20200202， 恰好是一个回文数。我们称这样的日期是回文日期。 有人表示 20200202 是 “千年一遇” 的特殊日子。对此小明很不认同，因为 不到 2 年之后就是下一个回文日期：20211202 即 2021 年 12 月 2 日。 也有人表示 20200202 并不仅仅是一个回文日期，还是一个 ABABBABA 型的回文日期。对此小明也不认同，因为大约 100 年后就能遇到下一个 ABABBABA 型的回文日期：21211212 即 2121 年 12 月 12 日。算不上 “千 年一遇”，顶多算 “千年两遇”。 给定一个 8 位数的日期，请你计算该日期之后下一个回文日期和下一个 ABABBABA 型的回文日期各是哪一天。 
>
>【输入格式】 输入包含一个八位整数 N，表示日期。 【
>
>输出格式】 输出两行，每行 1 个八位数。第一行表示下一个回文日期，第二行表示下 一个 ABABBABA 型的回文日期。 【
>
>样例输入】 20200202 
>
>【样例输出】 20211202 21211212 试题 G: 回文日期 9第十一届蓝桥杯大赛软件类省赛第二场 C/C++ 大学 B 组 【
>
>评测用例规模与约定】 对于所有评测用例，10000101 ≤ N ≤ 89991231，保证 N 是一个合法日期的 8 位数表示。

## 解析

知识点：大模拟

模拟日期，判断回文和ABABBABA朴素做法就是把其化简成字符串，然后比较即可。

``

```
#include<bits/stdc++.h>
using namespace std;
//   01234567
bool ABABBABA(int y, int m, int d) {
	string str = "00000000";
	str[0] = (y / 1000) +'0';//1234
	str[1] = (y / 100)%10 + '0';//12%10
	str[2] = (y / 10) % 10 + '0';
	str[3] = (y % 10) + '0';
	str[4] = (m / 10) + '0';
	str[5] = (m % 10) + '0';
	str[6] = (d / 10) + '0';
	str[7] = (d % 10) + '0';
	if (str[0] == str[2] && str[0] == str[5] && str[0] == str[7]
		&& str[1] == str[3] && str[1] == str[4] && str[1] == str[6]) {
		return true;
	}
	else {
		return false;
	}
}
bool huiwen(int y, int m, int d) {//20 21 12 02
	string str = "00000000";
	str[0] = (y / 1000) + '0';//1234
	str[1] = (y / 100) % 10 + '0';//12%10
	str[2] = (y / 10) % 10 + '0';
	str[3] = (y % 10) + '0';
	str[4] = (m / 10) + '0';
	str[5] = (m % 10) + '0';
	str[6] = (d / 10) + '0';
	str[7] = (d % 10) + '0';
	if (str[0]==str[7] &&
		str[1]==str[6] &&
		str[2]==str[5] &&
		str[3]==str[4]){
		return true;
	}
	else {
		return false;
	}
}

bool isrun(int i) {
	if (i % 4 == 0 && i % 100 != 0 || i % 400 == 0) {
		return true;
	}
	else {
		return false;
	}
}

int day[] = { 0,31,28,31,30,31,30,31,31,30,31,30,31 };
int main() {
	string str;
	int y=0, m=0, d=0;
	cin >> str;
	//解析字符串
	//0*10 +('2'-'0') 2
	//2*10 +('0'-'0')=20
	//20*10+('2'-'0')=202
	//202*10+('1'-'0'）=2021
	for (int i = 0; i <= 3; i++) {
		y = y * 10 + (str[i] - '0');
	}
	for (int i = 4; i <= 5; i++) {
		m = m * 10 + (str[i] - '0');
	}
	for (int i = 6; i <= 7; i++) {
		d = d * 10 + (str[i] - '0');
	}
	bool f = 0;
	//遍历并判断
	while (1) {
		if (isrun(y)) {
			day[2] = 29;
		}
		else {
			day[2] = 28;
		}
		d++;
		if (d > day[m]) {
			d = 1;
			m++;
			if (m > 12) {
				m = 1;
				y++;
			}
		}
		if (huiwen(y, m, d)) {		
			if (f == 0) {//第一个回文日期
				f = 1;
				printf("%02d%02d%02d\n", y, m, d);
			}
			if (ABABBABA(y, m, d)) {//ABABBABA肯定也是回文日期，因此直接跳出循环
				printf("%02d%02d%02d\n", y, m, d);
				break;
			}
		}

	}
}
```



# 子串分值和 

>【问题描述】 对于一个字符串 S，我们定义 S 的分值 f(S ) 为 S 中出现的不同的字符个 数。例如 f(”aba”) = 2，f(”abc”) = 3, f(”aaa”) = 1。 现在给定一个字符串 S [0..n − 1]（长度为 n），请你计算对于所有 S 的非空 子串 S [i.. j](0 ≤ i ≤ j < n)，f(S [i.. j]) 的和是多少。 
>
>【输入格式】 输入一行包含一个由小写字母组成的字符串 S。 
>
>【输出格式】 输出一个整数表示答案。 
>
>【样例输入】 ababc 
>
>【样例输出】 28

想不出来的话，优先**写出暴力的写法**。



## 解析一：暴力

1. 暴力获取每个子串的时间复杂度为$O(n^2)$，并且对字符串的长度进行遍历，累加结果，时间复杂度为$O(n^3)$，大概过40%样例点

2. 暴力获取每个子串的时间复杂度为$O(n^2)$，对字符串的处理，考虑前缀和思想，时间复杂度为$O(n^2)$,大概过50%~60%样例点，具体可以练习我校OJ[好看的字符串](http://www.xujcoj.org/Home/Problems/status/pro_id/1340)      [题解](https://www.cnblogs.com/Fanxw/p/14407751.html)

3. 由于100%样例点 $1<=n<=100000$,因此我们需要想出一个$O(n)$的算法去解决此问题。

   我们追求$O(n)$的算法，所以我们**不考虑子串的遍历**。

## 解析二：思维

枚举每个左端点（l）, 每个字母独立计算贡献，枚举每个字母，在【l, n】 中找到该字母第一次出现的位置p, 则左端点为 l, 右短点在【p, n】的子串都得到该字母的贡献，贡献 n - p + 1。
例如 :

```
#include <iostream>
using namespace std;

int main()
{
	long long cnt=0;
	int last[26]={};
	string s;
	cin>>s;
	int n=s.length();
	for(int i=1;i<=n;i++)
	{
		int j=s[i-1]-'a'; 
		cnt+=(i-last[j])*(n-i+1);
		last[j]=i;
	}
	cout<<cnt;
	
	return 0;
} 
```

## 解析三：(队列+前缀和)



对于字符串$ababd$,我们定义一整个字符串的贡献值之和为$\sum_i^nf(s[0...i])$

![image-20210411104001102](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210411104001102.png)

我们发现一整个完整的串**献值之和**为**10**，现在我们把第一个a删掉。

![image-20210411104723756](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210411104723756.png)

我们发现，在**下一个a以前的所有字符**的贡献值都减少了1，包括a自己。

所以我们新串$babd$的贡献值为,**10-2**

继续删掉b

我们发现，在**下一个b以前的所有字符**的贡献值都减少了1，包括b自己。

所以我们新串$abd$的贡献值为**8-2**

![image-20210411111001550](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210411111001550.png)

* 继续删掉a

  我们发a后续没有a了，因此在**a之后的所有字母**的贡献值都减少了1，包括a自己，

  所以我们新串$bc$的贡献值之和是6-3=3

* 继续删掉b

  我们发b后续没有b了，因此在**b之后的所有字母**的贡献值都减少了1，包括b自己，

  所以我们新串$c$的贡献值之和是3-2=1

* 继续删掉d

  我们发d后续没有d了，因此在**d之后的所有字母**的贡献值都减少了1，包括d自己，

  所以我们新串$c$的贡献值之和是1-1=0

累加上述每一个串的贡献值就是我们的答案。



我们发现，我们删掉一个字母的时候面临**两种情况**



1. 后续**有同类**

   后续有同类的时候，原本的串到下一个同类的之间$dis$个数字，所有**字母的贡献值**都变少了一个，那我们新串的贡献值自然就减去$dis$

2. 后续**无同类**

   无同类，我们后续的所有字母的贡献值都少了一个，因此我们新串的贡献值要减去后面的所有的字母个数。



但是如何去快速计算字母和下一个同类字母之间的距离呢？

利用**队列**就可以非常快速的判断出字母和下一个同类字母之间的距离，具体细节看代码

而且还能判断这个字母后续是否还有字母

![image-20210411111857417](C:\Users\37802\AppData\Roaming\Typora\typora-user-images\image-20210411111857417.png)

```

#include<bits/stdc++.h>
typedef long long ll;
using namespace std;
int main() {
	string s;
	cin >> s;
	vector<queue<int>>v(26);//每一个队列存对应字母的下标
	ll sum = 0;
	ll ans = 0;
	int cnt[26] = {};
	int value = 0;
	for (int i = 0; i < s.length(); i++) {
		int el = s[i] - 'a';//字母
		v[el].push(i);
		cnt[el]++;
		if (cnt[el] == 1) {
			value++;
		}
		sum += value;
	}
	ans = sum;
	for (int i = 0; i < s.length(); i++) {//删掉下标i的字母
		int el = s[i] - 'a';//字母
		if (v[el].size()>1) {//表示后有
			int top = v[el].front();
			v[el].pop();
			int top2 = v[el].front();
			int dis = top2 - top;//距离
			sum -= dis;
		}
		else {//没有
			int top = v[el].front();
			v[el].pop();
			sum -= (s.length() - top);
		}
		ans += sum;
	}
	cout << ans;
	
}
```





## 

# 平面切分

> 平面上有 N 条直线，其中第 i 条直线是 y = Ai · x + Bi。
>
> 【输入格式】 第一行包含一个整数 N。 以下 N 行，每行包含两个整数 Ai , Bi。
>
> 【输出格式】 一个整数代表答案	
>
> 

加线=非平行线的个数+1

用set存储交点

```
#include <iostream>
#include <set>
using namespace std;

const int N = 1e3 + 10;
typedef long long LL;
typedef long double LD;
LD s[N][2];
LL res = 0;
bool st[N];
pair<LD, LD> p; //用来记录交点

int main() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> s[i][0] >> s[i][1];
        set<pair<LD, LD>> points;
        for (int j = 0; j < i; j++) {
            if (st[j]) continue; //说明是重边 直接忽略
            if (s[i][0] == s[j][0]) { //如果两条直线斜率相等，判断平行还是重合
                if (s[i][1] == s[j][1]) { //如果新添加的直线是重边 退出循环 
                    st[i] = true;
                    break;
                } else {
                    continue;
                }
            }
            p.first = (s[j][1] - s[i][1]) / (s[i][0] - s[j][0]);
            p.second = s[j][0] * p.first + s[j][1];
            points.insert(p);
        }
        if (!st[i]) res += points.size() + 1; //如果st[i]不是重边 计入答案
    }
    cout << res + 1 << endl;
    return 0;
}
```

