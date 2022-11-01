# chapter 5：树

> 有概率考察代码题。

## 习题5.3

### 二叉树结构体定义

```C++
struct BiTNode{
  int data;
  BiTNode *lchild,*rchild;
  
  BiTNode():data(-1),lchild(NULL),rchild(NULL){}
  BiTNode(int _data):data(_data),lchild(NULL),rchild(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

BiTree L = new BiTNode(); // 正确定义
BiTree L = new BiTree(); // 错误定义
```

### 5.求二叉树高度

1.非递归版本

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct BiTNode{
  int data;
  BiTNode *lchild,*rchild;
  
  BiTNode():data(-1),lchild(NULL),rchild(NULL){}
  BiTNode(int _data):data(_data),lchild(NULL),rchild(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

void insert(BiTree &t,int val){
    if (!t) t = new BiTNode(val);
    else if (val == t->data) t->lchild = new BiTNode(val);
    else if (val < t->data) insert(t->lchild,val);
    else if (val > t->data) insert(t->rchild,val);
}

int getHeight(BiTree t){ // 非递归求二叉树高度
    BiTree Q[N];int front = 0,rear = -1; // 数组模拟队列
    int last = 0,level = 0; // level记录当前结点所在层数，last指向当前层的最右结点
    
    Q[++rear] = t;
    while (front <= rear){
        BiTree k = Q[front ++];
        if (k->lchild) Q[++rear] = k->lchild;
        if (k->rchild) Q[++rear] = k->rchild;
        if (last == front-1){ // 如果本轮last出队，层数++，最后一个入队的更新为last
            level ++,last = rear;
        }
    }
    return level;
}

int main(){
    int a[8] = {45,24,53,12};
    BiTree root = NULL;
    for (int i = 0;i < 4;i ++) insert(root,a[i]); // BST构建二叉树

    cout << getHeight(root) << '\n';
    return 0;
}
```

2.递归版本

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct BiTNode{
  int data;
  BiTNode *lchild,*rchild;
  
  BiTNode():data(-1),lchild(NULL),rchild(NULL){}
  BiTNode(int _data):data(_data),lchild(NULL),rchild(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

void insert(BiTree &t,int val){
    if (!t) t = new BiTNode(val);
    else if (val == t->data) t->lchild = new BiTNode(val);
    else if (val < t->data) insert(t->lchild,val);
    else if (val > t->data) insert(t->rchild,val);
}

int getHeight(BiTree t){ // 递归求二叉树高度
    if (!t) return 0;
    int a = getHeight(t->lchild);
    int b = getHeight(t->rchild);
    return a > b ? a+1 : b+1;
}

int main(){
    int a[8] = {45,24,53,12};
    BiTree root = NULL;
    for (int i = 0;i < 4;i ++) insert(root,a[i]); // BST构建二叉树

    cout << getHeight(root) << '\n';
    return 0;
}
```

### 6.先序+中序建立二叉树

二叉树的每个结点值互不相同，先序和中序序列存在两个数组A和B中。

[18.重建二叉树](https://www.acwing.com/problem/content/23/)

```C++
class Solution {
public:
    int rootInorder(int root){
        for (int i = 0;i < _inorder.size();i ++){
            if (root == _inorder[i]) return i;
        }
    }
    
    vector<int> _preorder,_inorder;
    TreeNode* build(int a,int b,int x,int y){ // 根据先序[a,b]和中序[x,y]建树
        if (a > b) return NULL;
        int root = _preorder[a];
        TreeNode* t = new TreeNode(root);
        int k = rootInorder(root);
        t->left = build(a+1,a+k-x,x,k-1);
        t->right = build(a+k-x+1,b,k+1,y);
        return t;
    }
    
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        _preorder = preorder,_inorder = inorder;
        int n = preorder.size();
        return build(0,n-1,0,n-1);
    }
};
```

### 7.完全二叉树判定

判定一个给出的二叉树是否为完全二叉树。

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct BiTNode{
  int data;
  BiTNode *lchild,*rchild;
  
  BiTNode():data(-1),lchild(NULL),rchild(NULL){}
  BiTNode(int _data):data(_data),lchild(NULL),rchild(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

void insert(BiTree &t,int val){ // 构造二叉排序树
    if (!t) t = new BiTNode(val);
    else if (val == t->data) t->lchild = new BiTNode(val);
    else if (val < t->data) insert(t->lchild,val);
    else if (val > t->data) insert(t->rchild,val);
}

int solve(BiTree t){ // 判定完全二叉树
    if (!t) return 1;
    // 层次遍历，空结点也要入队，如果空结点后面存在非空结点，则非完全二叉树
    BiTree Q[N];int front = 0,rear = -1; // 数组模拟队列
    Q[++rear] = t;
    
    while (front <= rear){
        BiTree k = Q[front++];
        if (k){
            Q[++rear] = k->lchild,Q[++rear] = k->rchild;
        }else{ // 对于空结点，没有子结点要入队，如果它在层次序列的后面存在非空结点，则非完全二叉树
            while (front <= rear){
                k = Q[front++];
                if (k) return 0;
            }
        }
    }
    return 1;
}

int main(){
    int a[8] = {4,2,6,1,3,5,7};
    //int a[8] = {4,5,1,2,3};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树

    cout << solve(root) << '\n';
    return 0;
}
```

### 8.二叉树的双分支结点个数

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct BiTNode{
  int data;
  BiTNode *lchild,*rchild;
  
  BiTNode():data(-1),lchild(NULL),rchild(NULL){}
  BiTNode(int _data):data(_data),lchild(NULL),rchild(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

void insert(BiTree &t,int val){ // 构造二叉排序树
    if (!t) t = new BiTNode(val);
    else if (val == t->data) t->lchild = new BiTNode(val);
    else if (val < t->data) insert(t->lchild,val);
    else if (val > t->data) insert(t->rchild,val);
}

int solve(BiTree t){ // 递归求二叉树的双分支结点个数
    if (!t) return 0;
    int ans = t->lchild && t->rchild;
    if (t->lchild) ans += solve(t->lchild);
    if (t->rchild) ans += solve(t->rchild);
    return ans;
}

int main(){
    int a[8] = {4,2,6,1,3,5,7};
    //int a[8] = {4,5,1,2,3};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树

    cout << solve(root) << '\n';
    return 0;
}
```

### 9.交换二叉树的左右子树

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct BiTNode{
  int data;
  BiTNode *lchild,*rchild;
  
  BiTNode():data(-1),lchild(NULL),rchild(NULL){}
  BiTNode(int _data):data(_data),lchild(NULL),rchild(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

void insert(BiTree &t,int val){ // 构造二叉排序树
    if (!t) t = new BiTNode(val);
    else if (val == t->data) t->lchild = new BiTNode(val);
    else if (val < t->data) insert(t->lchild,val);
    else if (val > t->data) insert(t->rchild,val);
}

void bfs(BiTree t){
    BiTree Q[N];int front = 0,rear = -1; // 数组模拟队列
    Q[++rear] = t;
    if (!t) return;
    while (front <= rear){
        BiTree k = Q[front++];cout << k->data << ' ';
        if (k->lchild) Q[++rear] = k->lchild;
        if (k->rchild) Q[++rear] = k->rchild;
    }
    cout << '\n';
}

void swap(BiTree &t){ // 递归交换二叉树的左右子树
    if (!t) return;
    BiTree temp = t->lchild;
    t->lchild = t->rchild;
    t->rchild = temp;
    swap(t->lchild),swap(t->rchild);
}

int main(){
    int a[8] = {4,2,6,1,3,5,7};
    //int a[8] = {4,5,1,2,3};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树

    bfs(root);
    swap(root);
    bfs(root);
    return 0;
}
```

### 10.二叉树先序第K个结点

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct BiTNode{
  int data;
  BiTNode *lchild,*rchild;
  
  BiTNode():data(-1),lchild(NULL),rchild(NULL){}
  BiTNode(int _data):data(_data),lchild(NULL),rchild(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

void insert(BiTree &t,int val){ // 构造二叉排序树
    if (!t) t = new BiTNode(val);
    else if (val == t->data) t->lchild = new BiTNode(val);
    else if (val < t->data) insert(t->lchild,val);
    else if (val > t->data) insert(t->rchild,val);
}

int r = 0; // 记录当前遍历到先序第r个结点（从1开始）
int preOrder(BiTree t,int k){ // 递归求先序第K个结点值
    if (!t) return -1;
    r ++;
    if (r == k) return t->data;
    int a = preOrder(t->lchild,k); // 递归左子树
    if (a != -1) return a;
    a = preOrder(t->rchild,k); // 递归右子树
    if (a != -1) return a;
}

int main(){
    int a[8] = {4,2,6,1,3,5,7};
    //int a[8] = {4,5,1,2,3};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树

    cout << preOrder(root,5) << '\n';
    return 0;
}
```

### 11.删除二叉树给定元素子树

对于树中每个元素值为X的结点，删除以它为根的子树，释放空间。

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct BiTNode{
  int data;
  BiTNode *lchild,*rchild;
  
  BiTNode():data(-1),lchild(NULL),rchild(NULL){}
  BiTNode(int _data):data(_data),lchild(NULL),rchild(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

void insert(BiTree &t,int val){ // 构造二叉排序树
    if (!t) t = new BiTNode(val);
    else if (val == t->data) t->lchild = new BiTNode(val);
    else if (val < t->data) insert(t->lchild,val);
    else if (val > t->data) insert(t->rchild,val);
}

void bfs(BiTree t){
    BiTree Q[N];int front = 0,rear = -1; // 数组模拟队列
    Q[++rear] = t;
    if (!t) return;
    while (front <= rear){
        BiTree k = Q[front++];cout << k->data << ' ';
        if (k->lchild) Q[++rear] = k->lchild;
        if (k->rchild) Q[++rear] = k->rchild;
    }
    cout << '\n';
}

void Delete(BiTree &t){ // 后序遍历递归删除一棵树
    if (!t) return;
    Delete(t->lchild),Delete(t->rchild);
    free(t);
}

void dfs(BiTree &t,int x){ // 递归删除所有以X为根的子树
    if (!t) return;
    if (t->data == x){
        Delete(t);
        t = NULL; // 删除以t为根的树后记得把t置空！！！
        return;
    }
    dfs(t->lchild,x),dfs(t->rchild,x);
}

int main(){
    int a[8] = {4,2,6,1,3,5,7};
    //int a[8] = {4,5,1,2,3};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树

    bfs(root);
    dfs(root,1);
    bfs(root);
    return 0;
}
```

### 12.求二叉树结点的祖先

给定查找结点值X，输出它的所有祖先（X不重复！）

**非递归后序遍历出栈可以得到X的所有祖先，选择题考过！**

把后序遍历递归栈的变化模拟出来就可以知道：**当访问到结点x时，有三种情况：①. x的左右子树均未访问，未入栈。②. x的左子树已经访问完成且出栈，x的右子树未访问。③. x的所有子结点均已访问且出栈**。无论哪种情况，当访问到结点x时，栈中都不会有x的子结点，此时的栈中结点均为x的祖先节点。

这里用先序递归来解决，后序非递归比较麻烦（王道写法）。

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct BiTNode{
  int data;
  BiTNode *lchild,*rchild;
  
  BiTNode():data(-1),lchild(NULL),rchild(NULL){}
  BiTNode(int _data):data(_data),lchild(NULL),rchild(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

void insert(BiTree &t,int val){ // 构造二叉排序树
    if (!t) t = new BiTNode(val);
    else if (val == t->data) t->lchild = new BiTNode(val);
    else if (val < t->data) insert(t->lchild,val);
    else if (val > t->data) insert(t->rchild,val);
}

void bfs(BiTree t){
    BiTree Q[N];int front = 0,rear = -1; // 数组模拟队列
    Q[++rear] = t;
    if (!t) return;
    while (front <= rear){
        BiTree k = Q[front++];cout << k->data << ' ';
        if (k->lchild) Q[++rear] = k->lchild;
        if (k->rchild) Q[++rear] = k->rchild;
    }
    cout << '\n';
}

int ans[N],k = 0; // 存放x的所有祖先
bool find(BiTree t,int x){ // 递归输出x的所有祖先，当结点t为x的祖先时会标记为true
    if (!t) return false;
    if (t->data == x) return true;
    if (find(t->lchild,x)){
        ans[k++] = t->data;
        return true;
    }
    if (find(t->rchild,x)){ // 左右子树最多只有一个包含x，t最多添加一次
        ans[k++] = t->data;
        return true;
    }
    return false;
}

int main(){
    int a[8] = {4,2,6,1,3,5,7};
    //int a[8] = {4,5,1,2,3};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树

    find(root,4);
    for (int i = 0;i < k;i ++) cout << ans[i] << ' ';
    cout << '\n';
    return 0;
}
```

### 14.求二叉树宽度

求非空二叉树宽度（结点数最多那层的结点数）。

和第5题非递归求二叉树高度做法类似。

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct BiTNode{
  int data;
  BiTNode *lchild,*rchild;
  
  BiTNode():data(-1),lchild(NULL),rchild(NULL){}
  BiTNode(int _data):data(_data),lchild(NULL),rchild(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

void insert(BiTree &t,int val){ // 构造二叉排序树
    if (!t) t = new BiTNode(val);
    else if (val == t->data) t->lchild = new BiTNode(val);
    else if (val < t->data) insert(t->lchild,val);
    else if (val > t->data) insert(t->rchild,val);
}

int getWidth(BiTree t){ // 求二叉树宽度，结点数最多的一层
    BiTree Q[N];int front = 0,rear = -1; // 数组模拟队列
    Q[++rear] = t;
    
    int last = 0,ans = 1;
    // last维护上一层的最右结点在队列的下标，ans更新每层最大结点数
    while (front <= rear){
        BiTree k = Q[front++];
        if (k->lchild) Q[++rear] = k->lchild;
        if (k->rchild) Q[++rear] = k->rchild;
        if (last == front-1){ // 上层最右结点出队，此后入队的都是本层结点
            ans = max(ans,rear-last);
            last = rear; // 更新最右结点
        }
    }
    return ans;
}

int main(){
    int a[8] = {4,2,6,1,3,5,7};
    //int a[8] = {4,5,1,2,3};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树

    cout << getWidth(root) << '\n';
    return 0;
}
```

### 15.已知满二叉树的先序求后序

满二叉树的所有结点值各不相同，已知先序求后序。

不会。模仿前面第6题，递归处理。

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct BiTNode{
  int data;
  BiTNode *lchild,*rchild;
  
  BiTNode():data(-1),lchild(NULL),rchild(NULL){}
  BiTNode(int _data):data(_data),lchild(NULL),rchild(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

void insert(BiTree &t,int val){ // 构造二叉排序树
    if (!t) t = new BiTNode(val);
    else if (val == t->data) t->lchild = new BiTNode(val);
    else if (val < t->data) insert(t->lchild,val);
    else if (val > t->data) insert(t->rchild,val);
}

int n = 7;
int post[N]; // 存放后序序列
void postOrder(int pre[],int a,int b,int x,int y){ // 已知满二叉树的先序求后序
    if (a > b) return;
    post[y] = pre[a]; // 确定根的位置
    int k = (b-a)/2;
    postOrder(pre,a+1,a+1+k-1,x,x+k-1); // 自己对着先序、后序序列画个图就清楚了
    postOrder(pre,a+k+1,b,x+k,y-1);
}

void dfs(BiTree t){
    if (!t) return;
    dfs(t->lchild);
    dfs(t->rchild);
    cout << t->data << ' ';
}

int main(){
    int a[8] = {4,2,6,1,3,5,7};
    //int a[8] = {4,5,1,2,3};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树
    
    int pre[N+1] = {4,2,1,3,6,5,7};
    dfs(root);
    cout << '\n';
    postOrder(pre,0,n-1,0,n-1);
    for (int i = 0;i < n;i ++) cout << post[i] << ' ';
    cout << '\n';
    return 0;
}
```

### 19.（2014真题）二叉树的带权路径长度

带权路径长度（WPL）指的是二叉树中所有叶结点的带权路径长度之和。

[3766. 二叉树的带权路径长度](https://www.acwing.com/problem/content/3769/),     DFS遍历树。

```C++
class Solution {
public:
    int dfs(TreeNode* root,int distance){
        if (!root) return 0;
        if (!root->left && !root->right) return distance*root->val;
        return dfs(root->left,distance+1) + dfs(root->right,distance+1);
    }
    
    int pathSum(TreeNode* root) {
        return dfs(root,0);
    }
};
```

### 20.（2017真题）表达式树转中缀表达式

将给定表达式树（二叉树）转化为等价的中缀表达式。

[3765. 表达式树](https://www.acwing.com/problem/content/3768/)

```C++
class Solution {
public:
    string str;
    void dfs(TreeNode* t){
        if (!t) return;
        if (!t->left && !t->right){
            str += t->val;return;
        }
        
        str += "(";
        dfs(t->left),str += t->val,dfs(t->right);
        str += ")";
    }
    
    string expressionTree(TreeNode* root) {
        dfs(root->left),str += root->val,dfs(root->right);
        return str;
    }
};
```

## 习题5.4

### 4.孩子兄弟法的森林叶子结点数

孩子兄弟表示法的森林（二叉树），通过二叉树计算森林的叶子结点数。

递归处理，根无孩子的则根必然是森林的叶子结点，根有孩子则森林的叶子结点数就是左右子树之和。

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct BiTNode{
  int data;
  BiTNode *child,*sibling;
  
  BiTNode():data(-1),child(NULL),sibling(NULL){}
  BiTNode(int _data):data(_data),child(NULL),sibling(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

void insert(BiTree &t,int val){ // 构造二叉排序树
    if (!t) t = new BiTNode(val);
    else if (val == t->data) t->child = new BiTNode(val);
    else if (val < t->data) insert(t->child,val);
    else if (val > t->data) insert(t->sibling,val);
}

int leaves(BiTree t){ // 求孩子兄弟表示法的森林的叶子结点数
    if (!t) return 0;
    if (!t->child) return 1 + leaves(t->sibling); // 根（叶结点） + 兄弟子树叶结点 
    else return leaves(t->child) + leaves(t->sibling); // 孩子、兄弟子树叶结点
}

int main(){
    int a[8] = {4,2,6,1,3,5,7};
    //int a[8] = {4,5,1,2,3};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树
    
    cout << leaves(root) << '\n';
    return 0;
}
```

### 5.孩子兄弟法表示的森林的深度

以孩子兄弟法表示森林，定义森林的深度为所有树中深度最大值。

```C++
#include <bits/stdc++.h>
using namespace std;
const int N = 100;
struct BiTNode{
  int data;
  BiTNode *child,*sibling;
  
  BiTNode():data(-1),child(NULL),sibling(NULL){}
  BiTNode(int _data):data(_data),child(NULL),sibling(NULL){}
};
typedef BiTNode* BiTree; // 给指向树结点的指针取别名BiTree

void insert(BiTree &t,int val){ // 构造二叉排序树
    if (!t) t = new BiTNode(val);
    else if (val == t->data) t->child = new BiTNode(val);
    else if (val < t->data) insert(t->child,val);
    else if (val > t->data) insert(t->sibling,val);
}

int depth(BiTree t){ // 求孩子兄弟法表示的森林的深度，只有根深度算1
    if (!t) return 0;
    return max(depth(t->child) + 1, depth(t->sibling));
}

int main(){
    int a[8] = {4,2,6,1,3,5,7};
    //int a[8] = {4,5,1,2,3};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树
    
    cout << depth(root) << '\n';
    return 0;
}
```

