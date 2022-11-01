# chapter 5：树

> 有概率考察代码题，重要性仅次于链表。

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

### 3.非递归后序遍历二叉树

```C++
void reverse(int a[N],int l,int r){
    for (int i = l;i <= (l+r)>>1;i ++){
        swap(a[i],a[l+r-i]);
    }
}

int ans[N],k = 0; // 存放后序序列
void postOrder(BiTree root){ // 后序非递归遍历二叉树
    if (!root) return;
    BiTree stk[N];int top = -1; // 初始化栈
    stk[++top] = root;
    while (top != -1){
        BiTree t = stk[top--];
        ans[k++] = t->data;
        // 按照左->右的顺序入栈，右先出栈
        // 整体是根右左的顺序遍历，最后反转就行
        if (t->lchild) stk[++top] = t->lchild; thanks 
        if (t->rchild) stk[++top] = t->rchild;
    }
    reverse(ans,0,k-1); // 可以用栈访问来代替
}
```

### 4.二叉树的自下而上、从右到左层次遍历

 上题使用数组反转方式求的后序序列，本题使用栈来反转序列。

```C++
void level(BiTree root){ // 自下而上、从右到左层次遍历
    int stk[N],top = -1; // 初始化栈
    BiTree Q[N];int front = 0,rear = -1; // 初始化队列
    Q[++rear] = root;
    
    while (front <= rear) {
        BiTree t = Q[front++];
        stk[++top] = t->data;
        if (t->lchild) Q[++rear] = t->lchild;
        if (t->rchild) Q[++rear] = t->rchild;
    }
    
    while (top != -1) cout << stk[top--] << ' ';
}
```

### 16.二叉树叶子串成单链表

将二叉树中的叶子按从左到右的顺序串起来，用右指针域指向下个叶子。

二叉树中叶子的右指针域为空，在其存放指向下个叶子的单链表指针。

（并不是单独构建一个单链表，不懂题意的话看看王道习题讲解）

```C++
BiTree head = NULL,pre = NULL;
BiTree leaves(BiTree &root){ // 将二叉树的叶子从左到右串成单链表
    // 前中后序遍历中访问叶子的顺序都是从左到右，这里选择中序遍历
    if (root){
        leaves(root->lchild);
        if (!root->lchild && !root->rchild){
            if (!pre){
                head = pre = root;
            }
            else{
                pre->rchild = root;
                pre = root;
            }
        }
        leaves(root->rchild);
        pre->rchild = NULL; // 最后记得把尾结点指空
    }
    return head;
}
```

### 17.二叉树相似判定

```C++
bool similar(BiTree t1,BiTree t2){ // 判断给定的两颗二叉树是否相似
    // 相似指的是两树都空或者都只有根结点，或者两树的左子树相似都右子树相似
    if (!t1 && !t2) return true; // 两树都空则相似，如果都只有根会递归处理判断相似
    if (!t1 || !t2) return false; // 只有一树空不相似
    return similar(t1->lchild,t2->lchild) && similar(t1->rchild,t2->rchild);
}
```

## 习题5.4

### 6.构造孩子兄弟链表

已知树的层次序列和每个结点的度，构造孩子兄弟链表。

```C++
BiTree create(int post[],int degree[],int n){ // 已知树的层次序列和每个结点的度，构造孩子兄弟链表
  // 说明：构造孩子兄弟链表，这里就用二叉链表代替
  BiTree tree[N];
  for (int i = 0;i < n;i ++){
    tree[i] = new BiTNode(post[i]);
  }
  int k = 0;
  for (int i = 0;i < n;i ++){
    if (!degree[i]) break;
    tree[i]->lchild = tree[++k];
    for (int j = 2;j <= degree[i];j ++){
      tree[k]->rchild = tree[k+1];
      k ++;
    }
  }
  return tree[0];
}
```
