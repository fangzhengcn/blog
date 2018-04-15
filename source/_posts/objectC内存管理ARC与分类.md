---
title: 内存管理ARC与分类
date: 2018-04-15 13:20:33
tags: 
  - "内存"
categories: "objectC语言学习"
---
## ARC

1、 在ARC机制下，当两个对象相互引用的时候，如果两边都使用strong 那么就会先出现内存泄漏，解决方案：1端使用strong 1端使用weak

## @property参数总结

1、与多线程相关的参数

atomic： 默认值，安全，但是效率低下
nonatomic： 不安全，但是效率高

无论在ARC还是MRC都可以使用，使用建议：无论是ARC还是MRC，都使用onatomic

2、 retain： 只能用在MRC模式下，代表生成的setter方法是标准的内存管理代码，当属性的类型是OC对象的时候，绝大多数情况下，使用retain。只有在出现了循环引用的时候1边retain 1边assign

3、 assign： 在ARC和MRC的模式下都可以使用assign，当属性的类型是非OC对象的时候，使用assign。

4、 strong： 只能使用在ARC机制下，当属性的类型是OC对象类型的时候，绝大多数情况下使用strong，只有出现了循环引用的时候，一端strong，一端weak

5、 weak： 只能使用ARC机制下，当属性的类型是OC对象的时候，只有出现了循环引用的时候，1端strong，1端weak

6、 readonly readwrite 无论是ARC还是MRC 都可以使用
7、 setter getter 无论在ARC还是MRC下都可以改
8、 在ARC机制下，原来使用retain的用strong，出现循环引用的时候，MRC：1边retain，1边assign  ARC： 一边weak 一边strong

## ARC与MRC相互兼容

1、 程序使用的是ARC机制开发的，但是其中的某些类使用的是MRC，使用命令 -fno-objc-arc

2、 可以将整个MRC程序，转换为ARC程序 菜单栏-》Edit-》Convert-》To Object-C ARC

## 分类、类别、类目、category

1、 写一个学生类：类中有很多方法，如果将这些方法都写在类模块中，就显得臃肿，不好维护。默认情况下1个类1个模块，这个时候将所有的成员都写在1个模块中，就很难管理，就让一个类占多个模块，将功能相似的方法定义在同一个模块当中，这样的好处，方便维护管理

2、 如何将1个类分成多个模块？ 分类就是顾名思义：将一个类分为多个模块

- NewFile-》ObjectC File -》 选择Category
- 会生成一个.h和一个.m的模块，模块的文件名： 本类名+分类名.h 本类名+分类名.m
- 添加的分类也分为声明和实现，@interface 本类名（分类名）@end 代表不是创建一个类，而是对已有的类添加一个分类，小括弧中写上这个分类的名字。因为一个类可以添加多个分类，为了区分每一个分类，所以分类需要取个名字
- @implement 本类名 （分类名） @end，分类的实现

3、 分类的使用

- 如果要访问分类中定义的成员，就要把分类的头文件引进来

4、 分类的作用：将一个类分成多个模块

5、 分类的使用注意

- 分类只能增加方法，不能增加属性
- 在分类之中可以写@property 但是不会自动生成私有属性，也不会自动生成getter setter的实现，所以，就需要自己写getter和setter的实现，也需要自己定义属性，这个属性必须定义在本类中
- 在分类的方法实现中不可以直接访问本类的真私有属性（定义在本类的@implementation之中）但是可以调用本类的 getter和setter方法来访问属性
- 当分类中有和本类中有同名方法的时候，优先调用分类的方法，哪怕没有引入分类的头文件。如果多个分类有相同的方法，优先调用最后编译的分类

## 非正式协议

1、 为系统自带的类写分类，这个就叫做非正式协议

2、 分类的第二个作用，为一个已经存在的类添加方法

3、 举个例子，判断一个字符串中有多少个数字，需要在NSString类中新增一个方法，代码如下

```
#import "NSString+stringExt.h"

@implementation NSString (stringExt)
- (int)numberCount
{
    int count=0;
    for(int i=0;i<self.length;i++)
    {
        unichar ch= [self characterAtIndex:(i)];
        // 注意，输出中文需要使用，%大C，一个中文字符在OC中占用两个字节，在C语言中占用3个字节
        NSLog(@"我是第%d个字符串，我的值为：%C,我的asiicode为%d",i+1,ch,ch);
       // 注意，一定要比较 字符串的0 和9 ，这样才是asc码的值
        if(ch>'0'&&ch<='9'){
            count=count+1;
        }
    }
    return count;
}

@end
```
