---
title: 04737《C++程序设计》概要笔记
date: 2024-03-07T15:41:50+08:00
slug: book/080901/04737/summary
categories:
  - 书籍
tags:
  - 计算机科学与技术
  - 笔记
  - C++程序设计
draft: false
image: https://ghjayce.github.io/asset/blog/pUUpVODdvSxxT4BxTflbMjAyNDAzMDhfMTU0NTM2.png
---

## 目录

- [第一章 C++语言简介](#第一章-C++语言简介) <small>P29 ~ 56（14页）</small>
- [第二章 面向对象的基本概念](#第二章-面向对象的基本概念) <small>P61 ~ 84（12页）</small>
- [第三章 类和对象进阶](#第三章-类和对象进阶) <small>P91 ~ 140（25页）</small>
- [第四章 运算符重载](#第四章-运算符重载) <small>P150 ~ 172（12页）</small>
- [第五章 类的继承与派生](#第五章-类的继承与派生) <small>P177 ~ 236（30页）</small>
- [第六章 多态与虚函数](#第六章-多态与虚函数) <small>P244 ~ 270（14页）</small>
- [第七章 输入/输出流](#第七章-输入/输出流) <small>P276 ~ 297（11页）</small>
- [第八章 文件操作](#第八章-文件操作) <small>P302 ~ 324（12页）</small>
- [第九章 函数模板与类模板](#第九章-函数模板与类模板) <small>P329 ~ 348（10页）</small>
- [考试重点](#考试重点)

## 前言
### hello world
创建文件`hello.cpp`，编写代码：
```c++
#include <iostream>
using namespace std;
int main() {
	cout << "hello world!" << endl;
	return 0;
}
```
编译并运行
```bash
g++ hello.cpp -o hello
./hello
```


## 第一章 C++语言简介
### 第二节 C++语言的特点
1. 编译式、通用的、大小写敏感的编程语言。
2. 可运行于多种平台，如Windows、Mac及Unix多种版本的操作系统。
3. 加入了面向对象的概念。
4. C语言的继承，可以像C语言那样进行结构化程序设计。

#### 一、基本的输入/输出
在C语言中，标准的键盘输入、屏幕输出分别是`scanf()`、`printf()`两个函数。

在C++中，类库中提供了输入流类`istream`、输出流类`ostream`，也就是`cin`、`cout`对象。

从输入流中获取数据的操作称为**提取操作**，向输出流中添加数据的操作称为**插入操作**。

运算符`>>`、`<<`是移位运算符，在C++类库提供的头文件`<iostream>`中重载了这两个符号，所以`>>`成了流提取运算符，`<<`成了流插入运算符。

cin语法：
```c++
cin >> 变量1 >> 变量2 >> ... >> 变量n;
```
当连续从键盘读取数据时，以空格、制表符、回车符作为分隔符。
- 如果输入的内容是分隔符开头，cin会忽略并清除掉，直到读取你输入的内容。
- cin会识别遇到的第一个分隔符作为整个输入过程使用的分隔符。

cout语法：
```c++
cout << 表达式1 << 表达式2 << ... << 表达式n;
```
cout会自动判断输出数据的类型，按相应类型输出对应的数据，同一个cout支持多个不同类型的数据。

代码实践下：
```c++
#include <iostream>
#include <string>

using namespace std;

int main() {
    int int1, int2;
    char strArray[20];
    string str1 = strArray;
    double double1;
    char char1 = 'a';

    cout << "输入两个整型值，一个单字符，一个字符串和一个浮点值" << "以空格/Tab键/Enter键分隔: " << endl;
    cin >> int1 >> int2 >> char1 >> str1 >> double1;
    cout << "输入内容分别是: " << endl << int1 << endl << int2 << endl << char1 << endl << str1 << endl << double1 << endl;
    return 0;
}
```
其中
- `endl`是换行作用，在屏幕输出时还能避免出现`%`符号。

#### 二、头文件和命名空间
##### 头文件
- 每条`#include`指令仅可包含一个头文件。
- 系统提供的头文件一般不以`.h`结尾，C语言的头文件是以`.h`结尾。
- 使用尖括号括住系统提供的头文件：`#include <iostream>`，C++编译器首先在系统设定的目录中寻找要包含的文件，如果没有找到，再到指令中指定的目录中查找。
- 使用双引号括住自定义的头文件：`#include "e:\myprog\ex1.h"`，C++编译器在用户当前目录或指令中指定的目录下寻找要包含的文件。
- C++常见头文件：
	- 标准输入输出流：`<iostream>` [microsoft](https://learn.microsoft.com/zh-cn/cpp/standard-library/iostream?view=msvc-170)、[cppreference](https://zh.cppreference.com/w/cpp/header/iostream)。
	- 标准文件流：`<fstream>` [microsoft](https://learn.microsoft.com/zh-cn/cpp/standard-library/fstream?view=msvc-170)、[cppreference](https://zh.cppreference.com/w/cpp/header/fstream)。
	- 标准字符串处理函数：`<string>` [microsoft](https://learn.microsoft.com/zh-cn/cpp/standard-library/string?view=msvc-170)、[cppreference](https://zh.cppreference.com/w/cpp/header/string)。
	- 标准数学函数：`<cmath>` [microsoft](https://learn.microsoft.com/zh-cn/cpp/standard-library/cmath?view=msvc-170)、[cppreference](https://zh.cppreference.com/w/cpp/header/cmath)。

##### 命名空间
- `using namespace std;`表示使用命名空间`std`，这样就无需使用这种写法`std::cin`、`std::cout`、`std::endl`

命名空间语法格式：
```c++
namespace 命名空间名
{
	命名空间的各种声明(函数声明、类声明、...)
}
```
参考：
```c++
#include <ios>
#include <streambuf>
#include <istream>
#include <ostream>
 
namespace std {
  extern istream cin;
  extern ostream cout;
  extern ostream cerr;
  extern ostream clog;
 
  extern wistream wcin;
  extern wostream wcout;
  extern wostream wcerr;
  extern wostream wclog;
}
```

#### 三、强制类型转换运算符


#### 四、函数参数的默认值
默认值可以是常数，还可以是任何有定义的表达式，但禁止是函数内部的局部变量。

定义：
```c++
void func(int a=11, int b=22, int c=33)
{
}
```

声明和调用示例：
```c++
func(); // 对
func(55); // 对

void test1(int =2, double =3.0); // 对
void test2(int a, double b=3.0); // 对
void test3(int a=2, double b); // 错
void func1(int a, int b=2, int c=3); // 对
void func2(int a=1, int b, int c=3); // 错
void func3(int a=1, int b=2, int c); // 错

func1(1, 22, 33); // 对
func1(); // 错
func1(1, 29); // 对
func1(5, , 9); // 错

int Max(int m, int n);
int a, b;
void func4(int x, int y=Max(a, b), int z=a-b)
{
}

func4(4); // 对
func4(4, 9); // 对
```

#### 五、引用和函数参数的传递
简单总结：引用相当于变量的别名，别名和引用的变量实际是同一个首地址。

##### 引用的定义
定义格式：
```c++
类型名 &引用名 = 同类型的某个变量名;
```
例：
```c++
int int1;
int &int2 = int1; // int2 = 0
```

- 不能空引用，引用的变量必须初始化（引用必须指向某个已存在的内存区域的首地址）。C++98标准下，系统自动将`int1`，初始化为`0`。
- 不能声明引用的引用，`int &int3 = &int2`是错误的。
- 引用分为**常引用**和**普通引用**，不能通过常引用修改其引用的变量，而普通引用可以。

```c++
#include <iostream>
using namespace std;

int main()
{
	int int1 = 123;
	int &int2 = int1;
	int2 = 321;
	// 结果: int1: 321; int2: 321
	cout << "int1: " << int1 << "; int2: " << int2 << endl;

	const int &int3 = int2;
	//int3 = 456;//会报错error: assignment of read-only reference
	int1 = 123;
	// 结果: int1: 123; int2: 123; int3: 123
	cout << "int1: " << int1 << "; int2: " << int2 << "; int3: " << int3 << endl;
	return 0;
}
```

##### 引用在函数中的使用
定义：
```c++
void func1(int &int1, string &str1)
{
}
```

引用可以作为函数的返回值，例：
```c++
#include <iostream>
using namespace std;

int &refVal(int &x)
{
	return x;
}

int main()
{
	int int1 = 10;
	int int2 = 20;
	refVal(int1) = 40;
	refVal(int2) = 30;
	// 结果: int1: 40; int2: 30
	cout << "int1: " << int1 << "; int2: " << int2 << endl;
	return 0;
}
```

函数的返回值还可以是指针，这样的函数称为**指针函数**。格式如下：
```c++
数据类型 *函数名(形参列表){}
```
例：
```c++
#include <iostream>
using namespace std;

int *findLarger(int *num1, int *num2) {
    // *num1是取内存地址的值，num2是内存地址
    // 结果: *num1: 10; num2: 0x7fff363ab310
    cout << "num1: " << *num1 << "; num2: " << num2 << endl;
    if (*num1 > *num2) {
        return num1;
    } else {
        return num2;
    }
}

int main() {
    int a = 10;
    int b = 20;
    int *larger_ptr = findLarger(&a, &b);
    // 结果: 较大值为: 20
    cout << "较大值为: " << *larger_ptr << endl;
    return 0;
}
```

#### 六、const与指针共同使用
const是类型限定符的一种，其作用是限定访问权限。

[类型限定符](https://zh.cppreference.com/w/cpp/language/cv)：
- `const`，声明变量只读，变量的值不能被修改，例：`const int MAX_VALUE = 100;`。
- `volatile`，声明变量易变，变量的值可能在未知的情况下发生改变，防止编译器对该变量进行优化，例：`volatile int sensorValue;`。
- `mutable`，类中的成员变量可以在常量成员函数中被修改。
```c++
class example {
public:
	mutable int counter;
	void incrementCounter() const {
		counter++;
	}
}
```

`const`有三种使用情况：
1. `const`在符号`*`左侧，表示指针所指数据是常量，数据禁止由本指针改变，但可以通过其他方式修改。指针本身是变量，可以指向其他的内存地址。
```c++
string str0 = "hello";
const string *str1 = &str0;
cout << str1 << endl; // str1: 0x7ffc033659c0

string str2 = "world";
str1 = &str2; // 可以
cout << str1 << endl; // str1: 0x7ffc033659a0

//*str1 = "test"; // 不可以
```
2. `const`在符号`*`右侧，表示指针本身是常量，禁止本指针指向其他地址，指针所指的数据可以由本指针修改。
```c++
string str0 = "hello";
string *const str1 = &str0;
cout << *str1 << endl; // str1: hello

string str2 = "world";
//str1 = &str2; // 不可以

*str1 = "test"; // 可以
cout << *str1 << endl; // str1: test
```
3. 符号`*`左右都有`const`，表示指针本身和所指数据都是常量，禁止修改指针本身和修改指针所指数据。
```c++
string str0 = "hello";
const string *const str1 = &str0;
cout << *str1 << endl; // str1: hello

string str2 = "world";
//str1 = &str2; // 不可以
//*str1 = "test"; // 不可以
```

#### 七、内联函数
`inline`是声明内联函数。
```c++
#include <iostream>

// 内联函数定义
inline int max(int a, int b) {
    return (a > b) ? a : b;
}

int main() {
    int num1 = 10;
    int num2 = 20;

	// 在编译期间在这个调用点，将函数的代码插入到这个位置
    int maxValue = max(num1, num2); // 编译期间，此时并不会产生函数调用

    std::cout << "较大的数字是: " << maxValue << std::endl; // 此时才产生函数调用

    return 0;
}
```

#### 八、函数的重载
重载的目的：使用相同的函数名调用功能相似的函数，减少命名空间的浪费。

重载满足条件：
1. 形参表中对应的形参类型不同。
2. 形参表中形参个数不同。
```c++
#include <iostream>
#include <string>
using namespace std;

int bigger(int x, int y)
{
    cout << "int bigger: ";
    return x > y ? x : y;
}
float bigger(float x, float y)
{
    cout << "float bigger: ";
    return x > y ? x : y;
}
double bigger(double x, double y)
{
    cout << "double bigger: ";
    return x > y ? x : y;
}
double bigger(int x, double y)
{
    cout << "double2 bigger: ";
    return x + y;
}
string bigger(string x, string y)
{
    cout << "string bigger: ";
    return x > y ? x : y;
}
string bigger(string x, string y, string z)
{
    cout << "string2 bigger: ";
    return x + y > z ? 'yes' : 'no';
}

int main() {
    cout << bigger(1, 2) << endl; // int bigger: 2
    cout << bigger(1.1, 1.2) << endl; // double bigger: 1.2
    cout << bigger(1.1f, 1.2f) << endl; // float bigger: 1.2
    cout << bigger("a", "A") << endl; // string bigger: a
    cout << bigger("A", "B", "C") << endl; // string2 bigger: no
    cout << bigger(1, 1.2) << endl; // double2 bigger: 2.2
	return 0;
}
```

错误的重载情况
```c++
// 二义性情况
int sum(int x, int y, int z = 0);
int sum(int x, int y);
// 编译器不能确定你到底调用的是哪个函数
sum(1, 2);

// 形参类型和数量都一样，仅返回值不一样，也不足以编译器区分，会报错
int sum(int x, int y);
double sum(int x, int y);
```

#### 九、指针和动态内存分配
指针定义：
```c++
int a = 100;
int *p = &a; // *表示是一个指针，*p是指针变量
cout << p << endl; // 内存地址
cout << *p << endl; // 100
```

静态内存分配：编译时确定数组空间大小，例：`int arr[10];`
动态内存分配：内存分配是在程序运行期间进行的，运行期间才能确定占用内存的大小。
动态内存分配，语句格式：`p = new T`，p表示指针变量，T任意数据类型。
```c++
int *p;
p = new int; // 动态分配4个字节大小的内存空间
*p = 5; // *p向内存空间写入数值5

// 分配任意大小，语句格式 p = new T[n];
int *pArr;
int i = 5;
pArr = new int[i*20]; // 分配了100个元素的整型数组
pArr[100]; // 下标越界，超出了范围，编译不报错，也不会提示，运行和使用会得到意料之外的结果。
```
动态申请的内存空间需要再使用完以后释放，语法：
```c++
delete 指针;
```

`delete`不能用在非`new`动态申请的内存空间，编译过程不报错不提示，运行时报错。

`delete`释放掉指针所指向的空间后，再访问这个空间时，会得到意料之外的结果。

如果是一个动态分配的数组，语法：
```c++
delete []指针;
```
如果动态分配了一个数组，但却用delete 指针的方式，编译不报错不提示但实际没有被完全释放

#### 十、用string对象处理字符串
C++标准模板库中提供了string数据类型，专门用于处理字符串。

string是一个类，这个类型的变量称为“string对象”。

使用：
```c++
#include <iostream>
#include <string>
using namespace std;

int main()
{
	string str1;
	string city = "hello"; // h的下标是0
	char name[] = "world";
	string str2 = name;
	string citys[] = {"Beijing", "Shanghai", "Guangzhou"};
	cout << citys[0] << endl; // Beijing
	cout << sizeof(citys) / sizeof(string) << endl; // 数组元素个数
	return 0;
}
```

- `length()`和`size()`没有区别，都是获取字符长度，前者是后者的别名函数。
- string类型的字符串必须用双引号包起来，单引号只能用于单个字符。
- string对象之间可以用`<`、`<=`、`==`、`!=`、`>=`、`>`运算符进行比较，大小的判定标准是按字典序（基于ASCII码）进行的，而且是大小写相关的。
	- 例如：大写字母的ASCII码小于小写字母的ASCII码，`A`~`Z`的ASCII码是`0x41`~`0x5a`，`a`~`z`是`0x61`~`0x7a`，[ASCII码对照表_百度](https://baike.baidu.com/item/ASCII/309296?fr=ge_ala#3)、[ASCII 码表_cppreference](https://zh.cppreference.com/w/cpp/language/ascii)。
- string对象、字符之间可以使用`+`运算符进行拼接。

```c++
string str = "hello world";
char str1 = str[5];
char strArr1[] = "jayce";
cout << str + str1 + string(strArr1) << endl;
```

常用成员函数：

| 函数                                                  | 功能                                                           |
| --------------------------------------------------- | ------------------------------------------------------------ |
| `const char *c_str() const;`                        | 返回一个指向字符串的指针，字符串内容与本`string`串相同，用于将`string`转换为`const char *` |
| `int size() const;`                                 | 返回当前字符串的长度，一个汉字长度为2                                          |
| `int length() const;`                               | `size()`的别名函数                                                |
| `bool empty() const;`                               | 字符串判空                                                        |
| `size_type find(const char *str, size_type index);` | 返回`str`在字符串中第一次出现的位置（从`index`开始查找），如果没找到返回 -1                |
| `size_type find(char ch, size_type index);`         | 返回字符`ch`在字符串中第一次出现的位置（从`index`开始查找），如果没找到返回 -1               |
| `string &insert(int p, const string &s);`           | 在`p`位置插入字符串`s`                                               |
| `string &append(const char *s);`                    | 将字符串`s`连接到当前字符串的结尾处                                          |
| `string substr(int pos = 0, int n = npos) const;`   | 返回从`pos`开始的`n`个字符组成的字符串                                      |

使用示例：
```c++
#include <iostream>
#include <string>
#include <cstring> // strcpy

using namespace std;

int main()
{
	string str1 = "hello world";
	cout << "str1: " << str1.size() << endl;

	string str2;
	cout << "str2: " << str2 << endl;

	cout << "find: " << str1.find("o") << " " << str1.find("d", 11) << endl << string::npos << endl; // 实测并不是返回-1，而是18446744073709551615，也就是判断找没找到要用string::npos
	
	cout << "substr: " << str1.substr(0, 5) << endl;

	string str3 = "abc";
	char str4[20];
    strcpy(str4, str3.c_str());

	cout << "c_str: " << str4 << endl;


	string str5 = "hello";
	cout << "insert: " << str5.insert(5, " world") << endl;
	cout << "append: " << str5.append("! jayce") << endl;
	return 0;
}
```


## 第二章 面向对象的基本概念
### 第一节 结构化程序设计
略

### 第二节 面向对象程序设计的概念和特点
#### 一、面向对象思想的提出
略
#### 二、面向对象程序设计的特点
面向对象的程序设计有4个基本特点：
- 抽象
- 封装
- 继承
- 多态
### 第三节 类的初步知识
#### 一、类的定义

类是用户自定义的数据类型，定义类时系统并不会为类分配存储空间，而是把类看作是一种模板。

- 访问范围说明符，一共有3种：`public`、`private`、`protected`，在类中不限制出现顺序和出现次数。
- 在C++98标准下，类中声明的任何成员不能使用`auto`、`extern`、`register`关键字进行修饰。
- 类中的成员变量不能在声明时进行初始化。
- 类成员函数允许重载。

类定义语法格式：
```c++
class 类名
{
访问范围说明符:
	成员变量1
	成员变量2
	...
	成员函数声明1
	成员函数声明2
	...
...
}
```
成员函数既可以在类体内定义，也可以在类体外定义。

如果在类体内定义，默认是内联函数。
```c++
class Book
{
private:
	int page;
public:
	void nextPage(int p)
	{
		page = p;
	};
	void prevPage(int p); // 即使在体外定义也要先定义声明
};
```
在类体外定义，关键点给函数名加上`类名::`
```c++
void Book::prevPage(int p)
{
	page = p;
}
```

定义在哪里比较好？

答：函数体代码通常比较长，所以在类体内仅定义成员函数的原型，在体外定义函数体。

#### 二、类的定义示例
```c++
#include <iostream>
using namespace std;

class MyDate
{
public:
	MyDate(); // 构造函数
	MyDate(int, int, int); // 构造函数重载
	void setDate(int, int, int);
public:
	void setDate(MyDate);
    void print();
private:
	int year, month, day;
};
MyDate::MyDate()
{
	year = 1970, month = 1, day = 1;
}
MyDate::MyDate(int y, int m, int d)
{
	setDate(y, m, d);
}
void MyDate::setDate(int y, int m, int d)
{
	year = y; month = m; day = d;
	return;
}
void MyDate::setDate(MyDate theDate)
{
	setDate(theDate.year, theDate.month, theDate.day);
	return;
}
void MyDate::print()
{
    cout << year << '/' << month << '/' << day << endl;
    return;
}


int main() {
	// 创建类对象
    MyDate date1 = MyDate(), date2 = MyDate(2024, 3, 21), date3;
	
    date1.print();
    date2.print();
    date3.print();
    return 0;
}
```


### 第四节 类的示例程序剖析
#### 一、程序结构
略
#### 二、成员变量与成员函数的定义
- 成员变量一般均定义为私有访问权限。
- 使用类，类型定义的变量称为类的对象，例如：用string对象的变量。
- 每个对象都有各自的存储空间。
- 成员函数并非每个对象都各存一份，和普通函数一样，在内存中只有一份。

#### 三、创建类对象的基本形式

创建对象变量的两种基本方式：

方式一：
```c++
类名 对象名;
类名 对象名(参数);
类名 对象名 = 类名(参数);
类名 对象名1, 对象名2, ...;
类名 对象名1(参数), 对象名1(参数), ...;
```
创建对象后（运行时），C++会为它分配相应的空间，用来存储对象所有的成员变量，而类中定义的成员函数则被分配到存储空间中的一个公用区域，由该类的所有对象共享。

> 在编译阶段并不会分配内存。

方式二：
```c++
// A
类名 *对象指针名 = new 类名;
// B
类名 *对象指针名 = new 类名();
// C
类名 *对象指针名 = new 类名(参数);
```
用new创建对象时返回的是一个对象指针，这个指针指向本类刚创建的这个对象。C++分配给指针的仅仅是存储指针值的空间，而对象所占用的空间分配在堆上。

> 使用new创建的对象，必须用delete来删除。

- 用A方法创建的对象时，调用无参的构造函数，如果这个构造函数是由编译器为类提供的，则类中成员变量不进行初始化。
- 用B方法，和上面一样，区别在类中的成员变量会进行初始化。

和基本数据类型一样，还可以声明对象的引用、对象的指针、对象的数组：
```c++
类名 &对象引用名 = 对象;

类名 *对象指针名 = 对象的地址;

类名 对象数组名[数组大小];
```

### 第五节 访问对象的成员
#### 一、使用对象访问成员变量与调用成员函数
```c++
对象名.成员变量名
对象名.成员函数名(参数表)
```


#### 二、使用指针访问对象的成员
除了使用`.`的方式，还可以使用指针或引用的方式来访问类成员，如果通过指针访问，将`.`换成`->`。
```c++
int main()
{
    // 创建类对象
    MyDate date1;

    MyDate *p = new MyDate(); // 使用 new 关键字动态分配内存并将地址赋给指针
    MyDate *p2 = &date1; // p2 指针指向 date1

    p->print();
    p2->print();

    delete p; // 释放动态分配的内存

    return 0;
}
```

#### 三、使用引用访问对象的成员

### 第六节 类成员的可访问范围
#### 一、访问范围说明符的含义
访问修饰符：
- `public`公有
- `private`私有
- `protected`保护

3种关键字出现的次数和先后次序都没有限制。

如果类中某个成员（变量/函数）没有访问范围说明符，默认是私有成员。

```c++
class A
{
	int m, n;
public:
	int a, b;
	int func1();
private:
	int c, d;
	void func2();
public:
	char e;
	int f;
	int func3();
}
```
- 公有成员变量有`a`、`b`、`e`、`f`，公有成员函数有`func1`、`func3`。
- 私有成员变量有`m`、`n`、`c`、`d`，私有成员函数有`func2`。

#### 二、成员的访问
#### 三、隐藏的作用

设置私有成员的机制叫作”隐藏“。

目的是强制对私有成员变量的访问一定要通过公有成员函数进行。

好处是成员变量类型变动时，只需要更改成员函数即可，否则所有直接访问成员变量的语句都需要修改。

### 第七节 标识符的作用域与可见性
#### 函数原型作用域
#### 局部作用域
#### 类作用域
#### 命名空间作用域

## 第三章 类和对象进阶
### 第一节 构造函数
#### 一、构造函数的作用
用于给对象进行初始化，实际上是用来为成员变量赋值初值的。

构造函数由程序员编写，如果程序员未编写，由系统自动添加一个不带参数的构造函数。

在对象生成时（任何方式声明/定义时），系统自动调用构造函数，程序员无需主动调用。
#### 二、构造函数的定义
在类体外的三种定义形式：
```c++
class 类名
{
private:
	x1,
	x2,
	...
	xn
}

// 形式一
类名::类名(形参1, 形参2, ..., 形参n): x1(形参1), x2(形参2), ..., xn(形参n) {}

// 形式二
类名::类名(形参1, 形参2, ..., 形参n)
{
	x1 = 形参1;
	x2 = 形参2;
	...
	xn = 形参n;
}

// 形式三
类名::类名()
{
	x1 = 初始化表达式1;
	x2 = 初始化表达式2;
	...
	xn = 初始化表达式n;
}
```

方式一中的`x1(形参1), x2(形参2), ..., xn(形参n)`称为**构造函数初始化列表**。

实例一：
```c++
class Math
{
protected:
	int k;
public:
	Math(int n=5): k(n)
	{
		cout << k << endl;
	}
};

Math test(3); // 输出3
```

实例二：
```c++
class MyDate
{
public:
	MyDate(); // 构造函数
	void setDate(int, int, int);
public:
	void setDate(MyDate);
    void print();
private:
	int year, month, day;
};
MyDate::MyDate(): year(1970), month(1), day(25) {}
MyDate::MyDate(int d): year(1970), month(1)
{
	day = d;
}
MyDate::MyDate(int m, int d): year(1970)
{
	month = m;
	day = d;
}
MyDate::MyDate(int y, int m, int d)
{
	year = y;
	month = m;
	day = d;
}
```

错误示例：
```c++
// 示例一，编译错误，函数重复定义，函数参数列表式相同的
myDate::myDate(int d): year(1970), month(1)
{
	day = d;
}
myDate::myDate(int d): year(1970), day(1)
{
	month = d;
}

// 示例二，编译不提示错误，参数表带入的实参值为最终值
myDate::myDate(int d): year(1970), day(1)
{
	day = d;
}
```

#### 三、构造函数的使用
默认值
```c++
class A
{
	int a, b;
public:
    A(int, int);
};
A::A(int k, int j = 2)
{
	a = k;
	b = j;
    cout << "a:" << a << ";b:" << b << endl;
};
A a(3); // a:3;b:2
```

使用构造函数创建对象指针
```c++
MyDate *pd = new MyDate();
MyDate *pd = new MyDate;
```
使用new创建对象时，加不加括号都可以，但处理上有差异。
- 如果有自定义构造函数，都调用构造函数进行初始化。
- 如果类中没有自定义构造函数。
	- 不加括号时，系统只为成员变量分配内存空间，不进行内存的初始化，成员变量的值是随机值。
	- 加了括号时，系统在为成员变量分配内存的同时，将其初始化为0。

```c++
// 对象数组
MyDate A[3]; // 3个元素均调用了无参数的构造函数
MyDate A[3] = {MyDate(1), MyDate(10, 25), MyDate(1980, 9, 10)}; // 调用有参数构造函数
```

看看调用了多少次构造函数？
```c++
AB a(4), b[3], *p;
```
共4次，创建a对象时1次，创建b对象数组时3次，而指针p仅是个声明，并不会调用构造函数。

### 第三节 类的静态成员
#### 一、静态变量
#### 二、类的静态成员
```c++

```

## 第五章 类的继承与派生
### 一、继承的概念
通过已有的类建立新类的过程，叫作类的派生。

原来的类称为基类/父类/一般类，新类称为派生类/子类/特殊类。

派生类派生自基类，或继承与基类，也可以说基类派生了派生类。

若派生类中定义了一个与基类同名的成员，成员可以是变量/函数，是允许的：
- 覆盖（重定义/重写）：在派生类的成员函数中访问这个同名成员（或者通过派生类对象访问）。
- 隐藏（函数重定义/同名隐藏），对于成员函数，派生类既继承了基类的同名成员函数，又在派生类中重写了这个成语函数。
	- 隐藏是指使用派生类对象调用这个成语函数时，调用的是派生类的函数，隐藏了基类的成员函数。

### 二、派生类的定义与大小
#### 派生类的定义
```c++
// 定义
class 派生类名:继承方式说明符 基类名
{
	类体
}

// 实例
class BaseClass // 基类
{
	int v1, v2;
};
class DerivedClass:public BaseClass // 派生类
{
	int v3;
}
```

继承方式说明符是指如何控制基类成员在派生类中的访问属性，有3种：
- `public`公有继承，常用，基类的私有成员不可访问外，公有和保护成员均可访问。
- `private`私有继承，基类的私有成员不可访问，公有和保护成员以私有成员出现在派生类中。
- `protected`保护继承，基类的私有成员不可访问，公有和保护成员以保护成员出现在派生类中。

##### 改变基类成员的访问权限
```c++
class BaseClass
{
public:
	int v1, v2;
	BaseClass(): v1(1), v2(1) {}
	int temp1() {}
};
class DerivedClass:public BaseClass
{
// 变成私有成员了
	int v1;
	int temp1(){}
public:
	DerivedClass(): v1(10) {}
	void printv() {}
}
```

#### 类的大小
派生类对象占用的存储空间大小 = 基类成员变量占用的存储空间大小 + 派生类对象自身成员占用的存储空间大小。

为变量分配内存时，会进行边界对齐。

> 常见内存对齐是4字节、8字节。
> - 在32位系统，通常以4字节为基本单位对齐。
> - 在64位，通常以8字节。

```c++
class BaseClass
{
	int v1, v2;
	char v4;
public:
	int temp1() {}
};
class DerivedClass:public BaseClass
{
	int v3;
	int *p;
public:
	int temp(){}
};

sizeof(BaseClass); // 12
sizeof(DerivedClass); // 24
```
- 基类占用9个字节（4+4+1），内存对齐分配12个字节。
	- int占4个字节 × 2
	- char占1个字节
- 派生类占用24个字节（12+4+8），正好对齐。
	- int × 1
	- 指针变量64位系统下占8个字节（32位系统减半）。

### 三、继承关系的特殊性
- 如果基类中有友元类 / 友元函数，派生类不会继承这个友元关系。
- 如果基类是某个类的友元，友元关系可被继承。

```c++
#include <iostream>
using namespace std;
class another;
class Base
{
private:
	float x;
public:
	void print(const another &K);
};
class Derived:public Base
{
private:
	float y;
};
class another
{
private:
	int aaa;
public:
	another()
	{
		aaa = 100;
	}
	friend void Base::print(const another &K);
};
void Base::print(const another &K)
{
	cout << "Base:" << K.aaa << endl;
}
int main()
{
	Base a;
	Derived d;
	another ano;  // aaa 初始化100
	a.print(ano); // Base:100
	d.print(ano); // Base:100
	return 0;
}
```