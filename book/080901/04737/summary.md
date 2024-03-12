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
    // 结果: num1: 10; num2: 0x7fff363ab310
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



#### 七、内联函数

#### 八、函数的重载

#### 九、指针和动态内存分配

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
	- 例如：大写字母的ASCII码小于小写字母的ASCII码，`A`~`Z`的ASCII码是`0x41`~`0x5a`，`a`~`z`是`0x61`~`0x7a`，[ASCII码对照表](https://baike.baidu.com/item/ASCII/309296?fr=ge_ala#3)。
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