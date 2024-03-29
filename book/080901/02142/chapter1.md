---
title: 第一章 概论 - 《数据结构导论》笔记
date: 2023-08-20T16:36:20+08:00
slug: book/080901/02142/chapter1
categories:
  - 书籍
tags:
  - 数据结构导论
  - 计算机科学与技术
  - 笔记
draft: false
image: https://ghjayce.github.io/asset/blog/k13uqmFROI7ZE6i8K9HpMjAyNDAzMDFfMTM1MDM2.png
---

## 目录

- [**第一章 概论**](../chapter1) <small>P21 ~ 34（13页）</small>
- [第二章 线性表](../chapter2) <small>P35 ~ 58（23页）</small>
- [第三章 栈、队列和数组](../chapter3) <small>P59 ~ 92（33页）</small>
- [第四章 树和二叉树](../chapter4) <small>P93 ~ 128（35页）</small>
- [第五章 图](../chapter5) <small>P129 ~ 160（31页）</small>
- [第六章 查找](../chapter6) <small>P161 ~ 182（21页）</small>
- [第七章 排序](../chapter7) <small>P183 ~ 204（27页）</small>
- [考试重点](../exam-focus)

## 阅读说明
1. 本书
	- 书名：《数据结构导论》
	- 出版：外语教学与研究出版社
	- 版本：2012年版
2. 全书以类C语言来描述相关内容，例如：存储结构、算法等。

### 本文涉及知识点
1. 数学知识
	1. 乘法分配率
	2. [对数](https://ghjayce.github.io/p/subject/math/sequence/logarithm/)
	3. [等差数列](https://ghjayce.github.io/p/subject/math/sequence/arithmetic/)、[高斯求和](https://ghjayce.github.io/p/subject/math/sequence/arithmetic/#求和)

## 概要
1. 数据、数据元素和数据项
	1. 基本概念
	2. 三者的关系
2. 数据的逻辑结构和存储结构
	1. 四种基本的逻辑结构和特点
		- 集合
		- 线性结构
		- 树形结构
		- 图结构
	2. 两种基本的存储结构
		- 顺序存储
		- 链式存储
	3. 逻辑结构与存储结构的关系
3. 运算与逻辑结构的关系
4. 算法分析
	1. 描述方法
	2. 评价因素
	3. 时间复杂度的分析方法
	4. 空间复杂度的分析方法

## 数据、数据元素和数据项
### 概念和联系

- 数据项：在数据表中指的是一个个的字段，又称**字段**或**域**，数据的最小标识单位。
- 数据元素：在数据表中相当于一行记录，又称**结点**，由若干个数据项组成。
- 数据：被计算机处理、存储的对象，由若干个数据元素组成。

## 数据的逻辑结构和存储结构
### 概念
- 数据结构：包含数据的逻辑结构、存储结构和基本运算。
- 逻辑结构：整个数据元素之间的逻辑关系。
	- 逻辑关系：指单个数据元素之间的关联方式或邻接关系，也就是单个数据元素之间的组织形式。
- 存储结构：也称物理结构，数据的逻辑结构在计算机中的实现称为数据的存储结构。
	- 简单点说：为了保存数据的逻辑结构到计算机中而实现的存储结构。

### 逻辑结构
四类基本的逻辑结构：
- 集合：任意两个结点之间都没有邻接关系，**组织形式松散，就像抽奖箱里的乒乓球**。
	- 结点之间除了同属一个集合，并没有别的关系。
- 线性结构：结点之间一个个依次相邻接排列，**形成一条“链”，就像一条绳子上的多个绳结**。
	- 结点之间是一对一的相互关系。
- 树形结构：上层结点有多个下层结点，下层结点只有一个上层结点，**具有分支、层次特性，就像一颗树**。
	- 结点之间是一对多的相互关系。
- 图结构：最为复杂的结构，**任意两个结点都可以相邻接，就像地铁线路图、人与人之间的社交网络**。
	- 结点之间是多对多的相互关系。

### 存储结构
存储结构一般包括两个部分：
- 需要存储的数据元素
- 结点之间的逻辑关系

实现结点之间的逻辑关系的存储结构，一般有四种形式，主要掌握顺序存储和链式存储：
- 顺序存储：所有结点存储在一个连续的存储区域里，**利用结点在存储器中的相对位置，来表示结点之间的逻辑关系**。
- 链式存储：除了存储结点本身，还需要一个指针，**指针指向有逻辑关系的结点，也就是利用指针表示结点之间的逻辑关系**。
- 索引存储
- 散列存储

#### 如何描述
怎么描述存储结构是哪种类型？哪种实现形式呢？分为两种方式：
- 机器级：即存储结构在计算机存储器里的表示形式，以内存地址的方式。
- 语言级：即用程序设计语言中的类型说明、变量说明，例如，数据类型：数组、结构体和指针等。

## 运算
### 概念
运算是指在逻辑结构上施加的操作，也就是对逻辑结构的加工。

这些运算操作包括：
- 建立
- 查找
- 读取
- 插入
- 删除
- 等...

## 算法分析
### 评价算法
评价算法好坏的因素分为几个方面：
1. 正确性：能正确地实现预定的功能，满足具体问题的需要。
2. 易读性：易于阅读、理解和交流，便于调试、修改和扩充。
3. 健壮性：能处理不同的输入环境，即使是非法数据，也不会产生预料不到的运行结果。
4. 时空性：指算法的时间性能（所需计算量）和空间性能（所需存储量）。

### 时间复杂度
#### 计算量
来算一算函数执行了几次

例子A：
```c
int func1(void)
{
	printf("hello ghjayce");
	return 0;
}
```
共执行了2次，`printf`1次 + `return`1次

例子B：
```c
int func2(int n)
{
	for (int i = 0; i < n; ++i) {
		printf("hello ghjayce");
	}
	return 0;
}
```
共执行了3n + 3次

分析过程：
1. `int i = 0`，由始至终仅会执行1次。
2. `i < n`，执行n + 1次。
3. `++i`，执行n次。
4. `printf`，执行n次。
5. `return`，执行1次。

要估算某段代码的执行次数，可以用`T(n)`来表示：
- `T`：某段代码的总执行次数。
- `n`：输入数据的规模大小或者数量。

也就是一个算法的计算量是问题规模n的函数，代入以上例子来表示：
- 例子A：T(n) = 2
- 例子B：T(n) = 3n + 3

#### 转换
得出代码的执行次数估算值后，需要转换成时间复杂度的表示方式，以下是转换规则：
1. 如果T(n)是常数的话，时间复杂度直接估算为 1。
2. 如果T(n)不是常数的话，例如：`常数 × n + 常数`
	- 仅保留最高次项，也就是`常数 × n`
	- 常数化为1，也就是`1 × n`，这里的系数可以直接省略。
	- 因此时间复杂度估算为：n。

表达方式还不完整，需要加上大O表示法，也称渐进表示法，例如：`O(时间复杂度估算值)`

结合以上例子，那么时间复杂度表示为：
- 例子A：`O(1)`
- 例子B：`O(n)`

--- 

如果代码量比较多或者函数调用比较多的情况下这样估算会相当麻烦，所以下面是简化估算过程：

例子C：
```c
void func3(int n)
{
	for (int i = 0; i < n; ++i) {
		for (int j = 0; j < n; ++j) {
			printf("hello ghjayce");
		}
	}
}
```
分析过程：
1. 里面的for循环的时间复杂度：`n + 1`
	1. 这里for的时间复杂度：n
	2. for里面的语句的时间复杂度：1
2. 外面的for循环的时间复杂度：`(n + 1) × n`，也就是`(n × n) + (n × 1)`，即`n² + n`

最终时间复杂度为：`O(n²)`

---

例子D：
```c
void func(int n)
{
	for (int i = 0; i < n; ++1) {
		for (int j = i; j < n; ++j) {
			printf("hello ghjayce");
		}
	}
}
```

最终时间复杂度为：`O(n²)`，待补充...

---

例子E：
```c
void func(int n)
{
	for (int i = 1; i < n; i *=2) {
		printf("hello ghjayce");
	}
}
```

最终时间复杂度为：`O(logn)`，待补充...
#### 性能对比
1. 常数阶：O(1)
2. 对数阶：O(log<sub>2</sub>n)，log的底数可以省略不写，也就是：O(logn)，下面也是一样
3. 线性阶：O(n)
4. 线性对数阶：O(nlogn)
5. 多项式阶：O(n<sup>c</sup>)，C为大于1的正整数，下面也是一样
	1. 平方阶：O(n²)
	2. 立方阶：O(n<sup>3</sup>)
	3. K次方阶：O(n<sup>k</sup>)
6. 指数阶：O(C<sup>n</sup>)，常见是O(2<sup>n</sup>)

以上时间复杂度，复杂程度和耗时从上往下依次增加，即越往下时间复杂度越高，所需耗时越多。

通常认为，具有指数阶的算法是不可计算的，而阶数低于平方阶的算法是高效率的。

另外，时间复杂度的性能还会受到输入数据的变化而有所影响，基于相同输入数据量的不同输入数据，分为：
- 最坏时间复杂度：算法时间用量的最大值。
- 平均时间复杂度：算法时间用量的平均值。

### 空间复杂度
一个算法的空间复杂度定义为该算法所耗费的存储空间，它也是问题规模n的函数，记为：
$$S(n)=O(g(n))$$
其中，g(n)为问题规模n的某个函数。一个算法在执行期间所需的存储空间量分为：
1. 程序代码所占用的空间，对不同算法来说也不会有数量级的差别。
2. 输入数据所占用的空间，由问题规模决定的，不随算法的不同而改变。
3. 辅助变量所占用的空间，也称附加存储空间，它所占用的空间会受到问题规模和不同的算法所影响。

来个例子实践一下，假设，n=100。

例子A：
```c
void f1(int a[], int n)
{
	int i, temp;
	for (i = 0; i <= n/2; i++) {
		temp = a[i];
		a[i] = a[n-1-i];
		a[n-1-i] = temp;
	}
}
```
f1所需要的辅助变量为2个整型变量i和temp，与问题的规模无关，空间复杂度为：O(1)。

例子B：
```c
void f2(int a[], int n)
{
	int i, b[100];//随n的大小
	for (i = 0; i <= n-1; i++)
		b[i] = a[n-1-1];
	for (i = 0; i <= n-1; i++)
		a[i] = b[i];
}
```
f2所需要的辅助变量为1个整型变量i和随n大小的数组b，b与问题的规模有关，空间复杂度为：O(n)。

---

如果是递归调用呢？

待补充...
## 参考
1. [指数和对数](https://www.bilibili.com/video/BV1Li4y1M7St)
2. [小学生也能看懂的时间复杂度](https://www.bilibili.com/video/BV1nE411x7qP)
3. [小学速算技巧15讲：简便计算，认知乘法分配率](https://haokan.baidu.com/v?vid=9346000793839061664&collection_id=9980245270058884401&)