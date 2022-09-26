---
title: Data Structure based on 408
subtitle: From cskaoyan.com
summary: For 408 learners!
authors: 
  - admin
tags: []
categories: []
projects: []
date: '2022-02-01T00:00:00Z'
lastMod: '2019-09-05T00:00:00Z'
image:
  caption: ''
  focal_point: ''
---

#  Ch1. 绪论

## 1.1 数据结构的基本概念

### 1.1.1 基本概念

1. 数据：数据是信息的载体，是所有能输入到计算机中并被计算机程序识别和处理的符号的集合

2. 数据元素：是数据的基本单位，可由若干数据项组成

3. 数据对象：是具有相同性质的数据元素的集合

4. 数据类型：是一个值的集合和定义在此集合上的一组操作的总称

   - 原子类型：值不可再分
   - 结构类型：值可以在被分解为若干成分的数据类型
   - 抽象数据类型ADT：抽象数据组织及与之相关的操作

5. 数据结构：相互之间存在一种或多种特定关系的数据元素的集合。这种关系称为结构

   ​	数据结构包括3方面内容：逻辑结构、存储结构和数据的运算

### 1.1.2 数据结构的三要素

1. 逻辑结构

- 集合：数据元素之间只有“同属于一个集合”的关系
- 线性结构：元素间1 to 1
- 树形结构：元素间1 to many 
- 图状或网状结构：元素间many to many

2. 存储结构（物理结构）

- 顺序存储：把逻辑上相邻的元素存储在物理位置上也相邻的存储单元中

  ​	优点：可以实现随机的存取，每个元素占用最少的存储空间

  ​	缺点：只能使用相邻的一整块存储单元，可能产生较多外部碎片

- 链式存储：不要求逻辑上相邻的元素在物理位置上相邻

  ​    优点：不会出现碎片现象，能充分利用所有的存储单元

  ​    缺点：存储指针会占用额外的存储空间，且只能实现顺序存取

  ​    [PS]：结点内存储单元地址必须连续

- 索引存储：建立附加的索引表，每一项是索引项

  ​	优点：检索速度快

  ​	缺点：索引表占用额外存储空间，增减数据同样需要修改索引表

- 散列存储（Hash存储）：根据元素的关键字直接计算出该元素的存储地址

  ​	优点：操作很快

  ​	缺点：if散列函数不好，可能出现元素存储单元的冲突，解决冲突费时间和空间

3. 数据的运算：运算的定义和实现。定义是针对逻辑结构的，实现是针对存储结构的

## 1.2 算法和算法评价

### 1.2.1 算法的基本概念

def：算法是对特定问题求解步骤对一种描述

特性：

1. 有穷性：有限时间内能执行完
   [PS]：算法是有穷的，程序可以是无穷的
2. 确定性：相同输入只会产生相同输出
3. 可行性：可以用已有的基本操作实现算法
4. 输入
5. 输出

### 1.2.2 算法效率的度量

**时间复杂度**
$$
T(n)=O(f(n))
$$
计算步骤：

1. 找到一个基本操作语句（最深层循环）
2. 分析该基本操作执行次数x与问题规模n的关系x=f(n)

规则：

- 加法规则
  $$
  O(f(n))+O(g(n))=O(max(f(n),g(n)))
  $$
  
- 乘法规则

$$
O(f(n))*O(g(n))=O(f(n)*g(n))
$$

- 时间复杂度比较：常<对<幂<指<阶<O(n^n)

**空间复杂度**
$$
S(n)=O(g(n))
$$
计算步骤：

- 普通程序
  1. 找到所占空间大小与问题规模相关的变量
  2. 分析所占空间x与问题规模n的关系x=f(n)
  3. x的数量级O(x)就是算法空间复杂度S(n)

- 递归程序
  1. 找到递归调用的深度x与问题规模n的关系x=f(n)
  2. x的数量级O(x)就是算法空间复杂度S(n)

规则同时间复杂度

算法原地工作是指算法所需的辅助空间为常量O(1)

# Ch2. 线性表

## 2.1 线性表的定义和基本操作

### 2.1.1 线性表的定义

线性表是具有相同数据类型的n个数据元素的有限序列。

Denote L as a 线性表：
$$
L=(a_1,a_2,...,a_i,...,a_n)
$$
特点：

1. 随机访问，O(1)
2. 存储密度高，每个节点只存储数据元素
3. 插入删除和扩展容量不方便

### 2.1.2 线性表的基本操作

- InitList(&L)
- Length(L)
- LocateElem(L, e)：按值查找
- GetElem(L, i)：按位查找
- ListInsert(&L, i ,e)：在位置i上插入e
- ListDelete(&L, i, &e)：删除位置i上的元素，用e返回删除元素的值
- PrintList(L)
- Empty(L)
- DestroyList(&L)

## 2.2 线性表的顺序表示

### 2.2.1 顺序表的定义

用顺序存储的方式实现线性表的顺序存储，逻辑上相邻的元素物理位置上也相邻

- 静态分配

```c++
#define MaxSize = 50
typedef struct{
  ElemType data[MaxSize];
  int length;
}SqList;
```

- 动态分配

```c++
#define InitSize 100
typedef struct{
  ElemType *data;
  int length, MaxSize;
}SqList;

#include<stdlib.h>
L.data = (ElemType*)malloc(sizeof(ElemType)*InitSize); //C
//malloc函数会申请一整块连续的存储空间并给你返回起始位置的指针(一定需要强制转换指针类型)，free 释放空间
L.data = new ElemType[InitSize]; //C++； delete来释放空间
```

### 2.2.2 顺序表基本操作实现

- ListInsert(&L, i, e) - 插入

```c++
bool ListInsert(SqList &l, int i ,ElemType e){
  if(i<1 || i>L.length+1)
    return false;
  if(L.length >= MaxSize)
    return fasle;
  for(int j=L.length; j>=i; j--)
    L.data[j] = L.data[j-1];
  L.data[i-1] = e;
  L.length++;
  return true;
}
```

平均时间复杂度O(n)
$$
\sum_{i=1}^{n+1}p_i(n-i+1)=\sum_{i=1}^{n+1}\frac{1}{n+1}(n-i+1)=\frac{n}{2}
$$

- ListDelete(&L, i, &e) - 删除

```c
bool ListInsert(SqList &l, int i ,ElemType &e){
  if(i<1 || i>L.length+1)
    return false;
  e = L.data[i-1];
  if(L.length >= MaxSize)
    return fasle;
  for(int j=i; j<L.length; j--)
    L.data[j-1] = L.data[j];
  L.length--;
  return true;
}
```

平均时间复杂度O(n)
$$
\sum_{i=1}^{n}p_i(n-i)=\sum_{i=1}^{n}\frac{1}{n}(n-i)=\frac{n-1}{2}
$$

- LocateElem(SqList L, ElemType e) - 按值查找

```c
int LocateElem(SqList L, ElemType e){
  int i;
  for(i=0; i<L.length; i++)
    if(L.data[i] == e)
      return i+1;
  return 0;
}
```

平均时间复杂度O(n)
$$
\sum_{i=1}^{n}p_i*i=\sum_{i=1}^{n}\frac{1}{n}i=\frac{n+1}{2}
$$

## 2.3 线性表的链式表示

### 2.3.1 单链表的定义

通过一组任意的存储单元来存储线性表中的数据元素。每个链表节点存放自身信息和一个后继指针

```c
typedef struct LNode{
  ElemType data;
  struct LNdode *next;
}LNode, *LinkList; 
//（LNode *）强调是一个结点；LinkList强调是一个单链表
bool InitList(LinkList &L){ // 带头节点的初始化函数
  L = (LNode *)malloc(sizeof(LNode)); //头结点不存放数据
  if(L == NULL)
    return false;
  L->next = NULL;
  return true;
}
```

### 2.3.2 单链表基本操作实现

- 单链表的建立

时间复杂度O(n)

```c++
LinkList List_TailInsert(LinkList &L){//尾插法，正向建立表
  int x; // 假设数据类型是int
  L = (LinkList)malloc(sizeof(LNode));
  LNode *s,*r = L; //r是指向表尾的指针
  cin>>x;
  while(x!=9999){
    s = (LNode *)malloc(sizeof(LNode));
    s->data = x;
    r->next = s;
    r = s;
    cin>>x;
  }
  r->next = NULL;
  return L;
}
LinkList List_HeadInsert(LinkList &L){//头插法，反向建立表
  LNode *s;
  int x;
  L = (LinkList)malloc(sizeof(LNode));
  L->next = NULL;
  cin>>x;
  while(x!=9999){
    s = (LNode *)malloc(sizeof(LNode));
    s->data = x;
    s->next = L->next;
    L->next = s;
    cin>>x;
  }
  return L;
}
```

- 按位查找

```c
LNode *GetElem(LinkList L, int i){
  if(i==0)
    return L;
  if (i<1)
    return NULL;
  int j=1;
  LNode *p = L->next;
  while(p && j<i){
    p = p->next;
    j++;
  }
  return p;
}
```

- 按值查找

```c
LNode *LocateElem(LinkList L, ElemType e){
  LNode *p = L->next;
  while(p!=NULL && p->data!=e)
    p=p->next;
  return p;
}
```

- InsertNode(LinkList &L, int i, LNode *s) - 插入

```c
p=GetElem(L, i-1);
s->next = p->next;
p->next = s;
//----------------------------
//前插:*s插入到*P之前
s->next = p->next;
p->next = s;
temp = p->data; //交换数据部分
p->data = s->data;
s->data = temp;
```

- DeleteNode(LNdoe *p) - 删除

```c
p=GetElem(L, i-1);//O(n)
q = p->next;
p->next = q->next;
free(q);
//-------------------------------
//通过删除*p的后继结点来删除结点O(1)
q = p->next;
p->data = p->next->data; // 和后继结点交换数据
p->next = q->next; //断开*q的链接
free(q);
```

### 2.3.3 双链表

- 结点类型描述

```c
typedef struct DNode{
  ElemType data;
  struct DNode *prior, *next;
}DNode, *DLinkList;
```

- 插入

```c
//将结点s插入到结点p之后
s->next = p->next;
p->next->prior = s;
s->prior = p;
p->next = s;
```

- 删除

```c
//删除p结点的后继结点q
p->next = q->next;
q->next->prior = p;
free(q);
```

### 2.3.4 循环链表

- 循环单链表

与单链表的区别就是最后一结点的指针不是NULL，而是指向头结点形成闭环。
在任何一个位置上进行插入删除都是等价的
当多数情况需要在表头表尾进行操作时，仅对循环单链表设置尾指针即可达到O(1)，因为r->next就是头指针L

- 循环双链表

头结点L->prior = 尾结点r;
r->next = L;
[PS]：当是空表的时候，L->prior = L; L->next = L; 形成自环

### 2.3.5 静态链表

静态链表是借助数组来描述线性表的链式储存结构，结点中有数据data和指针next（这里的指针是结点的相对地址即数组下标=游标）
静态链表同样需要申请一块连续的内存空间

- 结构类型

```c
#define MaxSize 50
typedef struct{
  ElemType data;
  int next; //下一个元素的数组下标，表尾的next=-1
}SLinkList[MaxSize];
```

[PS]：为防止分配的内存中有脏数据，可以把next全部初始化为-2

### 2.3.6 顺序表和链表的比较

- 存取方式

顺序表可以顺序存取，也可以随机存取，存储密度大
链表只能从表头顺序存取，存储密度小

- 逻辑和物理结构

顺序表逻辑和物理结构相同
链表逻辑相邻物理不一定相邻

- 基本操作

按值查找，顺序表无序时，两者均O(n)；有序时，顺序表可以采用折半查找O(log2n)
按位查找，顺序表O(1)，链表O(n)
插入删除操作，顺序表平均需要移动半个表长，链表只需修改相关结点的指针

- 空间分配

顺序表静态分配可能回过小溢出或过大浪费，动态分配扩充每次需要移动大量元素，效率低
链表只在需要添加结点的时候申请内存，高效

**应该如何选择存储结构**

1. 基于存储

难以估计线性表的长度 = 链表

2. 基于运算

经常做按位查找 = 顺序表（可以按序号访问元素）
经常进行插入删除操作 = 链表

3. 基于环境

有些高级语言没有指针，不容易实现链表

# Ch3. 栈、队列和数组

## 3.1 栈

### 3.1.1 栈的基本概念

**栈的定义**

是只允许在一段进行插入或删除操作的线性表

*栈顶*：线性表允许插入的一端
*栈底：*线性表固定的一端
*空栈：*不含任何元素的空表

操作特性：后进先出（LIFO）

栈的数学性质（卡特兰数）：n个元素依次进栈时，出栈元素不同排列的个数是：
$$
\frac{1}{n+1}C^n_{2n}
$$
**栈的基本操作**

- InitStack(&S)
- StackEmpty(S)
- Push(&S, x)：进栈，if栈S未满，则将x加入使之成为新栈顶
- Pop(&S, &x)：出栈，if栈S非空，则弹出栈顶元素，并用x返回
- GetTop(S, &x)：返回栈顶元素
- DestroyStack(&S)：销毁栈

### 3.1.2 栈的顺序存储结构

**1.顺序栈的实现**

利用一组连续的存储空间存放元素，同时设有一个指针指向当前栈顶元素的位置

```c
#define MaxSize 50
typedef struct{
  ElemType data[MaxSize];
  int top; //初始时top=-1，有的也会=0（直接指向第一个元素位置）
}SqStack;
//栈空条件：S.top=-1; 
//栈满条件：S.top=MaxSize-1; 
//栈长：S.top+1;
```

**2.顺序栈的基本运算**

[PS]：当元素出栈后，只是指针下移，原内存块中仍存有已出栈的元素

- 初始化

```c
void InitStack(SqStack &S){
  S.top = -1;
}
```

- 判断空栈

```c
bool StackEmpty(SqStack S){
  if(S.top==-1)
    return true;
  else
    return false;
}
```

- 进栈

```c
bool Push(SqStack &S, ElemType x){
  if(S.top==MaxSize-1)
    return false;
  S.data[++S.top] = x;//if初始值top=0，这里变为S.top++
  return true;
}
```

- 出栈

```c
bool Pop(SqStack &S, ElemType &x){
	if(S.top==-1)
    return false;
  x = S.data[S.top--];//if初始值top=0，这里变为--S.top
  return true;
}
```

- 读栈顶元素

```c
bool GetTop(SqStack S, ElemType &x){
  if(S.top==-1)
    return false;
  x = S.data[top];
  return true;
}
```

**3.共享栈**

利用栈底位置的相对不变性，可以让两个顺序栈共享一个一位数组空间，将两个栈底设置在数组的两端，栈顶向中间延伸

初始设置：top0=-1; top1=MaxSize;
栈满判断条件：top1-top0 = 1（即两指针相邻）

[PS]：注意两个栈进出的时候top移动方向正好相反++(--)

存取时间复杂度位O(1)

### 3.1.3 栈的链式存储结构

链栈，优点：便于多个栈共享存储空间和提高其效率，且不存在栈满溢出的情况

```c
typedef struct Linknode{
  ElemType data;
  struct Linknode *next;
} *LiStack;
```

[PS]：入栈和出栈都在链表的表头进行，带头结点与否影响具体实现

## 3.2 队列

### 3.2.1 队列的基本概念

**1.队列的定义**

队列是只允许在一端进行插入，另一端进行删除的线性表

先进先出FIFO

**2.基本操作**

- InitQueue(&Q)
- QueueEmpty(&Q)
- EnQueue(&Q, x)
- DeQueue(&Q, &x)
- GetHead(Q, &x)

### 3.2.2 队列的顺序存储结构

**1.队列的顺序存储**

```c
#define MaxSize 10
typedef struct {
  ElemType data[MaxSize];
  int front, rear;
}SqQueue;
```

初始状态：Q.front == Q.rear == 0
进队操作：队未满时，Q.data[Q.rear] = x; Q.rear++;
出队操作：队不空时，x = Q.data[Q.front]; Q.front++;

**2.循环队列**

初始状态：Q.front = Q.rear = 0
队首指针进1:Q.front = (Q.front+1)%MaxSize
队尾指针进1:Q.rear = (Q.rear+1)%MaxSize
队列长度：(Q.rear+MaxSize-Q.front)%MaxSize

不难发现目前判断队列满和空的条件都是Q.front == Q.rear
区分队空和满的三种处理方法：

1. 牺牲一个单元来区分
   队满：Q.front == (Q.rear+1)%MaxSize
   队空：Q.front == Q.rear
   队列中元素的个数：(Q.rear+MaxSize-Q.front)%MaxSize
2. 增加一个数据成员int size =0
   队空：size == 0
   队满：size == MaxSize
3. 增加一个数据成员tag，tag=0表示上一操作是出队，tag=1表示上一操作是进队
   队空：Q.front==Q.rear && tag==0
   队满：Q.front==Q.rear && tag==1

**3.循环队列队操作实现**

- 初始化

```c
void InitQueue(SqQueue &Q){
  Q.rear = Q.front =0;
}
```

- 判断队空

```c
bool isEmpty(SqQueue Q){
  if(Q.rear == Q.front)
    //size=0
    //Q.rear==Q.front && tag=0
    return true;
  else
    return false;
}
```

- 入队

```c
bool EnQueue(SqQueue &Q, ElemType x){
  if((Q.rear+1)%MaxSize == Q.front)
    //size<MaxSize
    //Q.rear==Q.front && tag=1
    return false;
  Q.data[Q.rear] = x;
  Q.rear = (Q.rear+1)%MaxSize;
  return true;
}
```

- 出队

```c
bool DeQueue(SqQueue &Q, ElemType &x){
  if(isEmpty(Q))
    return false;
  x = Q.data[Q.front];
  Q.front = (Q.front+1)%MaxSzie;
  return true;
}
```

### 3.2.3 队列的链式存储结构

**1.队列的链式存储**

链队列：一个同时带有队头指针（有头结点则指向头结点）和队尾指针（指向最后一个结点）的单链表

```c
typedef struct LinkNode{
  ElemType data;
  struct LinkNode *next;
}LinkNode;
typedef struct{
  LinkNode *front, *rear;
}LinkQueue;
```

当Q.front==Q.rear==NULL，链式队列为空

**2.基本操作实现**

- 初始化

```c
void InitQueue(LinkQueue &Q){
  Q.front = Q.rear = (*LinkNode)malloc(sizeof(LinkNode));//建立头结点
  Q.front->next = NULL;
}
```

- 判断队空

```c
bool IsEmpty(LinkQueue Q){
  if(Q.front == Q.rear)
    return true;
  else
    return false;
}
```

- 入队

```c
void EnQueue(LinkQueue &Q, ElemType x){
  LinkNode *s = (*LinkNode)malloc(sizeof(LinkNode));//创建新结点
  s->data=x; s->next=NULL;
  Q.rear->next = s;
  Q.rear=s;
}
```

- 出队

```c
bool DeQueue(LinkQueue &Q, ElemType &x){
  if(Q.front==Q.rear) return false;//空队
  LinkNode *p = Q.front->next;//取队首结点
  x = p->data;
  Q.front->next = p->next;
  if(Q.rear==p)//队列中只有一个结点，删除后要置为空队
    Q.rear=Q.front;
  free(p);
  return true;
}
```

### 3.2.4 双端队列

只允许从两端插入删除的线性表
当只允许一端进行插入删除的时候就退化成了‘栈’

考点：对输出序列合法性的判断（输出或者输入受限的双端队列）

## 3.3 栈和队列的应用

⚠️栈的应用补充：进制转换，迷宫求解

### 3.3.1 栈的应用 - 括号匹配

算法思想：

1. 初始设置一个空栈，顺序读入括号
2. 若是右括号，则弹出栈顶顶左括号，匹配则继续扫描，不匹配则终止程序
3. 若是左括号，压入栈中。

算法结束时栈为空，否则序列不匹配，报错！

### 3.3.2 栈的应用 - 表达式求值

[PS]：考点多为后缀表达式

*用栈实现中缀表达式的计算（机算）*：

用栈实现中缀表达式的计算:
初始化两个栈，操作数栈和运算符栈

- 若扫描到操作数，压入操作数栈
- 若扫描到运算符或界限符，则按照“中缀转后缀”相同的逻辑压入运算符栈(期间也会弹出运算符，每当弹出一个运算符时，就需要再弹出两个操作数栈的栈顶元素并执行相应运算，
  后缀表达式求值

**1.中->后缀**

左优先原则：只要左边的运算符能先运算，就优先算左边的

后缀表达式的手算方法：从左往右扫描，每遇到一个运算符，就让运算符和前面最近的两个操作数执行对应运算，合体为一个操作数 

*用栈实现后缀表达式的计算（机算）*：

初始化一个栈，用于保存暂时还不能确定运算顺序的运算符。
从左到右处理各个元素，直到末尾。可能遇到三种情况:

- 遇到操作数。直接加入后缀表达式。

- 遇到界限符。遇到“(”直接入栈;遇到“)”则依次弹出栈内运算符并加入后缀表达式，直到 弹出“(”为止。注意:“(”不加入后缀表达式。

- 遇到运算符。依次弹出栈中优先级高于或等于当前运算符的所有运算符，并加入后缀表达式， 若碰到“(” 或栈空则停止。之后再把当前运算符入栈。

**2.中->前缀**

右优先原则：只要右边的运算符能先计算，就优先算右边的

*中缀转前缀的手算方法*：

1. 确定中缀表达式中各个运算符的运算顺序
2. 选择下一个运算符，按照【运算符，左操作数，右操作数】的方式组合合成一个新的操作数
3. 如果还有运算符没被处理，就继续2.

*用栈实现前缀表达式的计算*：

1. 从右往左扫描下一个元素，直到处理完所有元素
2. 若扫描到操作数则压入栈，并回到1.；否则执行3.
3. 若扫描到运算符，则弹出两个栈顶元素，执行相应运算，运算结果压入栈顶，回到1.

### 3.3.3 栈的应用 - 递归

递归的精髓在于能否把原始问题转换为属性相同但规模较小的问题，且需满足两个条件：

- 递归表达式（递归体）
- 边界条件（递归出口）

函数调用的特点：最后被调用的函数最先执行结束（LIFO）

函数调用栈：其中存有调用返回地址，实参和局部变量

每进入一层递归，就将递归调用所需信息压入栈顶
每退出一层递归，就从栈顶弹出相应信息
缺点：太多层递归可能会导致栈溢出；在计算机实际执行中含有很多重复计算，效率低

⚠️消除递归不一定需要使用栈：比如可以不需要递归实现的算法

### 3.3.4 队列的应用 - 树的层次遍历

⚠️队列的应用补充：缓冲区，页面替换算法，广度优先搜索

1. 根结点入队
2. If队空（则所有结点处理完毕），则结束遍历，否则重复3.
3. 队列中第一个结点出队，并访问。然后左子右子相继入队

## 3.4 数组和特殊矩阵

### 3.4.1 数组

数组是由n>=1个相同类型的数据元素构成的有限序列，在内存中占用一块连续的内存空间
维界：下标的取值范围
一维数组可以看成一个线性表

**一维数组A[n]存储结构：**
$$
LOC(a_i)=LOC(a_0)+i*L\space(0<=i<n)
$$
其中L是每个数组元素所占的存储单元的内存大小

**多维数组A[h1,h2]的存储结构：**

- 行优先存储

$$
LOC(a_{i,j})=LOC(a_{0,0})+(i*(h_2+1)+j)*L
$$

- 列优先存储

$$
LOC(a_{i,j})=LOC(a_{0,0})+(j*(h_1+i))*L
$$

### 3.4.2 特殊矩阵

**1.对称矩阵A\[n][n]**

Def：Ai,j=Aj,i

压缩策略：建立一个一维数组B\[n(n+1)/2]，把对称矩阵A的下(上)三角部分和主对角线部分存入B，A\[i][j]=B[k]

- 行优先

$$
k=
\begin{numcases}{}
i(i-1)/2+j-1,i>=j(下三角)\\
j(j-1)/2+i-1,i<j(上三角)
\end{numcases}
$$

- 列优先

$$
k=
\begin{numcases}{}
(2n-j+2)(j-1)/2+i-j+1-1,i>=j(下三角)\\
(2n-i+2)(i-1)/2+j-i+1-1,i<j(上三角)
\end{numcases}
$$

**2.三角矩阵A\[n][n]**

Def：只有下三角部分和主对角线有元素，上三角区所有元素均为同一Const.

压缩策略：按行优先或列优先的原则把下三角区的元素存入一维数组B[n(n+1)/2+1]，并在最后一个位置存放上三角区的Const.

- 下三角矩阵

$$
k=
\begin{numcases}{}
i(i-1)/2+j-1,i>=j(下三角和主对角线)\\
n(n+1)/2,i<j(上三角)
\end{numcases}
$$

- 上三角矩阵

$$
k=
\begin{numcases}{}
(i-1)(2n-i+2)/2+(j-i),i<=j(上三角和主对角线)\\
n(n+1)/2,i<j(下三角)
\end{numcases}
$$

**3.三对角矩阵（带状矩阵）**

Def：When|i - j|>1，A\[i][j]= 0

压缩策略：把带状矩阵A压缩到一位矩阵B[3n-2]中

- 已知i,j求k

$$
行优先：k=2i+j-3\\
列优先：k=2j+i-3
$$

- 已知k求i,j

下标为k，则是第k+1个元素。前i-1行有3(i-1)-1个元素，前i行有3i-1个元素
=> 3(i-1)-1<k+1<=3i-1 => i>=(k+2)/3 => i=[(k+2)/3]向上取整
再利用k=2i+j-3 =>解出j

**4.稀疏矩阵**

Def：矩阵中非0元素的个数<<矩阵元素个数

压缩存储策略

1. 顺序存储 - 三元组<行，列，值>：⚠️失去了随机存取的特性

2. 链式存储 - 十字链表法：

<img src="/Users/liuxiaolin/Library/Application Support/typora-user-images/image-20220316160515853.png" alt="image-20220316160515853" style="zoom: 67%;" />

# Ch4. 串

## 4.2 串的模式匹配

### 4.2.1 朴素模式匹配算法

主串：被搜索到串，=m
子串：主串中的部分串
模式串：想要在主串中搜索到的串，=n

算法思想：将主串中所有长度为m的子串依次与模式串进行对比，直到找到一个完全匹配的子串，或者所有子串都不匹配为止（最多对比n-m+1个子串）

```c
int Index(SString, S, SString T){
  int i=1,j=1; //i是搜索主串的指针，j是搜索模式串的指针
  while(i<=S.length && j<=T.length){ 
    if(S.ch[i]==T.ch[j]){++i;++j;} //当前元素对比成功，向后移一个位置
    else{
      i=i-j+2; //子串头向后移一位，即下一子串的起始位置
      j=1; //模式串指针回到串头
    }
  }
  if(j>T.length) //匹配成功
    return i-T.length; //返回匹配串头的位置
  else
    return 0;
}
```

Assume 主串S.length = n; 模式串T.length = m; (一般情况下n>>m)
=>
最坏时间复杂度O(m(n-m+1)) = O(nm)：每一个子串都在最后一个字符匹配失败
最好时间复杂度O(n-m+1) = O(n)：每一个子串第一个字符就对比失败

### 4.2.2 KMP算法

⚠️多考选择题，会手算next[]数组

⚠️与朴素模式算法的区别：当字符匹配失败时，主串指针i不回溯

最坏时间复杂度：O(m+n) = O(n)

Steps:

1. 根据模式串，求出next[]数组
2. 利用next[]数组进行匹配（主串指针i不回溯）

```c
int Index_KMP(SString, S, SString T, int next[]){
  int i=1,j=1; //i是搜索主串的指针，j是搜索模式串的指针
  while(i<=S.length && j<=T.length){ 
    if(j==0 || S.ch[i]==T.ch[j]){
      ++i;++j; //继续比较后面的字符
    }
    else
      j=next[j]; //模式串向右移动
  }
  if(j>T.length) //匹配成功
    return i-T.length; //返回匹配串头的位置
  else
    return 0;
}
```

手算next数组方法：

- next[0]=0; next[1]=1
- 其他next：在不匹配的位置前画一个分界线，模式串一步步往后退，直到分界线之前“能对上”，或模式串完全跨过分界线为止。此时j指向哪，next值就为多少

### 4.2.3 KMP算法的优化

用nextval数组来代替next数组

```c
void get_nextval(String T, int nextval[]){
  nextval[1]=0;
  for(int j=2; j<T.length; j++){
    if(T.ch[next[j]] == T.ch[j])
      nextval[j] = nextval[next[j]];
    else
      nextval[j] = next[j];
  }
}
```

⚠️掌握手算即可

# Ch5. 树与二叉树

## 5.1 树的基本概念

树是一种递归的数据结构，既是逻辑结构，也是分层结构

1. 树的根结点没有前驱，除了根结点外，任何一个结点都有且只有一个前驱
2. 树中的所有结点可以有0个或多个后继
3. n个结点的树中有n-1条边

**树的描述：**

- 结点的度 = 该结点的孩子数； 树的度 = 树中结点的最大度
- 分支结点：度>0；叶子结点：度=0
- 结点的深度，高度和层次
  - 层次：根结点为第一层，自顶向下递增，同层的结点互为堂兄弟
  - 深度：从根结点自顶向下累加
  - 高度：从叶结点自底向上累加
  - 树的高度（or 深度）：树中结点的最大层数
- 有序树：树中结点的各子树从左到右右次序，不能互换
  无序树：反之
- 路径：两结点从上至下所经过的结点序列
  路径长度：路径上边的个数
  树的路径长度 = 根到每个结点的路径长度之和
- 森林：是m(m>=0)棵互不相交的树的集合

**树的性质：**

1. 树的结点数 = 所有结点的度数和+1
2. 度=m的树第i层至多右m^(i-1)个结点
3. 高度=h的m叉树至多有(m^h-1)/(m-1)个结点（等比数列求和），至少有h+m-1个结点
4. 有n个结点的m叉树的最小高度为「logm(n(m-1)+1)」[向上取整]

## 5.2 二叉树

### 5.2.1 二叉树的定义和特性

 **1.二叉树的定义**

二叉树的结点至多有两棵子树，且树有左右之分，结点次序不能颠倒 — 有序树

**2.几种特殊二叉树**

- 满二叉树：一棵高度=h，且含有2^h-1个结点的二叉树
  特点：
  1. 只有最后一层有叶结点，除叶结点外所有结点的度=2
  2. 按层序自上而下，自左向右从1开始编号，结点i的左孩子=2i，右孩子=2i+1；结点i的父结点=i/2
- 完全二叉树：高度=h，结点数=n的二叉树当且仅当其每个结点都与高度为h的满二叉树中编号1～n的结点一一对应
  [注]：满二叉树是一种特殊的完全二叉树
  特点：
  1. 只有最后两层出现叶结点
  2. 最多只有一个度=1的结点
  3. 按层序自上而下，自左向右从1开始编号，结点i的左孩子=2i，右孩子=2i+1；结点i的父结点=i/2
  4. i <= [向下取整]\(n/2) 为分支结点，i > [向下取整]\(n/2)为叶结点
  5. When n为奇数，每个分支结点度均=2；反之，编号最大度分支结点只有左孩子，其余分支结点度均=2

- 二叉排序树：左子树上所有结点的关键字小于根结点的关键字；右子树上所有结点的关键字大于根结点关键字；其左子树和右子树同样为二叉排序树
- 平衡二叉树：任何一个结点的深度差<=1
  [注]：可以提高二叉排序树搜索效率

**3.二叉树的性质**

- 非空二叉树上的叶结点数 = 度为2的结点数 + 1: n0=n2+1
- 非空二叉树上第k层至多有2^(k-1)个结点
- 高度为h的二叉树至多有2^h-1个结点
- 完全二叉树常考：
  1. 根据具有n个结点推出完全二叉树的高度
  2. 根据n推出n0,n1,n2
     n0=0 or 1； n0 = n2 + 1 可以看出n0+n2=奇数

### 5.2.2 二叉树的存储结构

**1.顺序存储结构**

```c
#define MaxSize 100
typedef struct TreeNode{
  ElemType value;
  bool isEmpty; //defalut = true
};
```

二叉树的顺序存储结构只适合存储完全二叉树

⚠️：数组应该从1开始存储

**2.链式存储结构**

```c
typedef struct BiTNode{
  ElemType data;
  struct BiTNode *lchild, *rchild; //左右孩子指针
}BiTNode, *BiTree;
```

在含有n个结点的二叉链表中，含有n+1个空链域（因为每个结点都有两个指针，n个结点有n-1条边，也就是有n-1个指针被使用，2n-(n-1)=n+1）

## 5.3 二叉树的遍历和线索二叉树

### 5.3.1 二叉树的遍历

递归遍历的时间复杂度O(h)

**1.先序遍历** ——根左右

```c
void PreOrder(BiTree T){
  if(T!=NULL){
    visit(T); //访问根结点
    PreOrder(T->lchild); //递归遍历左子树
    PreOrder(T->rchild); //递归遍历右子树
  }
}
```

每个结点路过三次，第一次路过时才访问该结点

**2.中序遍历**——左根右

```c
void InOrder(BiTree T){
  if(T!=NULL){
    InOrder(T->lchild); //递归遍历左子树
    visit(T); //访问根结点
    InOrder(T->rchild); //递归遍历右子树
  }
}
```

每个结点路过三次，第二次路过时才访问该结点

⚠️前序序列和中序序列的关系相当于以前序序列进栈，以中序序列出栈

**3.后序遍历**——左右根

```c
void PostOrder(BiTree T){
  if(T!=NULL){
    PostOrder(T->lchild); //递归遍历左子树
    PostOrder(T->rchild); //递归遍历右子树
    visit(T); //访问根结点
  }
}
```

每个结点路过三次，第三次路过时才访问该结点

⚠️前序序列和后序序列可以确定两结点的关系：
		前序为XY，后序为YX，则X为Y 的祖先
		前后序都是XY，则XY为兄弟

**4.非递归算法**

- 非递归中序遍历算法思想：

1. 沿着根的左孩子，依次入栈，直到左孩子为空
2. 栈顶元素出栈并访问
3. 若右孩子为空，继续进行2.；反之执行1.

```c
void InOrder2(BiTree T){
  InitStack(S); BiTree p = T; // p是遍历指针
  while(p || !IsEmpty(S)){
    if(p){
      Push(S, p);
      p = p->lchild;
    }
    else{
      Pop(S); visit(p);
      p = p->rchild;
    }
  }
}
```

- 非递归先序遍历算法思想：

只需把访问操作改到入栈操作的前面即可

```c
void PreOrder2(BiTree T){
  InitStack(S); BiTree p = T; // p是遍历指针
  while(p || !IsEmpty(S)){
    if(p){
      visit(p); Push(S, p);
      p = p->lchild;
    }
    else{
      Pop(S, p); 
      p = p->rchild;
    }
  }
}
```

**5.层序遍历**

算法思想：

1. 初始化一个辅助队列
2. 根结点入队
3. 若队列非空，则队头结点出队，访问该结点，并将其左右孩子插入队尾（若存在）
4. 重复3.直到队列空

```c
typedef struct BiTNode{
  int data;
  struct BiTNode *lchild, rchild;
}BiTNode, *BiTree;

typedef struct LinkNode{
  BiTNode *data;
  struct LinkNode *next;
}LinkNode;

typedef struct{
  LinkNode *front, *rear;
}LinkQueue;

void LevelOrder(BiTree T){
  LinkQueue Q;
  InitQueue(Q);
  BiTree p;
  EnQueue(Q, T);
  while(!IsEmpty(Q)){
    DeQueue(Q, p); //队首元素出队并赋给p
    visit(p);
    if(p->lchild != NULL)
      EnQueue(Q, p->lchild);
    if(p->rchild != NULL)
      EnQueue(Q, p->rchild);
  }
}
```

 **6.由遍历序列构造二叉树**

如果只给出一种遍历序列，不能唯一确定一颗二叉树 

只要给出中序遍历+另一种遍历，就可	以得出二叉树

根据递归还原二叉树

<img src="/Users/liuxiaolin/Library/Application Support/typora-user-images/image-20220320163925802.png" alt="image-20220320163925802" style="zoom: 33%;" />

### 5.3.2 线索二叉树

二叉树是一种逻辑结构，线索二叉树是加上连接后的链式结构，即为存储结构

**1.线索二叉树的概念**

利用二叉树中的n+1个空链域来存放其前驱和后继指针

规定：若无左孩子，* lchild指向前驱结点；若无右孩子，* rchild指向后继结点
标志域：tag=0，tag=0表示连接的是孩子，tag=1表示连接的是前驱或后继

```c
typedef struct ThreadNode{
  ElemType data;
  struct ThreadNode *lchild, *rchild; // 左右孩子指针
  int ltag, rtag; // 左右线索标志，default ltag=rtag=0
}ThreadNode, *ThreadTree;
```

**2.二叉树线索化**

- 中序

```c
void InThread(ThreadTree p, ThreadTree &pre){
  if(p!=NULL){
    InThread(p->lchild,&pre); //递归线索化左子树
    // -----------------------------------------------------------
    // 中间部分相当于visit(p);
    if(p->lchild == NULL){ //建立当前结点的前驱结点
      p->lchild = pre;
      p->ltag =1
    }
    if(pre!=NULL && pre->rchild=NULL){ //建立前驱结点的后继结点
      pre->rchild = p;
      p->rtag = 1;
    }
    pre=p;
    // -----------------------------------------------------------
    InThread(p->rchild,&pre); //递归线索化右子树
  }
}
void CreateInThread(ThreadTree T){
  ThreadTree pre = NULL;
  if(T!=NULL){
    InThread(T,pre);
    pre->rchild = NULL; //处理遍历的最后一个结点，此时pre=p
    pre->rtag = 1;
  }
}
```

- 先序

```c
void InThread(ThreadTree p, ThreadTree &pre){
  if(p!=NULL){
    // -----------------------------------------------------------
    // 中间部分相当于visit(p);
    if(p->lchild == NULL){ //建立当前结点的前驱结点
      p->lchild = pre;
      p->ltag =1
    }
    if(pre!=NULL && pre->rchild=NULL){ //建立前驱结点的后继结点
      pre->rchild = p;
      p->rtag = 1;
    }
    pre=p;
    // -----------------------------------------------------------
    if(p->ltag == 0) //⚠️防止形成闭环无限循环，判断左指针是否是孩子指针
      InThread(p->lchild,&pre); //递归线索化左子树
    InThread(p->rchild,&pre); //递归线索化右子树
  }
}
void CreateInThread(ThreadTree T){
  ThreadTree pre = NULL;
  if(T!=NULL){
    InThread(T,pre);
    if(pre->rchild = NULL) //处理遍历的最后一个结点，此时pre=p
    	pre->rtag = 1;
  }
}
```

- 后序：与中序类似

**3.中序线索二叉树的遍历**

- 求中序序列下的第一个结点

```c
ThreadNode *FirstNode(ThreadNode *p){
  while(p->ltag == 0) //最左下结点，⚠️不一定是叶结点
    p = p->lchild;
  return p;
}
```

- 求结点p的后继

```c
ThreadNode *NextNode(ThreadNode *p){
  if(p->rtag == 0)
    return FirstNode(p->rchild); //找到右子树中最左下结点
  else 
    return p->rchild; //tag==1之间返回后继
}
```

- 不含头结点的中序遍历（非递归）空间复杂度=O(1)

```c
void Inorder(ThreadNode *T){
  for(ThreadNode *p = FirstNode(T);p!=NULL;p=NextNode(p))
    visit(p);
}
```

- 中序遍历下的最后一个结点

```c
ThreadNode *LastNode(ThreadNode *p){
  while(p->rtag == 0)
    p = p->rchild;
  return p;
}
```

- 求p结点的前驱

```c
ThreadNode *PreNode(ThreadNode *p){
  if(p-<ltag == 0)
    return LastNode(p->lchild); //左子树最右下结点
  else
    return p->lchild; //tag==1直接返回前驱
}
```

- 不含头结点的中序逆向遍历

```c
void ReInorder(ThreadNode *T){
  for(ThreadNode *p = LastNode(T);p!=NULL;p=PreNode(p))
    visit(p);
}
```

## 5.4 树、森林

### 5.4.1 树的存储结构

**1.双亲表示法**（顺序）

Def：采用一组连续空间来存储每个结点，每个结点保存指向双亲的伪指针。根结点下标为0，伪指针域为-1

```c
#define MAX_TREE_SIZE 100
typedef struct{
  ElemType data;
  int parent; //指针域
}PTNode;
typedef struct{
  PTNode nodes[MAX_TREE_SIZE];
  int n; //结点数
}PTree;
```

**2.孩子表示法**（顺序+链式）

 Def：顺序存储各个结点，将每个结点的孩子结点都用单链表连接在一起形成一个线性结构，每个结点中保存孩子链表头指针

**3.孩子兄弟表示法**（链式 ）

Def：又叫做二叉树表示法，以二叉链表作为树的存储结构

```c
typedef struct CSNode{
  ElemType data;
  struct CSNode *firstnode, *nextsibling; //第一个孩子和右兄弟指针
}CSNode, *CSTree;
```

- 优点：可以方便的实现树到二叉树到转化，易于查找结点的孩子
- 缺点：从当前结点查找双亲复杂，可以考虑设置一个parent域

### 5.4.2 树、森林与二叉树的转化

- 树 => 二叉树
  1. 兄弟结点之间加一条线
  2. 对每个结点，只保留它与第一个孩子的连线
  3. 以树根为轴心，顺时针旋转45度

- 森林 => 二叉树
  1. 将森林中每棵树转换成二叉树
  2. 每棵树的根视为兄弟关系，根之间连线
  3. 以第一棵树根为轴心，顺时针旋转45度

### 5.4.3 树和森林的遍历

**1.树的遍历**

- 先根遍历：若树非空，先访问根结点，再依次遍历根结点的每棵子树。其遍历序列 = 对应二叉树的先序序列
- 后根遍历：若树非空，先依次遍历根结点的每棵子树，再访问根结点。其遍历序列 = 对于二叉树的中序序列

**2.森林的遍历**

- 先序遍历 = 先根遍历每棵树 = 森林对应二叉树的先序序列
- 中序遍历 = 后根遍历每个树 = 森林对应二叉树的中序序列

## 5.5 树与二叉树的应用

### 5.5.1 哈夫曼树和哈夫曼编码

**1.哈夫曼树的定义**

权：树中的结点被赋予的值

带权路径长度WPL：从根到任意结点的路径长度（经过的边数）✖️该结点的权值

树的WPL：
$$
WPL=\sum_{i=1}^{n}w_il_i
$$
哈夫曼树（最优二叉树）：含有n个带权叶结点的树，WPL最小的二叉树

**2.哈夫曼树的构造**

给定n个权值为w1,w2,w3...的结点
Steps：

1. 将n个结点分别作为n棵仅含一个结点的二叉树，构成森林F
2. 构造一个新结点，从F中选取两棵根结点权值最小的树作为新结点的左右子树，并且将新结点的权值置为左右子树根结点的权值之和
3. 从F中删除刚才选出的两棵树，同时将新得到的树加入F
4. 重复2. & 3.，直到F只剩下一棵树

特点：

- 权值越小路径越长
- 新建了n-1个结点（双分支结点），结点总数为2n-1
- 不存在度为1的结点

**3.哈夫曼编码**

固定长度编码：对每个字符用相等长度的二进制位表示
可变长度编码：对每个字符用不等长的二进制位表示

前缀编码：没有一个编码是另一个编码的前缀

构造哈夫曼编码Steps：

1. 将每个出现的字符当作一个独立的结点，其权值为它出现的频度（or 次数）
2. 构造出对应的哈夫曼树
3. 将字符的编码解释为从根至该字符路径上边的标记的序列（0左1右）

哈夫曼二进制编码长度 = 构造出的哈夫曼树的WPL（单位：位）

### 5.5.2 并查集

并查集是一种双亲表示法存储的树

```c
//结构定义--------------------------------------------
#define SIZE 100
int UFSets[SIZE]; //集合元素数组（双亲指针数组）
//初始化操作------------------------------------------
void Initial(int S[]){
  for(int i=0;i<SIZE;i++)
    S[i]=-1; //将每个结点设置为一个单结点的树，双亲指针为-1
}
//Find操作-------------------------------------------
int Find(int S[], int x){
  while(S[x]>=0) //循环找x的根结点
    x=S[x];
  return x;
}
//Find优化——压缩路径
算法描述：先找到根节点，在把查找路径上的所有结点都挂到根节点下；
int Find(int S[], int x){
  int root=x;
  while(S[root]>=0) //循环找到根节点
    root=S[root];
  while(x!=root){ //压缩路径
    int t=S[root]; //t指向x的父节点
    S[x]=root; //x直接挂到根节点下
    x=t;
  }
  return root;
}
//Union操作-------------------------------------------
void Union(int S[], int Root1, int Root2){
  //要求R1与R2互斥
  S[Root2]=Root1; //将Root2连接到Root1的根下
  //Step1：先找到根节点
  //Step2：再将小树合并到大树下面
}
//Union优化——小树合并到大树
void Union(int S[], int Root1, int Root2){
  if(Root1==Root2)
    return;
  if(S[Root2]>S[Root1]){ //Root2的结点数更少
    S[Root1]+=S[Root2]; //累加结点总数
    S[Root2]=Root1; //小树合并到大树
  }else{
    S[Root2]+=S[Root1];
    S[Root1]=Root2;
  }
}
```

⚠️根结点的双亲结点为负数，该子集有x个元素就 = -x

# Ch6. 图

## 6.1 图的基本概念

图G由顶点集V + 边集E构成，G=（V，E）。|V|表示图G种顶点的个数 = 图G的阶；|E|表示图G边的条数
⚠️线性表可以是空表，树可以是空树，图不可以是空图，即V一定是非空集

### 6.1.1 图的定义

**1.有向图**：<v, w>，v -> w

入度：以顶点v为终点的有向边的数目 = ID(v)
出度：以顶点v为起点的有向边的数目 = OD(v)
顶点的度TD(v) = ID(v) + OD(v)
$$
\sum_{i=1}^n{ID(v_i)} = \sum_{i=1}^n{OD(v_i)} = e（n个顶点，e条边）
$$
**2.无向图**：(v, w)， v - w

顶点的度：依附于该顶点的边的条数 = TD(v)
$$
\sum_{i=1}^n{TD(v_i)} = 2e（n个顶点，e条边）
$$
**3.简单图、多重图**

简单图：不存在重复边，不存在顶点到自身的边
多重图；反之

**4.完全图（简单完全图）**

完全图：有n(n-1)/2条边
有向完全图：有n(n-1)条边

**5.顶点间关系描述**

路径：顶点Vp到Vq之间的一条路径 = 顶点序列：Vp, V1, V2, ..., Vq；由顶点和相邻顶点序偶构成的边所形成的序列
回路（环）：起点和终点相同的路径
简单路径：路径序列不出现重复顶点
简单回路：除了起点和终点无其他重复顶点
路径长度：路径上边的数目
点到点的距离：若存在最短路径，即为距离；若不存在路径，距离为无穷（∞）
连通：无向图中若v到w有路径存在，则v和w是连通；任意两点都连通的图叫做连通图
		若是连通图，最少有n-1条边
		若是非连通图，最多有C_{n-1}^2条边（n-1个点构成完全图）
强连通：有向图中，v -> w和w -> v都存在，则称v和w强连通；任意两点都强连通的图叫做强连通图
		若是强连通图最少要n条边（形成回路）

**6.子图**

V'是V的子集，E‘是E的子集，则G’是G的子图
若满足V(G') = V(G)，则G‘是G的生成子图

**7.连通分量**

无向图中的极大连通子图（子图必须连通且饱含尽可能多的顶点和边）

**8.强连通分量**

有向图中的极大强连通子图（子图必须强连通同时保留尽可能多的边）

**9.生成树**

无向连通图的生成树是包含图中全部顶点的一个极小连通子图（边尽可能的少，但是要保持连通性）
若砍去一条边则变成非连通图，若加上一条边则会形成一个回路

**10.生成森林**

无向非连通图中，连通分量的生成树构成了非连通图的生成森林

**11.边的权、带权图/网**

边的权：每条边上表上的数值
带权图/网：边上带有权值的图
带权路径的长度：当图是带权图时，一条路径上所有边的权值之和，称为该路径的带权路径长度
**12.有向树**

一个顶点的入度=0，其余顶点入度均为1的有向图

## 6.2 图的存储

### 6.2.1 邻接矩阵法

用一个一维数组存储顶点信息，用一个二维数组存储图中边的信息

*无权图：*
$$
A[i][j]=
\begin{numcases}{}
1,\space if(v_i,v_j)or<v_i,v_j>\space \in E(G) \\
0,\space if(v_i,v_j)or<v_i,v_j>\space not\space \in E(G) \\
\end{numcases}
$$
*带权图：*
$$
A[i][j]=
\begin{numcases}{}
w_{ij},\space if(v_i,v_j)or<v_i,v_j>\space \in E(G) \\
0或∞,\space if(v_i,v_j)or<v_i,v_j>\space not\space \in E(G) \\
\end{numcases}
$$
性质：

1. 空间复杂度O(|V|^2)，适合存储稠密图
2. 无向图的邻接矩阵为对称矩阵（且唯一），可以采用对称矩阵压缩
3. 对于无向图，邻接矩阵第i行（列）的非零元素个数 = TD(Vi)
   对于有向图，邻接矩阵第i行非零元素个数 = 出度OD(Vi)；第i列非零元素个数 = 入度ID(Vi)
4. 设G的邻接矩阵为A（元素为0/1），则A^n的元素A^n[i]\[j]表示由顶点i到顶点j的长度为n的路径的数目

### 6.2.2 邻接表法

顺序存储+链式存储
对图G中的每个结点Vi建立一个单链表，第i个单链表中的结点表示与顶点Vi的边，这个单链表称为边表

```c
#define MaxVertexNum 100
typedef struct ArcNode{ //边表结点
  int adjvex; //该弧指向下一个顶点的位置
  struct ArcNode *next; //下一条弧的指针
}ArcNode;
typedef struct VNode{ //顶点表结点
  VertexType data; //顶点信息
  ArcNode *first; //指向第一条依附该顶点的弧的指针
}VNode, AdjList[MaxVertexNum];
typedef struct { 
  AdjList vertices; //邻接表
  int vexnum, arcnum; //图的顶点数和弧数
}ALGraph; //ALGraph是以邻接表存储的图类型
```

性质：

1. 空间复杂度：无向图O(|V| + 2|E|)；有向图O(|V| + |E|) 
2. 适合用于稀疏图，表示方式不唯一（边表中顶点的顺序不唯一）
3. 计算有向图的入度需要遍历全表

### 6.2.3 十字链表

⚠️只用来存储有向图

十字链表是有向图的一种链式存储结构（顶点之间顺序存储），表示仍不唯一，对应于有向图中的每条弧都有一个结点，对应于每一个顶点也有一个结点

- 弧结点

  | tailvex | headvex | hlink | tlink | info |
  | :-----: | :-----: | :---: | :---: | :--: |

  tailevx和headvex分别是弧尾和弧头在图中的位置；
  hlink指向弧头相同的下一个弧，tlink指向弧尾相同的下一条弧

- 顶点结点

  | data | firstin | firstout |
  | :--: | :-----: | :------: |

  data存放顶点的相关信息比如name；
  firstin和firstout分别指向以该结点为弧头和弧尾的第一个弧结点

性质：

1. 空间复杂度：O(|V| + |E|)
2. 找到指定顶点的出边：顺着tlink找
   找到指定顶点的入边：顺着hlink找

### 6.2.4 邻接多重表

⚠️只用来存储无向图

- 边结点

  | mark | ivex | ilink | jvex | jlink | info |
  | :--: | :--: | :---: | :--: | :---: | :--: |

  mark是标志域，标记该边是否被搜索过
  ivex和jvex是该边的两个顶点在图中的位置
  ilink和jlink分别是是下一条依附于ivex和jvex的边
  info来存放边的各种信息的指针域

- 顶点结点

  | data | firstedge |
  | :--: | :-------: |

  data域存储该顶点的相关信息
  fitstedge指向第一条依附于该顶点的边

邻接多重表和邻接表的差别：同一条边邻接表用两个结点表示，邻接多重表用一个结点表示

性质：

1. 空间复杂度O(|V| + |E|)
2. 一个图对应多个邻接多重表，一个邻接多重表对应一个图
3. 删除插入操作方便

### 6.2.5 图的基本操作

- Adjacent(G,x,y)：判断图G是否存在边<x, y>或(x, y)。
- NeighborsiG,x)：列出图G中与结点x邻接的边.
- Insertvertex(G,x)：在图G中插入顶点x
- Deletevertex(G,x)：从图G中删除顶点x。
- AddEdgeiGx.y：若无向边(x, y)或有向边<x, y>必不存在，则向图G中添加该边
- RemoveEdge (G,x,y)：若无向边(x, y)以或有向边<x, y>必存在，则从图G中删除该边
- **FirstNeighbor(G,x)**：求图G中顶点x的第一个邻接点，若有则返回顶点号。若x没有邻接点
  或图中不存在x，则返回-1
- **NextNeighbor(G,x,y)**：假设图G中顶点y是顶点x的一个邻接点，返回除y之外顶点x的下
  个邻接点的顶点号，若y是x的最后一个邻接点，则返回-1
- Get_edge_ value( G,x,y)：获取图G中边<x, y>或(x, y),必对应的权值
- Set edge value(G,x,yw)：设首图G中边<x, y>或(x, y)对应的权值为v。

## 6.3 图的遍历

### 6.3.1 BFS

```c
bool visited[MAX_VERTEX_NUM]; 
void BFSTraverse(Graph G){ //对图G进行BFS
  for(int i=0;i<G.vexnum;++i) //初始化标记数组
    visited[i]=false;
  InitQueue(Q); //初始化辅助队列
  for(int i=0;i<G.vexnum;++i) //对每个连通分量进行BFS
    if(!visited[i]) //若vi没访问过，则访问vi
      BFS(G, i);
}
void BFS(Graph G, int v){ //从顶点v出发BFS
  visit(v);
	visited[v]=TRUE;
	Enqueue(Q,v);
	while(!isEmpty(Q)){
    DeQueue(Q,v);
    for(w=FirstNeighbor(G,v):w>=0;v-NextNeighbor(G,v,w)) //检测v的所有邻接点
      if(!visited[w]){
        visit(w);
        visited[w]=TRUE; //对w数己访问标记
        EnQueue(Q,w); //顶点W入队列
      }//if
  }//while
}
//对于无向图，调用BFS的次数 = 连通分量的个数
```

**1.BFS性能分析**

空间复杂度O(|V|)：因为BFS需要借助一个辅助队列，最坏的情况n个结点都需要入队

时间复杂度（主要是访问结点和边）

- 采用邻接表O(|V| + |E|)：每个顶点和边至少访问一次
- 采用邻接矩阵O(|V|^2)：访问每个顶点的时间为O(|V|)，访问每个边的时间为O(|V|^2)

⚠️采用邻接表产生的BFS序列不一定相同，因为取决于边表；而采用邻接矩阵产生的BFS序列一定唯一（各层序号由小到大）

**2.BFS求解单元最短路径问题**

若图G为非带权图，d(u,v)为u到v到任何路径中最少的边数；若uv不通，则d(u,v) = ∞

```c
void BFS_MIN_Distance(Grap G, int u){
  for(int i=0;i<G.vexnum;++i){
    d[i]=∞; //d[i] = u到i的最短路径
    path[i]=-1; //最短路径从哪个顶点过来
  }
  visited[u]=true; d[u]=0;
  EnQueue(Q,u);
  while(!isEmpty(Q)){
    DeQueue(Q);
    for(w=FirstNeighbor(G,v):w>=0;v-NextNeighbor(G,v,w)) //检测v的所有邻接点
      if(!visited[w]){
        visit(w):
        visited[w]=TRUE; //对w数己访问标记
        d[w]=d[u]+1; //路径长度+1
        path[w]=u; //最短路径从u到w
        EnQueue(Q,w);
      }//if
  }
}
```

**3.BFS生成树和生成森林**

BFS遍历过程中得到的遍历树

### 6.3.2 DFS

```c
bool visited[MAX VERTEX NUM]:
void DFSTraverse(Graph G) { //对图G进行DFS
	for (v=0; v<G.vexnum;++v)
		visited[v]=FALSE; //初始化标记数组
	for(v=0; v<G.vexnum;++v)
		if(!visited(v])
			DFS(G, V);
void DFS(Graph G, int v){ //从顶点v出发进行DFS
	visit(v);
  visited[v]=TRUE:
  for (w=FirstNeighbor(G,v);w>=0;w=NextNeighor(G,v,w))
  	if(!visited[w]) //w为v尚未访问的邻接点
 	 		DFS (G, w);
}
```

**1.DFS性能分析**

空间复杂度O(|V|)

时间复杂度（主要是访问结点和边）

- 采用邻接表O(|V| + |E|)：每个顶点和边至少访问一次
- 采用邻接矩阵O(|V|^2)：访问每个顶点的时间为O(|V|)，访问每个边的时间为O(|V|^2)

⚠️采用邻接表产生的DFS序列不一定相同，因为取决于边表；而采用邻接矩阵产生的DFS序列一定唯一（各层序号由小到大）

**2.DFS生成树和生成森林**

### 6.3.3 图的遍历与图的连通性

调用BFS和DFS函数的次数 = 连通分量数

## 6.4 图的应用

### 6.4.1 最小生成树MST

⚠️不太可能考code

**1.Prim算法**

从某一个顶点开始构建生成树，每次将代价最小的新顶点纳入生成树，直到所有顶点都纳入为止（类似于Dijkstra寻路算法）

时间复杂度 = O(|V|^2)，适用于边稠密图

**2.Kruskal算法**

每次选择一条权值最小的边，使这条边的两头连通（原本已经连通的就不选），直到所有顶点都连通

时间复杂度 = O(|E|log2|E|)，适用于边稀疏图

### 6.4.2 最短路径

⚠️6.3.1-2BFS求单源最短路径问题（无权图）

**1.Dijkstra算法求单源最短路径**（贪心策略）

构造两个辅助数组

- dist[]：记录从原点到其他各点的最短路径
- path[]：记录各个顶点的前驱结点

Steps：

1. 初始化：集合S初始为{0}，dist[]初始值dist[i] = arcs[0]\[i]
2. 从顶点结合V-S中选出vj，满足dist[j] = Min{dist[i] | v in V-S}，然后将vj结点加入S={0,j}
3. 修改v0到集合V-S上任一点的最短路径长度
   if(dist[j] + arcs[j]\[k] < dist[k])
   dist[k] = dist[j]+arcs[j]\[k];
4. 重复2.～3.操作共n-1次，直到所有顶点都包含在S中

时间复杂度O(n^2) = O(|V|^2)，无论是邻接矩阵还是邻接表

⚠️Dijkstra算法不适用于有负权值的带权图

**2.Floyd算法求各顶点间最短路径**（DP）

构造两个矩阵：

- 图的邻接矩阵
- 图的path矩阵：记录每个结点最短路径的前驱结点

Steps：

1. 不允许在其他顶点中转

2. 允许在V0中转

3. 允许在V0、V1中转

   ......

n-1. 允许在V0、V1、... 、Vn-1中转

其中每一次都检查是否有可修改的最短路径

时间复杂度O(n^3)

⚠️Floyd算法允许图中带有负权值的边，但不允许有包含负权值边组成的回路

**3.不通最短路径算法法对比**

<img src="/Users/liuxiaolin/Library/Application Support/typora-user-images/image-20220401162903088.png" alt="image-20220401162903088" style="zoom:50%;" />

### 6.4.3 有向无环图描述表达式

有向无环图DAG：有向图中不存在环

Steps：

1. 把各个操作数不重复地排成一排
2. 标出各个运算符 的生效顺序（先后顺序有点出入无所谓）
3. 按顺序加入运算符，注意“分层”
4. 自底向上逐层检查同层的运算符是否可以合体

### 6.4.4 拓扑序列

AOV网（顶点表示活动的网络）：用DAG图表示一个工程。顶点表示活动，有向边<Vi,Vj>表示活动Vi必须先于活动Vj进行

拓扑排序（找到做事的先后顺序）：
	每一个AOV网都有一个或者多个拓扑排序

- 图论Def：由一个DAG图顶点组成的序列，满足：
  1. 每个顶点只出现一次
  2. 若顶点A在序列中排在B的前面，则在图中不存在从B->A的路径
- Def：拓扑排序是对DAG顶点的一种排序，使得若存在A->B，则在排序中B在A的后面

AOV => 拓扑排序的Steps：
	①从AOV网中选择一个没有前驱（入度为0） 的顶点井输出。
	②从网中删除该顶点和所有以它为起点的有向边
	③重复(①和②直到当前的AOV网为空或当前网中不存在无前驱的顶点为止（后一种情况说明存在环）

```c
bool ropologicalsort (Graph G){
  InitStack(S); //初始化栈，用来储存入度为0的顶点
	for(int i=0;i<G.vexnum;i++)
  	if(indegree[i]==0)
  		Push(S,i); //将所有入度=0的点入栈
  int count=0; //计数输出的顶点数
  while(!IsEmpty(S)){ //栈不空则存在入度=0的点
  	Pop(S,i); //栈顶元素出栈
	  print[count++]=i; //输出顶点i
    for(p=G.vertices[i].firstarc;p;p=p->nextarc){
      //将所有i指向的顶点入度-1，并将入度减为0的顶点压入栈
      v=p->adjvex;
      if(! (--indegree[v]))
        Push(S,v); //入度=0，入栈
    }//for
  }//while
  if (count<G.vexnum)
		return false; //排序失败，存在回路
	else
		return true; //排序成功
}
```

时间复杂度（主要是访问结点和边）

- 采用邻接表O(|V| + |E|)：每个顶点和边至少访问一次
- 采用邻接矩阵O(|V|^2)：访问每个顶点的时间为O(|V|)，访问每个边的时间为O(|V|^2)

**逆拓扑排序**

可以用DFS + 回路判断来实现

AOV => 拓扑排序的Steps：
	①从AOV网中选择一个没有后继（出度为0） 的顶点井输出。
	②从网中删除该顶点和所有以它为起点的有向边
	③重复(①和②直到当前的AOV网为空

⚠️若一个顶点有多个直接后继，则拓扑排序结果通常不唯一
	 有向无环图的拓扑序列唯一也不能确定唯一的图
	 若邻接矩阵储存有向图，且矩阵为上三角阵，可以推出该图无环，必存在拓扑序列，但是不一定唯一

### 6.4.5 关键路径

AOE网（表示活动的网络）：在带权有向图中，以顶点表示事件，以有向边表示活动，以边上的权值表示完成该活动的开销（如完成活动所需的时间)

AOE网的性质：

- 只有在某顶点所代表的时间发生后，从该顶点出发的各有向边所代表的活动才能开始
- 只有在进入某顶点的各有向边所代表的活动都结束时，该顶点代表的事件才能发生

开始顶点（源点）：AOE网中仅有的一个入度=0的点
结束顶点（汇点）：AOE网中仅有的一个出度=0的点
关键路径：从源点到汇点到所有路径中具有最大路径长度的路径，其长度就是完成整个工期最短时间
关键活动：关键路径上的活动

**几个重要参数**

1. 事件vk最早发生时间ve(k)：指从源点v1到顶点vk的最长路径长度，决定了所有从根vk开始的活动能够开工的最早时间
   $$
   ve(k)=Max\{ve(j)+Weight(v_j,v_k)\},\space v_j\rightarrow v_k
   $$
   
2. 事件vk的最迟发生时间vl(k)：指在不推迟整个工程完成的前提下，该事件最迟发生的时间。
   $$
   vl(k)=Min\{vl(j)-Weight(v_k,v_j\},\space v_k\rightarrow v_j \\
   vl(k)=Min\{以该事件为尾的弧的活动都最迟开始时间,最迟结束时间与该活动持续时间的差\}
   $$
   
3. 活动ai的最早开始时间e(i)：指该活动弧起点所表示的事件的最早发生时间
   $$
   e(i)=ve(k),<v_k,v_j>
   $$
   
4. 活动ai的最晚开始时间l(i)：指该活动弧终点所表示事件的最迟发生时间与该活动所需时间之差
   $$
   l(i)=vl(j)-Weight(v_k,v_j),<v_k,v_j>，
   $$
   
5. 时间余量：一个活动ai的最迟开始时间l(i)和最早开始时间e(i)的差额。当d(i)==0，则ai是关键活动
   $$
   d(i)=l(i)-e(i)
   $$
   

**求关键活动的Steps**：

1. 求所有事件的最早发生时间 ve()
2. 求所有事件的最迟发生时间v()
3. 求所有活动的最早发生时间el()
4. 求所有活动的最迟发生时间l()
5. 求所有活动的时间余量 d()

⚠️关键活动缩短到一定程度可能会变成非关键活动
	 网中的关键路径可能不唯一

# Ch7. 查找

## 7.1 基本概念

1. 查找——在数据集合中寻找满足某种条件的数据元素的过程称为查找

2. 查找表（查找结构）——用于查找的数据集合称为查找表，它由同一类型的数据元素(或记录）组成

3. 关键字——数据元素中唯一标识该元素的某个数据项的值，使用基于关键字的查找，查找结果应该是唯一的。

4. 静态查找表——仅关注查找速度即可

5. 动态查找表——除了查找速度，也要关注插/删操作是否方便实现

6. 查找长度——在查找运算中，需要对比关键字的次数称为查找长度

7. 平均查找长度ASL——所有查找过程中进行关键字的比较次数的平均值
   $$
   ASL=\sum P_iC_i
   $$
   ⚠️ASL的数量级反应了查找算法的时间复杂度，且一般分为查找成功/失败两种情况

## 7.2 顺序和折半查找

### 7.2.1 顺序查找（线性查找）

**1.一般线性表的顺序查找**

```c
typedef struct{
  ElemType *elem;
  int TableLen;
}SSTable;
int Search_Seq(SSTable ST, ElemType key){
  ST.elem[0]=key; //哨兵
  for(i=ST.TableLen;ST.elem[i]!=key;--i)
  return i;
}
```

$$
ASL_{成功}=\sum P_i(n-i+1)=\frac{n+1}{2} \\
ASL_{不成功}=n+1
$$

**2.有序表的顺序查找**

用判定树分析ASL：

- 一个成功结点的查找长度 = 自身层数
- 一个失败结点的查找长度 = 其父结点所在层数【⚠️看书P258】

$$
ASL_{成功}=\sum P_i(n-i+1)=\frac{n+1}{2} \\
ASL_{不成功}=\sum q_i(l_i-1)=\frac{1+2+...+n+n}{n+1}=\frac{n}{2}+\frac{n}{n+1}
$$

### 7.2.2 折半查找（二分查找）

⚠️只适用于有序的顺序表

```c
int Binary_Search(SeqList L, ElemType key){
  int low=0,high=L.TableLen-1,mid;
  while(low<=high){
    mid = (low+high)/2;
    if(L.elem[mid]==key)
      return mid;
    else if(L.elem[mid]>key)
      high=mid-1;
    else
      low=mid+1;
  }
  return -1;
}
```

$$
ASL_{成功}=\sum\frac{h\times2^{h-1}}{n}=\frac{n+1}{n}log_2(n+1)-1\approx log_2(n+1)-1
$$

时间复杂度O(logn)

⚠️不考虑失败结点时且mid向下取整，右子树结点数 - 左子树结点数 = 0 or 1（若mid改为向上取整反之），所以折半查找树一定是一个平衡二叉树

### 7.2.3 分块查找（索引顺序查找）

特点：块内无序，块间有序

算法过程：

1. 在索引表中确定待查记录所属的分块（可顺序、可折半）
   ⚠️在折半查找索引表且关键字！=索引值的时候，最终停在low>high，要在low所指分块中查找！
2. 在块内顺序查找

$$
假设长度为n的查找表被均匀地分成b块，每块s个元素\\
设索引查找和块内查找的平均查找长度分别为L_I、L_S，则分块查找的平均查找长度为：\\
1.顺序查找：ASL=L_I+L_S=\frac{b+1}{2}+\frac{s+1}{2}=\frac{s^2+2s+n}{2s}\\
此时若s=\sqrt{n}，则平均查找长度min=\sqrt{n}+1\\
2.折半查找：ASL=L_I+L_S=\lceil log_2(b+1)\rceil+\frac{s+1}{2}
$$

⚠️如果用顺序存储，插入新元素时需要移动很多元素，可以选择链式存储

## 7.3 树型查找

### 7.3.1 二叉排序树BST

Def：左子树结点值 < 根结点值 < 右子树结点值 => 中序遍历可以得到一个递增序列

**BST的查找**

```c
//非递归
BSTNode *BST_Search(BSTree T,int key){
	while(T!=NULL &&& key!=T->key){
    if(key< T->key)
      T=T->lchild;
    else
      T=T->rchild;
  }
  return T;
}
//递归
BSTNode *BST_Search(BSTree T,int key){
  if(T==NULL)
    return NULL;
  if(key == T->key)
    return T;
  else if(key< T->key)
    return BSTSearch(T->lchild,key);
  else
    return BSTSearch(T->rchild,key);
}
```

⚠️非递归最坏空间复杂度O(1)，递归最坏空间复杂度O(h)

**BST的插入**

```c
//递归
int BST_Insert(BSTree &T,int k){
  if(T==NULL){
    T(BSTree)malloc(sizeof(BSTNode));
    T->key=k;
    T->lchild=T->rchild=NULL;
    return 1;
  }
  else if(k == T->key) //树中有相同关键字结点，插入失败
    return 0;
  else if(k< T-<key)
    return BST_Insert(T->lchild,k);
  else
    return BST_Insert(T->rchild,k);
}
//非递归。。。。。。待完成
```

⚠️递归算法最坏时间复杂度O(h)

**BST的构造**

```c
void Creat_BST(BSTree &T, int strll, int n){
	T=NULL; //初始时T为空树
	int 1=0;
	while(i<n){ //依次将每个关键字插入到二叉排序树中
		BST_Insert(T,str[i]);
		i++;
  }
}
```

**BST的删除**

删除结点有三种情况：

1. 若被删除结点z是叶结点，则直接删除，不会破坏BST的性质
2. 若结点z只有一棵左子树或右子树，则让z的子树成为z父结点的子树，代替z的位置
3. 若结点z有左右子树，则令z的直接后继 （或直接前驱）代替z，然后从BST中删去这个直接后继（或直接前驱），这样就转换成了第一种或第二种情况

**BST的查找效率分析**

查找长度：在查找运算中，需要对比关键字的次数称为查找长度，反映了查找操作时间复杂度

平均查找长度ASL = O(logn)

### 7.3.2 平衡二叉树（AVL树）

  Def：树上任一结点的左子树和右子树的高度之差不超过1
结点的平衡因子 = 左子树高- 右子树高

**AVL的插入**

⚠️每次调整对象都是最小不平衡子树（以插入路径上离插入结点最近的平衡因子的绝对值大于1的结点作为根的子树）

调整最小不平衡子树（假设A为插入后的最小不平衡树）：

1. LL：在A的左孩子的左子树插入结点，A的平衡因子由1增加到2 ➡️ 右单旋转

   ```c
   //f是爹，p是左孩子，gf是f的爹
   f->lchild = p->rchild;
   p->rchild = f;
   gf->lchild/rchild = p;
   ```

   

2. RR：在A的右孩子的右子树中插入结点，A的平衡因子由-1变成-2➡️左单旋转

   ```c
   //f是爹，p是左孩子，gf是f的爹
   f->rchild = p->lchild;
   p->lchild = f;
   gf->lchild/rchild = p;
   ```

   

3. LR：在A的左孩子的右子树插入结点，A的平衡因子由1增加到2➡️先左后右双旋转

4. RL：在A的右孩子的左子树插入结点，A的平衡因子由-1变成-2➡️先右后左双旋转

**AVL查找效率分析**

时间复杂度不超过O(h)

假设n_h表示深度为h的平衡树中含有的最少结点数
$$
n_h=n_{h-1}+n_{h-2}+1\\
h_{max}=log_2n
$$
所以时间复杂度=查找长度=logn

### 7.3.3 红黑树RBT

⚠️AVL树：插入/删除很容易破坏平衡特性，需要频繁调整树的形态
	 RBT：插入/删除很多时候不会破坏红黑特性，无需频繁调整树的形态；若需调整一般在常数级时间完成

**1.RBT的定义&性质**

红黑树RBT是一种BST的优化（⚠️左根右，根叶黑，不红红，黑路同）

1. 每个结点是红色或黑色
2. 根结点黑色
3. 叶结点（外部结点，失败结点，NULL结点）黑色
4. 不存在两个相邻的红结点（父结点和孩子结点都是黑色）
5. 对每个结点，从该结点到任一叶结点的简单路径上，所含黑结点的数量相同
   结点黑高bh——从某结点出发（不含该结点）到达任一空叶结点的路径上黑结点总数

```c
struct RBnode{
  int key;
  RBnode* parent;
  RBnode* lChild;
  RBnode* rChild;
  int color;
}
```

RBT性质：

1. 从根结点到叶结点的最长路径不大于最短路径的2倍
   =>左右子树的高度差不超过两倍
2. 由n个内部结点的红黑树高度h<=2log(n+1)
   => 红黑树的查找操作时间复杂度 = O(logn)
3. 新插入的结点初始为红色

**2.RBT的插入**

插入策略（如何调整看叔叔脸色）：
⚠️非根结点的插入只需要看是否违反不红红

1. 先查找，确定插入位置（原理同二叉排序树），插入新结点
2. 新结点是根一一染为黑色
3. 新结点非根——染为红色
   - 若插入新结点后依然满足红黑树定义，则插入结束
   - 若插入新结点后不满足红黑树定义，需要调整，使其重新满足红黑树定义
   - 黑叔：旋转＋染色
     - LL型：右单旋，父换爷＋染色（红<->黑）
     - RR型：左单旋，父换爷＋染色（红<->黑）
     - LR型：左、右双旋，儿换爷＋染色（红<->黑）
     - RL型：右、左双旋，儿换爷＋染色（红<->黑）
   - 红叔：染色＋变新
     - 叔父爷染色，爷变为新结点

Thinking：根结点黑高为h的红黑树，内部结点数至少多少个
Ans：内部结点数最少的情况——总共h层黑结点的满树形态，所以内部结点至少2^h -1个 =>性质2

**3.RBT的删除**

⚠️几乎不会考

① 红黑树删除操作的时间复杂度=0(log,n)
② 在红黑树中删除结点的处理方式和“二叉排序树的删除”一样
③按②删除结点后，可能破坏“红黑树特性”，此时需要调整结点颜色、位置，使其再次满足“红黑树特性”

## 7.4 B树和B+树

### 7.4.1 B树

保证查找效率：

1. m叉查找树中，规定除根结点外，任何结点至少有\ceil[m/2]个分叉，即至少含有\ceil[m/2]-1个关键字
2. 绝对平衡——m叉查找树中，规定任何一个结点，其所有子树的高度都要相同

**B树的定义**

B树，又称多路平衡（绝对平衡）查找树，B树中所有结点的孩子个数的最大值称为B树的阶，通常用m表示。一棵m阶B树或为空树，或为满足如下特性的m叉树：

1) 树中每个结点至多有m棵子树，即至多含有m-1个关键字。
2) 若根结点不是终端结点，则至少有两棵子树。
3) 除根结点外的所有非叶结点至少有[m/2]棵子树，即至少含有[m/2]-1个关键字
4) 所有的叶结点都出现在同一层次上，并且不带信息(可以视为外部结点或类似于折半查找判定树的查找失败结点，实际上这些结点不存在，指向这些结点的指针为空）

**m阶B树的核心特性**

1) 根节点的子树数 in [2，m]，关键字数 in [1，m-1]。
其他结点的子树数 in [[m/2]，m];关键字数 in [[m/2]-1,m-1]
2) 对任一结点，其所有子树高度都相同
3) 关键字的值：子树0<关键字1<子树1<关键字2<子树2<..（类比二叉查找树 左<中<右）

**1.B树的高度**

⚠️大部分算B树的高度不包括叶子结点（失败结点）

Ques：含n个关键字的m阶B树，最小高度，最大高度=？

Ans：

- 最小高度——让每个结点尽可能的满，有m-1个关键字，m个分叉
  $$
  n\le(m-1)(1+m+m^2+...+m^{h-1})=m^h-1\\
  h\ge log_m(n+1)
  $$

- 最大高度——让各层的分叉尽可能的少，即根结点只有两个分叉，其他结点只有[m/2]个分叉
  各层结点至少有：1st - 1；2nd - 2；3rd - 2[m/2]...... hth - 2[m/2]^(h-2)
  第h+1层共有叶子结点2[m/2]^(h-1)个
  $$
  n个关键字的B树必有n+1个叶子结点\\
  =>n+1\ge 2\lceil m/2\rceil^{h-1}\\
  =>h\le log_{\lceil m/2\rceil}\frac{n+1}{2}+1
  $$
  从关键字入手求：

  <img src="/Users/liuxiaolin/Library/Application Support/typora-user-images/image-20220414211215297.png" alt="image-20220414211215297"  />

**2.B树的插入**

⚠️新元素插入一定在终端结点中

核心要求：
①对m阶B树除根节点外，结点关键字个数[m/2]- 1≤n≤m-1
②子树0<关键字1<子树1<关键字2<子树2<......

在插入key后，若导致原结点关键字数超过上限：

1. 从中间位置([m/2]） 将其中的关键字分为两部分
2. 左部分包含的关键字放在原结点中，右部分包含的关键字放到新结点中
3. 中间位置 （[m/2]） 的结点插入原结点的父结点。
4. 若此时导致其父结点的关键字个数也超过了上限，则继续进行这种分裂操作，直至这个过程传到根结点为止，进而导致B树高度增1。

**3.B树的删除**

- 若被删除关键字在终端结点，则直接删除该关键字（要注意结点关键字个数是否低于下限[m/2] - 1）

  - 若关键字个数低于下限
    - 兄弟够借（父子换位法）：当右兄弟宽裕时，用当前结点的后继、后继的后继来填补空缺；当左兄弟宽裕时，使用当前结点的前驱、前驱的前驱来填补空缺
    - 兄弟不够借：将关键字删除后与兄弟结点及双亲结点中的关键字进行合并

  ⚠️若根结点关键字个数减少到0，则直接删除根结点

- 若被删除关键字在非终端结点，则用直接前驱或直接后继来替代被删除的关键字（转化为删除终端结点）

  - 直接前驱：当前关键字的左侧指针所指子树中“最右下”的元素
  - 直接后继：当前关键字的左侧指针所指子树中“最左下”的元素

### 7.4.2 B+树

![image-20220414214325262](/Users/liuxiaolin/Library/Application Support/typora-user-images/image-20220414214325262.png)

⚠️无论查找成功与否，最终一定都要做到最下面一层结点
	 叶子结点代表最下面一层的一块，一个叶子结点中含若干个关键字

**一棵m阶的B+树需满足下列条件：**

1. 每个分支结点最多有m棵子树（孩子结点）
2. 非叶根结点至少有两棵子树，其他每个分支结点至少有[m/2] 棵子树。
3. 结点的子树个数与关键字个数相等。
4. 所有叶结点包含全部关键字及指向相应记录的指针，叶结点中将关键字按大小顺序排列，并且相邻叶结点按大小顺序相互链接起来。

**m阶B树 vs B+树：**

<img src="/Users/liuxiaolin/Library/Application Support/typora-user-images/image-20220414215838487.png" alt="image-20220414215838487" style="zoom:80%;" />

⚠️在B+树中，非叶结点不含有该关键字对应记录的存储地址。可以使一个磁盘块可以包含更多个关键字，使得B+树的阶更大，树高更矮，
读磁盘次数更少，查找更快。

## 7.5 散列表（Hash Table）

### 7.5.1 散列表的基本概念

Hash Table：是一种数据结构，数据元素的关键字与其存储地址直接相关
Hash Function：一个把查找表中关键字映射成该关键字对应的地址的函数，Hash(key)=Addr
Hash查找：空间换时间

同义词：不同的关键字通过散列函数映射到同一个值
冲突：通过散列函数确定的位置已经存放了其他元素

装填因子a=表中记录数/散列表长度

理想状态下，散列查找的时间复杂度为O(1)

### 7.5.2 散列表的构造方法

设计目标——让不同关键字的冲突尽可能少

1. 直接定址法——H(key) = key 或 H(key) = a*key+b
           其中，a和b是常数。这种方法计算最简单，且不会产生冲突。它适合关键字的分布基本连续的情况，若关键字分布不连续，空位较多，则会造成存储空间的浪费
2. 除数留余法——H(key) = key%p
           散列表表长m，取一个不大于m但最接近或等于m的质数p
3. 数字分析法——选取数码分布较为均匀的若干位作为散列地址
           设关键字是r进制数 （如十进制数），而r个数码在各位上出现的频率不一定相同，可能在某些位上分布均匀一些，每种数码出现的机会均等；而在某些位上分布不均匀，只有某几种数码经常出现，此时可选取数码分布较为均匀的若干位作为散列地址。这种方法适合于已知的关键字集合，若更换了关键字，则需要重新构造新的散列函数。
4. 平方取中法——取关键字的平方值的中间几位作为散列地址
           具体取多少位要视实际情况而定。这种方法得到的散列地址与 关键宇的每位都有关系，因此使得散列地址分布比较均匀，适用于关键字的每位取值都不够均匀或均小于散列地址所需的位数。

### 7.5.3 处理冲突的方法

**1.开放定址法**

⚠️删除结点时不能简单地将被删除结点的空间置为空，否则将截断在它之后填入Hash表的同义词结点的查找路径，可以做一个删除标记，进行逻辑上的删除

Def：是指可存放新表项的空闲地址既向它的同义词表项开放，又向它的非同义词表项开放
$$
H_i=(H(key)+d_i)\%m\\
i=0,1,2...,k(k\le m-1),表长m,d_i为增量序列;i可理解为发生第i次冲突
$$

- 线性探测法——di=0,1,2,...,m-1，即发生冲突时，每次往后探测相邻的下一个单元是否为空，直到找出下一次空闲单元
  ⚠️探测到表尾地址m-1时，下一个探测地址是表首地址0

- 平方探测法（二次探测法）——di=0^2,1^2,-1^2,2^2,...,k^2,-k^2（k<=m/2）
  比起线性探测法更不容易产生聚集问题
  ⚠️要求散列表长度必须是4k+3的素数，才能探测到所有位置；若不满足条件，也至少能探测一半的位置

- 再散列法——除了原始散列函数H(key)外，多准备几个散列函数，当散列函数冲突时，用下一个散列函数计算一个新地址，直到不冲突为止
  $$
  H_i=RH_i(key)\space (i=1,2,3,...,k)
  $$
  
- 伪随机序列法——di是一个伪随机序列

**2.拉链法(Chaining)**

Def：把所有的同义词存储在一个线性链表中

⚠️适用于经常进行插入删除的情况

### 7.5.4 散列查找及性能分析

给定关键字Key，根据Hash函数计算散列地址，Steps：

- 初始化：Addr=Hash(key)

1. 检查Addr位置上是否有记录，若无则返回查找失败；若有记录，则比较与key的值，若等，则返回查找成功，否则执行下一步
2. 用给定的冲突处理办法计算下一个Hash地址，赋给Addr，执行Step1.

- 要求会算ASL

装填因子 = 表中记录数n/散列表长度m

散列表的查找效率的决定因素：散列函数、处理冲突的方法、装填因子

散列表的ASL依赖于散列表的装填因子，而不直接依赖于n、m

# Ch8. 排序

## 8.1 排序的基本概念

算法的稳定性：关键字的相同的元素在排序之后的相对位置不变，则为稳定的，反之不稳定（稳定 != 优秀）
⚠️证明算法不稳定只需提出一种实例即可

**排序算法分类**：

- 内部排序：数据都在内存中（关注算法的时间空间复杂度）
- 外部排序：数据太多，无法全部放入内存（还要关注读写磁盘次数）

## 8.2 插入排序

### 8.2.1 直接插入排序

算法思想：每次将一个待排序的记录按其关键字大小插入到前面已排好序的子序列中，直到全部记录插入完成

```c
void InsertSort(int A[], int n){//不带哨兵
  int i,j,temp;
  for(i=1;i<n;i++){
    if(A[i]<A[i-1]){
      temp = A[i];
      for(j=i-1;j>=0 && A[j]>temp;--j)
        A[j+1] = A[j];
      A[j+1]=temp;
    }//if
  }//for_1
}
```

空间复杂度O(1)

时间复杂度：主要来自关键字的对比和移动

- 最好：本来就有序，只需要对比n-1次，复杂度O(n)
- 最坏：逆序，复杂度O(n^2)
- 平均复杂度O(n^2)

算法稳定性——稳定

### 8.2.2 折半插入排序

算法思路：先用折半查找找到应该插入的位置，再插入元素
当low>high时停止折半查找，应将[low, i-1]内的元素全部右移，并将A[0]复制到low所指位置
为了保证算法的稳定性，当A[mid]=A[0]时，应继续在mid所指位置右边寻找插入位置

```c
void InsertSort(int A[], int n){//带哨兵
  int i,j,low,mid,high;
  for(i=2;i<=n;i++){
    A[0]=A[i]; //把要插入的值赋值给哨兵
    low=1; high=i-1;
    while(low<high){
      mid = (low+high)/2;
      if(A[0]<A[mid])
        high=mid-1; //查找左半表
      else
        low=mid+1; //查找右半表
    }//终止后low=high+1
    for(j=i-1;j>=high+1;--j)
      A[j+1]=A[j];
    A[high+1]=A[0]; //high+1=low=插入位置
  }
}
```

比较关键字的时间复杂度O(nlogn)，但是移动次数未改变，所以总平均时间复杂度仍未O(n^2)

### 8.2.3 希尔排序

算法思想：先将待排序表分割成若干形如L[i, i+d, i+2d, ..., i+kd]的特殊子表，对各个子表分别进行直接插入排序。然后缩小增量d（一般d=d/2），重复上述过程，直到d=1为止

```c
void ShellSort(int A[], int n){//METHOD_1：逐个遍历排序，每次++i切换就切换子表
  int d,i,j;
  //A[0]只是暂存单元，不是哨兵，当j<=0时，插入位置已到
  for(d=n/2;d>=1;d=d/2)
    for(i=d+1;i<=n;++i)//从子表的第二个开始，++i是逐个子表切换排序
      if(A[i]<A[i-d]){
        A[0]=A[i];
        for(j=i-d;j>0 && A[0]<A[j];j-=d)//直接插入排序
          A[j+d]=A[j];
        A[j+d]=A[0];
      }//if
}
```

```c
void ShellSort(int A[], int n){//METHOD_2:逐个子表排序，i+=d替换++i👍
    int d,i,j;
    for(d=n/2;d>=1;d=d/2)
        for(i=d+1;i<=n;i+=d){
            if(A[i]<A[i-d]){
                A[0]=A[i];
                for(j=i-d;j>=0 && A[j]>A[0];j-=d)
                    A[j+d]=A[j];
                A[j+d]=A[0];
            }
        }
}
```

空间复杂度O(1)

时间复杂度：最坏O(n^2)，当n在某个范围内O(n^1.3)

算法稳定性：不稳定！

⚠️仅适用于顺序表，不适用于链表

## 8.3 交换排序

### 8.3.1 冒泡排序

算法思想：从后往前（或从前往后）两两比较相邻元素的值，若为逆序（A[i-1]>A[i]），则交换它们，直到序列比较完。这就是一趟冒泡排序，若完成一个序列排序，最多需要n-1趟。

```c
void swap(int &a, int &b){
  int temp=a;
  a=b;
  b=temp;
}
void BubbleSort(int A[], int n){
  for(int i=0;i<n-1;i++){
    bool flag=false; //标识本趟冒泡是否发生了交换
    for(int j=n-1;j>i;j--)
      if(A[j-1]>A[j]){ //保证算法稳定性
        swap(A[j-1], A[j]);
        flag=true;
      }//if
    if(flag==false) //表示本次冒泡没有发生交换，可以提前结束
      return;
  }//for_1
}
```

空间复杂度O(1)

时间复杂度：

- 最好（本来有序）：需要对比n-1次，O(n-1)
- 最坏（本来逆序）：比较次数 = 交换次数 = n(n-1)/2，O(n^2)
- 平均O(n^2)

算法稳定性：稳定！

可以适用于链表，改为从头到尾冒泡

⚠️元素移动次数 = 3 * 交换次数 = 3n(n-1)/2（因为swap函数交换两个元素实际进行了3次移动）
	 每一趟都会将一个元素放到最终位置上

### 8.3.2 快速排序

算法思想（分治法）：在待排序表L[1...n]中任取一个元素pivot作为枢轴（或基准，通常取首元素)，通过一趟排序将待排序表划分为独立的两部分L[1...k-11和L[k+1.n]， 使得L[1..k-1]中的所有元素小于pivot， L[k+1...n]中的所有元素大于等于pivot，则pivot放在了其最终位置L(k)上，这个过程称为一次“划分”。然后分别递归地对两个子表重复上述过程，直至每部分内只有一个元素或空为止，即所有元素放在了其最终位置上。

```c
int Partition(int A[], int low, int high){
  int pivot=A[low];
  while(low<high){
    while(low<high && A[high]>=pivot)
      --high;
    A[low]=A[high];
    while(low<high && A[low]<=pivot)
      ++low;
    A[high]=A[low];
  }
  A[low]=pivot; //此时low=high
  return low;
}
void QuickSort(int A[], int low, int high){
  if(low<high){
    int pivotpos=Partition(A,low,high);
    QuickSort(A,low,pivotpos-1);
    QuickSort(A,pivotpos+1,high);
  }
}
```

时间复杂度=O(n*递归层数)

- 最好=O(nlogn)
- 最坏=O(n^2)
- 平均=O(nlogn)

空间复杂度=O(递归层数)

- 最好=O(logn)
- 最坏=O(n)

 算法优化思路：

1. 选头、中、尾三个位置的元素，取中间值作为枢轴元素
2. 随机选一个元素作为枢轴元素

算法的稳定性：不稳定！

## 8.4 选择排序

选择排序思想：每一趟在待排序元素中选取关键字最小（或最大）的元素加入有序子序列

### 8.4.1 简单选择排序

```c
void SelectSort(int A[], int n){
  int i,j;
  for(i=0;i<n-1;i++){ //一定需要n-1次
    min=i; //记录待排序列最小元素位置
    for(j=i+1;j<n;j++)
      if(A[j]<A[min])
        min=j; //更新最小元素
    if(min!=i)
      swap(A[i],A[min]); //封装的swap函数共移动元素3次
  }
}
```

空间复杂度O(1)
时间复杂度O(n^2)：一定需要进行n-1趟处理，对比关键字次数 = n-1 + n-2 +...+1 = n(n-1)/2；元素交换次数 < n-1

算法稳定性：不稳定！

⚠️适用于顺序表和链表

### 8.4.2 堆排序

堆Heap：

1. 大根堆：L[i] >= L[2i]和L[2i+1] （1 <= i <=n/2），根大于左右孩子

2. 小根堆：L[i] <= L[2i]和L[2i+1] （1 <= i <=n/2），根小于左右孩子

   ⚠️把heap看成一个完全二叉树的顺序存储序列

- 建立大根堆

```c
void BuildMaxHeap(int A[], int len){
  for(int i=len/2;i>0;i--)
    HeadAdjust(A,i,len);
}
void HeadAdjust(int A[], int k, int len){
  //该函数是将k为根的子树进行大根堆调整
  A[0]=A[k]; //暂存k子树根结点
  for(int i=2*k;i<=len;i*=2){ //小元素下坠
    if(i<len && A[i]<A[i+1]) //选出k的大孩子
      i++;
    if(A[0]>=A[i]) //k比两个孩子都大，筛选结束
      break;
    else{
      A[k]=A[i]; //将大孩子换到双亲结点上
      k=i; //修改k可能的最终位置，方便继续下坠
    }//else
  }//for
  A[k]=A[0];
}
```

- 堆排序算法

```c
void HeapSort(int A[], int n){
  BuildMaxHeap(A,len);
  for(i=len;i>1;i--){ //n-1趟交换即可
    swap(A[i],A[1]); //输出堆顶元素（和堆底元素交换）
    HeadAdjust(A,1,i-1) //重新调整i-1个元素的堆
  }
}
```

结论：一个结点每下坠一层最多只需比较关键字两次
若树高h，某结点在i层，将这个结点向下调整最多需要下坠h-i层，关键字对比最多不超过2(h-i)
n个结点的完全二叉树树高h=[logn]+1，第i层最多只有2^(i-1)个结点

空间复杂度O(1)
时间复杂度：建立堆的时间为O(4n) = O(n)；每次调整的时间为O(h) = O(logn)，共n-1次调整；故堆排序的时间复杂度为O((n-1)logn+ n) = O(nlogn)

算法的稳定性：不稳定！

⚠️向有n个结点的堆中插入一个新元素和删除一个元素的时间复杂度 = O(logn)



## 8.5 归并排序和基数排序

### 8.5.1 归并（Merge）排序

归并：把两个或多个有序的序列合并成一个
“n路”归并：每选出一个元素需要对比关键字n-1次

核心操作：把数组中两个有序序列合并成一个

```c
int *B = (int*)malloc(n*sizeof(int)); //创建一个和A等长的辅助数组B
void Merge(int A[],int low,int mid,int high){
  //A[low,mid]和A[mid+1,high]分别为两个有序的子序列
  int i,j,k;
  for(k=low;k<=high;k++) //将需要排序的两个子序列从A复制到B
    B[k]=A[k];
  for(i=low,j=mid+1,k=i;i<=mid&&j<=high;k++){
    if(B[i]<=B[j]) //<=保证算法的稳定性
      A[k]=B[i++]; //先赋值B[i]在进行i++
    else
      A[k]=B[j++];
  }//for
  while(i<=mid) A[k++]=B[i++];
  while(j<=high) A[k++]=B[j++];
  //两个while最后只有一个会执行
}
void MergeSort(int A[],int low,int high){ //递归归并
  if(low<high){
    int mid = (low+high)/2;
    MergeSort(A,low,mid);
    MergeSort(A,mid,high);
    Merge(A,low,mid,high);
  }//if
}
```

“2路”归并可以把归并过程看成一个“归并树”（倒立的二叉树），二叉树的h层最多2^(h-1)个结点，即应满足n<=2^(h-1)，所以n个元素进行“2路”归并排序，归并趟数 = h-1 = \ceil[logn]，每趟归并的时间复杂度O(n)

=>算法的总时间复杂度为O(nlogn)
空间复杂度O(n)
算法稳定性：稳定！

### 8.5.2 基数（Radix）排序——链式存储

$$
假设长度为n的线性表中每个结点a_j的关键字由d元组(k_j^{d-1},k_j^{d-2},k_j^{d-3},...,k_j^{2},k_j^{1},k_j^{0})组成\\
其中，0\leq k_j^{i}\leq r-1(0\leq j<n,0\le i\le d-1)，r称为'基数'
$$

基数排序得到递增序列的过程：

1. 初始化：设置r个空队列，Q0, Q1, ...,Qr-1，按照各个关键位权重递增的次序对d个关键字位分别做“分配”和“收集”

2. 分配：顺序扫描各个元素，若当前处理的关键字位=x，则将元素插入Qx队尾

3. 收集：把Q0, Q1, Q2, ...,各个队列中的结点依次出队并链接

   ⚠️若要得到递减序列，在收集时从Qr-1逆向收集即可

空间复杂度O(r)
时间复杂度：基数排序需要进行d趟分配和收集，一趟分配需要O(n)，一趟收集需要O(r)，所以总的时间复杂度为O(d(n+r))，与序列的初始状态无关
算法的稳定性：稳定！

基数排序擅长解决的问题：

1. 数据元素关键字可以方便的拆分为d组，且d较小；反例：身份证号排序
2. 每组关键字的取值范围不大，即r较小；反例：中文人名排序
3. 数据元素个数n较大；such as：给全国人民身份证号排序

## 8.7 外部排序

Def：数据元素太多，无法一次全部读入内存进行排序

外部排序时间开销 = 读写外存的时间（占大比重） + 内部排序所需时间 + 内部归并所需时间

### 8.7.2 多路归并与败者树

**多路归并优化**

⚠️在做m路平衡归并排序时，至少需要m个输入缓冲区和1个输出缓冲区；若还要实现输入/内部归并/输出的并行处理，则需要*2

- 可以减少归并趟数，从而减少磁盘IO次数

$$
对于r个初始归并段，做k路归并，则归并树可以用k叉树表示\\
若树高为h，则归并趟数S = h-1 = \lceil log_kr\rceil
$$

得出结论：k越大，r越小，归并趟数越小，磁盘IO操作次数越少

在k个元素中选择最小的关键字需比较k-1次
每趟归并n个元素需要做(n-1)(k-1)次比较
=>S趟归并总共需要比较次数 = 
$$
S(n-1)(k-1)=\lceil log_kr\rceil(n-1)(k-1)=\lceil log_2r\rceil(n-1)\frac{k-1}{log_2k}
$$
多路归并带来的负面影响：

1. k路归并需要开辟k个缓冲区，增加内存开销
2. 每挑选一个关键字需要对比关键字k-1次，内部归并所需时间增加

- 减少初始归并段数量r

生成初始归并段的“内存工作区”越大，初始归并段越长，则初始归并段数量r越少

⚠️若共N个记录，内存工作区可以容纳L个记录，则初始归并段数量r=N/L

- k路平衡归并
  1. 最多只能由k个段归并为一个
  2. 每一趟归并中，若有m个归并段参加归并，则经过这一趟处理得到 \ceil[m-k] 个新的归并段

**败者树**

败者树解决的问题：使用多路平衡归并可减少归并趟数，但是用老土方法从k 个归并段选出一个最小/最大元素需要对比关键字 k-1次，构造败者树可以使关键字对比次数减少到 \ceil[logk]

败者树可视为一棵完全二叉树（多了一个头头）。。人个叶结点分别对应 k 个归并段中当前参加比较的元素，非叶子结点用来记忆左右子树中的“失败者”，而让胜者往上继续进行比较，一直到根结点

k路归并排序败者树的深度为 \ceil[logk]，因此k个记录中选择最小关键字最多需要比较 \ceil[logk]次 => S趟归并总共需要比较次数 = 
$$
S(n-1)\lceil log_2k\rceil=\lceil log_kr\rceil(n-1)\lceil log_2k\rceil=\lceil log_2r\rceil(n-1)
$$
可以见得通过败者树，内部归并排序比较的次数和k路无关了

⚠️仍然不能无限制增大k，因为k对应着开辟的缓冲区的数量，若内存总空间不变，增大缓冲区的数量就要减少单个缓冲区的容量，这样就会反过来增加硬盘IO次数

### 8.7.4 置换-选择排序（生成初始归并段）

Steps：
设初始待排文件为F1，初始归并段输出文件为FO，内存工作区为WA，FO和WA的初始状态为空，WA可容纳w个记录

1. 从F1输入w个记录到工作区WA
2. 从WA中选出其中关键字取最小值的记录，记为MINIMAX记录
3. 将MINIMAX记录输出到FO中去
4. 若F1不空，则从F1输入下一个记录到WA中
5. 从WA中所有关键字比MINIMAX记录的关键字大的记录中选出最小关键字记录，作为新的MINIMAX记录
6. 重复3.～5.，直至在WA中选不出新的MINIMAX记录为止，由此得到一个初始归并段，输出一个归并段的结束标志到FO
7. 重复2.～6.，直至WA为空。由此得到全部归并段

### 8.7.5 最佳归并树

作用：设计k路归并排序的最佳方案

重要结论：归并过程中的磁盘IO次数 = 归并树的WPL * 2
=> 最佳归并树就是构造归并哈夫曼树

⚠️对于k叉归并，若初始归并段的数量无法构成严格的k叉归并树，则需补充几个长度为0的“虚段”，再进行k叉哈夫曼树的构造

**如何判断添加虚段的数目？**

k叉的最佳归并树一定是一颗严格的k叉树，即树中只包含度=k, 0的结点。
设度=k的结点有nk个，度=0的结点有n0个，归并树总结点数=n，则：

- 初始归并段数量 + 虚段数量 = n0（都是叶子结点）
  $$
  \begin{numcases}{}
  n=n_0+n_k \\
  k*n_k=n-1
  \end{numcases}
  \rightarrow
  n_0=(k-1)n_k+1\rightarrow n_k=\frac{n_0-1}{k-1}
  (n_k \in Z)
  $$

1. 若（初始归并段-1）%（k-1）=0，说明不需要添加虚段刚好可以构成严格k叉树
2. 若（初始归并段-1）%（k-1）！=0，说明需要补充(k-1)-u个虚段
