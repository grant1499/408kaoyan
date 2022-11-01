# chapter 7：查找

> 代码题极少考察。

## 习题7.3

### 6.判定二叉排序树

```C++
int pre = INT_MIN;
bool judge(BiTree root){ // 判断给定二叉树是否为BST，BST的中序递增有序
  if (!root) return true;
  if (!judge(root->lchild) || pre > root->data) return false;
  pre = root->data;
  if (!judge(root->rchild)) return false;
  return true;
}
```

### 7.求结点在二叉排序树中的层次

与求二叉树高度不同，在BST中每查找一次就下降一层。

```C++
int n = 0;
int level(BiTree root,int x){ // 求给定结点x在BST中的层次
    if (!root) return -1;
    n ++;
    if (x == root->data) return n;
    if (x < root->data) return level(root->lchild,x);
    else return level(root->rchild,x);
}
```

### 8.判定平衡二叉树

```C++
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
```

### 9.求二叉排序树的最小和最大关键字

```C++
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
```

### 10.从大到小输出二叉排序树所有>=x关键字

```C++
void solve(BiTree root,int x){  // 从大到小输出BST所有>= x的关键字
    if (!root) return;
    solve(root->rchild,x); // 右根左的顺序遍历
    if (root->data >= x) cout << root->data << ' ';
    solve(root->lchild,x);
}
```
