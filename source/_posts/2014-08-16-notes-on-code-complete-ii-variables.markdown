---
layout: post
title: "Notes on Code Complete II: Variables"
date: 2014-08-16 12:00
comments: true
categories: software_craftsmanship
---

## III Variables

### 10 General Issues in Using Variables

**Abstract** 变量的声明与使用遵循精准确定、单一集中原则

**Keywords** proximity principle

尽可能显式声明变量类型

在靠近使用的地方初始化变量，而不是一开始的附近。因为 **Principle of Proximity(相关的东西要放一起)**

1. 可能在使用的时候早就被其他(很可能后来新加的)代码修改了
2. 集中性的一次性初始化多个变量，会造成这些变量将在整个代码中用到的错觉
3. 围绕这小片代码将来可能会加循环之类的结构，导致bug

在靠近使用的地方声明、定义变量

尽量使用final和const

初始化0是一个不错的选择，Intel处理器0xCC, 有些debugger容易识别的有0xDEADBEEF

尽量让变量短命 live time = 最初到最后对变量引用的行数，比如，一开始选择最保守的visibility，需要时再拓展

使用常量不要hard-coding，用类似`private static final int COLOR_BLUE = 0xFF;` 或者`color = ReadTitleBarColor()` 越早绑定优化程度越高，越迟绑定灵活性越高（更高的灵活性会导致更高的复杂性）。看如何权衡。

Use each variable for EXACTLY one purpose.

避免hybrid coupling: `pageCount=-1`表示错误

### 11 The Power of Variable Names

**Abstract** 变量的命名极为重要且有章可循

**Keywords** 8~20, qualifier, naming conventions

命名要充分、准确。当然一串对于该变量的描述是最准确的，但是，很可能会长得离谱。

按what命名，不按how命名。能用problem domain的名称就不要用计算机方面的名称。

研究表明平均变量名长度10~16是debug最容易的，其次是8~20。

Good example: numTeamMembers, teamMemberCount, numSeatsInStadium, seatCount, teamPointsMax, pointsRecord.

太长或太短的命名就完全不能用吗？当然不是，更长的变量适合于少用到的变量或者全局变量，更短的变量适合于局部或者loop里的变量。

为了避免命名冲突，有namespace的编程语言用namespace，没有的在variable名称前加前缀。

如果命名要用qualifier（限定词）,比如Total, Sum, Average, Max, Min, Record, String, or Pointer, 把限定词放到命名最后。除了num，放在前面代表总数，放在后面代表index，所以尽量不要用num，而是用Count后缀和Index后缀。(我个人喜欢用i前缀)

用约定俗成的反义词

- begin/end
- first/last
- locked/unlocked
- min/max
- next/previous
- old/new
- opened/closed
- visible/invisible
- source/target
- source/destination
- up/down

循环: 如果index仅限于循环内部，用i,j,k，如果1）循环外部也要用 2）循环过长 3）nested 则考虑有意义的名称

状态: 用比flag更有意义的状态变量名称

临时变量: 对临时变量持怀疑态度，能不用就不用

bool: 用done, error, found, success or ok. 用is前缀是一个不错的做法。用正向含义的名词，不要用类似于notFound这种。

不用的语言和项目有不同的naming conventions，P277有例子

如果两个变量的命名可以互换而不会影响程序，两者的命名是有问题的。

用数组`file[]`不要用`file1`, `file2`.

### 12 Fundamental Data Types

**Abstract** 数据类型们有着各自的使用规则。本章单独列出了许多C语言的注意事项，足以说明C语言在数据类型方面的使用很容易出错。

**Keywords** int, float, char, string, bool, enum, named constants, array, user-defined types

Avoid "magic numbers"(literal numbers such as 100 or 47524 出现在程序中而不加解释)唯一能够出现的数字literals应该是0和1，作为初始量或者增量。

Chars and Strings

- All strings in Java are Unicode. 但是C/C++处理Unicode要用专门的库
- 在软件开发早期就考虑国际化的问题

Strings in C

- 区分string pointers和char arrays
    - C几乎所有的字符串操作都是通过strcmp(), strcpy(), strlen()等完成的，`StringPtr = "text";`此处“text”是指向literal text的指针，`=`并没有拷贝内容的作用
- `char string[NAME_LENGTH + 1] = { 0 };` 尽量用array of chars
- 用`strncpy()`, `strncmp()` 不用`strcpy()`, `strcmp()`，因为至少有一个parameter保证函数一定会终止

bool: 当判断条件太多，用bool variable表示可读性更高

Enum

- 与switch合用的时候，default处理错误的case
- 为enum定义first last 元素用于enumeration (First(0), A(0), B(1), C(2), Last(2)) 这样一个loop就能搞定了

Array

- 在C里面可以用宏`#define ARRAY_LENGTH(x) (sizeof(x)/sizeof(x[o]))`表示数组的长度

用户自定义类型的时候尽量以现实世界的实体命名，尽量用class而不是typedef

### 13 Unusual Data Types

**Abstract** 什么时候使用struct? pointer有哪些注意事项？使用全局数据导致的问题如何解决？

**Keywords** struct, pointer, memory corruption, global data

### struct

group related data

> C/C++ struct vs. class: In C++, structs and classes are pretty much the same; the only difference is that where access modifiers (edit: for member variables, methods, and for base classes) in classes default to private, access modifiers in structs default to public. However, in C, a struct is just an aggregate collection of (public) data, and has no other class-like features: no methods, no constructor, no base classes, etc. Although C++ inherited the keyword, it extended the semantics. (This, however, is why things default to public in structs—a struct written like a C struct behaves like one.)

### pointer

= position + how to interpret contents from that position

最常见的错误是指针指向了不该指向的地方，（这时候向该内存写数据）会导致memory corruption.

解决策略很简单，尽早甚至一开始就规避这种错误

- 把指针操作限制在函数或者类的内部
- 定义声明和初始化在一起(proximity principle)
- 对指针持怀疑态度，无论是指针本身，还是指针指向的变量，使用之前都要检查，怀疑他们超出范围了
- 用额外的dog-tag空间(和原空间在一起)以检查memory corruption. e.g. isOK tag for `free`
- 不要链条似地使用指针
- 画图让问题更直观
- 预留空间以在空间耗尽时备用
- free/allocate pointers 放在同一scoping level
- delete前要检查（并填充专门的垃圾数据），delete完要归NULL
- 能不用指针还是不用指针的好

#### C++ Pointer Pointers

pointers vs. references?

- The most significant differences are that a reference must always refer to an object, whereas a pointer can point to NULL; and what a reference refers to can’t be changed after the reference is initialized.

Use pointers for "pass by reference". Use `const` references for "pass by value".

用smart pointer: [unique_ptr vs. shared_ptr](http://stackoverflow.com/questions/6876751/differences-between-unique-ptr-and-shared-ptr) (there can only be one unique_ptr to any resource, any attempt to make a copy of a unique_ptr will cause a compile-time error.)

#### C-pointer Pointers

使用显示类型的指针

尽量避免type-casting

使用 sizeof() 内存分配时决定变量大小

### Global Data

全局变量因为scope过大，导致该变量会对其他模块产生不可预料的影响，不到万不得已，不用全局变量。

不要直接访问类成员变量就好像他们是全局变量似的，即便编程语言允许你这样做。

- 用access routines (like getter, setter in Java) 而非直接访问成员有什么好处呢？好处极多(P352)，简单来讲
    - 模块化、灵活性：允许变量和内部的实现与外部的交互分离，而外部的访问方式保持不变，代码改变的时候(变量变量，变化的情况还蛮多的)就不会影响到太多其他文件。getter,setter两者的访问级别可以不同。

同样因为模块化，在调用数据成员时，能用access routines尽量用access routines，[即便在类的内部也不例外](http://stackoverflow.com/questions/8466790/java-conventions-use-getters-setters-within-the-class)

Don’t pretend you’re not using global data by putting all your data into a monster object and passing it everywhere 这是不必要的开销，要用就光明正大地用
