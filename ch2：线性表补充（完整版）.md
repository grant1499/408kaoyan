# chapter 2：线性表
## 习题2.3

> 链表代码最题常考。

### 单链表的定义

```C++
struct Node{
    int data;
    Node *next;
    
    Node():data(-1),next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList; // 给指向链表节点的指针取别名LinkList

LinkList L = new Node(); // 正确定义
LinkList L = new LinkList(); // 错误定义
```

### 5.链表翻转

将带头结点链表就地翻转。

```C++
#include <algorithm>
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node* next;

    Node():next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList;

void reverse(LinkList &L){
    if (!L->next || !L->next->next) return;
    LinkList p = L->next,q = p->next;
    p->next = NULL; // 这一步千万别忘
    while (q){
        auto t = q->next;
        q->next = p;
        p = q,q = t;
    }
    L->next = p;
}

int main(){
    LinkList head = new Node(-1);
    int a[] = {1,2,3,4,5,6};
    auto p = head;
    for (int i = 0;i < 6;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }

    for (auto p = head->next;p;p = p->next) cout << p->data << ' ';
    cout << '\n';
    reverse(head);
    for (auto p = head->next;p;p = p->next) cout << p->data << ' ';
    cout << '\n';
    return 0;
}
```

### 7.删除指定范围结点

带头结点链表，无序，删除表中所有介于给定的两个值之间的结点（如果存在）。

```C++
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node* next;

    Node():next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList;

void Delete(LinkList &L,int a,int b){ // 删除带头结点单链表中属于(a,b)的结点
    if (!L->next) return;
    for (auto p = L,q = p->next;q;){
        if (q->data > a && q->data < b) p->next = q->next;
        else p = p->next;
        q = q->next;
    }
}

int main(){
    LinkList head = new Node(-1);
    int a[] = {4,1,5,7,8,2};
    auto p = head;
    for (int i = 0;i < 6;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }

    for (auto p = head->next;p;p = p->next) cout << p->data << ' ';
    cout << '\n';
    Delete(head,3,8);
    for (auto p = head->next;p;p = p->next) cout << p->data << ' ';
    cout << '\n';
    return 0;
}
```

### 10.奇偶分解

将带头结点链表分解为两个带头结点的链表A和B，A中存放原链表中序号为奇数的结点，保持相对顺序，B中

存放原链表中序号为偶数的结点。

```C++
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node* next;

    Node():next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList;

void Divide(LinkList &L,LinkList &A,LinkList &B){
    // 将带头结点单链表分解为两个带头结点单链表，分别存放原表中序号为奇数和偶数的元素
    // 保持相对顺序不变
    if (!L->next) return;
    int id = 1; // 从1开始编号
    LinkList p = A,q = B,r = L->next;
    while (r){
        if (id ++ % 2){
            p->next = r;p = p->next;
        }
        else{
            q->next = r;q = q->next;
        }
        r = r->next;
    }
    p->next = q->next = NULL;
}

int main(){
    LinkList head = new Node(-1);
    int a[] = {4,1,5,7,8,2,11};
    auto p = head;
    for (int i = 0;i < 7;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }

    for (auto p = head->next;p;p = p->next) cout << p->data << ' ';
    cout << ' \n';
    LinkList A = new Node(-1),B = new Node(-1);
    Divide(head,A,B);
    for (auto p = A->next;p;p = p->next) cout << p->data << ' ';
    cout << ' \n';
    for (auto p = B->next;p;p = p->next) cout << p->data << ' ';
    cout << ' \n';
    return 0;
}
```

### 14.求公共链表

给定两个带头结点单链表，递增有序，构造含公共结点的链表，不破环原链表。

```C++
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node* next;

    Node():next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList;

void Divide(LinkList &L,LinkList &A,LinkList &B){
    // 将带头结点单链表分解为两个带头结点单链表，分别存放原表中序号为奇数和偶数的元素
    // 保持相对顺序不变
    if (!L->next) return;
    int id = 1; // 从1开始编号
    LinkList p = A,q = B,r = L->next;
    while (r){
        if (id ++ % 2){
            p->next = r;p = p->next;
        }
        else{
            q->next = r;q = q->next;
        }
        r = r->next;
    }
    p->next = q->next = NULL;
}

int main(){
    LinkList head = new Node(-1);
    int a[] = {4,1,5,7,8,2,11};
    auto p = head;
    for (int i = 0;i < 7;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }

    for (auto p = head->next;p;p = p->next) cout << p->data << ' ';
    cout << '\n';
    LinkList A = new Node(-1),B = new Node(-1);
    Divide(head,A,B);
    for (auto p = A->next;p;p = p->next) cout << p->data << ' ';
    cout << '\n';
    for (auto p = B->next;p;p = p->next) cout << p->data << ' ';
    cout << '\n';
    return 0;
}
```

### 16.判断连续子序列

给定两个单链表，无头结点，判断B是否为A的子序列。

```C++
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node* next;

    Node():next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList;

bool Pattern(LinkList A,LinkList B){ // 判断B序列是否为A的连续子序列
    // 给定单链表都带头结点
    if (!A->next || !B->next) return false;
    LinkList p = A->next,q = B->next,st = p; // st指针记录上次A串的起始匹配位置
    // 如A = 2，3，4，5，6，7 B = 2，3，7
    // 第一次st = 2，第二次st = 3
    while (p && q){
        if (p->data == q->data) p = p->next,q = q->next;
        else{
            st = st->next,p = st; // 匹配失败从上次的下个位置继续匹配
            q = B->next;
        }
    }
    if (!q) return true;
    return false;
}

int main(){
    LinkList head1 = new Node(-1);
    int a[] = {2,2,2,2,2,2,2,4};
    auto p = head1;
    for (int i = 0;i < 8;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }
    LinkList head2 = new Node(-1);
    auto q = head2;
    int b[] = {2,2,2,1};
    for (int i = 0;i < 4;i ++){
        q->next = new Node(b[i]);
        q = q->next;
    }
    
    cout << Pattern(head1, head2);
    cout << '\n';
    return 0;
}
```
