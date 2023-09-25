---
title: 第三章 栈、队列和数组 - 《数据结构导论》笔记
date: 2023-09-18T12:24:10+08:00
updateDate: 2023-09-25T18:23:50+08:00
slug: book/080901/02142/chapter3
image: 
categories:
  - 书籍
tags:
  - 数据结构导论
  - 计算机科学与技术
  - 笔记
draft: false
---
## 目录

- [第一章 概论](../chapter1)
- [第二章 线性表](../chapter2)
- [**第三章 栈、队列和数组**](../chapter3)
- [第四章 树和二叉树](../chapter4)
- [第五章 图](../chapter5)
- [第六章 查找](../chapter6)
- [第七章 排序](../chapter7)

## 阅读说明
1. 本文涉及到的代码描述均为C语言。

## 概要
1. 栈
	1. 概念
	2. 特征
	3. 用C语言描述
		- 顺序栈
		- 链栈
	4. 基本运算
		- 初始化
		- 栈空
		- 栈满
		- 进栈
		- 出栈
		- 取栈顶元素
	5. 应用
		- 解决简单问题
2. 队列
	1. 概念
	2. 特征
	3. 用C语言描述
		- 顺序队列
		- 循环队列
	4. 基本运算
		- 初始化
		- 队列空
		- 队列满
		- 入队列
		- 出队列
		- 取队列首元素
	5. 应用
		- 解决简单问题
3. 数组
	1. 一维和二维数组
		- 逻辑结构
		- 存储结构
			- 存储位置计算
	1. 矩阵的压缩存储
		- 特殊矩阵
			- 对称矩阵
			- 三角矩阵
				- 上三角矩阵
				- 下三角矩阵
		- 稀疏矩阵
			- 三元组表示法
1. 简单的综合运用

## 栈
### 概念
栈Stack可以看作是特殊的线性表，特殊性在于基本运算是线性表运算的子集，是一种运算受限的线性表。而受限是指插入和删除运算限定在表的某一端进行。
- 进栈：指插入运算。
- 出栈：指删除运算。
- 栈顶：允许进栈和出栈的一端。
- 栈底：栈顶的另一端。
- 空栈：不含任何数据元素的栈。
- 栈顶元素：处于栈顶位置的数据元素。
- 上溢：栈的容量已经满了，此时再进行进栈就会发生上溢。
- 下溢：空栈做出栈就会产生下溢，因为栈中没有任何数据元素。

> 栈是计算机科学中广泛使用的数据结构之一，例如：函数的嵌套调用和程序递归的处理都是用栈来实现的。

### 特征
栈的修改原则是**后进先出（Last In First Out）**，好比桌面上叠放着待清洗的碗碟，收拾以后的碗碟被放在栈顶，而在栈顶的碗碟会优先得到清洗。因此又称为**后进先出线性表**，简称后进先出表。

#### 生活中的例子
具有栈特征的生活场景：
1. 购物篮：顾客往购物篮放商品会放在栈顶，结账时从栈顶取出商品。
2. 购物推车：顾客的购物车在结账完以后会被回收，叠放在栈顶，下一个顾客需要时会从栈顶取出。
3. 射击：先装的子弹最后才被打出来，特殊子弹夹除外，例如左轮。
4. 添饭：新加的米饭放在栈顶，栈顶的米饭先吃。
5. 水杯加水：这个例子可能不太严谨，因为水流动的特性也有可能喝到非栈顶的水。

### 实现
栈的实现有两种方式：
- 顺序栈：用一组连续的存储单元存放数据元素，通常用一个一维数组和一个记录栈顶位置的变量来实现顺序存储。
- 链栈：用带头结点的单链表实现，命名LS（linkStack），LS指向头结点，`LS->next`指向栈顶结点，也是首结点，尾结点是栈底结点，通过各结点的指针连接组成栈。

栈的基本运算的功能描述：
1. 初始化`InitStack(S)`：构造一个空栈S。
2. 判断栈空`EmptyStack(S)`：若为空栈返回1，否则返回0。
3. 进栈`Push(S, x)`：将元素x插入栈S中，x成为栈顶元素。
4. 出栈`Pop(S)`：删除栈顶元素。
5. 取栈顶`GetTop(S)`：返回栈顶元素。

### 顺序栈
#### 定义
> 顺序栈相关代码放在了[GHBJayce/code-example - sequence stack](https://github.com/GHBJayce/code-example/tree/main/code/c/data-structure/list/stack/sequence/1)中，感兴趣可以clone到本地查看运行结果。

```c
const int maxSize=10; // 顺序栈的容量

typedef struct sequenceStack
{
    dataType data[maxSize]; // 存储栈中数据元素的数组
    int top; // 指向栈顶位置
} seqStack;
```
#### 初始化
```c
seqStack init()
{
    seqStack stack;
    stack.top = 0;
    return stack;
}
```
#### 栈空
```c
int isEmpty(seqStack *stack)
{
	if (stack->top == 0) {
		return 1;
	}
	return 0;
}
```
#### 栈满
```c
int isFull(seqStack *stack)
{
	if (stack->top == maxSize-1) {
		return 1;
	}
	return 0;
}
```
#### 进栈
```c
int push(seqStack *stack, dataType x)
{
    if (isFull(stack)) {
        printf("%s\n", "栈已满");
        exit(EXIT_FAILURE);
    }
    stack->top++;
    stack->data[stack->top] = x;
    return 1;
}
```
#### 出栈
```c
int pop(seqStack *stack)
{
    if (isEmpty(stack)) {
        printf("%s\n", "栈空");
        exit(EXIT_FAILURE);
    }
    stack->top--;
    return 1;
}
```
#### 取栈顶元素
```c
dataType getTop(seqStack *stack)
{
    if (isEmpty(stack)) {
        printf("%s\n", "栈空");
        exit(EXIT_FAILURE);
    }
    return stack->data[stack->top];
}
```

### 双栈
待补充...

### 链栈
#### 定义
> 链栈相关代码放在了[GHBJayce/code-example - link stack](https://github.com/GHBJayce/code-example/tree/5dcbf53aebbc1141bff179a4e7571c43956b31ef/code/c/data-structure/list/stack/link/1)中，感兴趣可以clone到本地查看运行结果。

```c
typedef struct
{
    int id;
} dataType;

typedef struct node
{
    dataType data;
    struct node *next;
} linkStack;
```
#### 初始化
```c
linkStack *init()
{
    linkStack *stack = (linkStack *)malloc(sizeof(linkStack));
    stack->next = NULL;
    return stack;
}
```
#### 栈空
```c
int isEmpty(linkStack *stack)
{
    if (stack->next == NULL) {
        return 1;
    }
    return 0;
}
```
#### 进栈
```c
void push(linkStack *stack, dataType x)
{
    linkStack *newNode = (linkStack *)malloc(sizeof(linkStack));
    newNode->data = x;
    newNode->next = stack->next;
    stack->next = newNode;
}
```
#### 出栈
```c
int pop(linkStack *stack)
{
    if (!isEmpty(stack)) {
        linkStack *removeNode = stack->next;
        stack->next = removeNode->next;
        free(removeNode);
        return 1;
    }
    return 0;
}
```
#### 取栈顶元素
```c
dataType *getTop(linkStack *stack)
{
    if (isEmpty(stack)) {
        return NULL;
    }
    return &(stack->next->data);
}
```
#### 应用
1、将链栈中的结点进行逆置
```c
linkStack *reverse(linkStack *stack)
{
    linkStack *newStack = init();
    linkStack *stackNode = stack->next;
    while (stackNode != NULL) {
        push(newStack, stackNode->data);
        stackNode = stackNode->next;
    }
    stackNode = stack->next;
    while (!isEmpty(newStack)) {
        dataType *data = getTop(newStack);
        if (data != NULL) {
            stackNode->data = *data;
            pop(newStack);
        }
        stackNode = stackNode->next;
    }
    return stack;
}
```
思路：
1. 遍历（逐个访问链表元素）要逆置的链栈（称为A链栈）的结点，将结点通过进栈的方式放到新的链栈（称为B链栈）中，此时B链表中结点的顺序已经与A链栈相反。
2. 对B链栈进行逐一出栈，将出栈的结点按A链表的顺序对结点进行覆盖，完成逆置。

2、递归和栈，用递归实现阶乘函数

待补充...

### 练习
1、设栈S的初始状态为空，若元素a、b、c、d、e、f依次进栈，得到的出栈序列是b、d、c、f、e、a，则栈S的容量至少是多少？

答：至少3。
- a进，b进，容量2。
- b出，容量1。
- c进，d进，容量3。
- d出，c出，容量1。
- e进，f进，容量3。
- f出，e出，a出，容量0。

---

2、设一个链栈的输入序列为A、B、C，试写出所得到的所有可能得输出序列。

答：共有5种可能的输出序列。
- 输出ABC，A进，A出，B进，B出，C进，C出。
- 输出ACB，A进，A出，B进，C进，C出，B出。
- 输出BAC，A进，B进，B出，A出，C进，C出。
- 输出BCA，A进，B进，B出，C进，C出，A出。
- 输出CBA，A进，B进，C进，C出，B出，A出。

---

3、有一个整数序列，其输入顺序为20，30，90，-10，45，78，试利用栈将其输出序列改变为30，-10，45，90，78，20，试给出该整数序列进栈和出栈的操作步骤，用push(x)表示x进栈，pop(x)表示x出栈。

答：
1. push(20)，20
2. push(30)，20、30
3. pop(30)，20
4. push(90)，20、90
5. push(-10)，20、90、-10
6. pop(-10)，20、90
7. push(45)，20、90、45
8. pop(45)，20、90
9. pop(90)，20
10. push(78)，20、78
11. pop(78)，20
12. pop(20)
## 队列
### 概念
队列Queue，和栈一样可以看作是特殊的运动受限的线性表，受限在于插入和删除分别在两端进行。

- 入队列：在队列尾部进行插入运算。
- 出队列：在队列首部进行删除运算。
- 队列首元素：队列首部第一个数据元素。
- 队列尾元素：队列尾部最后一个数据元素。

> 队列也是计算机科学中广泛使用的数据结构之一，例如：操作系统中进程调度、网络管理中的打印服务等都是用队列来实现的。

### 特征
队列的修改原则是**先进先出（First In First out）**，就像排队一样，但不允许插队，新加入的成员只能排在队列尾部，先加入的成员先离开队伍。

#### 生活中的例子
- 排队做核酸。
- 银行办业务。
- 排队上车/登机等。

### 实现
队列也有两种存储实现方式：
- 顺序队列：由一个一维数组以及两个分别指向队列首部元素和队列尾部元素的指针。
- 链队列：由一个带头结点的单链表实现，头指针front指向链表的头结点，头结点的next指向队列首结点，尾指针rear指向队列的尾结点。

队列的基本运算的功能描述：
1. 队列初始化`InitQueue(Q)`：设置一个空队列Q。
2. 判断队列为空`EmptyQueue(Q)`：若队列Q为空，返回1，否则返回0。
3. 入队列`EnQueue(Q, x)`：将数据元素x从队尾一段插入队列。
4. 出队列`OutQueue(Q)`：删除队列首元素。
5. 取队列首元素`GetHead(Q)`：返回队列首元素的值。

### 顺序队列
#### 定义
```c
const int maxSize = 20;
typedef struct sequenceQueue
{
	dataType data[maxSize];
	int front, rear;
} seqQueue;
seqQueue SQ;
```
front表示队列首部，rear表示队列尾部，默认值均为0，也就是指向数组的0下标，该位置不存放数据元素。

那么入队列的操作核心关键是：
```c
SQ.rear = SQ.rear+1;
SQ.data[SQ.rear] = x;
```
出队列的操作：
```c
SQ.front = SQ.front+1;
```
> 这里出队列并不会去删除队列首元素，而是移动指向队列首部的指针。

但是会存在一个问题，举一个例子：
- a)为空队列，`SQ.rear`指向0的下标，`SQ.front`指向0的下标。
- b)为20入队后，`SQ.rear`指向下标1，`SQ.front`为0。
- c)为入队列30、40、50后的队列，`SQ.rear`为4，`SQ.front`为0。
- d)为出队列20、30、40、50后的队列，`SQ.rear`为4，`SQ.front`为4。
- e)为入队列60后，`SQ.rear`为5，`SQ.front`为4。

![顺序队列的假溢出问题](https://raw.githubusercontent.com/GHBJayce/Assets/v1.0.0/computer/data-structure/list/queue/sequence-queue-fake-overflow.png) 

如果在e)的状态下，要做入队列的操作，`SQ.rear`将会超出数组下标的范围，也就是新元素没有办法入队列，但是1-4的位置明明是空的，数组的实际空间并没有沾满，这就是**假溢出**现象。

> 另外还有一个问题是，队列空和队列满没有办法区分，就如a)和d)的情况。

### 循环队列
为了解决顺序队列的假溢出问题，于是就有了循环队列，它将顺序队列数组的首尾进行相接，形成了一个环状，思路就是当入队列操作`SQ.rear`即将要超出数组下标范围的时候，将`SQ.rear=0`进行重置，也就是`SQ.data[0]`作为新的队列尾，这样新的元素就能够利用上空闲的空间，达到循环使用。

至于没有办法区分队列空和队列满的解决方法有两种：
1. 为队列另设一个标志，也就是多加一个变量用作区分。
2. 队列少用一个元素空间，当只剩最后一个空间时就认为队列满，也就是`CQ.rear`差一步就要追上`CQ.front`，如图b)，书中采用了该解决方法。

![循环队列、队列满和队列空示意图](https://raw.githubusercontent.com/GHBJayce/Assets/v1.0.0/computer/data-structure/list/queue/cycle-queue.png) 

> 有点像MySQL redo log的checkpoint和write pos。

#### 定义
> 循环队列相关代码放在了[GHBJayce/code-example - cycle queue](https://github.com/GHBJayce/code-example/tree/efc87b84c0232d1747226820a36fd4e8460466c2/code/c/data-structure/list/queue/cycle/1)中，感兴趣可以clone到本地查看运行结果。

```c
typedef struct cycleQueue
{
	dataType data[maxSize];
	int front, rear;
}
cycleQueue CQ;
```
按以上的解决思路，以下是几个操作的关键核心：

**入队列的操作：**
```c
CQ.rear = (CQ.rear + 1) % maxSize;
CQ.data[CQ.rear] = x;
```
**出队列的操作：**
```c
CQ.front = (CQ.front + 1) % maxSize;
```
**队列满的条件：**
```c
(CQ.rear + 1) % maxSize == CQ.front;
```
**队列空的条件：**
```c
CQ.rear == CQ.front;
```

#### 初始化
```c
void init(cycleQueue CQ)
{
	CQ.front = 0;
	CQ.rear = 0;
}
```
#### 判断队列空
```c
int isEmpty(cycleQueue CQ)
{
	if (CQ.front == CQ.rear) {
		return 1;
	}
	return 0;
}
```
#### 入队列
```c
int push(cycleQueue CQ, dataType x)
{
	if ((CQ.rear + 1) % maxSize == CQ.front) {
		printf("\s\n", "队列满");
		exit(EXIT_FAILURE);
	}
	CQ.rear = CQ.rear + 1;
	CQ.data[CQ.rear] = x;
	return 1;
}
```
#### 出队列
```c
int pop(cycleQueue CQ)
{
	if (isEmpty(CQ)) {
		return 0;
	}
	CQ.front = (CQ.front + 1) % maxSize;
	return 1;
}
```
#### 取队列首元素
```c
dataType getHead(cycleQueue *CQ)
{
    dataType data;
    if (isEmpty(CQ)) {
        return data;
    }
    return CQ->data[(CQ->front + 1) % maxSize];
}
```

### 链队列
再重复一遍，链队列是一个带有头结点的单链表组成，队列首部的指针指向头结点，头指针指向首结点，队列尾部的指针指向尾结点，**队列空的时候，队列首部和尾部的指针均指向头结点**。

链接的实现需要动态申请内存空间，所以不会出现队列满的情况。
#### 定义
> 链队列相关代码放在了[GHBJayce/code-example - link queue](https://github.com/GHBJayce/code-example/tree/efc87b84c0232d1747226820a36fd4e8460466c2/code/c/data-structure/list/queue/link/1)中，感兴趣可以clone到本地查看运行结果。

```c
typedef struct
{
    int id;
} dataType;
typedef struct linkQueueNode
{
    dataType data;
    struct linkQueueNode *next;
} linkQueueNode;
typedef struct linkQueue
{
    linkQueueNode *front, *rear;
} linkQueue;
```
#### 初始化
```c
linkQueue *init()
{
	linkQueue *LQ = (linkQueue *)malloc(sizeof(linkQueue));
	// 生成队列的头结点
	linkQueueNode *node = (linkQueueNode *)malloc(sizeof(linkQueueNode));
	LQ->front = node;
	LQ->rear = node;
	(LQ->front)->next = NULL;
	reutrn LQ;
}
```
#### 判断队列空
```c
int isEmpty(linkQueue LQ)
{
	if (LQ.rear == LQ.front) {
		return 1;
	}
	return 0;
}
```
#### 入队列
```c
void push(linkQueue *LQ, dataType x)
{
    linkQueueNode *newNode = (linkQueueNode *)malloc(sizeof(linkQueueNode));
    newNode->data = x;
    newNode->next = NULL;
    (LQ->rear)->next = newNode;
    LQ->rear = newNode;
}
```
#### 出队列
```c
void pop(linkQueue *LQ)
{
    if (isEmpty(LQ)) {
        printf("%s\n", "队列空");
        exit(EXIT_FAILURE);
    }
    linkQueueNode *deleteNode = LQ->front->next;
    LQ->front->next = deleteNode->next;
    if (deleteNode->next == NULL) {
        LQ->rear = LQ->front;
    }
    free(deleteNode);
}
```
取队列首元素
```c
dataType *getHead(linkQueue *LQ)
{
    if (isEmpty(LQ)) {
        return NULL;
    }
    linkQueueNode *node = LQ->front->next;
    return &(node->data);
}
```
### 应用
待补充...

## 数组
### 概念
数组array可以看成是线性表的一种推广，由一组相同类型的数据元素组成，并存储在一组连续的存储单元中。

- 一维数组：又称向量。
- 二维数组：一维数组中的数据元素是一维数组，一般写成`a[m][n]`，可看作由m行n列组成的线性表。
- 三维数组：一维数组中的数据元素是二维数组。
- n维数组：一维数组中数据元素为n-1维数组。

### 运算
通常只有两种基本运算：
1. 读：返回指定下标的数据元素。
2. 写：修改指定下标的数据元素。

### 存储结构
一维数组数据元素的内存地址是连续的，而二维数组的数据元素有两种存储方法：
1. 以列序为主序的存储。
2. 以行序为主序的存储，C语言编译数组采用的正是该方法。

存储结构演示和数组的逻辑结构插图.png（待补充...）

#### 计算存储位置
有一个二维数组`a[m][n]`，每个数据元素占用`k`个存储单元大小，以行为主序，数组元素`a[i][j]`的存储位置是多少？要怎么计算？
- m：行数量。
- n：列数量。
- i：m行的下标。
- j：n列的下标。

下标是从0开始，那么数据元素`a[i][j]`：
- 该下标之前总共有：`n * i + j + 1`个数据元素，其中：
	- 每行有`n`（列的数量）个元素。
	- 在第`i`行，有`i`行元素，在第`j`列，有`j+1`个元素。
- 第一个元素`a[0][0]`与`a[i][j]`相差`n * i + j + 1 - 1`个位置。

`a[i][j]`的存储位置计算公式为：
$$loc[i, j] = loc[0, 0] + (n * i + j) * k$$
实战一下，例如：

有一个二维数组`a[10][20]`，每个数据元素占用2个存储大小，数组的起始内存地址为2000，求：
1. `a[5][10]`的存储位置是多少？
	- 整理数据：
		- m：10。
		- n：20。
		- i：5。
		- j：10。
		- k：2。
	- 将数据套入公式：`2000 + (20 * 5 + 10) * 2`，存储位置最终为：`2220`。
1. `a[8][19]`呢？留给你来计算。

### 矩阵的压缩存储
待补充...
#### 特殊矩阵
##### 对称矩阵
##### 三角矩阵
#### 稀疏矩阵

## 综合运用

待补充...