# 1. PHP EOF(heredoc) 定界符

* 格式：

```
echo<<<定界标识符
html内容
定界标识符;
```

* 注意事项：结束标识必须顶格独自占一行(即必须从行首开始，前后不能衔接任何空白和字符)。



# 2. echo硬核输出

利用echo把HTML代码直接输出出来，比较麻烦...

```php+HTML
echo '<table>';
foreach ($a as $key => $value) {
    echo '<tr>'.'<td colspan="3">'.$key.'</td>'.'<td>'.$value.'</td>'.'</tr>';
}
echo '</table>'
```

```
#include<iostream>
using namespace std;
int n;
bool f(int i){
    string s;
    int a=0;
    int cnt=0;
    while(i){
        s=char(i%10+'0')+s;
        cnt+=i%10;
    }
    for(int i=0;i<s.length();i++){
        a=a*10+(s[i]-'0');
    }
    return (a==i && cnt==n)? true:false;
}

int main()
{  
    bool q=1;
    for(int i=10000;i<1000000;i++){
        if(f(i)){
            cout<<i<<endl;
            q=0;
        }
    }
    if(q){
        cout<<"-1";
    }
}
```

