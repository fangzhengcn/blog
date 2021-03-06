---
title: 内存管理
date: 2018-04-15 13:20:33
tags: 
  - "内存"
categories: "objectC语言学习"
---

## 范围

1、 内存管理的范围，只需要管理存储在堆中的OC对象的回收，其他区域中的数据回收是系统自动管理的

2、 对象应该是什么时候被回收？当有人使用这个对象的时候，这个对象千万不能被回收。

3、引用计算器，每一个对象都有一个属性，叫做retainCount，叫做引用计算器，类型是unsigned long 占据8个字节。引用计数器的作用：用来记录当前这个对象有多少个人在使用它，默认情况下，创建一个对象出来 ，这个对象的引用计数器的默认值为1
4、 当多1个人使用这个对象的时候，这个对象的引用计数器的值加1，代表这个对象多一个人使用。当这个对象的引用计数器变为0时候，代表这个对象无人使用，这个时候，系统就会自动的回收这个对象

5、 如何去操作引用计数器

- 为对象发送1条retain消息，对象的引用计数器就会加1，当多1个人使用对象的时候才发
- 为对象发送1条release消息，对象的引用计数器就会减1，当少一个人使用对象的时候才发
- 为对象发送1条retainCount消息，就可以取到对象的引用计数器的值
- 当对象的引用计数器变为0的时候，对象就会被系统立即回收，在对象被回收的时候，会自动调用对象的dealloc方法

## 内存管理的分类

1、MRC：Manual Reference Counting 手动引用计数，手动内存管理
当多1个人使用的时候，要求程序员手动的发送retain消息，少一个人使用的时候，要求程序员手动发送release消息
2、ARC： Automatic Reference Counting 自动引用计数 自动内存管理，系统自动的在合适的地方发送retain release 消息


## 第一个MRC程序

1、 Xcode默认支持ARC开发，关闭ARC开启MRC，在ARC机制下，retain release dealloc这些方法无法调用
2、 当对象的引用计数器变为0的时候，系统会自动回收对象，在 系统回收对象的时候，会自动调用对象的dealloc方法。

- 重写dealloc方法的规范： 必须要调用父类的dealloc方法，且要放在最后一句代码
3、 测试引用计数器
- 新建一个对象，这个对象的引用计数器的默认值为1
- 当对象的引用计数器变为0的时候，对象就会被系统立即回收，并且自动调用dealloc方法
- 为对象发送retain消息 对象的引用计数器加1
- 为对象发送release消息，并不是回收对象，而是让对象的引用计数器减1，当对象的引用计数器的值变为0的时候，对象才会被系统回收

4、 内存管理的重点

- 什么时候为对象发送retain消息，当多一个人使用这个对象的时候，应该先为这个对象发送retain消息
- 什么时候为对象发送release消息，当少一个人使用这个对象的时候，应该先为这个对象发送release消息

5、 内存管理的原则

- 有对象的创建，就要匹配一个release
- retain的次数和release的次数要匹配
- 谁用谁retain，谁不用谁release
- 只有在多一个人用的时候，才retain，少一个人使用的时候，才release，有始有终，有加就有减，有retain就应该匹配一个release

6、 野指针
- C语言中的野指针： 定义一个指针变量，没有初始化，这个指针的值是一个垃圾值，指向随机的一块空间，这个指针叫做野指针。
- OC中的野指针： 指针指向的对象已经被回收了，这样的指针叫做野指针

7、 对象回收的本质：所谓的对象的回收，指的是对象占用的空间可以分配给别人，当这个对象占用的空间没有分配给别人之前，其实对象数据还在

8、 僵尸对象，1个已经被释放的对象，但是这个对象所占用的空间，还没有分配给别人，这样的对象就叫做僵尸对象。僵尸对象的实时检查机制，可以将这个机制打开，打开之后，只要访问的僵尸对象，无论空间是否分配，就会报错

9、 使用野指针访问僵尸对象会报错，如何避免僵尸对象报错，当一个指针成为野指针以后，将这个指针的值设置为nil。当1个指针的值为nil，通过这个指针去调用对象的方法的时候，不会报错，只是没有任何反应

10、 内存泄漏：指的是1个对象没有及时的回收，在该回收的时候没有回收，一直驻留在内存中，直到程序结束的时候才回收

11、 单个对象的内存泄漏的情况

- 有对象的创建，而没有对应的release
- retain的次数和release的次数不匹配
- 在不适当的时候，为指针赋值为nil
- 在方法中为传入的对象进行不适当的retain

12、在MRC的开发模式下，1个类的属性如果是1个OC对象类型的，那么这个属性的setter就应该按照下面的格式来写 

```
- (void)setCar:(Car *)car
{
    if(_car!=car)
    {
        _car = [car retain];
    }
}

// 还要重写dealloc方法

- (void)dealloc
{
    [_car release];
    [super dealloc]; 
}

```

如果属性的类型不是OC对象类型的，不需要像上面这样写。

13 、 @property

- 自动生成私有属性
- 自动生成这个属性的getter 、 setter方法的声明
- 自动生成这个属性的getter 、 setter方法的实现
- 特别注意，生成的setter方法的实现中，无论是什么类型，都是直接赋值

14、 @property参数

- @property可以带参数，@property（参数1，参数2，参数3……)数据类型 名称;
- @property的四种参数
  * 与多线程相关的两个参数 atomic、nonatomic
  * 与生成的setter方法的实现相关的参数 assign、 retain
  * 与生成只读、读写相关的参数 readonly readwrite
  * 与生成的getter、setter方法名字相关的参数 getter setter


15、 与多线程相关的参数

  - atomic： 默认值。如果写atomic，这个时候生成的setter的代码就会被加上一把线程安全锁。特点：安全、效率低下
  - nonatomic: 如果写nonatomic 这个时候生成的setter方法的代码就不会加上线程安全锁。特点： 不安全，效率高


16、 与生成的setter方法的实现相关的参数

- assign： 默认值 生成的setter方法的实现就是直接赋值
- retain： 生成的setter方法的实现就是标准的MRC内存管理代码，也就是，先判断新旧对象是否为同一对象，如果不是 release旧的， retain新的
当属性的类型是OC对象的类型的时候，那么就使用retain，当属性的类型是非OC对象的时候，使用assign。千万注意： retain参数，只是生成标准的setter方法为标准的MRC内存管理代码，不会自动的在dealloc中生成release的代码，所以，我们还要自己手动的在dealloc中release

17、 与生成只读、 读写的封装
readwrite： 默认值，代表同时生成getter setter
readonly： 只会生成getter不会生成setter

18、与生成getter、 setter方法名称相关的参数
默认情况下，@property生成的getter setter方法的名字都是标准的名字。其实可以通过参数来指定@property生成指定的名字。 getter = getter方法名字，用来指定@property生成的指定的名字； setter = setter方法名字，用来制定@property生成的setter名字，注意 setter方法是带参数的，所以要加上1个冒号。如果使用了getter setter修改了生成方法的名字，在使用点语法的时候，编译器会自动转换为修改后的名字方法。

## @class

1、 当两个类互相包含的时候， 当Person.h中包含Book.h，而Book.h中又包含Person.h。这个时候，就会出现循环引用的问题，导致无法编译通过。

2、 解决方案： 其中一个类，不要使用#import引入对方的头文件，而是使用@class 类名；来标注这是一个类，这样就可以在不引用对方头文件的情况下，告诉编译器这是一个类

3、 @class与#import的区别

- #import是将指定的文件内容拷贝到指定的地方
- @class并不会导入拷贝任何内容，只是告诉编译器这是一个类，这样编译器在编译的时候，知道这是一个类
- 在.m文件中，再#import对方的头文件，就有方法提示使用了

## 循环retain

1、 当两个对象相互引用的时候，A对象的属性是B对象，B对象的属性是A对象，这个就叫相互引用，如果两边都使用retain，那么就会发生内存泄漏。

2、 解决方案： 1端使用retain 另外一端使用assign，使用assign的那一端再dealloc中不再使用release了

## 自动释放池

1、 自动释放池的原理： 存入到自动释放池的对象，在自动释放池被销毁的时候，会自动调用存储在该自动释放池中的所有对象的release方法。可以解决的问题：将创建的对象，存入到自动释放池之中，就不需要手动的release这个对象了，因为池子销毁的时候，就会自动的调用池中所有的对象的release。自动释放池的好处：将创建的对象存储到自动释放池中，不需要再写release

2、 如何创建自动释放池
@autoreleasepool
{

}
这对大括弧代表这个自动释放池的范围

3、 如何将对象存储到自动释放池之中，在自动释放池之中调用对象的autorelease方法，就会将这个对象存入到当前自动释放池之中。这个autorelease方法返回的是对象本身，所以，可以这样写
@autoreleasepool
{
    Person *p1 = [[[Person alloc] init] autorelease];

}
这个时候，当这个自动释放池执行完毕之后，就会立即为这个自动释放池中的对象发送一条release消息

4、 使用注意

- 只有在自动释放池之中调用了对象的autorelease方法，这个对象才会存储到这个自动释放池之中。如果只是将对象的创建代码写在自动释放之中，而没有调用对象的autorelease方法，是不会将这个对象存储到这个自动释放池之中的

- 对象的创建可以在自动释放池的外面，在自动释放池之中，调用对象的autorelease方法，就可以将这个对象存储到这个自动释放池之中

- 如果对象的autorelease方法的调用是在自动释放池外面，是无法将其存储到这个自动释放池之中的

- 当自动释放池结束的时候，仅仅是对存储在自动释放池中的对象发送一条release消息，而不是销毁对象

- 如果在自动释放池在调用多次autorelease，就会将这个对象存储到自动释放池中多少次。在自动释放池结束的时候，会为这个对象发送多条release消息，那么这个时候就会出现僵尸对象错误

- 如果在自动释放池中，调用了存储到自动释放池中的对象的release方法，在自动释放池结束的时候，还会再次调用对象的 release方法，这个时候就会有可能造成野指针错误，也可以调用存储在自动释放池中的retain方法

- 将对象存储到自动释放池之中，并不会使对象的引用计算器加1，所以好处是，创建对象将对象存储在自动释放池，就不需要再写release了

- 自动释放池可以嵌套，调用对象的autorelease方法，会将对象存储到当前对应的自动释放池之中去


