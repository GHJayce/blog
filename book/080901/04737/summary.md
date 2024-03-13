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