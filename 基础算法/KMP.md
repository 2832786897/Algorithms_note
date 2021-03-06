### KMP

|i| 1 | 2 | 3 | 4 | 5 | 6 |
|:---| ---- | ---- | ---- | ---- | ---- |----|
|p[N]| a    | a    | b    | a    | a    |f|
|ne[N]| 0 | 1 | 0 | 1 | 2 |0|


```
模式串长度为n,模式串定义为p[N]
ne[N]前缀数组
for(int i = 2, j = 0; i <= n; i ++ )
{
	while(j && p[i] != p[j + 1]) j = ne[j];
	if(p[i] == p[j + 1]) j ++;
	ne[i] = j;
}
```

#### KMP字符串

给定一个模式串 *S*，以及一个模板串 *P*，所有字符串中只包含大小写英文字母以及阿拉伯数字。

模板串 *P* 在模式串 *S* 中多次作为子串出现。

求出模板串 *P* 在模式串 *S* 中所有出现的位置的起始下标。

#### 输入格式

第一行输入整数 *N*，表示字符串 *P* 的长度。

第二行输入字符串 *P*。

第三行输入整数 *M*，表示字符串 *S* 的长度。

第四行输入字符串 *S*。

#### 输出格式

共一行，输出所有出现位置的起始下标（下标从 0 开始计数），整数之间用空格隔开。

#### 数据范围

1≤*N*≤10^5^
1≤*M*≤10^6^

#### 输入样例：

```
3
aba
5
ababa
```

#### 输出样例：

```
0 2
```

#### 代码：

```c++
#include<iostream>
using namespace std;

const int N = 1e5 + 10, M = 1e6 + 10;
char p[N], s[M];
int ne[N];
int n, m;

int main()
{
    cin >> n >> p+1 >> m >> s+1;
    for(int i = 2, j = 0; i <= n; i ++ )
    {
        while(j && p[i] != p[j+1]) j = ne[j];
        if(p[i] == p[j+1]) j ++;
        ne[i] = j;
    }
    
    for(int i = 1, j = 0; i <= m; i ++ )
    {
        while(j && s[i] != p[j + 1]) j =  ne[j];
        if(s[i] == p[j + 1]) j ++ ;
        if(j == n)
        {
            printf("%d", i - n);
            j = ne[j];
        }
    }
}
```



