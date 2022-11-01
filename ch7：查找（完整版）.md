# chapter 7：查找

> 代码题极少考察。

## 习题7.3

### 6.判定二叉排序树

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

int pre = INT_MIN;
bool judge(BiTree root){ // 判断给定二叉树是否为BST，BST的中序递增有序
  if (!root) return true;
  if (!judge(root->lchild) || pre > root->data) return false;
  pre = root->data;
  if (!judge(root->rchild)) return false;
  return true;
}

int main(){
    int a[8] = {45,24,53,12,1,4,6};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树

    cout << judge(root) << '\n';
    return 0;
}
```

### 7.求结点在二叉排序树中的层次

与求二叉树高度不同，在BST中每查找一次就下降一层。

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

int n = 0;
int level(BiTree root,int x){ // 求给定结点x在BST中的层次
    if (!root) return -1;
    n ++;
    if (x == root->data) return n;
    if (x < root->data) return level(root->lchild,x);
    else return level(root->rchild,x);
}

int main(){
    int a[8] = {45,24,53,12,1,4,6};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树

    cout << level(root,12) << '\n';
    return 0;
}
```

### 8.判定平衡二叉树

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

int cnt(BiTree root){ // 求二叉树的结点总数
    if (!root) return 0;
    return cnt(root->lchild) + cnt(root->rchild) + 1;
}

bool isBBST(BiTree root){ // 判断二叉树是否为BBST，看每个结点的平衡因子
    if (!root) return true;
    int fact = abs(cnt(root->lchild) - cnt(root->rchild));
    if (fact >= 2) return false;
    return isBBST(root->lchild) && isBBST(root->rchild);
}

int main(){
    int a[8] = {45,24,53,12,1,4,6};
    BiTree root = NULL;
    for (int i = 0;i < 5;i ++) insert(root,a[i]); // BST构建二叉树

    cout << isBBST(root) << '\n';
    return 0;
}
```

### 9.求二叉排序树的最小和最大关键字

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

int cnt(BiTree root){ // 求二叉树的结点总数
    if (!root) return 0;
    return cnt(root->lchild) + cnt(root->rchild) + 1;
}

int getMin(BiTree root){  // 求BST的最小关键字
    if (!root) return -1;
    if (root->lchild) return getMin(root->lchild);
    return root->data;
}

int getMax(BiTree root){  // 求BST的最大关键字
    if (!root) return -1;
    if (root->rchild) return getMax(root->rchild);
    return root->data;
}

int main(){
    int a[8] = {45,24,53,12,1,4,6};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树

    cout << getMin(root) << ", " << getMax(root) << '\n';
    return 0;
}
```

### 10.从大到小输出二叉排序树所有>=x关键字

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


void solve(BiTree root,int x){  // 从大到小输出BST所有>= x的关键字
    if (!root) return;
    solve(root->rchild,x); // 右根左的顺序遍历
    if (root->data >= x) cout << root->data << ' ';
    solve(root->lchild,x);
}

int main(){
    int a[8] = {45,24,53,12,1,4,6};
    BiTree root = NULL;
    for (int i = 0;i < 7;i ++) insert(root,a[i]); // BST构建二叉树

    solve(root,12);
    cout << '\n';
    return 0;
}
```
