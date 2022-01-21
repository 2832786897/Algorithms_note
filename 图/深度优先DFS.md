
### 排列数字

给定一个整数 *n*，将数字 1∼*n* 排成一排，将会有很多种排列方法。

现在，请你按照字典序将所有的排列方法输出。

#### 输入格式

共一行，包含一个整数 *n*。

#### 输出格式

按字典序输出所有排列方案，每个方案占一行。

#### 数据范围

1≤*n*≤7

#### 输入样例：

```
3
```

#### 输出样例：

```
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
```

![排列数字-深度优先遍历](/home/mint/文档/算法笔记/图片/排列数字-深度优先遍历.png)



#### 代码：

```c++
#include<iostream>
using namespace std;

const int N = 10;

int n;
int path[N];
bool st[N];//每个节点的状态，为true表示该点被用过

void dfs(int u)
{
    if(u == n)
    {
        for(int i = 0; i < n; i++)
            printf("%d ", path[i]);
        puts("");
       	return;
    }
    
	for(int i = 1; i <= n; i++)
        if(!st[i])
        {
            path[u] = i;
            st[i] = true;
            dfs(u + 1);
            st[i] = false; //恢复，回溯
        }
    
}

int main()
{
    cin >> n;
    dfs(0);
    return 0;
}
```

### N皇后问题

```c++
#include<iostream>
using namespace std;
const int N = 20;
int n;
char g[N][N];
bool col[N], dg[(N << 1) - 1], udg[(N << 1) - 1];//列，两条对角线，如果该列和该对角线上有皇后，则标记为true
long long cnt;

void dfs(int u) {
    if (u == n) {
        for (int i = 0; i < n; i++) puts(g[i]);
        puts("");
        cnt += 1;
        return;
    }

    for (int i = 0; i < n; i++)
        if (!col[i] && !dg[n - u + i - 1] && !udg[i + u]) {
            g[u][i] = 'Q';
            col[i] = dg[n - u + i - 1] = udg[i + u] = true;
            dfs(u + 1);
            col[i] = dg[n - u + i - 1] = udg[i + u] = false;
            g[u][i] = 'M';
        }
}

int main() {
    cin >> n;
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            g[i][j] = 'M';
    dfs(0);
    printf("%d\n", cnt);
    return 0;
}
```

