### Dijkstra求最短路

给定一个 *n* 个点 *m* 条边的有向图，图中可能存在重边和自环，所有边权均为正值。

请你求出 1 号点到 *n* 号点的最短距离，如果无法从 1 号点走到 *n* 号点，则输出 −1。

#### 输入格式

第一行包含整数 *n* 和 *m*。

接下来 *m* 行每行包含三个整数 *x*,*y*,*z*，表示存在一条从点 *x* 到点 *y* 的有向边，边长为 *z*。

#### 输出格式

输出一个整数，表示 1 号点到 *n* 号点的最短距离。

如果路径不存在，则输出 −1。

#### 数据范围

1≤*n*≤500
1≤*m*≤10^5^
图中涉及边长均不超过10000。

#### 输入样例：

```
3 3
1 2 2
2 3 1
1 3 4
```

#### 输出样例：

```
3
```

#### 代码：

```c++
#include<iostream>
#include<cstring>
#include<string>
using namespace std;

const int N = 510;

int n, m;
int g[N][N];	// 存储每条边
int dist[N];	// 存储1号点到每个点的最短距离
bool st[N];		// 存储每个点的最短路是否已经确定

int dijkstra()
{
    memset(dist, 0x3f, sizeof(dist));
    dist[1] = 0;
    for(int i = 0; i < n; i++)
    {
        int t = -1;		// 在还未确定最短路的点中，寻找距离最小的点
        for(int j = 1; j <= n; j++)
            if(!st[j] && (t == -1 || dist[t] > dist[j])) 
                t = j;
        
        st[t] = true;
        // 用t更新其他点的距离
        for(int j = 1; j <= n; j++)
        {
            dist[j] = min(dist[j], dist[t] + g[t][j]);
        }
    }
    
    if(dist[n] == 0x3f3f3f3f) 
        return -1;
    return dist[n];
}

int main()
{
    scanf("%d%d", &n, &m);
    memset(g, 0x3f, sizeof(g));
    while(m--)
    {
        int a, b, c;
        scanf("%d%d%d", &a, &b, &c);
        g[a][b] = min(g[a][b], c);
    }
    int t = dijkstra();
    printf("%d\n", t);
}
```

