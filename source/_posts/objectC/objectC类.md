---
title: objectC类
date: 2018-04-15 13:20:33
tags: 
  - "objectC"
categories: "objectC语言学习"
---

## 内存中的五大区域

- 栈 存储局部变量
- 堆 程序员手动申请的字节空间 malloc calloc realloc 函数
- BBS段 存储未被初始化的全局变量 静态变量
- 数据段（常量区） 存储已被初始化的全局变量 静态变量 常量数据
- 代码段  存储代码，存储程序的代码

## 类加载

1、 在创建对象的时候，肯定是要访问类的
2、 声明一个类的指针变量也会访问类的。在程序运行期间，当某个类第一次被访问到的时候，就会将这个类存储到内存中的代码段区域，这个过程叫做类加载。只有第一次访问的时候，才会做类加载，一旦类被加载到代码段中，程序结束后才会释放代码段空间。

## 对象在内存中是如何存储的

1、 Person *p1 = [Person new]; Person *p1;会在栈内存中申请一块空间，在栈内存中声明一个Person类型的指针变量p1。p1是一个指针变量，那么只能存储地址。
2、 [Person new];真正在内存中创建对象的是这句代码。new 的作用：

  * 在堆内存中申请一块合适大小的空间
  * 在这个空间当中根据这个类的模版来创建对象，类模版中定义了什么属性，就把这些属性依次的声明在对象之中。对象中还有另外一个属性，叫做isa 是一个指针，指向对象所属的类在代码段中的地址。
  * 初始化对象的属性，如果属性是基本类型，那么就赋值为0；如果属性是c语言的指针对象，那么就赋值为null；如果属性是oc的类指针类型 那么就赋值为nil;
  * 返回对象的地址

3、 注意

- 对象中只有属性，没有方法。自己类的属性外加一个isa指针指向代码段中的类
- 如何访问对象的属性  指针名->属性名； 根据指针 找到指针指向的对象 再找到对象中的属性来访问。
- 如何调用方法 [指针名 方法名]; 先根据指针名找到对象，对象再根据isa指针找到类，然后调用类的方法
- 为什么不把方法设计到对象中，因为每一个对象的方法实现都是一模一样的，没有必要为每一个对象都保存一个方法，这样的话就太浪费空间了

## NULL 与 nil

1、 NULL只能作为指针变量的值，如果一个指针变量的值是NULL值，代表这个指针不指向内存中的任何1块空间。NULL其实等价于0 NULL其实是一个宏，就是0

2、 nil 只能作为指针变量的值，代表指针变量不指向内存中的任何空间。nil等价于0，也是一个宏，就是0。所以，NULL和nil其实是一样的。

3、 虽然使用NULL的地方可以使用nil,使用nil的地方可以使用NULL，但是最好还是区分使用。C指针使用NULL，OC的类指针使用nil

4、 如果一个类指针的值为nil,代表这个指针不指向任何对象
Person *p1 = nil; 那么这个时候，如果通过指针去访问p1指向的对象的属性，这个时候会运行报错。这个时候，如果通过这个指针去调用对象的方法的时候，运行不会报错，但是方法不会执行。

## 分组 导航标记

1、 #pragam mark 分组名，就会在导航条对应的位置显示一个标题
2、 #pragam mark -,就会在导航条对应的位置显示水平分割线
3、 #pragam mark - 分组名，就会在导航条对应的位置显示水平分隔符再现实标题

## 函数与方法的对比

1、 相同点： 都是表示一段独立的功能
2、 不同点：

- 语法不同
- 定义的位置不同，OC方法的声明只写在@interface的大括弧外面，实现只能写在@implementation之中，函数除了在函数的内部和@interface的大括弧之中，其他地方都可以写
- 就算吧函数写在类中，这个函数仍然不属于类，所以创建的对象中也没有这个函数，注意，函数不要写在类中，然后可以，但是这种不规范
- 调用的方式不一样，函数可以直接调用，但是方法必须要创建对象，通过对象来调用。函数是独立的，方法是有依赖的。

## 多文件开发

1、 所有的类都写在main.m这个源文件之中，后果：后期的维护非常的不方便，不利于团队开发
2、 把一个类写在1个模块之中，一个模块至少包含两个文件

- .h 头文件 写类的声明

  因为要用到Foundation框架中的类 NSObject 所以在这个头文件中要引入Foundation框架的头文件，然后再将类的声明部分写在.h文件中

- .m 实现文件 写类的实现
  先引入模块的头文件 这样才会有类的声明，然后再写类的实现

- 使用此类

  只需要引入这个模块的头文件就可以直接使用了

3、 添加类模块更加简洁的方式 NewFile Cocoa Class 自动生成模块文件 .h .m 自动的将类的声明和实现写好。

## 类在方法中的使用

1、 作为方法的参数
- 语法 （void）test:(Dog *)dog;
- 调用方法的时候，如果方法的参数是一个对象，那么给实参的时候，实参也要求必须是一个符合要求的对象，否则会出问题
- 当对象作为方法的参数传递的时候，是地址传递，所以，在方法内部通过形参去修改形参指向的对象的时候，会影响实参变量指向的对象的值。

2、 作为方法的返回值，如果方法的返回值是一个对象，那么返回值应该写 类指针

## 异常处理

1、 目的：程序执行的时候，如果发生异常，为了不让程序崩溃，可以继续执行下去。

2、 语法：
@try
{

}
@catch(NSException *ex)
{

}
将有可能发生异常的代码放在@try中，当@try中的代码在执行的时候，如果发生异常而不崩溃，而是会立即跳转到@catch中去执行里面的代码。当@catch的代码执行完毕之后，继续往下执行。如果try中的代码没有异常，则会略过@catch往下执行。

3、 当@try中的代码在执行的时候发生了异常，@try块发生异常后面的代码是不会执行的，而是立即跳转到@catch中去。

4、 @catch中的代码只有在@try的代码发生异常的时候才会执行，所以@catch中一般情况写记录异常日志的代码，为后续处理问题提供线索。

5、 @catch的参数NSException *ex 通过%@打印出ex指向的对象的值，可以拿到发生异常的原因


6、 @try...@catch后面还可以跟一个@finally,@finally中的代码，无论@try中是否发生了异常都会执行
```
       @try
        {
        Person *p1=[Person new];
        [p1 sayHi];
        }
        @catch(NSException *ex)
        {
            NSLog(@"发生异常了，原因：%@",ex);
        }
        @finally
        {
            NSLog(@"无论发生不发生异常我都会走到的");
        }

```
7、 @try...@catch并不是万能的，不是所有运行的错误都可以处理，c语言的异常是无法处理的。避免异常最常用的方式，还是逻辑判断。


## OC中类中的方法分为两种

1、 对象方法或称实例方法： 要先创建对象，再通过对象名来调用

2、 类方法 ，不依赖于对象，直接可以使用类名来调用

- 语法： 类方法声明使用 + 号   + （返回值类型)方法名； 和对象方法的声明实现除了使用+号和-号以外，都是一样的

- 调用： 不需要创建对象，直接使用类名来调用 [类名 类方法名];

3、 类方法 不能直接访问属性，属性是跟随对象一起创建的。类方法，也不能通过self直接来调用对象方法

4、 在对象方法中可以直接调用类方法

5、 关于类方法的规范
 - 如果我们写一个类，那么就要求为这个类提供一个和类名同名的类方法，这个类方法创建一个纯净的类对象返回，苹果写的类都遵循此规范

 - 如果希望创建的对象属性的值由调用者指定，那么就为这个类方法带参数


 ## NSString

 1、 NSString是一个数据类型，用来保存OC字符串的
 2、 其实NSString是Foundation框架中的1个类，作用，是用来存储OC字符串的
 3、 完整标准的创建NSString对象的方式 为 
```
 // 这种方式创建的字符串是空字符串 @“”
 NSString *str1=[NSString new];
 NSString *str2=[NSString string];

 // 并为属性赋值
 NSString *str1=[NSString stringWithFormat:@"jack"];

// @"jack" 这个本质上是一个NSString对象 str2的值是这个对象的地址
NSString *str2=@"jack";
NSLog(@"str1指针变量的值%p",str2);
NSLog(@"str1指针变量的指向的对象%@",str2);

```

4、 NSString常用的方法
 
···


## 匿名对象

1、定义，没有名字的对象，如果创建一个对象，没有用一个指针存储这个对象的地址，也就是没有任何指针指向这个对象，那么这个对象就叫做匿名对象

2、使用
```
            [Person new]->_name=@"lisa";
            [[Person new] sayHi];
```

## _setter封装

1、 去掉@public 外界无法访问这个属性，也就无法赋值了。
2、 为类提供一个方法，这个方法专门为这个属性赋值，这个方法叫做setter，这个方法一定是一个对象方法，因为这个方法要为属性赋值。这个方法没有返回值，因为这个方法只为属性赋值。这个方法的名字必须以set开头，跟上，去掉下划线首字母大写的属性名。这个方法的参数跟属性的类型一致。参数的名称和属性的名称一致。
3、 外界想要为对象的属性赋值，那么就调用这个对象的setter方法，将要赋值的数据传入这个方法。方法会对这个数据进行验证，如果符合验证，就会把数据赋值给属性 否则会做默认处理


## _getter封装
1、 为对象属性赋值的逻辑验证已经解决了，但是外界无法取出属性的值，这个时候，就专门需要写个getter方法，来返回属性的值。
2、 这个方法一定是个对象方法，这个方法肯定有返回值，且和属性的类型一致，这个方法的名称直接就是属性名称（去掉下划线的属性名称），这个方法没有参数。


## static
1、 在C语言中的static
- 修饰局部变量
- 修饰全局变量
- 修饰函数

2、 OC中的static关键字
- static 不能修饰属性，也不能修饰方法
- static 可以修饰方法中的局部变量，如果方法中的局部变量被static修饰，那么这个变量就会变成静态变量，存储在常量区，当方法执行完毕之后，不会回收，下次执行这个方法的时候，不会再声明了。

3、 如果方法的返回值是当前类的对象，那么方法的返回值就写为instancetype


## self 

1、 在方法的内部可以定义一个和属性名相同的局部变量。这个时候，如果在方法中，访问同名的变量，访问的是局部变量。

2、 在一个对象方法中要调用当前对象的另外一个对象方法。
3、 可以在对象方法和类方法中使用，self是一个指针，在对象方法中self指向当前对象，在类方法中self指向当前类。
4、 self用在对象方法中

- self在对象方法中，指向当前对象，谁调用方法谁就是当前对象
- 在对象方法中，self可以显示的访问当前对象的属性
- 可以使用self来调用当前对象的其他的对象方法

5、 self用在类方法中

- 类加载，当类第一次被访问的时候，会将类的代码存储在代码区，代码区中用来存储类的空间也有1个地址

- 在类方法中，self也是一个指针，指针指向类在代码段中的地址。self在类方法中，就相当于当前类

6、 取到类在代码段中的地址方法

- 调试查看对象的isa指针的值
- 在类方法中查看self的值
- 调用对象的对象方法class 就会返回这个对象所属的类在代码段中的地址
- 调用类的类方法class 就会返回这个类在代码段中的地址

7、 对象方法可以声明多次，但是只会认为有一次，对象方法如果有多次声明只能实现一次，否则会报错。对象方法之间是不能重名的，类方法之间也是不可以重名的。但是，对象方法和类方法是可以重名的，通过类名来调用的是类方法，通过对象名来调用的是对象方法。

