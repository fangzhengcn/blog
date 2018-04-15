---
title: typedef
date: 2018-04-12 13:30:33
tags: 
  - "typedef"
categories: "c语言学习"
---

## 作用
为一个已经存在的数据类型取一个别名。
如果想要使用这个类型，直接使用这个别名就可以了。

## 语法
typedef 已经存在的数据类型 别名；

typedef int fzint; //为int类型取了一个别名，叫做fzint;

```
    typedef char* string;
    string name="李雷";
    string address="中科路37号";
    printf("Hello, World!my name is %s,my address is %s\n",name,address);
```

size_t 其实就是unsigned long

## 使用场景
当数据类型很长的时候，可以为数据类型取一个短一点的别名，用起来方便。

 1、使用typedef 为结构体类型取别名
 *  第一种方式 先声明一个一个结构体，然后使用typedef为这个结构体取一个短别名


```
struct Student
{
    char* name;
    int age;
    int score;
};
typedef struct Student Student;

int main(int argc, const char * argv[]) 
{
    Student stu1 ={"小明",18,89};
}
```

 * 第二种方式 声明结构体类型的同时，就用typedef来为结构体类型取1个短别名

 ```
typedef struct Student
{
    char* name;
    int age;
    int score;
}Student;

int main(int argc, const char * argv[]) 
{
    Student stu1 ={"小明",18,89};
}
 ```

 * 第三种方式，声明匿名结构体的同时，就使用typedef来为结构体类型取一个短别名,最常用的方式

 ```
typedef struct
{
    char* name;
    int age;
    int score;
}Student;

int main(int argc, const char * argv[]) {
    Student stu1 ={"小明",18,89};
}

 ```

 2、 使用typedef 为一个枚举类型取一个短别名

 * 第一种方式

```
enum Direction
{
    DirectionEast=10,
    DirectionSouth=20,
    DirectionWest=30,
    DirectionNorth=40
};
typedef enum Direction Direction;

int main(int argc, const char * argv[])
{
    Direction dir = DirectionEast;
}
```
* 第二种方式

```
typedef enum Direction
{
    DirectionEast=10,
    DirectionSouth=20,
    DirectionWest=30,
    DirectionNorth=40
}Direction;

int main(int argc, const char * argv[])
{
    Direction dir = DirectionEast;
}

```

* 第三种方式，使用上面这种方式的时候，不要写名称，直接使用匿名的方式来写

```
typedef enum
{
    DirectionEast=10,
    DirectionSouth=20,
    DirectionWest=30,
    DirectionNorth=40
}Direction;

int main(int argc, const char * argv[])
{
    Direction dir = DirectionEast;
}
```

