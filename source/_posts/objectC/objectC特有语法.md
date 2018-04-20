---
title: objectC特有语法
date: 2018-04-15 13:20:33
tags: 
  - "objectC"
categories: "objectC语言学习"
---
## 对象在内存中的分配
- 子类对象中有自己的属性和所有父类的属性
- 代码段中的每一个类都有一个叫做isa的指针，这个指针指向它的父类，一直指到NSObject。[p1 sayHi];//假设p1是Person对象，先根据p1指针找到p1指向的对象，然后根据对象的isa指针找到Person类，先搜索Person类中是否有这个sayHi方法，如果有执行，如果没有 就根据类的isa指针找到父类……，找到最后一个基类NSObject 中如果没有则报错


## SEL 

1、SEL全称叫做 selector 选择器。SEL 是一个数据类型，所以要在内存中存储数据。SEL其实是一个类，SEL对象是用来存储一个方法的。
2、 类是以Class对象的形式存储在代码段之中的。如何将方法存储在类对象之中
- 先创建一个SEL对象
- 将方法的信息存储在这个SEL对象之中
- 再将这个SEL对象作为类对象的属性

3、 拿到存储方法的SEL对象

- 因为SEL是一个typedef类型的 再自定义的时候已经加* 了，所以，我们在声明SEL指针的时候 不需要加* 了
- 取到存储方法的SEL对象， SEL s1=@selector(方法名)；

4、 调用方法的本质 [p1 sayHi];内部的原理，先拿到存储sayHi方法的SEL对象，也就是拿到sayHi方法的SEL数据，SEL消息。将这个SEL消息发送给p1对象。这个时候，p1对象接收到这个SEL消息以后，就知道要调用方法。根据对象的isa指针找到存储类的类对象。找到这个类对象以后，在这个类对象中去搜寻是否有和传入SEL数据相匹配的，如果有就执行，如果没有再找父类，直到NSObject

5、 OC最重要的一个机制：消息机制。调用方法的本质就是为对象发送SEL消息

6、 手动的为对象发送SEL消息

- 先得到方法的SEL数据
- 将这个SEL消息发送给P1对象
- 调用对象的方法 将SEL数据发送给对象 

```
    Person *p1= [Person new];
    SEL s1= @selector(sayHi);
    [p1 performSelector:s1];
```


7、 注意事项：

- 如果方法有参数 那么方法名是有冒号的
- 如果方法有参数，如何传递参数，那么就调用
```
    Person *p1= [Person new];
    SEL s1= @selector(sayHi);
    [p1 performSelector:s1];
    [p1 eatWithFood:@"榴莲"];
    SEL s2=@selector(eatWithFood:);
    [p1 performSelector:s2 withObject:@"苹果"];
```

## 点语法

1、 使用点语法来访问对象的属性。语法： 对象名.去掉下划线的属性名;

```
    Person * p2= [Person new];
    p2.name=@"方正";
    p2.age=18;
    p2.gender=GenderMale;
    
    NSString *name=p2.name;
    NSLog(@"%@",name);
```
2、 点语法原理： p2.age=18;//这句话的本质并不是把18赋值给p2对象的_age属性。点语法在编译的时候， 其实会将点语法转化为调用getter、setter的代码


3、 注意点：

- 在getter和setter中慎用点语法，因为可能会造成无限循环，而是程序崩溃
- 点语法在编译的时候， 会转换为调用setter getter方法的代码。如果我们的setter方法和getter方法名不符合规范，则点语法会出问题
- 如果属性没有封装getter setter 是无法使用点语法的

## property

1、@property 作用： 自动生成getter和setter方法的声明。因为是生成方法的声明，所以应该写在@interface类的声明之中。
2、语法 @property 数据类型 名称;
3、原理： 编译器在编译的时候，会根据@property生成getter和setter方法的实现

@property 数据类型 名称；
生成为：

- （void） set首字母大写的名称：(数据类型)名称;
-  (数据类型)名称；

@property int age;想当于下面两句话
- （void)setAge:(int)age;
-  (int)age;

4、 注意
 
 - @property的类型和属性的类型一致，@property的名称和属性的名称一致（去掉下划线）
 - @property的名称决定了生成getter和setter方法的名称，property的数据类型决定了生成getter和setter方法的返回类型
 - @property只是生成getter和setter方法的声明，实现还是需要自己实现，属性要自己定义


 ## synthesize

 1、@property 只能生成getter和setter的声明，实现还是需要自己实现。
 2、@synthesize作用： 自动生成getter、setter方法的实现，所以应该写在类的实现之中。
 3、语法： @synthesize @property名称；
 4、 @synthesize做的事情

 - 生成一个真私有的属性，属性的类型和@synthesize对应的@property类型一致，属性的名字和@synthesize对应的@property名字一致
 - 自动生成setter方法的实现，实现方式：将参数直接赋值给自动生成的那个私有属性。并且没有做任何的逻辑验证
 - 自动生成getter方法的实现，实现方式： 将生成的私有属性的值返回

 5、希望@synthesize不要去自动生成私有属性，getter、setter的实现中操作的是已经写好的属性即可。语法： @synthesize @property名称= 已经存在的属性名； @synthesize age=_age;

 - 不会再去生成私有属性
 - 直接生成setter、getter的实现，setter的实现：把参数的值直接赋值给指定的属性。getter的实现： 直接返回指定的属性的值

 6、注意：
 
 - 如果直接写1个@synthesize @synthesize name；
 - 如果指定操作的属性
 @synthesize name=_name;
 - 生成的setter方法实现中 是没有做任何逻辑验证的 是直接赋值的，生成的getter方法，是直接返回属性中的值

 7、 批量声明

 - 如果多个@property的类型一致，可以批量声明
 @property 类型 属性1，属性2；
 - @synthesize也可以批量声明，类型不一致都没有关系
 @synthesize name=_name,age=_age,weight=_weight,height=_height;

 8、 @property只是生成getter、setter的声明，@synthesize是生成getter、setter的实现，这种写法是Xcode4.4之前的写法，从Xcode4.4以后，Xcode对@property做了一个增强

 9、 @property增强，只需要写一个@property编译器就会自动生成

 - 生成私有属性
 - 生成getter、setter的声明
 - 生成getter、setter的实现

 10 @property增强使用注意：

 - @property的类型一定要和属性的类型一致，名称一定要和属性的名称一致，去掉下划线的名称
 - 也可以批量声明相同类型的@property
 - @property生成的方法实现没有做任何的逻辑验证，所以，如果需要可以重写setter和getter逻辑。两个方法如果同时重写，不会自动生成私有属性了
 - 如果想为类写一个属性，并且要为这个属性封装getter 、 setter，1个@property就搞定了
 - 继承 、 父类的@property一样可以被子类继承，@property生成的属性是私有的，在子类的内部中无法直接访问到私有属性，可以通过setter、getter访问到

## 动态类型和静态类型

1、OC是弱类型语言
2、 静态类型：指的是一个指针指向的是1个本类对象。动态类型：指的是一个指针指向的对象不是本类对象
3、 编译检查，编译器在编译的时候，能不能根据一个指针调用指针指向的对象的方法。判断原则为：看指针所属的类型之中是否有这个方法，如果有就认为可以调用，编译通过，如果没有则报错。这个叫做编译检查，在编译的时候，能不能调用对象的方法主要看指针的类型。我们可以将指针的类型作转换，来达到骗过编译的目的
4、 运行检查，编译检查只是骗过了编译器，但是这个方法究竟能不能运行，在运行时还是会检查。会去检查对象中是否真的有这个方法，如果有则执行，没有则报错

## NSObject 和id指针

1、 NSObject时OC 中所有类的基类，根据LSP NSObject指针就可以指向任意的OC对象，所以，NSObject指针是一个万能指针
缺点： 如果要调用指向的子类对象独有的方法，就必须做类型转换
2、id指针，是一个万能指针，可以指向任意的OC对象

- id是一个typedef自定义类型 在定义的时候已经加了*，所以，声明id指针的时候不需要加 * 了
- id指针是一个万能指针，任意的OC对象都可以指

3、NSObject 和id 的异同

- 通过NSObject指针去调用对象的方法的时候，编译器会去做编译检查。通过id类型的指针去调用对象的方法的时候，编译器不会检查，无论调用什么方法。
- id指针只能调用对象的方法，不能使用点语法，如果使用点语法就会报错


## instancetype

1、如果方法的返回值是instancetype，代表方法的返回值是当前这个类的对象。
2、使用建议

- 如果方法内部是在创建当前类的对象，不要写死为类名，而是用self来代替类名。
- 如果方法的返回值是当前类的对象，也不要写死，而是写instancetype
- id和instancetype的区别，instancetype只能作为方法的返回值，不能在别的地方使用，id即可以声明指针变量，也可以作为参数，也可以作为返回值。instancetype是有类型的，代表当前类的对象。id是一个无类型的指针。

## 动态类型检查

1、 苹果的编译器名称叫做LLVM
2、 判断指针指向的对象中是否有这个方法可以执行

```
    Person * p3=[Person new];
    BOOL b1=[p3 respondsToSelector:@selector(sayHi)];
    if(b1==YES)
    {
        [p1 sayHi];
    }
    else
    {
        NSLog(@"NO");
    }
```
3、 判断指定的对象是否是 指定类的对象或者子类的对象

```
    Person * p3=[Person new];
    BOOL b2= [p3 isKindOfClass:[Person class]];
    NSLog(@"b2=%d",b2);
```

4、 判断指定的对象是否为这个指定类的对象，不包括子类

```
    Person * p3=[Person new];
    BOOL b3= [p3 isMemberOfClass:[Student class]];
    NSLog(@"b3=%d",b3);//不包括Person的子类对象
```

5、 判断是否为另外一个类的子类
```
    BOOL b4=[Student isSubclassOfClass:[Person class]];
    NSLog(@"b4=%d",b4);
```

6、 判断类中是否有指定的类方法

```
    BOOL b5= [Person instancesRespondToSelector:@selector(test)];
    NSLog(@"b5=%d",b5);
```

## 构造函数

1、 类名 * 指针名 = [类名 new];
new 实际是一个类方法 

- 创建对象
- 初始化对象
- 把对象的地址返回

2、 new方法的内部，其实是先调用了alloc方法，再调用了init方法。alloc方法是一个类方法，作用：那个类调用这个方法，就创建这个类的对象，并把对象返回。init方法，作用：是一个对象方法，初始化对象。

3、 创建对象的完整步骤：

- 先使用alloc创建一个对象，然后再使用init初始化这个对象，才可以使用这个对象
- 虽然没有初始化的对象，有的时候，也可以使用，但是不能这么做，使用一个未经初始化的对象是极其危险的

4、 Person *p1 = [Person new];完全等价于
Person * p1=[[Person alloc] init];

5、init方法： 初始化对象，为对象的属性赋初始值，这个init方法我们叫做构造方法。为对象的属性赋默认值。如果属性的类型是基本数据类型就赋值为0，c指针为NULL， OC指针为nil。所以，如果我们创建一个对象，没有为属性赋值，这个对象的属性是有默认值的。


6、 我们想让创建的对象的属性的默认值不是nil NULL 0，而是我们自定义的，这个时候，我们就可以重写init方法，在这个方法中按照我们自己的想法为对象的属性赋值。

7、 重写init方法的规范

- 必须要先调用父类的init方法，然后将方法的返回值赋值给self
- 调用init方法初始化对象有可能会失败，如果初始化失败，返回的就是nil
- 判断父类是否初始化成功，判断self的值是否为nil 如果不为nil说明初始化成功
- 如果初始化成功 就初始化当前对象的属性
- 最后，返回self的值

8、为什么要调用父类的init方法：因为父类的init方法会初始化父类的属性，所以必须要保证当前对象中的父类属性也同时被初始化

```
- (instancetype)init
{
    self =[super init];
    if(self)
    {
        self.name=@"方正2";
    }
    return self;
}
```
9 、 自定义构造方法 规范

- 自定义构造方法的返回值必须是instancetype
- 自定义构造方法的名称必须以initWith开头
- 方法的实现和init的要求一样

```
- (instancetype)initWithName:(NSString *)name andAge:(int)age andGender:(Gender)gender
{
    if(self=[super init])
    {
        self.name=name;
        self.age=age;
        self.gender=gender;
    }
    return self;
}
```
