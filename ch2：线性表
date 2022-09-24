# chapter 2：线性表

## 习题2.2

### 2.翻转顺序表

```C++
#include <bits/stdc++.h>
using namespace std;
int a[105];

 // 翻转顺序表 a[n]
void Reverse(int a[],int l,int r){
    for (int i = l;i <= (l+r) >> 1;i ++){
        swap(a[i],a[l+r-i]);
    }
}

int main(){
    
    for (int i = 0;i < 10;i ++) cin >> a[i];
    
    int n = 10;
    Reverse(a,0,n-1); // 翻转顺序表 a[n]
    for (int i = 0;i < n;i ++) cout << a[i] << ' ';
    cout << endl;
    
    return 0;
}
```

### 5.删除顺序表指定范围元素

从顺序表中删除位于`a ~ b , a < b`的元素值。

```C++
#include <bits/stdc++.h>
using namespace std;

int a[10] = {1,2,3,4,5,6,7,8,9,10};
int len_a = 10;

void Del(int a[],int n,int s,int t){ // 删除顺序表a[n]中>= s && <= t 的元素，且s < t
    if (s >= t) return;
    
    int cnt = 0; // 记录待删除元素个数
    for (int i = 0;i < n;i ++){
        if (a[i] >= s && a[i] <= t) cnt ++;
        else a[i - cnt] = a[i]; // 不用删除的元素前移 cnt 个位置
    }
    len_a -= cnt; // 维护顺序表长度
}

int main(){
    Del(a,len_a,2,8);
    for (int i = 0;i < len_a;i ++) cout << a[i] << ' ';
    cout << endl;
    cout << "len_a : " << len_a << '\n';
    return 0;
}
```

### 6.删除有序表重复值

对于重复出现的值，只保留一个即可。

双指针扫描有序表。

```C++
#include <bits/stdc++.h>
using namespace std;

int a[10] = {1,2,2,2,3,4,4,5,5,5};
int len_a = 10;

void Del(int a[],int n){ // 删除有序表中重复元素，双指针算法
    int i = 0,j = 1;
    for (;j < n;j ++){ // i指针指向可存放元素的位置，j指针遍历数组
        // i~j元素相同时i指针不动
        if (a[i] != a[j]) a[++i] = a[j]; // 当前元素a[j]放在i指针指向位置
    }
    len_a = i + 1;
}

int main(){
    Del(a,len_a);
    for (int i = 0;i < len_a;i ++) cout << a[i] << ' ';
    cout << '\n';
    cout << "len_a : " << len_a << '\n';
    return 0;
}
```

### 8.顺序表交换

数组`a[m+n]`连续存放两个顺序表`(a1,a2,...,am)`和`(b1,b2,...,bn)`，将 a 数组重新排列为`(b1,b2,...,bn,a1,a2,...,am)`。

```C++
#include <bits/stdc++.h>
using namespace std;

int a[10] = {1,2,3,4,5,6,7,8,9,10}; // l1 = {1,2,3,4}, l2 = {5,6,7,8,9,10}
int m = 4,n = 6;

void Reverse(int a[],int l,int r){
    for (int i = l;i <= (l+r) >> 1;i ++){
        swap(a[i],a[l+r-i]);
    }
}

void Resort(int a[],int m,int n){
    Reverse(a,0,m+n-1);
    Reverse(a,0,n-1);
    Reverse(a,n,m+n-1);
}

int main(){
    Resort(a,m,n);
    for (int i = 0;i < m+n;i ++) cout << a[i] << ' ';
    cout << '\n';
    return 0;
}
```

### 9.有序表元素查找（二分）

找到元素 x 后将其与后继元素交换，否则插入表中并保持递增有序。

```C++
#include <bits/stdc++.h>
using namespace std;
int a[10] = {1,2,3,4,5,7,8,9,10};int m = 9;
int b[10] = {1,1,3,4,4,7,8,10,10};int n = 9;

bool Binarysearch(int a[],int n,int x){ // 二分查找x在a中出现的第一个位置
    // 注意二分的两种模板，l = mid + 1对应(l+r)>>1，l = mid对应(l+r+1)>>1
    int l = 0,r = n-1;
    while (l < r){
        int mid = (l+r) >> 1;
        if (a[mid] >= x) r = mid;
        else l = mid + 1;
    }
    
    if (a[l] == x && l < n-1)  swap(a[l],a[l+1]); // 查找到x且x有后继则交换
    else if (a[l] != x){
        // 若查找失败，l会指向比x大的第一个位置，比如查找a数组中的6会指向7
        for (int i = n;i >= l+1;i --) a[i] = a[i-1];
        a[l] = x; // 插入x
        return false;
    }
    return true;
}

int main(){
    if (!Binarysearch(a,m,6)) m ++;
    for (int i = 0;i < m;i ++) cout << a[i] << ' ';
    cout << '\n';
    cout << "m : " << m << '\n';
    
    if (!Binarysearch(b,n,7)) n ++;
    for (int i = 0;i < n;i ++) cout << b[i] << ' ';
    cout << '\n';
    cout << "n : " << n << '\n';
    return 0;
}
```

### 10.（2010真题）顺序表移位

将数组循环左移 p 位。

如`(A0,A1,...,An-1)`变`(Ap,Ap+1,..,Ap-1)`。

举例：`(1,2,3,4,5)`循环左移3位，变为`(4,5,1,2,3)`。翻转三次。

```C++
void Move(int a[],int n,int p){
    Reverse(a,0,n-1); // Reverse函数的实现参考上面第二题
    Reverse(a,0,n-p-1);
    Reverse(a,n-p,n-1);
}
```

### 11.（2011真题）两序列中位数

给定两个**等长升序**序列（长度为n）A和B，找出处于`ceil(n/2)`位置的中位数。

本题最优解法参考王道习题解答和力扣题解：[寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/) 。

这里采用合并序列再求中位数的暴力解法。

时间复杂度：O(n)，空间复杂度：O(n)。

```C++
#include <bits/stdc++.h>
using namespace std;
int s1[] = {11,13,15,17,19};
int s2[] = {2,4,6,8,20};
int n = 5;

int Get(int s1[],int s2[],int n){ // 合并升序序列s1和s2，并返回ceil(2*n/2)位置的数
    int temp[2*n];
    int l = 0,r = 0,k = 0;
    while (l < n && r < n){
        if (s1[l] <= s2[r]) temp[k++] = s1[l++];
        else temp[k++] = s2[r++];
    }
    while (l < n) temp[k++] = s1[l++];
    while (r < n) temp[k++] = s2[r++];
    return temp[n-1]; // 位置从1开始计算，第n个位置的下标是n-1
}

int main(){
    cout << Get(s1,s2,n) << '\n';
    return 0;
}
```

### 12.（2013真题）查找主元素

设整数序列A有n个元素，主元素即出现次数超过n/2的数；若存在主元素则输出，否则输出-1。

思路：快排，双指针查找主元素。

```C++
#include <bits/stdc++.h>
using namespace std;
int a[] = {0,5,5,3,5,7,5,5};int len_a = 8;
int b[] = {0,5,5,3,1,5,7};int len_b = 7;

void QuickSort(int a[],int l,int r){ // 参考王道的手写快排算法
    if (l >= r) return;
    int i = l,j = r,k = a[l]; // k表示枢轴，以k值将序列一分为二
    while (i < j){
        while (i < j && a[j] >= k) j --; // i < j必须加上
        while (i < j && a[i] <= k) i ++; // 两指针的移动顺序不要变！j在前！
        // 双指针扫描数组，i找到第一个>k的数，j找到第一个<k的数，然后交换
        if (i < j) swap(a[i],a[j]); // j指针先行最后i必然指向< k的位置
    }
    swap(a[i],a[l]); // 枢轴最后与i指针指向元素交换
    QuickSort(a,l,i-1),QuickSort(a,i+1,r);
}

int Get(int a[],int n){
    QuickSort(a,0,n-1);
    // 双指针判断主元素,i~j的数 >= n/2 + 1 时才是主元素，int下取整
    for (int i = 0,j = 1;j < n;j ++){
        while (a[j] == a[i]){
            if (j-i+1 < (n/2 + 1)) j ++;
            else return a[i];
        }
        i = j;
    }
    return -1;
}

int main(){
    cout << Get(a,len_a) << '\n';
    cout << Get(b,len_b) << '\n';
    cout << Get(c,len_c) << '\n';
    return 0;
}
```

### 13.（2018真题）数组未出现的最小整数

数组包含n个整数，找到未出现的最小整数。

```C++
#include <bits/stdc++.h>
using namespace std;
int a[] = {1,-2,3,4,7,8,7,9,10};
int len_a = 9;
int b[] = {1,2,3};
int len_b = 3;

int Get(int a[],int n){
    bool st[n+2];
    for (int i = 0;i < n;i ++){
        if (a[i] <= n && a[i] >= 1) st[a[i]] = true;
    }
    
    for (int i = 1;i <= n;i ++){
        if (!st[i]) return i;
    }
    return n+1;
}

int main(){
    cout << Get(a,len_a) << '\n';
    cout << Get(b,len_b) << '\n';
    return 0;
}
```

### 14.（2020真题）三元组最小距离

三元组距离定义为：在三个升序数组中各自任选一个数，两两作差取绝对值后再相加即为距离。

```C++
#include <bits/stdc++.h>
using namespace std;
typedef long long LL;
const int N = 1e5+5;
int a[N],b[N],c[N];

int main(){
    int l,m,n;
    cin >> l >> m >> n;
    
    for (int i = 0;i < l;i ++) cin >> a[i];
    for (int i = 0;i < m;i ++) cin >> b[i];
    for (int i = 0;i < n;i ++) cin >> c[i];
    
    int x = 0,y = 0,z = 0;
    LL ans = 1e10; // 距离可能爆int
    for (;x < l && y < m && z < n;){ // x,y,z三指针扫描
        int i = a[x],j = b[y],k = c[z];
        ans = min(ans,2*((LL)max(max(i,j),k) - min(min(i,j),k)));
		// 三元组距离等于最大数与最小数差的2倍
        if (i <= j && i <= k) x ++; // 最小的数向前移动一位
        else if (j <= i && j <= k) y ++;
        else if (k <= i && k <= j) z ++;
    }
    cout << ans << '\n';
    return 0;
}
```

## 习题2.3

### 1.递归删除链表指定值

递归删除不带头节点的单链表中所有值为x的节点。

```C++
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node *next;
    
    Node():data(-1),next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList; // 给指向链表节点的指针取别名LinkList

void Del(LinkList &L,int x){ // 递归删除不带头节点链表中所有x值节点
    if (!L) return; // 空表
    if (L->data == x){ // 当前节点L需要删除
        LinkList p = L->next;
        free(L);
        L = p;
        Del(L,x);
    }
    else Del(L->next,x); // 跳到下一节点继续删除
}

int main(){
    // 生成测试单链表（无头节点）head指向第一个节点（有值）
    LinkList head = new Node(1);
    LinkList p = head;
    int a[6] = {3,2,3,4,3,6};
    for (int i = 0;i <= 5;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }
    for (LinkList p = head;p != NULL;p = p->next) cout << (p->data) << ' ';
    cout << '\n';
    
    Del(head,3);
    for (LinkList p = head;p != NULL;p = p->next) cout << (p->data) << ' ';
    cout << '\n';
    return 0;
}
```

### 2.删除链表指定值

删除带头节点的单链表中所有值为x的节点。

```C++
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node *next;
    
    Node():data(-1),next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList; // 给指向链表节点的指针取别名LinkList

void Del(LinkList &L,int x){ // 删除带头节点链表中所有x值节点
    LinkList p = L,q = L->next;
    while (q != NULL){ // q指针遍历链表每个数据节点
        q = p->next;
        if (q && q->data == x){ // q指针指向x则删除
            p->next = q->next;
            free(q);
        }
        else p = p->next; // 否则前驱节点指针p后移
    }
}

int main(){
    // 生成测试单链表（有头节点）head即头节点
    LinkList head = new Node();
    LinkList p = head;
    // int a[6] = {3,2,3,4,3,6};
    int a[6] = {1,1,2,1,1,1};
    for (int i = 0;i <= 5;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }
    
    Del(head,1);
    for (LinkList p = head->next;p != NULL;p = p->next) cout << (p->data) << ' ';
    cout << '\n';
    return 0;
}
```

### 3.链表反向输出

反向输出带头节点链表中的所有节点。

思路：

1.遍历链表构建数组逆向输出；

2.遍历链表存入栈中弹栈输出；

3.反转链表再遍历输出；

3.递归输出链表（2的变形）。

```C++
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node *next;
    
    Node():data(-1),next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList; // 给指向链表节点的指针取别名LinkList

void Print(LinkList head){ // 反向输出带头节点链表
    if (head->next == NULL){
        cout << head->data << ' ';return;
    }
    Print(head->next); // 递归输出
    cout << head->data << ' ';
}

int main(){
    // 生成测试单链表（有头节点）head即头节点
    LinkList head = new Node();
    LinkList p = head;
    // int a[6] = {3,2,3,4,3,6};
    int a[6] = {1,2,3,4,5,6};
    for (int i = 0;i <= 5;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }
    
    Print(head->next);
    return 0;
}
```

### 4.删除链表唯一最小值

链表带头节点，最小值唯一。

```C++
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node *next;
    
    Node():data(-1),next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList; // 给指向链表节点的指针取别名LinkList

void DelMin(LinkList &L){ // 删除带头节点链表的唯一最小值
    LinkList t = L;int n = t->next->data; // 保证链表最少有一个节点
    for (LinkList p = t;p->next != NULL;p = p->next){
        LinkList q = p->next;
        if (q->data < n){
            n = q->data;t = p; // 找到最小节点的前驱节点
        }
    }
    
    LinkList p = t->next;
    t->next = p->next;
    free(p);
}

int main(){
    // 生成测试单链表（有头节点）head即头节点
    LinkList head = new Node();
    LinkList p = head;
    // int a[6] = {3,2,3,4,3,6};
    int a[6] = {5,2,3,0,5,1};
    for (int i = 0;i <= 5;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }
    
    DelMin(head);
    for (LinkList p = head->next;p != NULL;p = p->next){
        cout << p->data << ' ';
    }
    cout << '\n';
    return 0;
}
```

### 6.链表排序

带头节点链表升序排序。

数组快排然后复制到链表中。

```C++
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node *next;
    
    Node():data(-1),next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList; // 给指向链表节点的指针取别名LinkList

void QuickSort(int a[],int l,int r){ // 数组快排
    if (l >= r) return;
    int i = l,j = r,k = a[l]; // k是枢轴
    while (i < j){
        while (i < j && a[j] >= k) j --; // j指针先行
        while (i < j && a[i] <= k) i ++;
        if (i < j) swap(a[i],a[j]);
    }
    swap(a[i],a[l]); // 交换i指针元素与枢轴
    QuickSort(a,l,i-1),QuickSort(a,i+1,r); // 挖掉i
}

void Sort(LinkList &L,int n){ // 删除带头节点链表的唯一最小值
    int temp[n+1];
    int i = 0;
    for (LinkList p = L->next;p != NULL;p = p->next)
        temp[i++] = p->data;
    QuickSort(temp,0,n-1);
    
    i = 0;
    for (LinkList p = L->next;p != NULL;p = p->next)
        p->data = temp[i++];
}

int main(){
    // 生成测试单链表（有头节点）head即头节点
    LinkList head = new Node();
    LinkList p = head;
    // int a[6] = {3,2,3,4,3,6};
    int a[14] = {5,2,3,0,5,1,3,4,7,8,12,32,7,5};
    for (int i = 0;i <= 13;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }
    
    Sort(head,14);
    for (LinkList p = head->next;p != NULL;p = p->next){
        cout << p->data << ' ';
    }
    cout << '\n';
    return 0;
}
```

### 8.链表的公共结点

```C++
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node *next;
    
    Node():data(-1),next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList; // 给指向链表节点的指针取别名LinkList

LinkList Get(LinkList &L1,LinkList &L2){ // 查找两个带头节点单链表的第一个公共节点
    LinkList p = L1,q = L2;
    while (p != q){
        p = p ? p->next : L2; // 双指针扫描链表，p从L1扫完然后从L2开始，q相反
        q = q ? q->next : L1; // 它们走的路程相等，所以指针相等时找到的就是公共点
    }
    return p;
}

int main(){
    // 生成测试单链表（有头节点）head1,head2即头节点
    LinkList head1 = new Node(),head2 = new Node();
    LinkList p = head1;
    int a[5] = {3,2,3,1,1};
    int b[3] = {9,8,10};
    int c[3] = {11,12,10};
    for (int i = 0;i < 3;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }
    
    LinkList q = head2;
    for (int i = 0;i < 5;i ++){
        q->next = new Node(b[i]);
        q = q->next;
    }
    
    for (int i = 0;i < 3;i ++){
        p->next = q->next = new Node(c[i]);
        p = p->next,q = q->next;
    }
    
    LinkList t = Get(head1,head2);
    if (t) cout << t->data << '\n';
    return 0;
}
```

### 11.链表拆分

有一链表`(a1,b1,a2,b2,...,an,bn)`，带头节点，设计就地算法，拆分为两个链表`A = (a1,a2,...,an)`和`B = (b1,b2,...,bn)`，A表以原链表头节点为头，返回B链表头节点。

思路：对A头插，对B尾插。

```C++
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node *next;
    
    Node():data(-1),next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList; // 给指向链表节点的指针取别名LinkList

LinkList Divide(LinkList head,int n){ // 将链表一分为二
    LinkList A = head->next,B = new Node();
    LinkList p = A,q = B;
    for (int i = 0;i < n;i ++){
        LinkList t = p->next;
        p->next = t->next;
        t->next = q->next;
        // 链表A采用头插法，链表B采用尾插法
        p = p->next,q->next = t; // p指向A链表的尾节点，q指针保持B链表的头节点
    }
    return B;
}

int main(){
    // 生成测试单链表（有头节点）head即头节点
    LinkList head = new Node();
    LinkList p = head;
    int a[10] = {1,2,3,4,5,6,7,8,9,10};
    for (int i = 0;i < 10;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }
    
    LinkList B = Divide(head,5);
    Print(head),Print(B);
    return 0;
}
```

### 12.有序链表删除重复值

在递增有序的链表中删除重复值。

```C++
#include <bits/stdc++.h>
using namespace std;

struct Node{
    int data;
    Node *next;
    
    Node():data(-1),next(NULL){}
    Node(int _data):data(_data),next(NULL){}
};
typedef Node* LinkList; // 给指向链表节点的指针取别名LinkList

void Del(LinkList &L){ // 无头节点，删除链表重复值
    if (!L) return;
    LinkList p = L,q = L->next;
    while (q){
        while (q->data == p->data) q = q->next;
        p->next = q;p = q;q = q->next;
    }
}

int main(){
    // 生成测试单链表（有头节点）head即头节点
    LinkList head = new Node();
    LinkList p = head;
    int a[10] = {1,2,2,4,4,4,7,8,8,10};
    for (int i = 0;i < 10;i ++){
        p->next = new Node(a[i]);
        p = p->next;
    }
    
    Del(head);
    return 0;
}
```

### 13.递增链表归并为递减链表

```C++

```

### 15.有序链表的交集

```C++

```

### 17.对称循环双链表

```C++

```

### 18.合并循环链表

```C++

```
