---
title: objectC基本语法
date: 2018-04-15 13:20:33
tags: 
  - "objectC"
categories: "objectC语言学习"
---

1、 oc相对于c

- 在c的基础之上新增了1小部分面向对象的语法
- 将c一些复杂的语法简化了很多

2、 oc程序的源文件后缀名为.m，m代表message 代表oc中最重要的1个机制 消息机制 c程序的源文件后缀名为.c

3、 main函数仍然是oc程序的入口和出口

  - int 类型的返回值 代表程序的结束状态
  - main函数的参数： 仍然可以接收用户在运行的时候传递数据给程序，参数也可以不要

4、 #import 指令
  - 以#号开头，是一个预处理指令
  - 作用：是#include指令的增强版，将文件的内容在预编译的时候拷贝到写指令的地方
  - 增强： 同一个文件无论#import多少次，只会包含1次。在c语言中，如果使用#include命令实现这个功能，需要使用条件预编译指令来实现。

  - 原理： #import指令在包含文件的时候，底层会先判断这个文件是否被包含，如果被包含就会略过，否则才会包含


## 框架

1、 是一个功能集，苹果或者第三方事先将一些程序在开发程序的时候经常要用到的功能先写好，把这些功能封装在1个类或者函数之中，这些函数和类的集合就叫做框架，有点像c语言的函数库

2、 Foundation框架

- 名字释义： 基础，基本，这个框架提供一些基本的功能，输入输出，一些数据类型
- Foundation路径： Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/System/Library/Frameworks/Foundation.framework/Versions/C/Headers/Foundation.h

- Foundation.h这个头文件中包含了Foundation框架中的其他所有头文件，所以只要包含Foundation.h就相当于包含了Foundation框架的所有头文件。那么Foundation框架中的所有函数和类都可以直接使用了

- #import <Foundation/Foundation.h> 包含Foundation框架头

- @autoreleasepool是自动释放池，可以将代码写在自动释放池，或者直接删除也不会有影响

3、 NSLog函数
 
 - 作用： 是printf函数的增强版，向控制台输出信息
 - 语法： NSlog(@"格式控制字符串",变量列表);
 - 增强的地方 
   * 输出一些调试相关的信息，执行这段代码的时间，程序的名字,进程编号，线程编号，最后是输出的信息
   * 会自动换行，在输出完信息之后，会自动换行
   * OC中新增了一些数据类型，NSLog函数不仅可以输出c语言数据类型变量的值，也可以输出OC新增类型变量的值

> 2018-01-14 19:00:42.935906+0800 ObjectCDemo[36595:1039524] 
> Hello, World!


 - 用法：和printf函数差不多，前面有个@符号，占位符用法一样
  
 - 使用注意： 
    * 第一个参数前面必须要加一个@符号
    * 在字符串末尾如果手动加了一个`\n`代表换行，那么函数自带的自动换行就会失效


## 字符串

1、 c语言中字符串的存储方式

- 使用字符数组存储
- 使用字符指针

2、 OC中设计了一个更为好用的用来存储字符串的类型，NSString
NSString类型的指针变量，专门用来存储OC字符串的地址

3、 OC的字符串必须要使用1个前缀@符号

- "jack" 这是一个c语言的字符串
- @"jack" 这是一个oc的字符串
- NSString类型的指针变量，只能存储OC字符串的地址
- NSString *str = @"jack";

4、 注意：

- NSLog函数的第一个参数是NSString类型，所以必须用@开头
- 如果要使用NSLog函数输出字符串，占位符必须使用%@
```
        NSString *name=@"jack";
        NSLog(@"Hello, %@!",name);
```

5、 NS前缀的来历
NextStep 公司--->Cocoa---> Foundation 框架之中

6、 @符号
 
 - 将c字符串转换为OC字符串
 - OC中的绝大都数关键字都是以@符号开头

7、 和C语言中的注释一模一样，分为单行和多行注释

## 函数的定义和调用

1、 与C语言函数的定义和调用是一样的

## OC与C语言的不同

1、 OC程序的编译、链接、执行
  - 在.m文件中写上符合OC语法的源代码
  - 使用cc -c main.c, 编译源代码生成目标文件.o文件
  - 链接 cc main.o文件 ，会报错
  - 如果程序中使用到了框架中的函数或者类，那么在链接的时候，必须告诉编译器去哪个框架中去找对应的函数或者类
  - 正确的链接应该为 cc main.o -framework Foundation

2、 OC程序各个阶段后缀名对比

语言 | 源文件 | 目标文件 | 可执行文件
--- | ---  |  --- | ---
C   | .c   | .o   | .out
OC  | .m   | .o   | .out


## 数据类型

1、 OC中的数据类型，支持C语言中的所有数据类型
 
 - 基本数据类型 （int double float char）
 - 构造类型 (数组、结构体、枚举)
 - 指针类型 int* p1
 - 空类型 void
 - typedef自定义类型

 2、 新增了一些数据类型

- BOOL类型
  * 可以存储YES或者NO中的任意一个数据
  * 一般情况下BOOL类型的变量用来存储条件表达式的结果，如果条件表达式成立，那么结果就是YES，如果条件表达式不成立，结果就是NO
  * BOOL 的本质 typedef signed char BOOL,实际上BOOL类型的变量是一个有符号的char变量
  * #define YES ((BOOL)1)  #define NO ((BOOL)0) YES实际上就是1 NO实际上就是0


- Boolean类型
  * 可以存储true或者false
  * 一般情况下BOOL类型的变量用来存储条件表达式的结果，如果条件表达式成立，那么结果就是true，如果条件表达式不成立，结果就是false
  * Boolean的本质 typedef unsigned char Boolean,实际上Boolean类型的变量是一个无符号的char变量
  *  #define true 1  #define false 0 true实际上就是1 false实际上就是0

- class类型
- id类型 万能指针
- nil 与NULL差不多
- SEL 方法选择器
- block 代码段类型








