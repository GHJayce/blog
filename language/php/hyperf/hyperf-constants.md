---
title: hyperf constants实现机制
slug: language/php/hyperf/hyperf-constants
date: 2024-02-26T10:01:00+08:00
updateDate: 2024-03-02T11:56:12+08:00
categories:
  - 程序设计语言
tags:
  - PHP
  - Hyperf
  - 枚举
  - 异常
  - 源码阅读
draft: false
image: https://ghjayce.github.io/asset/blog/FhYnPBeIezDAGMZ0BQgUMjAyNDAzMDFfMTU0NzE4.jpeg
---
## 阅读说明
### 环境说明
- php v8.2.8
- hyperf/constants v3.1.0

## 前言
[Hyperf 枚举类](https://hyperf.wiki/3.1/#/zh-cn/constants)文档中，主要分为了两个部分：
- 枚举，主要是通过错误码获取到错误内容的作用。
- 异常，则是对抛异常的使用带来了便利，仅传入错误码，就能从枚举类中获得错误内容。

**本文主要分析枚举类获取错误内容的源码实现过程。**

### 代码示例
[hyperf/constants](https://github.com/hyperf/constants)的使用如下。
#### 枚举类
```php
<?php

declare(strict_types=1);

namespace App\Constants;

use Hyperf\Constants\AbstractConstants;
use Hyperf\Constants\Annotation\Constants;

#[Constants]
class ErrorCode extends AbstractConstants
{
    /**
     * @Message("Server Error！")
     */
    const SERVER_ERROR = 500;

    /**
     * @Message("系统参数错误")
     */
    const SYSTEM_INVALID = 700;
}
```

错误内容通过注释的方式定义，与错误码紧挨着，这样的实现有两个好处：
- 阅读方便，文件简洁。
- 书写便捷，省工作量。

为什么？看看传统的实现方式你就知道了。
```php
<?php
class ErrorCode
{
    const SERVER_ERROR = 500;
    const PARAMS_INVALID = 1000;

    public static $messages = [
        self::SERVER_ERROR => 'Server Error',
        self::PARAMS_INVALID => '参数非法'
    ];
}
$message = ErrorCode::messages[ErrorCode::SERVER_ERROR] ?? '未知错误';
```

当错误码定义足够多的情况下，就会显得臃肿和繁琐。

（如果你选择把错误码拆到多个文件，每个文件内容不超过一个屏幕画面的话，当我没说😂）

#### 异常类
```php
<?php

declare(strict_types=1);

namespace App\Exception;

use App\Constants\ErrorCode;
use Hyperf\Server\Exception\ServerException;
use Throwable;

class BusinessException extends ServerException
{
    public function __construct(int $code = 0, string $message = null, Throwable $previous = null)
    {
        if (is_null($message)) {
            $message = ErrorCode::getMessage($code);
        }

        parent::__construct($message, $code, $previous);
    }
}

```

#### 抛异常
```php
<?php

declare(strict_types=1);

namespace App\Controller;

use App\Constants\ErrorCode;
use App\Exception\BusinessException;

class IndexController extends AbstractController
{
    public function index()
    {
        throw new BusinessException(ErrorCode::SERVER_ERROR);
    }
}
```

## 分析

根据使用方式，先来猜测一下。

如果要获取类文件中的注释，那必定会用到反射类：[ReflectionClass](https://www.php.net/manual/zh/class.reflectionclass.php)或者是[ReflectionClassConstant](https://www.php.net/manual/zh/class.reflectionclassconstant.php)，然后再通过正则取出想要的内容。
### 疑问
- KQ1：怎么获取到注释里的错误内容的？
- KQ2：在什么时候进行获取的？
- Q3：难道每次获取错误内容都要反射一次？那会对性能有影响啊？
- Q4：都PHP8了为什么不用[注解](https://www.php.net/manual/zh/language.attributes.php)的写法？例如：`#[Message("Server Error！")]`

废话不多说，马上开始逐个分析问题。

### 获取错误内容
先从获取错误内容的代码入手：
```php
ErrorCode::getMessage($code);
```

再看下[枚举类](#枚举类)，是继承了`Hyperf\Constants\AbstractConstants`的抽象类。
```php
<?php

declare(strict_types=1);

namespace Hyperf\Constants;

/**
 * @method static string getMessage(int|string $code, array $translate = null)
 */
abstract class AbstractConstants
{
    use ConstantsTrait;
}
```

用了一个`Hyperf\Constants\ConstantsTrait`的trait，继续点进去。

```php
<?php

declare(strict_types=1);

namespace Hyperf\Constants;

use Hyperf\Constants\Exception\ConstantsException;
use Psr\Container\ContainerExceptionInterface;
use Psr\Container\NotFoundExceptionInterface;

/**
 * @method static string getMessage(int|string $code, array $translate = null)
 */
trait ConstantsTrait
{
    use GetterTrait;

    /**
     * @throws ConstantsException
     * @throws ContainerExceptionInterface
     * @throws NotFoundExceptionInterface
     */
    public static function __callStatic(string $name, array $arguments): string|array
    {
        return static::getValue($name, $arguments);
    }
}
```
是一个[\_\_callStatic](https://www.php.net/manual/zh/language.oop5.overloading.php#object.callstatic)魔术方法，再点进`Hyperf\Constants\GetterTrait`这个 trait。

```php
<?php

declare(strict_types=1);
/**
 * This file is part of Hyperf.
 *
 * @link     https://www.hyperf.io
 * @document https://hyperf.wiki
 * @contact  group@hyperf.io
 * @license  https://github.com/hyperf/hyperf/blob/master/LICENSE
 */
namespace Hyperf\Constants;

use Hyperf\Constants\Exception\ConstantsException;
use Hyperf\Context\ApplicationContext;
use Hyperf\Contract\TranslatorInterface;
use Psr\Container\ContainerExceptionInterface;
use Psr\Container\NotFoundExceptionInterface;

use function array_shift;
use function is_array;
use function sprintf;
use function strtolower;
use function substr;

trait GetterTrait
{
    /**
     * @throws ConstantsException
     * @throws ContainerExceptionInterface
     * @throws NotFoundExceptionInterface
     */
    public static function getValue(string $name, array $arguments): string|array
    {
        if (! str_starts_with($name, 'get')) {
            throw new ConstantsException("The function {$name} is not defined!");
        }

        if (empty($arguments)) {
            throw new ConstantsException('The Code is required');
        }

        $code = array_shift($arguments);
        $name = strtolower(substr($name, 3));

        $message = ConstantsCollector::getValue(static::class, $code, $name);

        $result = self::translate($message, $arguments);
        // If the result of translate doesn't exist, the result is equal with message, so we will skip it.
        if ($result && $result !== $message) {
            return $result;
        }

        if (! empty($arguments)) {
            return sprintf($message, ...(array) $arguments[0]);
        }

        return $message;
    }

    /**
     * @throws ContainerExceptionInterface
     * @throws NotFoundExceptionInterface
     */
    protected static function translate(string $key, array $arguments): array|string|null
    {
        if (! ApplicationContext::hasContainer() || ! ApplicationContext::getContainer()->has(TranslatorInterface::class)) {
            return null;
        }

        $replace = array_shift($arguments) ?? [];
        if (! is_array($replace)) {
            return null;
        }

        $translator = ApplicationContext::getContainer()->get(TranslatorInterface::class);

        return $translator->trans($key, $replace);
    }
}
```

**到这里可以得知：`ErrorCode::getMessage($code);` 的调用，最终是`Hyperf\Constants\GetterTrait::getValue()` 在处理。**

> 第一感觉有点绕啊，跳来跳去最后才见到了庐山真面目。
> 
> 为什么要这样设计呢？那么多层trait，不如直接写一个getMessage方法方便？挖坑#1002。

但是这样实现有一个好处，比如可以通过`ErrorCode::getTest($code);`获取到自定义注释的错误内容，这是文档中没有介绍到的，挖到一点宝藏用法，但是毕竟属于没公布的内容，在业务中使用还得谨慎一些。
```php
/**
 * @Message("Server Error！")
 * @Test("服务错误")
 */
const SERVER_ERROR = 500;
```

回到主线任务，从实现中可以看出，错误内容的获取关键代码是：
```php
$message = ConstantsCollector::getValue(static::class, $code, $name);
```
它是一个[自定义的收集器](https://hyperf.wiki/3.1/#/zh-cn/annotation?id=%e8%87%aa%e5%ae%9a%e4%b9%89%e6%b3%a8%e8%a7%a3%e6%94%b6%e9%9b%86%e5%99%a8)，在`Hyperf\Constants\ConfigProvider`中可以看到：
```php
<?php

declare(strict_types=1);
namespace Hyperf\Constants;

class ConfigProvider
{
    public function __invoke(): array
    {
        return [
            'annotations' => [
                'scan' => [
                    'collectors' => [
                        ConstantsCollector::class,
                    ],
                ],
            ],
        ];
    }
}
```
进去`Hyperf\Constants\ConstantsCollector`：
```php
<?php

declare(strict_types=1);

namespace Hyperf\Constants;

use Hyperf\Di\MetadataCollector;

class ConstantsCollector extends MetadataCollector
{
    protected static array $container = [];

    public static function getValue($className, $code, $key): string
    {
        return static::$container[$className][$code][$key] ?? '';
    }
}
```

错误内容是从一个数组类型的静态变量（这里称为容器）中获取的，并且容器中存储不止一个枚举类的数据。

> swoole静态变量的生命周期是多久？和传统PHP有什么区别？挖坑#1000。

把容器的数据结构打印出来看看：
```php
var_dump(ConstantsCollector::list());
[
  'App\Constants\ErrorCode' => [
    500 => [
      'message' => 'Server Error！',
      'test' => '服务错误',
    ],
    700 => [
      'message' => '系统参数错误',
    ],
  ],
];
```

也就是`ErrorCode::getMessage($code);`获取错误内容实际是从`Hyperf\Constants\ConstantsCollector::$container`静态变量里取出来的。

KQ2和Q3的问题回答了一半，那容器里的数据是怎么来的？也就是调用收集器的地方。

> hyperf注解和收集器的工作原理是怎样的？挖坑#1001。

### 获取时机

在对hyperf的配置加载、注解和收集器的机制了解完以后（挖坑#1003），我们知道，hyperf会在启动的时候对`annotations.scan.paths`配置的目录进行扫描，配置结构如下。
```php
'scan' => [
  'paths' => [
    BASE_PATH . '/app',
    BASE_PATH . '/src',
  ],
  ...
]
```
然后会把所有后缀是`.php`的类文件，进行`ReflectionClass`反射处理。

并通过[getAttributes()](https://www.php.net/manual/zh/language.attributes.reflection.php)逐个检查类文件中的`class`、`method`和`properties`是否存在注解，存在就会通过[newInstance()](https://www.php.net/manual/zh/reflectionattribute.newinstance.php)实例化这个注解类，然后执行注解类中的`collectClass()` / `collectProperty()` / `collectMethod()`方法，最后会将所有`collector`给序列化`serialize`然后存放到`./runtime/container/scan.cache`中以便下次启动时加速。

回到主线任务，也就是说，在启动的时候会扫描到[枚举类文件](#枚举类)，发现使用到了`#[Constants]`注解，对应`Hyperf\Constants\Annotation\Constants`会被实例化，看下这个文件的代码：
```php
<?php

declare(strict_types=1);
namespace Hyperf\Constants\Annotation;

use Attribute;
use Hyperf\Constants\AnnotationReader;
use Hyperf\Constants\ConstantsCollector;
use Hyperf\Di\Annotation\AbstractAnnotation;
use ReflectionClass;

#[Attribute(Attribute::TARGET_CLASS)]
class Constants extends AbstractAnnotation
{
    public function collectClass(string $className): void
    {
        $reader = new AnnotationReader();

        $ref = new ReflectionClass($className);
        $classConstants = $ref->getReflectionConstants();
        $data = $reader->getAnnotations($classConstants);

        ConstantsCollector::set($className, $data);
    }
}
```
`collectClass()`就会被执行，来分析一下这段代码。

这里的`$className`正是`App\Constants\ErrorCode`枚举类，可以看到用了`ReflectionClass`反射该文件来获取所有的常量。

> 这里为什么不用`Hyperf\Di\ReflectionManager::reflectClass`复用的方式来获取反射呢？毕竟composer都已经依赖`hyperf/di`包了🤔。
> 
> 哈哈，找到了一个优化点，想提交PR的心正在蠢蠢欲动🤣。

进去`Hyperf\Constants\AnnotationReader::getAnnotations()`方法得到了证实，代码太多我就不放出来，它是通过[getDocComment()](https://www.php.net/manual/zh/reflectionclassconstant.getdoccomment.php)和正则拿到了注释中的错误内容。

`$data`结构参考如下：
```php
500 => [
  'message' => 'Server Error！',
  'test' => '服务错误',
],
700 => [
  'message' => '系统参数错误',
],
```
最后将数据存到了`Hyperf\Constants\ConstantsCollector::container`静态变量中，梳理完毕。

整个过程简单点说就是，在hyperf启动时，枚举类文件会被扫描，由注解类将收集到的数据存放到收集器中，扫描逻辑将所有收集器的数据缓存到本地文件中，下次再启动时就不需要重复扫描，直接使用本地缓存文件。

至此，KQ1、KQ2、Q3都得到了回答。

等等，是不是还有Q4没有回答？

嘿嘿，偷个懒，留个课后作业给你思考一下。

## 总结
- 枚举类中的错误内容是通过`ReflectionClass`反射，对每个常量`getReflectionConstants()`的注释内容`getDocComment()`用正则的方式取出来。
- `ErrorCode::getMessage($code)`实际是从`Hyperf\Constants\ConstantsCollector::container`取出错误内容，数据来源则是在hyperf启动的时候，由`Hyperf\Constants\Annotation\Constants`注解将文件中所有常量的错误内容收集起来后，以文件类名为key，存放到`container`静态变量中，并且最后会将收集到的数据缓存到`./runtime/container/scan.cache`中，这样下次就不需要再重复这个收集过程。