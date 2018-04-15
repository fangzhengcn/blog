---
title: objectC面向对象
date: 2018-04-15 13:20:33
tags: 
  - "objectC"
categories: "objectC语言学习"
---
##  定义类的语法 

1、 位置：直接写在源文件中，不要写在main函数之中
2、 类的定义分为两部分
> 类的声明
> @interface 类名:NSObjct
> {
>    这类事物具有的共同特征，将他们定义为变量
> }
> 功能就是1个方法，将方法的声明写在这里
> @end

> 类的实现
> @implementation 类名
> 将方法的实现写在这里
> @end

3、 注意：
 
 - 类必须要有声明和实现
 - 类名用描述的事物的名称来命名就可以了，类名的首字母必须要以大写开头
 - 需要继承NSObject
 - 用来表示这类事物共同特征的变量，必须要定义在@interface的大括弧之中
 - 定义在大括弧之中用来表示这类事物共同特征的变量，叫做属性 成员变量 实例变量 字段
 - 为类定义属性的时候，属性的名称，一定要用下划线_开头


 ## 对象

 1、 语法
 > 类名* 对象名 = [类名 new];
 > Person* p1 = [Person new];
 > 根据Person这个类的模版，创建了一个对象叫做p1

 2、如何使用对象

 - 访问对象的属性
   * 默认情况下，对象的属性是不允许被外界访问的
   * 如果需要被外界访问，那么在声明的时候要加上一个@public关键字

- 访问方式
  * 对象名->属性名 = 值
  * 对象名->属性名

  * (*对象名).属性名；


```
@interface Student:NSObject
{
    @public
    NSString* _name;
    int _age;
    int _yuWen;
    int _shuXue;
    int _yinYu;
}
@end

@implementation Student

@end

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Student* s1 = [Student new];
        s1->_name = @"fangzheng";
        s1->_age = 17;
        s1->_yuWen =90;
        s1->_shuXue=93;
        s1->_yinYu=34;
        NSLog(@"s1学生的名称为%@，年龄为%d,语文为%d,数学为%d,英语为%d",s1->_name,s1->_age,s1->_yuWen,s1->_shuXue,s1->_yinYu);
    }
}
```

3、 无参数的方法

- 声明： 位置在@interface的大括弧外面
- 语法： - (返回值类型) 方法名称;
- 实现： 位置在@implementaction之中实现
- 语法； 将方法的声明拷贝到@implementaction之中去， 去掉分号，追加大括弧1对，将方法的实现写在大括弧之中
- 调用： 方法是无法直接调用的，因为类是不能直接使用的，必须要先创建对象，那么对象中就有类的属性和方法了，就可以调用对象的方法了
- 调用对象的方法： [对象名 方法名]

4、 带一个参数的方法
- 声明： 位置在@interface的大括弧外面
- 语法： - (返回值类型) 方法名称:(参数类型)形参名称;
- 实现： 位置在@implementaction之中实现
- 语法： 将方法的声明拷贝到@implementaction之中去， 去掉分号，追加大括弧1对，将方法的实现写在大括弧之中
- 调用： 方法是无法直接调用的，因为类是不能直接使用的，必须要先创建对象，那么对象中就有类的属性和方法了，就可以调用对象的方法了
- 调用对象的方法： [对象名 方法名:实参];
- 方法头中的数据类型都要用小括号括起来
- （返回值类型）方法名称：（参数类型）参数名称;

5、 带多个参数的方法
- 语法： - (返回值类型)方法名称:(参数类型)形参名称1 :(参数类型)形参名称2 :(参数类型)形参名称3;
- 实现和调用和上面一样，调用方法为： 返回值类型 变量=[对象名 方法名:实参1 :实参2]；

6、 带参数的方法声明规范：

- 如果方法只有一个参数，要求最好这个方法的名字叫做 xxxWith,或者xxxWithxxx,提高代码可读性
- 如果方法有多个参数，建议方法名称：
方法名With:(参数类型)参数名称 and:(参数类型)参数名称 and:(参数类型)参数名称；
- 更详细的写法： 方法名With参数1:(参数类型)参数名称 and参数2:(参数类型)参数名称 and：(参数类型)参数名称；

7、 方法中可以直接访问属，在方法当中访问的属性是对象的属性

**案例**

```
#import <Foundation/Foundation.h>
@interface Person:NSObject
{
    @public
    NSString* _name;
    int _age;
    float _height;
}
- (void)run;// 声明一个无参数的方法 方法名字叫做run
- (void)eat:(NSString*)foodName;//定义了一个方法，没有返回值，方法的名字为eat，这个方法有一个参数，类型是NSString，参数的名字叫做foodName
- (int)sum:(int)num1 :(int)num2; //定义了一个方法，返回值为int类型，方法的名称是sum,有两个参数num1和num2
@end

@implementation Person
- (void)run
{
    NSLog(@"我要跑步");
}
- (void)eat:(NSString*)foodName
{
    NSLog(@"%@真好吃",foodName);
}
- (int)sum:(int)num1 :(int)num2
{
    int  num3=num1+num2;
    return num3;
}
@end

@interface Student:NSObject
{
    @public
    NSString* _name;
    int _age;
    int _yuWen;
    int _shuXue;
    int _yinYu;
}
@end

@implementation Student

@end

int main(int argc, const char * argv[]) {
    @autoreleasepool {
        Student* s1 = [Student new];
        s1->_name = @"fangzheng";
        s1->_age = 17;
        s1->_yuWen =90;
        s1->_shuXue=93;
        s1->_yinYu=34;
        NSLog(@"s1学生的名称为%@，年龄为%d,语文为%d,数学为%d,英语为%d",s1->_name,s1->_age,s1->_yuWen,s1->_shuXue,s1->_yinYu);
        Person* p1 = [Person new];
        p1->_name =@"jack";
        p1->_age=19;
        p1->_height=168;
        NSLog(@"p1对象的名称为%@",p1->_name);
        // insert code here...
        NSString *name=@"jack";
        NSLog(@"Hello, %@!",name);
        [p1 run];
        [p1 eat:@"大米饭"];
        int sum= [p1 sum:1 :2];
        NSLog(@"sum=%d",sum);
    }
    return 0;
}

```








