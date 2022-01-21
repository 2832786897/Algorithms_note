```c++
#include <iostream>
#include <cstring>
#include <string>
using namespace std;

const int N = 5;

struct HTNode
{
    char word;
    int weight;                 //权重
    int lchild, rchild, parent; //左孩子，右孩子，双亲节点
} huff[2 * N];

int w[N] = {32, 28, 20, 4, 1};
char c[N] = {'B', 'D', 'E', 'C', 'H'};

int i1, i2;     //权值最小的两颗树根节点下标
int t = N - 1;  // t:数组中最后一个元素下标，元素个数-1
bool st[2 * N]; //标记，为true表示以该节点为根节点的树被合并到新树中

int sum() //求所有根节点权重和
{
    int s;
    for (int i = 0; i < N; i++)
        s += w[i];
    return s;
}

int a, b, ans;
void select() //选择出两个根节点权值最小的树
{
    ans = sum(); //任意两个节点权重和不会大于权重总和，所以初值设为总和
    for (a = 0; a < t; a++)
        for (b = a + 1; b <= t; b++)
        {
            if (!st[a] && !st[b] && huff[a].weight + huff[b].weight < ans)
            {
                i1 = a, i2 = b;
                ans = huff[a].weight + huff[b].weight;
            }
        }
    st[i1] = true;
    st[i2] = true;
}

void add() //合并两棵树
{
    huff[++t].weight = huff[i1].weight + huff[i2].weight;
    huff[i1].parent = t;
    huff[i2].parent = t;
    huff[t].lchild = i1;
    huff[t].rchild = i2;
}

void HuffmanTree() //构建HuffmanTree
{
    memset(huff, -1, sizeof(huff));
    for (int i = 0; i <= t; i++)
    {
        huff[i].weight = w[i];
        huff[i].word = c[i];
    }

    while (t < 2 * N - 2)
    {
        select();
        add();
    }
}

string code[N];
int tt = 0;
string temp;
void huffmanCoding(int pos, string temp)
{
    if (huff[pos].rchild == -1 && huff[pos].lchild == -1) //到达叶子节点，将该点对应的huffman编码存起来
    {
        code[tt++].append(temp);
    }
    else
    {
        if (huff[pos].lchild != -1) //向左孩子节点遍历
        {
            temp.append("0");
            pos = huff[pos].lchild;
            huffmanCoding(pos, temp);
        }
        pos = huff[pos].parent;     //回到双亲节点
        temp.erase(temp.end() - 1); //恢复临时编码字符串
        if (huff[pos].rchild != -1) //向右孩子节点遍历
        {
            temp.append("1");
            pos = huff[pos].rchild;
            huffmanCoding(pos, temp);
        }
    }
}

int main()
{
    HuffmanTree();
    cout << "index\tweight\tparent\tlchild\trchild" << endl;
    for (int i = 0; i <= t; i++)
    {
        cout << i << "\t";
        cout << huff[i].weight << "\t" << huff[i].parent << "\t" << huff[i].lchild << "\t" << huff[i].rchild << endl;
    }
    huffmanCoding(t, temp);
    for (int i = 0; i < tt; i++)
    {
        cout << huff[i].word << "\'s Huffman Code: " << code[i] << endl;
    }
}
```

