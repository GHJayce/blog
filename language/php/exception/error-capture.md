---
title: "PHP报错解析和如何捕获错误信息"
date: 2017-11-16T16:38:34+08:00
updateDate: 2017-11-16T16:38:34+08:00
slug: language/php/exception/error-capture
image: https://ghjayce.github.io/asset/blog/mwZTYxkP4GSsgLbC5aXaMjAxNzExMTZfMTYzODM0.jpeg
categories:
  - 程序设计语言
tags:
  - PHP
  - 异常
draft: false
---

## 内容大纲
- php的报错类型都有哪些
- 常见的报错原因
- 报错的特性

本文部分信息源自以下文章，如有内容不正确的地方请指出。
- [PHP常见报错解析](https://www.cnblogs.com/shark1100913/p/5329544.html)
- [PHP错误异常处理详解](http://blog.csdn.net/hguisu/article/details/7464977)

## 错误异常
错误信息的格式：
**{错误类型}**: **{错误原因}** in **{错误文件}** on **{错误行数}**

### 错误类型列表

#### Parse error
栗子：
```php
<?php
qweqwe
echo 1;
```
运行结果：
```php
Parse error: syntax error, unexpected end of file in D:\XAMPP\htdocs\index.php on line 2
```
要看懂这个错误，首先要明白其中的一些关键词语：
- **Parse error**（解析错误）
- **syntax error**（语法错误）
- **unexpected**（意外的意思）
- **end of file**（文件结尾）

通过挑出的关键词语，再去看栗子中的错误，我想大家就能知道它所要传达的意思。

**Parse error**，一般伴随着**syntax error**的错误原因，说明程序不符合PHP的语法，**它是级别最高的错误**。

> 常见语句的末尾没有加`;`号

在通过系统语法检测的过程中，**如果遇到语法错误，整个脚本将不会执行**，直接抛出错误。**并且这个错误不能被捕获**。

一般语法错误会带一个解析器的代号，例如：**T_STRING**、**T_VARIABLE**，通过它可以更直接的了解到错误的原因。[解析器的代号](http://www.php.net/manual/zh/tokens.php)


#### Fatal error
栗子：
```php
<?php
echo 1;
func();
$aa += 1;
include 'abc.cc';
echo 2;
```
运行结果：
```php
Fatal error: Call to undefined function func() in D:\XAMPP\htdocs\index.php on line 3
```
**Fatal error**（致命错误），**级别仅次于Parse error错误，这个错误能够被捕获，但是捕获的函数必须是在错误的前面**。

常见报错原因：
- 调用未定义的函数
- require一个不存在的文件
- 死循环导致程序超时等

在通过程序语法检测后，**程序执行到发生错误的行时，脚本会终止运行**。
也就是说，`echo 1;`能正常输出，而因为找不到func()这个函数，所有就会从第二行就开始停止运行。


#### Warning
栗子：
```php
<?php
echo 1;
$aa += 1;
include 'abc.cc';
echo 2;
```
运行结果：
```php
1
Notice: Undefined variable: aa in D:\XAMPP\htdocs\php_share\exception_handle\index.php on line 4

Warning: include(abc.cc): failed to open stream: No such file or directory in D:\XAMPP\htdocs\php_share\exception_handle\index.php on line 5

Warning: include(): Failed opening 'abc.cc' for inclusion (include_path='D:\XAMPP\php\PEAR') in D:\XAMPP\htdocs\php_share\exception_handle\index.php on line 5
2
```

常见发生警告原因：
- include一个不存在的文件
- 调用函数没传参数，而参数没有默认值的情况下

**Warning**（警告），**程序不会因为脚本发生警告而终止运行**，虽然可以用`@`符号屏蔽警告信息，**但是不推荐这么做**。**警告能够被捕获**
```php
<?php
@include 'abc.cc'; // 屏蔽错误，不推荐
```


#### Notice
栗子：
```php
<?php
echo 1;
$a += 1;
echo 2;
```
运行结果：
```bash
1
Notice: Undefined variable: aa in D:\XAMPP\htdocs\php_share\exception_handle\index.php on line 3
2
```

常见发生通知原因：
- 变量或者数组下标未定义的情况

**Notice**（通知），**级别最低**。和警告一样（可以加`@`抑制符，**不推荐**），**程序不会因为脚本发生通知而终止运行，能够被捕获**。


------------


### 捕获错误，自定义报错信息

#### 相关函数
[PHP官方手册 错误处理 函数](http://php.net/manual/zh/ref.errorfunc.php)
##### set_error_handler
一般用于捕获：
- E_NOTICE
- E_USER_ERROR
- E_USER_WARNING
- E_USER_NOTICE

> 不能捕获：
- E_ERROR
- E_PARSE
- E_CORE_ERROR
- E_CORE_WARNING
- E_COMPILE_ERROR
- E_COMPILE_WARNING