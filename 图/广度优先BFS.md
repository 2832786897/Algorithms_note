### 走迷宫

给定一个 *n*×*m* 的二维整数数组，用来表示一个迷宫，数组中只包含 0 或 1，其中 0 表示可以走的路，1 表示不可通过的墙壁。

最初，有一个人位于左上角 (1,1) 处，已知该人每次可以向上、下、左、右任意一个方向移动一个位置。

请问，该人从左上角移动至右下角 (*n*,*m*) 处，至少需要移动多少次。

数据保证 (1,1) 处和 (*n*,*m*) 处的数字为 0，且一定至少存在一条通路。

#### 输入格式

第一行包含两个整数 *n*n 和 *m*m。

接下来 *n* 行，每行包含 *m*m 个整数（0 或 1），表示完整的二维数组迷宫。

#### 输出格式

输出一个整数，表示从左上角移动至右下角的最少移动次数。

#### 数据范围

1≤*n*,*m*≤100

#### 输入样例：

```
5 5
0 1 0 0 0
0 1 0 1 0
0 0 0 0 0
0 1 1 1 0
0 0 0 1 0
```

#### 输出样例：

```
8
```

#### 代码：

```c++
#include<iostream>
#include<algorithm>
#include<cstring>
using namespace std;

typedef pair<int, int> PII;

const int N = 110;

int n, m;
int g[N][N];
int d[N][N];
PII q[N * N];

int bfs()
{
    int hh = 0, tt = 0;
    q[0] = {0, 0};
    memset(d, -1, sizeof(d));
    d[0][0] = 0;
    
    int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};//横纵坐标的增量
    while(hh <= tt)
    {
        auto t = q[hh ++];
        
        for(int i = 0; i < 4; i++)//朝4个方向拓展
        {
            int x = t.first + dx[i], y = t.second + dy[i];
            if(x >= 0 && x < n && y >= 0 && y < m && g[x][y] == 0 && d[x][y] == -1)//如果该点可以走，且没有被走过
            {
                d[x][y] = d[t.first][t.second] + 1;
                q[++tt] = {x, y};
            }
        }
    }
    return d[n-1][m-1];
}

int main()
{
    cin >> n >> m;
    for(int i = 0; i < n; i++)
        for(int j = 0; j < m; j++)
            cin >> g[i][j];
    cout << bfs() << endl;
}
```

### 点的层次

给定一个 *n* 个点 *m* 条边的有向图，图中可能存在重边和自环。

所有边的长度都是 1，点的编号为 1∼*n*。

请你求出 1 号点到 *n* 号点的最短距离，如果从 1 号点无法走到 *n* 号点，输出 −1。

#### 输入格式

第一行包含两个整数 *n* 和 *m*。

接下来 *m* 行，每行包含两个整数 *a* 和 *b*，表示存在一条从 *a* 走到 *b* 的长度为 1 的边。

#### 输出格式

输出一个整数，表示 1 号点到 *n* 号点的最短距离。

#### 数据范围

1≤*n*,*m*≤10^5^

#### 输入样例：

```
4 5
1 2
2 3
3 4
1 3
1 4
```

#### 输出样例：

```
1
```

#### 代码：

```c++
#include<iostream>
#include<cstring>
#include<algorithm>
using namespace std;

const int N = 1e5 + 10;

int n, m;
int h[N], e[N], ne[N], idx;
int d[N], q[N];

int add(int a, int b)
{
    e[idx] = b;
    ne[idx] = h[a];
    h[a] = idx++;
}

int bfs()
{
    int hh = 0, tt = 0;
    q[0] = 1;
    memset(d, -1, sizeof(d));
    d[1] = 0;
    while(hh <= tt)
    {
        int t = q[hh++];
        for(int i = h[t]; i != -1; i = ne[i])
        {
            int j = e[i];
            if(d[j] == -1)
            {
                d[j] = d[t] + 1;
                q[++tt] = j;
            }
        }
    }
    return d[n];
}

int main()
{
    cin >> n >> m;
    memset(h, -1, sizeof(h));
    while(m -- )
    {
        int a, b;
        cin >> a >> b;
        add(a, b);
    }
    cout << bfs() << endl;
}

```

