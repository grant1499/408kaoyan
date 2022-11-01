# chapter 6：图

> 代码题考察较少。

## 习题6.2

### 6.（2021真题）无向连通图的欧拉回路

看王道答案。

## 习题6.3

### 2.判断无向图是否为树

判断条件：如果无向图有n个顶点和n-1条边且是连通的，则是树。

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct Graph{   // 图的邻接矩阵表示
    int Vex[N]; // 顶点表，N为顶点上限
    int Edge[N][N]; // 边表
    int vexnum; // 实际顶点数
    
    bool st[N]; // 标记顶点是否访问过
    Graph(int n):vexnum(n){}
};

void add(Graph &g,int a,int b){ // 无向图
    g.Edge[a][b] = g.Edge[b][a] = 1;
}

void dfs(Graph &g,int vex,int &n,int &m){ // DFS遍历无向图的邻接矩阵表示
    g.st[vex] = true;n ++; // st[]顶点标记数组，点数 ++

    for (int i = 1;i <= g.vexnum;i ++){
        if (g.Edge[vex][i]) m ++; // 边数 ++
        if (!g.st[i]){
            g.st[i] = true;dfs(g,i,n,m);
        }
    }
}

bool isTree(Graph &g){ // 判断一个无向图是否为树
    int n = 0,m = 0;
    dfs(g,1,n,m);
    cout << n << ' ' << m/2 << '\n'; // 点数，边数
    return n == m/2+1; // 无向边每条边算了2遍，所以/2
}

int main(){
    Graph g = Graph(8);
    for (int i = 1;i <= g.vexnum;i ++) g.Vex[i] = i;
    int a,b;
    while (cin >> a >> b) add(g,a,b);
    // 邻接矩阵输出
    // for (int i = 1;i <= g.vexnum;i ++){
    //     for (int j = 1;j <= g.vexnum;j ++){
    //         cout << g.Edge[i][j] << ' ';
    //     }
    //     cout << '\n';
    // }
    
    cout << isTree(g);
    return 0;
}
```

### 3.DFS的非递归版本

图采用邻接表存储。

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct Node{ // 图的邻接表存储
    int val;
    Node* next;
    
    Node():next(NULL){}
    Node(int _val):val(_val),next(NULL){}
}* head[N];

bool st[N]; // 标记数组
int stk[N],top = -1;

void add(int a,int b){ // 有向图邻接表加边
    Node *p = new Node(b);
    p->next = head[a]; // 头插法
    head[a] = p;
    // 这样写：p->next = head[a]->next错误,head[a]初始为NULL
}

void dfs(int i){ // 非递归遍历图，用栈保存顶点
    st[i] = true;
    stk[++top] = i;
    while (top != -1){ // 栈空表示DFS结束
        int temp = stk[top--];
        cout << temp << ' '; // 先序先访问当前结点，然后在栈中出栈时访问子结点
        for (Node* p = head[temp];p != NULL;p = p->next){
            if (!st[p->val]){
                st[p->val] = true;
                stk[++top] = p->val;
            }
        }
    }
    cout << '\n';
}

int main(){
    int a,b;
    while (cin >> a >> b) add(a,b);
    for (int i = 1;i <= 6;i ++){
        if (!st[i]) dfs(i);
    }
    
    return 0;
}
```

### 4.判断路径是否存在

邻接表存储有向图，判断顶点$V_i$到$V_j$是否存在路径。

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct Node{ // 图的邻接表存储
    int val;
    Node* next;
    
    Node():next(NULL){}
    Node(int _val):val(_val),next(NULL){}
}* head[N];

bool st[N]; // 标记数组

void add(int a,int b){ // 有向图邻接表加边
    Node *p = new Node(b);
    p->next = head[a]; // 头插法
    head[a] = p;
    // 这样写：p->next = head[a]->next错误,head[a]初始为NULL
}

bool dfs(int i,int j){
  st[i] = true;
  if (j == i) return true;
  for (Node* p = head[i]; p; p = p->next){
    int k = p->val;
    if (!st[k]){
      st[k] = true;
      if (k == j) return true;
      if (dfs(k,j)) return true;
    }
  }
  return false;
}

bool isPath(int vex1,int vex2){
  // 判断给定的邻接表存储的有向图的两顶点，判断是否存在路径
  // DFS和BFS都能实现，这里采用DFS
  memset(st,0,sizeof st);
  return dfs(vex1,vex2);
}

int main(){
    int a[] = {1,5,2,3,3};int b[] = {5,2,6,6,7};
    for (int i = 0;i < 5;i ++) add(a[i],b[i]);
    
    cout << isPath(2, 5) << '\n';
    cout << isPath(3, 6) << '\n';
    cout << isPath(6, 1) << '\n';
    cout << isPath(4, 5) << '\n';    
    return 0;
}
```

### 5.输出两顶点间的所有简单路径

邻接表存储图。

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct Node{ // 图的邻接表存储
    int val;
    Node* next;
    
    Node():next(NULL){}
    Node(int _val):val(_val),next(NULL){}
}* head[N];

bool st[N]; // 标记数组

void add(int a,int b){ // 有向图邻接表加边
    Node *p = new Node(b);
    p->next = head[a]; // 头插法
    head[a] = p;
    // 这样写：p->next = head[a]->next错误,head[a]初始为NULL
}

int path[N];
void dfs(int i,int j,int d){
  st[i] = true;
  path[d++] = i;
  if (i == j){ // 找到一条简单路径
    for (int t = 0;t < d;t ++) cout << path[t] << ' ';
    cout << '\n';
    st[i] = false;
    return;
  }

  for (Node* p = head[i];p;p = p->next){
    int v = p->val;
    if (!st[v]){
      dfs(v,j,d);
    }
  }
  st[i] = false;
}

void paths(int vex1,int vex2){
  // 判断给定的邻接表存储的有向图的两顶点，输出两顶点间的所有简单路径
  memset(st,0,sizeof st);
  dfs(vex1,vex2,0);
}

int main(){
    int a[] = {1,5,2,3,3,3,3};int b[] = {5,2,6,6,7,1,5};
    for (int i = 0;i < 7;i ++) add(a[i],b[i]);
    
    paths(3, 6);
    //paths(3, 5);
    return 0;
}
```
