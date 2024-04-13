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
- [第七章 输入/输出流](#第七章-输入输出流) <small>P276 ~ 297（11页）</small>
- [第八章 文件操作](#第八章-文件操作) <small>P302 ~ 324（12页）</small>
- [第九章 函数模板与类模板](#第九章-函数模板与类模板) <small>P329 ~ 348（10页）</small>
- [考试重点](#考试重点)

## 前言
### hello world
创建文件`hello.cpp`，编写代码：
```cpp
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
```cpp
cin >> 变量1 >> 变量2 >> ... >> 变量n;
```
当连续从键盘读取数据时，以空格、制表符、回车符作为分隔符。
- 如果输入的内容是分隔符开头，cin会忽略并清除掉，直到读取你输入的内容。
- cin会识别遇到的第一个分隔符作为整个输入过程使用的分隔符。

cout语法：
```cpp
cout << 表达式1 << 表达式2 << ... << 表达式n;
```
cout会自动判断输出数据的类型，按相应类型输出对应的数据，同一个cout支持多个不同类型的数据。

代码实践下：
```cpp
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
```cpp
namespace 命名空间名
{
	命名空间的各种声明(函数声明、类声明、...)
}
```
参考：
```cpp
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
当不同类型的量进行混合算术运算时，系统自动进行合理的类型转换，比如：
```cpp
int k;
double j=5;
```

`5*k+j`的值是什么类型？

答：根据C++的类型提升规则，当一个 `int` 类型的值和一个 `double` 类型的值进行算术运算时，`int` 类型的值会被提升（promoted）到 `double` 类型，以便两个操作数类型一致。这是因为 `double` 类型比 `int` 类型更宽泛（numerically wider），所以可以容纳 `int` 类型的值而不会丢失信息。所以值是double类型，`5*k`被提升为double类型和`j`进行相加。

也可以在程序中使用强制类型转换运算符，可以用`static_cast`或者`const_cast`进行转换。

`static_cast`用于将一种数据类型转换成另一种数据类型，格式如下：
```cpp
static_cast<类型名>(表达式)
```
`static_cast`也可以省略，有4种写法：
```cpp
int1 = static_cast<int>(double1);
int2 = int(double1); // 强制类型转换运算符的新写法
int2 = (int)double1; // 旧写法
int2 = double1; // 自动类型转换
```

`const_cast`用于去除指针和引用的常量性，但不能去除变量的常量性，格式如下：
```cpp
const_cast<类型名>(表达式)
```
功能是：
- 将常量指针转化成非常量的指针，并且仍然指向原来的对象；
- 或是将常量引用转换成非常量的引用，并且仍然指向原来的对象。

#### 四、函数参数的默认值
默认值可以是常数，还可以是任何有定义的表达式，**但禁止是函数内部的局部变量**。

C++语言规定，有默认值的形参必须在形参列表的最后。
- 如果某个形参没有默认值，则它左侧的所有形参都不能有默认值。
- 在有默认值的形参的右侧，不能出现无默认值的形参。

**在声明函数原型时，可以省略形参的名字，可以只给出它的类型及默认值**。

定义：
```cpp
// a、b、c都称作形参
// func(形参列表)
void func(int a=11, int b=22, int c=33)
{
}
```

声明和调用示例：
```cpp
func(); // 对
func(55); // 对

void test1(int =2, double =3.0); // 对
void test2(int a, double b=3.0); // 对
void test3(int a=2, double b); // 错
void func1(int a, int b=2, int c=3); // 对
void func2(int a=1, int b, int c=3); // 错
void func3(int a=1, int b=2, int c); // 错

// 1、22、33这些都称作实参
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
```cpp
类型名 &引用名 = 同类型的某个变量名;
```
例：
```cpp
int int1;
int &int2 = int1; // int2 = 0
```

- 不能空引用，引用的变量必须初始化（引用必须指向某个已存在的内存区域的首地址）。C++98标准下，系统自动将`int1`，初始化为`0`。
- 不能声明引用的引用，`int &int3 = &int2`是错误的。
- 引用分为**常引用**和**普通引用**，不能通过常引用修改其引用的变量，而普通引用可以。

```cpp
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
```cpp
void func1(int &int1, string &str1)
{
}
```

引用可以作为函数的返回值，例：
```cpp
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
```cpp
数据类型 *函数名(形参列表){}
```
例：
```cpp
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
```cpp
class example {
public:
	mutable int counter;
	void incrementCounter() const {
		counter++;
	}
}
```

`const`有三种使用情况：
1. `const`在符号`*`左侧，表示指针所指数据是常量，数据禁止由本指针改变，但可以通过其他方式修改。指针本身是变量，可以指向其他的内存地址。**简记数据是常量**。
```cpp
string str0 = "hello";
const string *str1 = &str0;
cout << str1 << endl; // str1: 0x7ffc033659c0

string str2 = "world";
str1 = &str2; // 可以
cout << str1 << endl; // str1: 0x7ffc033659a0

//*str1 = "test"; // 不可以
```
2. `const`在符号`*`右侧，表示指针本身是常量，禁止本指针指向其他地址，指针所指的数据可以由本指针修改。**简记指针是常量**。
```cpp
string str0 = "hello";
string *const str1 = &str0;
cout << *str1 << endl; // str1: hello

string str2 = "world";
//str1 = &str2; // 不可以

*str1 = "test"; // 可以
cout << *str1 << endl; // str1: test
```
3. 符号`*`左右都有`const`，表示指针本身和所指数据都是常量，禁止修改指针本身和修改指针所指数据。**简记指针和数据都是常量**。
```cpp
string str0 = "hello";
const string *const str1 = &str0;
cout << *str1 << endl; // str1: hello

string str2 = "world";
//str1 = &str2; // 不可以
//*str1 = "test"; // 不可以
```

#### 七、内联函数
`inline`是声明内联函数。
```cpp
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
重载的目的：**使用相同的函数名调用功能相似的函数，减少命名空间的浪费**。

重载满足条件：
1. 形参表中对应的形参类型不同。
2. 形参表中形参个数不同。
```cpp
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
```cpp
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
```cpp
int a = 100;
int *p = &a; // *表示是一个指针，*p是指针变量
cout << p << endl; // 内存地址
cout << *p << endl; // 100
```

静态内存分配：编译时确定数组空间大小，例：`int arr[10];`
动态内存分配：内存分配是在程序运行期间进行的，运行期间才能确定占用内存的大小。
动态内存分配，语句格式：`p = new T`，p表示指针变量，T任意数据类型。
```cpp
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
```cpp
delete 指针;
```

`delete`不能用在非`new`动态申请的内存空间，编译过程不报错不提示，运行时报错。

`delete`释放掉指针所指向的空间后，再访问这个空间时，会得到意料之外的结果。

如果是一个动态分配的数组，语法：
```cpp
delete[] 指针;
```
如果动态分配了一个数组，但却用delete 指针的方式，编译不报错不提示但实际没有被完全释放

#### 十、用string对象处理字符串
C++标准模板库中提供了string数据类型，专门用于处理字符串。

string是一个类，这个类型的变量称为“string对象”。

使用：
```cpp
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

```cpp
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
```cpp
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
面向对象技术把问题看成是相互作用的事物的集合，也就是对象的集合。

对象具有两个特性：
1. 状态，指对象本身的信息，也称为属性。
2. 行为，指对对象的操作。

对象是类的一个具象，类是对象的一个抽象。
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
```cpp
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
```cpp
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
```cpp
void Book::prevPage(int p)
{
	page = p;
}
```

定义在哪里比较好？

答：函数体代码通常比较长，所以在类体内仅定义成员函数的原型，在体外定义函数体。

#### 二、类的定义示例
```cpp
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
```cpp
类名 对象名;
类名 对象名(参数);
类名 对象名 = 类名(参数);
类名 对象名1, 对象名2, ...;
类名 对象名1(参数), 对象名1(参数), ...;
```
创建对象后（运行时），C++会为它分配相应的空间，用来存储对象所有的成员变量，而类中定义的成员函数则被分配到存储空间中的一个公用区域，由该类的所有对象共享。

> 在编译阶段并不会分配内存。

方式二：
```cpp
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
```cpp
类名 &对象引用名 = 对象;

类名 *对象指针名 = 对象的地址;

类名 对象数组名[数组大小];
```

### 第五节 访问对象的成员
#### 一、使用对象访问成员变量与调用成员函数
```cpp
对象名.成员变量名
对象名.成员函数名(参数表)
```

#### 二、使用指针访问对象的成员
除了使用`.`的方式，还可以使用指针或引用的方式来访问类成员，**如果通过指针或引用访问，将`.`换成`->`**。
```cpp
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
略。

### 第六节 类成员的可访问范围
#### 一、访问范围说明符的含义
访问修饰符：
- `public`公有
- `private`私有
- `protected`保护

3种关键字出现的次数和先后次序都没有限制。

如果类中某个成员（变量/函数）没有访问范围说明符，默认是私有成员。

```cpp
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
- 使用public修饰的成员，既可以在本类内和主函数内访问，也可以在其他类中访问，只要定义了该类的对象即可。
- 使用private修饰的成员，仅可以在本类内访问。
- 使用protected修饰的成员，既可以在本类内访问，也可以在子类内访问。

#### 三、隐藏的作用

设置私有成员的机制叫作”隐藏“。

目的是强制对私有成员变量的访问一定要通过公有成员函数进行。

好处是成员变量类型变动时，只需要更改成员函数即可，否则所有直接访问成员变量的语句都需要修改。

### 第七节 标识符的作用域与可见性
在不同的作用域声明的标识符，可见性有如下规则：
1. 标识符要声明在前，引用在后。
2. 在同一个作用域中，不能声明同名的标识符，在没有互相包含关系的不同作用域中声明的同名标识符，互不影响。
3. 如果存在两个或多个具有包含关系的作用域。
	- 在外层声明了一个标识符，而内层没有声明同名的标识符，那么此时外层的标识符对内层可见。
	- 如果在内层声明了一个和外层同名的标识符，则外层的标识符对内层不可见，称为内层标识符隐藏了外层同名标识符，这种机制称为**隐藏规则**，举个例子：。

作用域隐藏规则
```cpp
#include <iostream>
using namespace std;
int main()
{
	int a = 1;
	cout << a <<endl; // 1
	for (int i = 0; i < 2; i++)
	{
		int a = 2;
		cout << a << "\n";
	}
	cout << a <<endl; // 1
}
```

#### 函数原型作用域
在声明函数原型时形参的作用范围就是函数原型作用域，**这是C++程序中最小的作用域**，例：
```cpp
double ares(double radius);
```
为什么是最小的呢？因为标识符radius的作用范围就只在函数ares形参列表里，即左右括号之间。

#### 局部作用域
程序中使用一对花括号括起来的一段程序称为块，作用范围就是在块内，称为局部作用域。

#### 类作用域
类X的成员m具有类作用域，对m的访问有3种：
1. 在类X的成员函数中没有声明与m同名的变量（局部作用域标识符），那在该函数内可以直接访问成员m。
2. 在类体外，不违反m的访问修饰符限定的情况下，可以通过`x.m`，x是类X的对象，或者`X::m`。
3. 在类体外，不违反m的访问修饰符限定的情况下，可以通过`ptr->m`这样的表达式来访问，ptr为指向类X的一个对象的指针。

#### 命名空间作用域
略

## 第三章 类和对象进阶
介绍的内容：
- 构造函数
- 析构函数
- 类中特殊的成员
- 重载的成员函数
- this指针
- 友元概念

### 第一节 构造函数
#### 一、构造函数的作用
用于给对象进行初始化，主要是为成员变量赋值初始值的。

> 程序中涉及到的基本数据类型的变量都需要先声明并初始化，然后再使用，这样能保证变量的值是确定的。
> 
> 在C++的基本数据类型的变量声明中，分为全局变量和函数内部的局部变量（类中局部变量亦同）。
> - 全局变量，如果程序员仅声明没初始化，系统会在程序启动时自动为其初始化为0。
> - 局部变量，系统不进行自动初始化，它的值必须要程序员给定，否则将是一个随机值。

构造函数由程序员编写，如果程序员未编写，由系统自动添加一个不带参数的构造函数。

在对象生成时（任何方式声明/定义时），系统自动调用构造函数，程序员无需主动调用。
#### 二、构造函数的定义
在类体外的三种定义形式：
```cpp
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
```cpp
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
```cpp
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
```cpp
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
对象需要占据内存空间，创建对象时，为对象分配的内存空间的初始化由构造函数完成。

默认值
```cpp
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
```cpp
MyDate *pd = new MyDate();
MyDate *pd = new MyDate;
```
使用new创建对象时，加不加括号都可以，但处理上有差异。
- 如果有自定义构造函数，都调用构造函数进行初始化。
- 如果类中没有自定义构造函数。
	- 不加括号时，系统只为成员变量分配内存空间，不进行内存的初始化，成员变量的值是随机值。
	- 加了括号时，系统在为成员变量分配内存的同时，将其初始化为0。

> 实测new创建对象，加不加括号结果都一样（没有自定义构造函数的情况），在分配内存的同时并初始化为0，书上讲的知识点估计是以前编译器的特性，和现在不一样了。


```cpp
// 对象数组
MyDate A[3]; // 3个元素均调用了无参数的构造函数
MyDate A[3] = {MyDate(1), MyDate(10, 25), MyDate(1980, 9, 10)}; // 调用有参数构造函数
```

看看调用了多少次构造函数？
```cpp
AB a(4), b[3], *p;
```
共4次，创建a对象时1次，创建b对象数组时3次，而指针p仅是个声明，并不会调用构造函数。

#### 四、复制构造函数与类型转换构造函数
##### 复制构造函数
复制构造函数是构造函数的一种，也称为拷贝构造函数，是一种特殊的构造函数，作用是创建对象时用一个已存在的对象，用它的成员变量的值去初始化正在创建的这个对象。

复制构造函数**只有一个参数**，**参数的类型是本类的引用**，参数可以是const引用，也可以是非const引用。

一个类可以写两个复制构造函数，定义如下：
```cpp
A::A(const A&)

A::A(A &)
```

- 如果你没有定义复制构造函数（非const的），编译器会自动生成一个非const引用的复制构造函数，如果你用了const引用的方式，但没有定义const引用的复制构造函数，编译器会报错。
- 如果你定义了复制构造函数，编译器只调用你定义的复制构造函数，当然函数内部的实现由你自己决定。

```cpp
class TestA
{
public:
    int a;
    int b;
    void print();
};

void TestA::print()
{
    cout << "a:" << a << "; b:" << b << endl;
}

TestA varA;
varA.a = 1;
varA.b = 2;
varA.print(); // a:1; b:2

TestA varB;
varB.print(); // a:418708000; b:32765

TestA varC(varA);
varC.print(); // a:1; b:2

/* ================== */
class TestB
{
public:
    int a;
    int b;
    void print();
    void print() const;
    TestB(const TestB &); // 一旦定义了复制构造函数
    TestB() // 无参的构造函数也必须要定义，编译器不会替你生成
    {
        a = 123;
        b = 456;
    }
};

TestB::TestB(const TestB &obj)
{
    a = obj.a;
}

void TestB::print()
{
    cout << "a:" << a << "; b:" << b << endl;
}

void TestB::print() const
{
    cout << "a:" << a << "; b:" << b << endl;
}

const TestB varD; // const对象只能调用const成员函数，non-const对象只能调用non-const成员函数
varD.print(); // a:123; b:456
TestB varE(varD);
varE.a = 456;
varE.print(); // a:456; b:0
```

自动调用复制构造函数的3种情况：
1. 用一个对象去初始化另一个对象时
```cpp
类名 对象名2(对象名1);
// or
类名 对象名2 = 对象名1;
```
2. 作为函数参数时，在调用函数时，会调用实参的复制构造函数来初始化形参，即实参内存地址是A，形参从实参那复制一份数据出来，形参的内存地址是B。
```cpp
// 作为函数形参时，会调用一次复制构造函数，可以对比复制构造函数的类和obj的地址的变化
void func(TestB obj)
{
    obj.b = 789;
    cout << "func_obj(" << &obj << "): ";
    obj.print();
}
```
3. 作为函数返回时，和作为函数参数一样，返回时会调用一次复制构造函数。
```cpp
TestB func(TestB obj)
{
	return obj;
}
```

##### 类型转换构造函数
如果构造函数只有一个参数，则可以看作是类型转换构造函数，作用是进行类型的自动转换。
```cpp

#include <iostream>
#include <string>

using namespace std;

class Demo
{
    int id;
public:
    Demo(int i)
    {
        id = i;
        cout << "id=" << id << "构造函数" << endl;
    }
    void printDemo();
    ~Demo()
    {
        cout << "id=" << id << "析构函数" << endl;
    }
};

void Demo::printDemo()
{
    cout << "id=" << id << endl;
}

int main()
{
    Demo d4(4);
    d4.printDemo();
    d4 = 6;
    d4.printDemo();
    return 0;
}
```
输出结果：
```
id=4构造函数
id=4
id=6构造函数
id=6析构函数
id=6
id=6析构函数
```

### 第二节 析构函数
和构造函数一样，析构函数也是成员函数的一种，**它的名字和类名相同，区别是要在类名前面加`~`字符**。

**析构函数没有参数，也没有返回值。**

**一个类中有且仅有一个析构函数，如果没有定义析构函数，编译器自动生成默认的析构函数，默认的析构函数体为空。**

**析构函数不可以超过1个，不会有重载的析构函数**。

### 第三节 类的静态成员
#### 一、静态变量
什么是全局变量和局部变量？
- 全局变量是指在花括号之外声明的变量，作用域范围是全局可见的，即在整个项目文件内都有效。
- 局部变量是指在块内定义，即花括号内定义的变量。

静态变量分为2种：
1. 静态全局变量，使用static修饰的全局变量是静态全局变量。作用域范围是，在定义该变量的源文件内有效，项目中的其他源文件中不能使用它。
2. 静态局部变量，使用static修饰的局部变量是静态局部变量，**具有局部作用域，但却有全局生存期，即静态局部变量在程序的整个运行期间都存在，它占据的空间一直到程序结束时才释放**。

静态变量均存储在全局数据区，静态局部变量只执行一次初始化，如果程序未显示给出初始值，则相当于初始化为0，如果显示给出初始值，则在该静态变量所在块第一次执行时完成初始化。

#### 二、类的静态成员
类的静态成员有2种：
1. 静态成员变量
2. 静态成员函数

在类体内定义类的成员时，在前面加上`static`关键字就是静态成员了。

类的静态成员被类的所有对象共享，不论有多少对象存在，静态成员都只有一份保存在公用内存中，对于静态成员变量，各对象看到的值都是一样的。

定义静态成员注意点：
- **在类体内先声明静态变量，然后再到类体外定义静态变量的初始值，不能直接在类体内赋值**。
- 在类体外定义静态成员变量初值时，前面不能加`static`关键字，避免和一般的全局静态变量混淆，类体外静态成员函数也一样。

访问类静态成员有3种方式（注意区别于访问类成员仅能使用对象名或者对象指针作为前缀是不一样的）：
```cpp
类名::静态成员名
// or
类名.静态成员名
// or
对象指针->静态成员名
```

### 第四节 变量及对象的生存期和作用域
略，后补
#### 一、变量的生存期和作用域
#### 二、类对象的生存期和作用域


### 第五节 常量成员和常引用成员
`const`的修饰的含义是常量，作用是定义了以后不能再修改。

在类中，可以用`const`关键字定义成员变量、成员函数和类的对象。

**类的常量成员变量必须进行初始化，而且只能在构造函数的成员初始化列表的方式进行**，例如：
```cpp
class A
{
	const int x;
	const int y;
public:
	A(): x(100), y(200)
	{
	}
}
```

使用const修饰的函数称为常量函数，格式如下：
```cpp
返回类型 函数名(形参表) const;
```

定义类的对象时，前面加`const`，则对该对象称为常量对象，例如：
```cpp
const A a(); // a就是常量对象
```
常量对象只能调用常量函数，而且常量对象中的各个属性值均不能修改。


### 第六节 成员对象和封闭类
一个类的成员变量如果是另一个类的对象，则该成员变量称为“**成员对象**”，**包含成员对象的类称为封闭类**，这两个类为包含关系。

例如：
```cpp
class B
{
};
class A
{
	B b1;
};
class C
{
public:
	B print();
};
```
类A、类C都是封闭类。类C是因为有函数的返回值是类B（另一个类）。

#### 一、封闭类构造函数的初始化列表
如例子：
```cpp

```

#### 二、封闭类的复制构造函数
```cpp

```

### 第七节 友元
#### 一、友元
#### 二、友元函数
#### 三、友元类
假如声明了类B为类A的友元类，即类B是一个友元类，那么类B中的所有函数都是类A的友元函数，即类B的所有成员函数都可以访问类A的所有成员。

声明格式：
```cpp
friend class 类名;
```

注意点：
- 友元类的关系是单向的，还是上面的例子，类B是类A的友元类，但类A不是类B的友元类。
- 友元类的关系不能传递，即类B是类A的友元类，类C是类B的友元类，不等于类C是类A的友元类。

来个例子，方便理解：
```cpp
class C
{
}
class B
{
	friend class C;
}
class A
{
	friend class B;
}
```
- 类B的所有成员函数可以访问类A的所有成员，类C的所有成员函数可以访问类B的所有成员，但类C不能访问类A的所有成员。
- 类A不能访问类B、类C的所有成员。

> 除非确有必要，一般不把整个类声明为友元类，而仅把类中的某些成员函数声明为友元函数。

### 第八节 this指针
略


## 第四章 运算符重载
### 第四节 重载强制类型转换运算符
强制转换运算符是单目运算符，可以被重载，但只能重载为成员函数，不能重载为全局函数。

例如：`(类型名) 对象`等价于`对象.operator 类型名()`，即变成对运算符函数的调用。

```cpp
#include <iostream>
using namespace std;

class Complex
{
private:
    double real, image;
public:
    Complex(double r = 0, double i = 0): real(r), image(i)
    {
    }
    operator double()
    {
        cout << "调用了强制类型转换重载: double()" << endl;
        return real;
    }
};

int main()
{
    Complex a(1.2, -3.4);
    cout << (double)a << endl; // 1.2
    double n = 12 + a;
    cout << n << endl; // 13.2
    return 0;
}
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
```cpp
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
```cpp
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

```cpp
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

```cpp
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

## 第六章 多态与虚函数
### 第一节 多态的基本概念
#### 一、多态
基类和派生类都有同名虚函数时，通过基类的指针或引用调用：
- 如果基类指针指向的是基类对象，执行的就是基类的虚函数。
- 如果基类指针指向的是派生类对象，执行的就是派生类的虚函数，这就叫多态。
#### 二、虚函数
先来看一个例子：
```cpp
#include <iostream>
using namespace std;

class A {
public:
    void foo() {
        cout << "A::foo()" << endl;
    }
};

class B : public A {
public:
    void foo() {
        cout << "B::foo()" << endl;
    }
};

int main() {
    A *ptr = new B(); // 指向派生类对象的基类指针
    ptr->foo(); // 输出：A::foo()
    delete ptr;

    A a = B();
    a.foo(); // 输出：A::foo()
    return 0;
}
```

如果想要调用的是派生类类B的同名函数要怎么做到？答案就是虚函数。

修改上面的例子如下：
```cpp
#include <iostream>
using namespace std;

class A {
public:
    // 声明
    virtual void foo();
};
// 定义
void A::foo()
{
	cout << "A::foo()" << endl;
}

class B : public A {
public:
    virtual void foo() {
        cout << "B::foo()" << endl;
    }
};

int main() {
    A *ptr = new B();
    ptr->foo(); // 输出：B::foo()
    delete ptr;

    A a = B();
    a.foo(); // 输出：A::foo()
    return 0;
}

```

没错，区别就是在函数前面增加了`virtual`关键字，声明为虚函数。

这样实现了等到运行时根据引用或指针实际指向的对象来确定调用的版本，这叫动态多态。而不是在编译阶段就确定好了指向，即使在运行时也是固定的，不会发生变化，这叫静态绑定。

虚函数使用注意点：
1. 虚函数声明为内联函数不会引起错误，但内联函数是在编译阶段进行静态处理的，而虚函数的调用是动态绑定的，**所以虚函数一般不声明为内联函数**。
2. 派生类重写基类的虚函数实现多态，**要求函数名、参数列表及返回值类型要完全相同**。
3. **基类中定义了虚函数，在派生类中该函数始终保持虚函数的特性**，派生类中的同名函数虽然可以不用加`virtual`关键字，但为了代码清晰和可读，推荐显示声明。
4. **只有类的非静态成员函数才能定义为虚函数**，静态成员函数和友元函数不能定义为虚函数。
5. **虚函数定义在体外，只需在声明函数时添加`virtual`关键字，定义时不加**。
6. **构造函数不能定义为虚函数**。最好也不要将`operator =`定义为虚函数，避免使用时容易混淆。
7. **不要在构造函数和析构函数中调用虚函数**。因为对象是不完整的，可能会出现未定义的行为。
8. **最好将基类的析构函数声明为虚函数**。

哪些函数不能是虚函数？为什么？
1. **全局函数（非成员函数）**，虚函数是为了与继承机制配合实现多态的，而全局函数（非成员函数）不属于某个类，没有继承关系，只能被重载，不能被覆盖，声明为虚函数也起不到多态的作用。因为编译器会在编译时绑定全局函数。
2. **静态成员函数**，所有的对象都共享，都能使用，不归某个对象所有，没有动态绑定的必要性。
3. **内联成员函数**，内联函数的目的是在代码中展开，减少函数调用的开销，是静态的，虚函数的目的是继承对象后能够动态绑定函数，两个不同的概念，虽然没限制这么做，但大可不必，**上面用到了只是方便演示**。
4. **构造函数**，构造函数一般用来初始化对象，只有在一个对象生成之后，才能发挥多态作用，简单点说就是对象都还没初始化呢，哪来的多态啊，这不闹吗？还有一点就是，构造函数不能被继承，因此不能声明为虚函数。
5. **友元函数**，不属于类的成员函数，不能被继承，没有必要。


#### 三、通过基类指针实现多态
直接上例子：

通过基类指针实现多态，基类A，派生B、C，B派生D。

```cpp
#include <iostream>
using namespace std;

class A {
public:
    // 声明
    virtual void foo()
    {
        cout << "A::foo()" << endl;
    }
};
class B : public A {
public:
    virtual void foo() {
        cout << "B::foo()" << endl;
    }
};
class C : public A {
public:
    virtual void foo() {
        cout << "C::foo()" << endl;
    }
};
class D : public B {
public:
    virtual void foo() {
        cout << "D::foo()" << endl;
    }
};

int main()
{
    A a; B b; C c; D d;
    A *pa = &a;
    B *pb = &b;
    pa->foo(); // A::foo()

    pa = pb;
    pa->foo(); // B::foo()

    pa = &c;
    pa->foo(); // C::foo()

    pa = &d;
    pa->foo(); // D::foo()
    return 0;
}
```

如果所有类的`virtual`去掉后，最后的结果是什么？

答：全都输出`A::foo()`。

第二个例子：用基类指针访问基类对象及派生类对象
```cpp
#include <iostream>
#include <string>
using namespace std;

class A {
public:
    string name;
    virtual void say()
    {
        cout << "A::say(), name: " << name << endl;
    }
};
class B : public A {
public:
    string nickname;
    virtual void say() {
        cout << "B::say(), name: " << name << endl;
    }
    void talk() const
    {
        cout << "B::talk(), nickname: " << nickname << endl;
    }
};

int main()
{
    A *pa; // 声明一个类A的指针
    A a;
    B b;

    pa = &a; // 指向变量a的内存地址
    pa->name = "jayce";
    pa->say(); // A::say(), name: jayce
    a.say(); // A::say(), name: jayce

    pa = &b; // 指向变量b的内存地址
    pa->name = "jack";
    pa->say(); // 结果是 B::say(), name: jack。==== 多态，虚函数发挥了作用，否则调用的是基类(A)的say()
    b.say(); // B::say(), name: jack

    b.nickname = "jack chen";
    //pa->talk(); // 会报错，因为编译器在编译时只知道pa的静态类型是A，A类中并没有定义该函数，即使后面指向了派生类B（继承了A），也并不会产生动态影响
    ((B *)pa)->talk(); // 必须得强制类型转换 B::talk(), nickname: jack chen
    b.talk(); // B::talk(), nickname: jack chen
    return 0;
}

```
#### 四、通过基类引用实现多态
```cpp
#include <iostream>
using namespace std;

class A {
public:
    string name;
    virtual void say()
    {
        cout << "A::say(), name: " << name << endl;
    }
};
class B : public A {
public:
    virtual void say()
    {
        cout << "B::say(), name: " << name << endl;
    }
};

void say(A &obj)
{
    obj.say();
}

void talk(B &obj)
{
    obj.say();
}

int main()
{
    A a;
    B b;
    a.name = "jayce";
    b.name = "jack";

    say(a); // A::say(), name: jayce
    say(b); // B::say(), name: jack

    talk((B &)a); // A::say(), name: jayce
    talk(b); // B::say(), name: jack
    return 0;
}
```

#### 五、多态的实现原理
```cpp
#include <iostream>
using namespace std;

class A {
public:
    int age;
    virtual void func() {};
    virtual void func1() {};
};
class B :public A {
public:
    int weight;
    void func() {}
};

int main()
{
    cout << sizeof(A) << ", " << sizeof(B) << endl; // 64为系统下：16, 16 === 去掉virtual则是：4, 8
    return 0;
}
```
在64位系统下，结果是`16, 16`，如果去除`virtual`关键字则输出结果是`4, 8`。

- 先说没有`virtual`的情况：
	- 基类A只有一个`int`成员变量，占4个字节。
	- 而派生类B的内存空间大小是需要加上基类A的内存空间大小，类B也有一个`int`成员变量，所以占8个字节。
- 有`virtual`的情况，空间明显变大了，这是因为编译系统为类对象自动添加的部分，是一块连续的内存，其中存储的是**虚函数表**的地址。
	- 64位系统下，指针占8字节，8+4=12。
		- 内存对齐（8字节）的关系，所以类A的大小是`16`。
		- 而类B的大小则是8+4+4=16。

每一个有虚函数的类都有一个虚函数表，它是由编译器生成的，程序运行时被载入内存。一个类的虚函数表列出了该类的全部虚函数地址，虚函数表是类中所有对象共享的，该类的任何对象中都保存指向该虚函数表的指针。

同一个类的所有对象共享虚函数表，各个对象有自己的指向虚函数表的指针，而且各不相同。

虚函数执行过程：当程序执行到`pa->func();`时：
- 如果pa指向基类对象，则通过基类对象中保存的基类虚函数表的地址，找到基类对象的虚函数表，再从虚函数表中找到`A::func()`的入口地址，完成函数调用。
- 如果pa指向派生类对象，也是一样，从派生类的虚函数表中找到`B::func()`，完成函数调用。
	- 如果派生类B没有重写基类的某个虚函数，则此时虚函数表中保存的是基类虚函数的地址，也就是会调用基类的虚函数。

### 第二节 多态实例
略

### 第三节 多态的使用
```cpp
#include <iostream>
using namespace std;

class Cat {
public:
    void eat()
    {
        cout << "Cat::eat" << endl;
        searchingPrey();
        huntAndKill();
        rest();
    }
    virtual void searchingPrey()
    {
        cout << "Cat::寻找猎物" << endl;
    }
    virtual void huntAndKill()
    {
        cout << "Cat::猎杀" << endl;
    }
    void rest()
    {
        cout << "Cat::吃饱了休息" << endl;
    }
};
class GingerCat :public Cat {
public:
    virtual void searchingPrey()
    {
        cout << "GingerCat::寻找怨种铲屎官" << endl;
    }
    void huntAndKill()
    {
        cout << "GingerCat::猎杀？不存在的，我喵喵喵几声就有吃的" << endl;
    }
    void rest()
    {
        cout << "GingerCat::吃饱了睡觉" << endl;
    }
};

int main()
{
    GingerCat obj;
    obj.eat();
    return 0;
}
```

分析：
- 因为派生类没有重写`eat()`方法，所以调用的是基类的`Cat::eat()`。
- `Cat::eat()`调用了`searchingPrey()`、`huntAndKill()`等，因为是在基类的成员函数中调用的，相当于`this.searchingPrey()`、`this.huntAndKill()`。此时的`this`指针是`Cat *`类型的，即`this`是一个基类指针。
- 当调用`searchingPrey()`，因为它是虚函数，相当于通过基类指针调用虚函数，结果是多态的，也就是在声明时是一个`GingerCat`，所以调用的是`GingerCat::searchingPrey()`。`huntAndKill()`在基类也是一个虚函数，同理不再赘述。（即使在派生类的方法中并没有写上`virtual`关键字，但并不会有影响）
- 而`rest()`并不是虚函数，所以当然是调用基类的函数。

### 第四节 虚析构函数
如果一个基类指针指向的对象是用new运算符动态生成的派生类对象，在释放该对象所占用的空间时，仅基类的析构函数会被调用，而派生类的析构函数并未被调用，导致派生类的内存空间没有被释放，**导致内存泄漏**。

那怎么解决这个问题？就是虚析构函数。

格式如下：
```cpp
virtual ~类名();
```

如果一个类的析构函数是虚函数，则由它派生的所有子类的析构函数也是虚析构函数。

上例子：
```cpp
#include <iostream>
using namespace std;

class A {
public:
    A()
    {
        cout << "A构造函数" << endl;
    }
    ~A()
    {
        cout << "A析构函数" << endl;
    }
};
class B :public A {
public:
    int w, h;
    B()
    {
        cout << "B构造函数" << endl;
        w = 4;
        h = 7;
    }
    ~B()
    {
        cout << "B析构函数" << endl;
    }
};

class D {
public:
    D()
    {
        cout << "D构造函数" << endl;
    }
    virtual ~D()
    {
        cout << "D析构函数" << endl;
    }
};
class C :public D {
public:
    int w, h;
    C()
    {
        cout << "C构造函数" << endl;
        w = 12;
        h = 72;
    }
    ~C()
    {
        cout << "C析构函数" << endl;
    }
};

int main()
{
    A *pa = new B();
    delete pa; // 并没有调用到B的析构函数

    cout << "========" << endl;

    D *pb = new C();
    delete pb;
    return 0;
}
```
输出结果：
```
A构造函数
B构造函数
A析构函数
========
D构造函数
C构造函数
C析构函数
D析构函数
```

### 第五节 纯虚函数和抽象类
#### 一、纯虚函数
在一些情况下，基类中的某个虚函数给不出一个确切的定义，或者没有必要给出详细的定义，那么可以将它声明为一个纯虚函数。

格式如下：
```cpp
virtual 函数返回类型 函数名(参数表) = 0;
```

纯虚函数没有函数体，所以参数表后面必须要写`=0`，在派生类中必须要重写这个函数。

#### 二、抽象类
包含纯虚函数的类称为抽象类，因为有尚未完成的函数定义，所以它不能实例化为一个对象，直到有派生类实现了纯虚函数后，它才不再是抽象类，此时可以实例化为一个对象。

```cpp
#include <iostream>
using namespace std;

class A {
public:
    virtual void foo() = 0;
    virtual void bar() = 0;
    void baz()
    {
        cout << "A::baz()" << endl;
    }
};
class B :public A {
public:
    virtual void bar()
    {
        cout << "B::bar()" << endl;
    }
    void baz()
    {
        cout << "A::baz()" << endl;
    }
};
class C :public B {
public:
    virtual void foo()
    {
        cout << "C::foo()" << endl;
        bar();
    }
    void baz()
    {
        cout << "C::baz()" << endl;
    }
};
class D :public B {
public:
    virtual void foo()
    {
        cout << "D::foo()" << endl;
        bar();
    }
    void baz()
    {
        cout << "D::baz()" << endl;
    }
};

int main()
{
    // A a; // 报错，无法实例化
    // B b; // 报错，无法实例化
    C c;
    c.foo();

    cout << "=====" << endl;

    A *pa = &c;
    pa->foo();

    cout << "=====" << endl;

    D d;
    B *pb = &d;
    pb->baz();
    return 0;
}
```

输出结果：
```
C::foo()
B::bar()
=====
C::foo()
B::bar()
=====
A::baz()
```

#### 三、虚基类
也就是一个类同时继承多个继承了基类的类，为了避免二义性的情况。

格式如下：
```cpp
class 派生类名 :virtual 派生方式 基类名
{
	派生类体
};
```

例子：略

## 第七章 输入/输出流
### 第一节 流类简介
### 第二节 标准流对象
### 第三节 控制I/O格式
#### 流操纵符

## 第八章 文件操作
### 第一节 文件基本概念和文件流类
#### 一、文件的概念
从不同的角度来看待文件就可以得到不同的文件分类。
- C++根据文件数据的编码方式不同分为**文本文件**和**二进制文件**。
- 根据存取方式不同分为**顺序存取文件**和**随机存取文件**。

从数据存储的角度来说，所有的文件本质上都是一样的，都是由一个个字节组成，归根到底都是0、1字节串。文本文件和二进制文件只是格式不同而已。

计算机将文件看成一个有序排列的字节序列，在文件内部有一个读写文件的**位置指针**，用以记录文件内部正在进行操作的字节位置。操作文件时，系统会自动修改这个位置指针。
- 顺序存取文件，顾名思义，按照文件中数据的存储次序进行顺序操作，**整个文件的操作过程，将移动位置指针的工作交给系统自动完成**。
	- 例如：磁带文件。
- 随机访问文件，就是通过命令移动位置指针直接定位到文件内需要的位置进行数据操作。

文件的基本操作分为**读文件**和**写文件**。
- **读文件，将文件中的数据读入内存中，称为输入**。
- **写文件，将内存中的数据存入文件中，称为输出**。
#### 二、C++文件流类
流是一个逻辑概念，是对所有外部设备的逻辑抽象。C++的I/O系统将每个外部设备都转换成一个称为流的逻辑设备，由流来完成对不同设备的具体操作。

文件是一个物理概念，代表存储着信息集合的某个外部介质，它是C++语言对具体设备的抽象。

C++也是使用流类对文件进行处理，标准类库中有3个流类用于文件操作，统称为文件流类：
1. `ifstream`：用于从文件中读取数据。
2. `ofstream`：从文件中写入数据。
3. `fstream`：对文件既可以读又可以写。

文件流类关系：
- 类`ifstream`、类`fstream`都是从类`istream`派生出来的。
- 类`ofstream`是从类`ostream`派生出来的。
- `fstream`是同时继承了类`ifstream`和类`ofstream`。

文件操作：打开、读、写、关闭等。

### 第二节 打开和关闭文件
#### 一、打开文件
在对文件进行读写操作之前，要先打开文件，有两个目的：
1. 建立关联。通过指定文件名，建立起文件和文件流对象的关联，以后在对文件进行操作时，可以通过流对象来进行。
2. 指明文件的**使用方式**和**文件格式**。
	- 使用方式：只读、只写、既读又写、在文件末尾追加数据。
	- 文件格式：文本、二进制。

打开文件的两种格式：
```c++
// 方式一
流类名 对象名;
对象名.open(文件名, 模式);

// 方式二
流类名 对象名(文件名, 模式);
```
- 文件名，是一个字符串，可以是绝对路径（完整路径），也可以是相对路径。
- 模式，是类ios定义的打开模式标记常量，即表示文件的打开方式，这些标记可以单独使用，也可以组合使用。

| 模式标记常量                          | 适用对象                      | 作用                                   |
| ------------------------------- | ------------------------- | ------------------------------------ |
| `ios::in`                       | ifstream、fstream          | 以读方式打开文件，文件不存在，则打开出错                 |
| `ios::out`                      | ofstream、fstream          | 以写方式打开文件，文件不存在，新建该文件；文件存在，在打开时清除文件内容 |
| `ios::app`                      | ofstream                  | 以追加方式打开文件，在文件末尾添加数据，文件不存在则新建         |
| `ios::ate`                      | ofstream                  | 打开一个已有文件，将文件读指针指向文件末尾，文件不存在则打开出错     |
| `ios::trunc`                    | ofstream                  | 删除文件现有内容，单独使用时与ios::out相同            |
| `ios::binary`                   | ifstream、ofstream、fstream | 以二进制方式打开文件。不指定的话默认是文本模式              |
| `ios::in\|ios::out`             | fstream                   | 打开已存在的文件，可读可写，文件打开原内容保持不变，文件不存在则打开出错 |
| `ios::in\|ios::out`             | ofstream                  | 和上面一样，区别在不能读                         |
| `ios::in\|ios::out\|ios::trunc` | fstream                   | 一样，区别在文件不存在时新建，存在则打开时清除文件的内容         |

#### 二、关闭文件
当一个文件操作完毕应该及时关闭文件，发出关闭文件命令后，系统会将缓冲区中的数据完整地写入文件，同时添加文件结束标记，切断流对象与外部文件的连接。

例子：
```cpp
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

int main()
{
    string name;
    ifstream obj1;
    name = "test1.txt";
    obj1.open(name, ios::in);
    if (obj1) {
        cout << name << "打开成功" << endl;
    } else {
        cout << name << "打开失败" << endl;
    }

    name = "test2.txt";
    ofstream obj2(name, ios::out);
    if (obj2) {
        obj2 << "hello world" << endl; // 写入内容
        obj2.close(); // 关闭文件并将写入内容从缓冲区写入到文件中
    } else {
        cout << name << "打开失败" << endl;
    }
    return 0;
}
```
### 第三节 文件读写操作
#### 一、读写文本文件
C++将文件看成顺序排列的无结构的字节流。

对于长度为n的文件来说，字节号从0~n-1。

每个文件都有一个文件结束标识，也就是在n的位置上。对于文本文件来说，C++在iostream中定义了一个标识文件结束的标识常量`EOF`，其值为`0x1A`的字符。

在关闭文件流时，该字符将被自动加入到文件尾部。在键盘操作时，按下`Ctrl + Z`键，就可以在标准输入流cin中输入文件结束符。

使用文件流对象打开文件后，文件就成了一个输入流或输出流，对于文本文件，可以用cin、cout进行读写，在标准输入流/输出流中可以使用的成员函数和流操纵符同样适用于文件流。

例子一，对文件score.txt进行输入/输出：
```cpp
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

int main()
{
    char id[11], name[21];
    int score;

    string fileName = "./temp/score.txt";
    ofstream file1(fileName, ios::out);
    if (!file1) {
        cout << fileName << "创建失败" << endl;
        return 0;
    }
    cout << "请输入：学号 姓名 成绩（按Ctrl + Z结束输入）\n";
    while (cin >> id >> name >> score) {
        file1 << id << " " << name << " " << score << endl;
    }
    file1.close();
    return 0;
}
```

例子二，读取score.txt的内容并打印到屏幕
```cpp
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

int main()
{
    char id[11], name[21];
    int score;

    string fileName = "./temp/score.txt";
    ifstream file1(fileName, ios::in);
    if (!file1) {
        cout << fileName << "文件不存在，打开失败" << endl;
        return 0;
    }

    cout << "学生信息：" << endl;
    cout << left << "学号 姓名 成绩" << endl;
    while (file1 >> id >> name >> score) { // >> 按照空格或换行符进行分隔将数据存到不同的变量中
        cout << left << id << " " << name << " " << score << endl;
    }
    file1.close();
    return 0;
}
```
输出结果：
```
学生信息：
学号 姓名 成绩
00123 张三 90
00124 李四 9
```

例子三，逐行读取`score.txt`的内容并打印到屏幕：
`score.txt`文件内容：
```
00123 张三 90
00124 李四 9

```
代码：
```cpp
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

int main()
{
    string name = "./temp/score.txt";
    ifstream obj3(name, ios::in);
    char ch;
    bool newLine = true;
    int lineCount = 0;
    if (obj3) {
        cout << name << "打开成功" << endl;
        cout << "以下是文件内容：" << endl;
        while ((ch = obj3.get()) != EOF) {
            if (newLine) {
                cout << setw(4) << ++lineCount << ':';
                newLine = false;
            }
            if (ch == '\n') {
                newLine = true;
            }
            cout << setw(0) << ch;
        }
        obj3.close();
    }
    return 0;
}
```
输出结果：
```
./temp/score.txt打开成功
以下是文件内容：
   1:00123 张三 90
   2:00124 李四 9
```

`fstream`成员函数官方文档：[std::basic\_fstream - cppreference.com](https://zh.cppreference.com/w/cpp/io/basic_fstream)

例子四，对文本文件的内容排序，将结果输出到另一个文件：
```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <cstdlib>

using namespace std;

const int MAX_SIZE = 1000;

class Student
{
public:
    char id[11];
    char name[21];
    int score;
} stu[MAX_SIZE];

int compare(const void *obj1, const void *obj2)
{
    return (*(Student *)obj1).score - (*(Student *)obj2).score;
}

int main()
{
    string name = "./temp/score.txt";
    ifstream file1(name, ios::in);
    if (!file1) {
        file1.close();
        cout << name << "打开失败" << endl;
        return 0;
    }

    name = "./temp/result.txt";
    ofstream file2(name, ios::out);
    if (!file2) {
        file2.close();
        cout << name << "打开失败" << endl;
        return 0;
    }

    int n = 0;
    cout << "排序前: \n";
    while (file1 >> stu[n].id >> stu[n].name >> stu[n].score)
    {
        cout << stu[n].id << " " << stu[n].name << " " << stu[n].score << endl;
        n++;
    }

    qsort(stu, n, sizeof(Student), compare);

    cout << "排序后: \n";
    for (int i = 0; i < n; i++)
    {
        cout << stu[i].id << " " << stu[i].name << " " << stu[i].score << endl;
        file2 << stu[i].id << " " << stu[i].name << " " << stu[i].score << endl;
    }
    file1.close();
    file2.close();
    return 0;
}

```
结果：
```
排序前:
00123 张三 90
00124 李四 9
排序后:
00124 李四 9
00123 张三 90
```

`cstdlib`函数库官方文档：[标准库标头 \<cstdlib\> - cppreference.com](https://zh.cppreference.com/w/cpp/header/cstdlib)

#### 二、读写二进制文件
文本文件在存储数据时以ASCII码保存数据的，虽然使用上比较方便，但文本格式存储数据占用的空间大，而且数据输入/输出还要在内存和外存之间做数据格式的转换。

二进制数据文件以基本类型数据的二进制格式存放，即内存和外存所存储的数据格式都是一致的二进制格式。

存储长度仅与数据类型有关，例如一个double类型数据，无论是`-12345.678`，还是`3.1415926`，在内存中都采用二进制存储，占用8个字节，将这些数据保存到二进制文件中，也还是占用8个字节，与内存中的表示完全一致。

**都不需要做数据格式的转换了，处理数据那不得比文本格式快多了，就好比国道和高速公路。**

因此二进制数据文件又称为类型文件，由“数据类型”定义的一个“单元”通常包含若干个数据项，由若干个字节组成，称为一个文件的“记录”，或是文件的“元素”。

##### 存储说明
二进制数据文件的读写操作完全由程序控制，一般的文字处理软件不能直接参与编辑。

例如用以下的类表示学生的信息：
```cpp
class Student
{
public:
    char id[11];
    char name[21];
    int score;
};
```
用文本文件存储内容如下：
```
00123 ZhangSan 90
00124 LiSi 100
00125 WangWu 78
```

在这种存储格式下，每个学生信息占一行，每行的长度都不一样，即使整个文件中的学生信息都是按姓名排好序的，要根据名字进行查找，仍然没有什么好办法，只能从头到尾对整个文件进行查找。

如果把整个文件都读入内存，排序后再进行查找，当然速度会很快，但如果文件非常大，数据量巨大，把所有信息都读入内存根本不现实。

而二进制的格式存储，则是把Student对象作为一个整体直接写入文件，在该文件中，每个学生的信息都占sizeof(Student)个字节，对象写入文件后一般称为“记录”，每个学生都对应一条记录，除了免去内存和外存之间的格式转换，也免去了对文件存储格式的设计，减少了工作量。

那怎么读写二进制文件呢？

答：可以用ios::binary()的方式打开二进制文件，调用`ifstream`或`fstream`的`read()`读数据，写数据则是调用`ofstream`或`fstream`的`write()`，**但不能用`cin`、`cout`从流中读写数据**。

##### 用ostream::write()成员函数写文件
原型如下：
```cpp
ostream & write(char *buffer, int nCount);
```
该成员函数将内存中buffer所指向的nCount个字节的内容写入文件，返回值是对函数所作用的对象的引用。

例子一，以二进制文件保存学生信息
```cpp
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

class Student
{
public:
    string id;
    string name;
    int score;
};

int main()
{
    Student stu;
    string fileName = "./temp/student.dat";
    ofstream file1(fileName, ios::out | ios::binary);
    if (!file1) {
        cerr << "无法打开文件: " << fileName << endl;
        return 1;
    }

    cout << "请输入学生信息, 学号、姓名、成绩, 按'enter'输入下一个信息，输入'exit'退出" << endl;
    while (true) {
        cout << "请输入学号: ";
        cin >> stu.id;
        if (stu.id == "exit") break;

        cout << "请输入姓名: ";
        cin.ignore(); // 忽略之前留在输入流中的换行符
        getline(cin, stu.name);

        cout << "请输入成绩: ";
        cin >> stu.score;

        if (cin.fail()) {
            cerr << "输入错误，请重新输入。" << endl;
            cin.clear(); // 清除错误标志
            cin.ignore(numeric_limits<streamsize>::max(), '\n'); // 忽略错误输入直到下一个换行符
            continue;
        }

        cout << "学号: " << stu.id << ", 姓名: " << stu.name << ", 成绩: " << stu.score << endl;
        // file1.write(reinterpret_cast<const char *>(&stu), sizeof(stu));
        file1.write((char *)(&stu), sizeof(stu));
    }
    file1.close();
    return 0;
}
```

说明：

调用write()函数将对象stu作为一条记录完整写入文件，第一个参数`(char *) &stu`的含义是，通过运算符`&`得到对象stu的地址，然后通过强制类型转换成指向char型的指针，即转换为要写入文件的内存缓冲区的地址。第二个参数`sizeof(stu)`是得到对象stu占用的内存大小，`sizeof(stu)`的结果为36字节，既是对象stu在内存中占用的字节数，也是写入文件中一个记录的大小。

> `sizeof(stu)`的结果等价于`sizeof(Student)`。

例子二，向二进制文件追加数据：
```cpp
#include <iostream>
#include <fstream>
#include <string>

using namespace std;

class Student
{
public:
    string id;
    string name;
    int score;
};

int main()
{
    char ch;
    string fileName = "./temp/student.dat";
    cout << "确定要向" << fileName << "文件中追加新的数据吗？(Y/N)" << endl;
    cin >> ch;
    if (ch != 'Y' && ch != 'y') {
        cout << "输入错误" << endl;
        return 1;
    }
    Student stu;
    ofstream file1(fileName, ios::app | ios::binary);
    if (!file1) {
        cerr << "无法打开文件: " << fileName << endl;
        return 1;
    }

    cout << "请输入学生信息, 学号 姓名 成绩, 每行一条学生记录，按'enter'输入下一个学生信息，输入'exit'退出" << endl;
    while (true) {
        cin >> stu.id;
        if (stu.id == "exit") {
            break;
        }
        cin >> stu.name >> stu.score;
        file1.write((char *)(&stu), sizeof(stu));
    }
    file1.close();
    return 0;
}
```

##### 用istream::read()成员函数读文件
原型如下：
```cpp
istream &read(char *buffer, int nCount);
```
该成员函数从文件中读取nCount个字节的内容，存放到buffer所指向的内存缓存区中，返回值是对函数所作用的对象的引用。该函数是非格式化操作，对读取的字节内容不进行处理，直接放入buffer中。

##### 用ostream::gcount()成员函数得到读取字节数
如果要知道每次读操作成功读取了多少个字节，可以在read()函数执行后立即调用文件流对象的成员函数`gcount()`，它的返回值就是最近一次read()函数执行时成功读取的字节数。

原型：
```cpp
int gcount();
```

例子，从二进制文件中读取数据：
```cpp
#include <iostream>
#include <fstream>
#include <string>
#include <iomanip>

using namespace std;

class Student
{
public:
    string id;
    string name;
    int score;
};

int main()
{
    Student stu;
    int count = 0, byte = 0;
    string fileName = "./temp/student.dat";
    ifstream file1(fileName, ios::in | ios::binary);
    if (!file1) {
        cerr << "无法打开文件: " << fileName << endl;
        return 1;
    }

    cout << "学生信息：学生学号 姓名 成绩\n";
    while (file1.read((char *)&stu, sizeof(stu))) { // 读取记录直到文件结束
        cout << stu.id << " " << stu.name << " " << stu.score << endl;
        count++;
        byte += file1.gcount();
    }
    cout << "共有记录数: " << count << "; 字节数: " << byte << endl;
    file1.close();
    return 0;
}
```
输出结果：
```
学生信息：学生学号 姓名 成绩
00123 张三 98
00124 李四 100
00125 王五 80
00126 老六 69
00127 小七 87
00128 老八 60
00129 馋九 75
共有记录数: 7; 字节数: 392
```

#### 三、用成员函数put()和get()读写文件
略

#### 四、文本文件与二进制文件的异同
两者本质上没有差别，只在一些细节上存在差异。
- 文本文件，以文本形式存储数据。
	- 优点：具有较高的兼容性。
	- 缺点：
		- 存储一批纯数值信息时，要在数据之间人为地添加分隔符，在输入/输出过程中，系统要对内外存的数据格式进行相应转换。
		- 不便于对数据进行随机访问。
- 二进制文件，以二进制形式存储数据。
	- 优点：方便对数据进行随机访问，相同数据类型的数据所占用空间的大小均是相同的，不必在数据之间人为地添加分隔符，在输入/输出的过程中，系统不需要对数据进行任何转换。
	- 缺点：数据兼容性差，当在不同的系统或程序之间采用二进制文件进行数据交换时，读取文件的一方必须非常了解写文件的一方采用的是什么数据类型、数据格式等非常详细的信息之后，才能将二进制文件正确解读。
### 第四节 随机访问文件
略

## 第九章 函数模板与类模板
### 第一节 函数模板
#### 一、函数模板的概念
在写代码的过程中，会遇到一种情况，除了函数形参的类型不一样，功能实现完全一样。这种情况往往需要对每个类型都定义一个单独的版本，代码重复度非常高且重复劳动。

为了解决这个问题，C++提供了一种处理机制，就是函数模板。函数在设计时并不使用实际的类型，而是使用虚拟的类型参数，这样的好处是只需要写一个版本。

格式如下：
```cpp
template <模板参数表>
返回类型 函数模板名(参数表)
{
	函数体的定义
}
```

执行过程：当用实际的类型去调用函数模板时，编译器将以函数模板为样板，生成这个类型的函数代码，这个过程称为函数模板实例化。

模板参数表的写法和函数形参列表的写法很相似，用逗号进行分隔，形式如下：
```
<类型 参数名, 类型 参数名, ...>
```
- 类型也称为占位符，参数名可以是C++类型，也可以是具体的值，如数字、指针等。
- 如果是一个类型，需要使用`typename`或`class 关键字`来表示参数的类型。
- 如果参数是一个值，那就是这个值的类型。

#### 二、函数模板的示例
例子一，简单示例：
```cpp
#include <iostream>
using namespace std;

template <typename T>
T abs(T var)
{
    return var > 0 ? var : -var;
}

int main()
{
    int n = -5;
    int m = 10;
    double d = -.5;
    float f = 3.2;
    cout << n << "的绝对值是：" << abs(n) << endl;
    cout << m << "的绝对值是：" << abs(m) << endl;
    cout << d << "的绝对值是：" << abs(d) << endl;
    cout << f << "的绝对值是：" << abs(f) << endl;
    return 0;
}
```
输出结果：
```
-5的绝对值是：5
10的绝对值是：10
-0.5的绝对值是：0.5
3.2的绝对值是：3.2
```

例子二，对象交换的模板函数示例：
```cpp
#include <iostream>
using namespace std;

template <class T>
void Swap(T &x, T &y)
{
    T temp = x;
    x = y;
    y = temp;
}

class MyDate
{
    int year, month, day;
public:
    MyDate()
    {
        year = 1970;
        month = 5;
        day = 04;
    }
    MyDate(int y, int m, int d): year(y), month(m), day(d)
    {
    }
    void print()
    {
        cout << "y:" << year << "; m:" << month << "; d:" << day;
    }
};

int main()
{
    int n = 1, m = 2;
    cout << "转换前: n=" << n << "; m=" << m << endl;
    Swap(n, m);
    cout << "转换后: n=" << n << "; m=" << m << endl;

    double f = 1.2, g = 2.3;
    cout << "转换前: f=" << f << "; g=" << g << endl;
    Swap<double>(f, g);
    cout << "转换后: f=" << f << "; g=" << g << endl;

    MyDate d1, d2(2000, 1, 1);
    cout << "转换前: d1="; d1.print(); cout << "; d2="; d2.print(); cout << endl;
    Swap<MyDate>(d1, d2);
    cout << "转换后: d1="; d1.print(); cout << "; d2="; d2.print(); cout << endl;
    return 0;
}
```
输出结果：
```
转换前: n=1; m=2
转换后: n=2; m=1
转换前: f=1.2; g=2.3
转换后: f=2.3; g=1.2
转换前: d1=y:1970; m:5; d:4; d2=y:2000; m:1; d:1
转换后: d1=y:2000; m:1; d:1; d2=y:1970; m:5; d:4
```

`Swap(double)(f, g)`是显示实例化模板的写法，和`Swap(f, g)`结果一样。

显示实例化的格式如下：
```cpp
模板名 <实际类型参数1, 实际类型参数2, ...>
```

例子三，对象排序：
```cpp
#include <iostream>
#include <string>

using namespace std;

template <typename T>
int myCompare(const T &left, const T &right)
{
    if (left < right) {
        return -1;
    }
    if (right < left) {
        return 1;
    }
    return 0;
}

template <typename T>
void swap(T &x, T &y)
{
    T temp = x;
    x = y;
    y = temp;
}

int main()
{
    string strArr[10] = {"shang", "xia", "zuo", "you", "qian", "hou", "dong", "xi", "nan", "bei"};
    int j;
    string temp;
    for (int i = 1; i < 10; i++)
    {
        j = i;
        while (j > 0 && myCompare<string>(strArr[j-1], strArr[j]) > 0)
        {
            swap(strArr[j], strArr[j-1]);
            j--;
        }
    }
    for (int i = 0; i < 10; i++)
        cout << strArr[i] << ","; // bei,dong,hou,nan,qian,shang,xi,xia,you,zuo,
    cout << endl;
    return 0;
}
```

#### 三、函数或函数模板调用语句的匹配顺序
例子一，函数调用顺序：
```cpp
#include <iostream>

using namespace std;

template <class T>
void max(T a)
{
    cout << "Template 1" << endl;
}

template <class T1, class T2>
void max(T1 a, T2 b)
{
    cout << "Template 2" << endl;
}

void max(double x, double y)
{
    cout << "Function max" << endl;
}

int main()
{
    int i = 4, j = 5;
    max(1.2, 3.4); // Function max
    max(i, j); // 并没有调用到函数，为什么？
    max(1.2, 3); // Template 2
    return 0;
}
```

例子二，重载函数模板
```cpp
#include <iostream>

using namespace std;

class MyDate
{
    int year, month, day;
public:
    MyDate()
    {
        year = 1970;
        month = 1;
        day = 1;
    }
    MyDate(int y, int m, int d): year(y), month(m), day(d)
    {
    }
    friend ostream &operator <<(ostream &os, MyDate &obj);
};

ostream &operator <<(ostream &os, MyDate &obj)
{
    os << obj.year << "/" << obj.month << "/" << obj.day;
    return os;
}

template <class T>
void print(T x, T y)
{
    cout << x << "," << y << endl;
}

template <class T1, class T2>
void print(T1 x, T2 y)
{
    cout << x << "," << y << endl;
}

int main()
{
    int i = 4, j = 5;
    print(i, j); // 4,5
    MyDate d1, d2(2024, 4, 10);
    print(d1, d2); // 1970/1/1,2024/4/10
    print(i, d2); // 4,2024/4/10
    return 0;
}
```

### 第二节 类模板
#### 一、类模板概念
跟函数模板一样，类也有模板。不展开赘述。

格式如下：
```cpp
template <模板参数表>
class 类模板名
{
	类体定义
}
```

类模板的成员函数既可以在类体内定义，这样就是内联函数，也可以在体外定义，格式如下：
```cpp
template <模板参数表>
返回类型名 类模板名<模板参数标识符列表>::成员函数名(参数表)
{
	函数体
}
```
类模板声明本身并不是一个类，不能直接生成对象，因为类型参数是不确定的，必须先为模板参数指定“实参”，即模板要“实例化”后，才可以创建对象。格式如下：
```cpp
类模板名 <模板参数表> 对象名1, ..., 对象名n;
// or
类模板名 <模板参数表> 对象名1(构造函数实参), ..., 对象名n(构造函数实参);
```
编译器由类模板生成类的过程称为类模板的实例化。**由类模板实例化得到的类称为模板类**。
#### 二、类模板示例
例子一，简单示例：
```cpp
#include <iostream>

using namespace std;

template <class T>
class Pair
{
private:
    T first;
    T second;

public:
    Pair(T f, T s) : first(f), second(s) {}

    T getFirst() const
    {
        return first;
    }

    T getSecond() const
    {
        return second;
    }

    void display() const {
        cout << "First: " << first << ", Second: " << second << endl;
    }
};

int main() {
    Pair<int> intPair(10, 20);
    intPair.display(); // First: 10, Second: 20

    Pair<double> doublePair(3.14, 6.28);
    doublePair.display(); // First: 3.14, Second: 6.28

    Pair<char> charPair('a', 'b');
    charPair.display(); // First: a, Second: b
    return 0;
}
```

例子二，使用普通参数的类模板：
```cpp
#include <iostream>
#include <string>

using namespace std;

template <int size, typename T>
class Test
{
public:
    T data;
    double buffer[size];
    T getData(T v);
    double get(int index);
};

template <int i, typename A>
A Test<i, A>::getData(A v)
{
    return v;
}

template <int x, typename y>
double Test<x, y>::get(int j)
{
    return buffer[j];
}

int main() {
    Test<6, string> obj;
    int i;
    double arr[6] = {12.1, 23.2, 34.3, 45.4, 56.5, 67.6};
    for (i = 0; i < 6; i++)
    {
        obj.buffer[i] = arr[i] - 10;
    }

    obj.data = "demo";
    for (i = 0; i < 6; i++)
    {
        cout << obj.get(i) << " "; // 2.1 13.2 24.3 35.4 46.5 57.6
    }
    cout << endl;
    return 0;
}
```

例子三，在类模板中使用函数模板：
```cpp
#include <iostream>
#include <string>

using namespace std;

template <class T1, class T2>
class Pair
{
public:
    T1 first;
    T2 second;
    Pair(T1 f, T2 s) : first(f), second(s) {}
    bool operator <(const Pair<T1, T2> &p) const;

    template <class T>
    void print(T x) const
    {
        cout << "first: " << x.first << "; second: " << x.second << endl;
    }
};

template <class A1, class A2>
bool Pair<A1, A2>::operator <(const Pair<A1, A2> &p) const
{
    return first < p.first;
}

int main() {
    Pair<string, int> var1("Tom", 19);
    Pair<string, int> var2("Jim", 21);
    bool a = var1 < var2;
    if (a == false) {
        cout << "Jim排在Tom前面" << endl;
    }
    var1.print(var2); // first: Jim; second: 21

    Pair<int, int> var3(20, 10);
    var1.print(var3); // first: 20; second: 10

    Pair<string, string> var4("word", "单词");
    var1.print(var4); // first: word; second: 单词
    return 0;
}
```

例子四，类模板中使用静态成员：
```cpp
#include <iostream>
#include <string>

using namespace std;

template <typename T>
class Test
{
public:
    static T k;
    static void increment()
    {
        k++;
    }
    Test()
    {
        k += 1;
    }
    Test(T num)
    {
        k += num;
    }
};

template <typename T>
T Test<T>::k = 0; // 静态成员变量的初始化

int main() {
    Test<int> a;
    cout << "a: " << a.k << endl;

    Test<double> b(3.6);
    cout << "b: " << b.k << endl;

    Test<int>::increment();
    cout << "int increment以后，k: " << Test<int>::k << endl;

    Test<double>::increment();
    cout << "double increment以后，k: " << Test<double>::k << endl;
    return 0;
}
```
输出结果：
```
a: 1
b: 3.6
int increment以后，k: 2
double increment以后，k: 4.6
```
#### 三、类模板与继承
类之间允许继承，类模板同样也可以，分别是以下4种情况：
1. 普通类继承模板类。
2. 类模板继承普通类。
3. 类模板继承模板类。
4. 类模板继承模板类。

例子一，普通类继承模板类：
```cpp
#include <iostream>

using namespace std;

template <typename T>
class Base
{
public:
    T data;
};

class Test :public Base<int>
{
public:
    void print()
    {
        cout << "data: " << data << endl;
    }
};

int main() {
    Test obj;
    obj.print();
    return 0;
}
```

例子二，类模板继承普通类：
```cpp
#include <iostream>

using namespace std;

class Base
{
public:
    int k;
    void print()
    {
        cout << "Base::print(), k: " << k << endl;
    }
};

template <typename T>
class Test :public Base
{
    T data;
public:
    void set(T d)
    {
        data = d;
    }
    void print()
    {
        Base::print();
        cout << "Test::print(), data: " << data << endl;
    }
};

int main() {
    Test<string> obj;
    obj.set("hello world");
    obj.print();
    return 0;
}
```

输出结果：
```
Base::print(), k: 0
Test::print(), data: hello world
```

例子三，类模板继承模板类：
```cpp
#include <iostream>

using namespace std;

template <typename T>
class Base
{
public:
    T data1;
    Base() {}
    Base(T val): data1(val)
    {
    }
    void print()
    {
        cout << "Base::print(), data1: " << data1 << endl;
    }
};

template <class T1, class T2>
class Test :public Base<T1>
{
public:
    T2 data2;
    Test() {}
    Test(T1 val1, T2 val2): Base<T1>(val1), data2(val2)
    {
    }
    void print()
    {
        Base<T1>::print();
        cout << "Test::print(), data2: " << data2 << endl;
    }
};

int main() {
    Test<int, string> obj1(666, "hello");
    obj1.print();

    Test<int, double> obj2;
    obj2.print();
    return 0;
}
```

输出结果：
```
Base::print(), data1: 666
Test::print(), data2: hello
Base::print(), data1: 0
Test::print(), data2: 6.95313e-310
```

例子四，类模板继承模板类：
```cpp
#include <iostream>

using namespace std;

template <typename T>
class Base
{
public:
    T data1;
    Base() {}
    Base(T val): data1(val)
    {
    }
    void print()
    {
        cout << "Base::print(), data1: " << data1 << endl;
    }
};

template <class T>
class Test :public Base<int>
{
public:
    T data2;
    Test() {}
    Test(T val, int val2 = 123): Base<int>(val2), data2(val)
    {
    }
    void print()
    {
        Base<int>::print();
        cout << "Test::print(), data2: " << data2 << endl;
    }
};

int main() {
    Test<int> obj1(666);
    obj1.print();

    Test<string> obj2("hello world", 9527);
    obj2.print();
    return 0;
}
```

输出结果：
```
Base::print(), data1: 123
Test::print(), data2: 666
Base::print(), data1: 9527
Test::print(), data2: hello world
```

## 考试重点
想啥呢？都是重点，不好好实操怎么可能学的会呢？笔试都是综合应用占比多，选择题20分，填空题15分，程序填空题20分，程序分析题30分，程序设计题15分，就问你想不想考过吧 （狗头）。
