---
layout: post
title: "Notes on Code Complete II: Layout and Style"
date: 2014-08-16 12:00
comments: true
categories: software_craftsmanship
---

### 31 Layout and Style

**Abstract** Layout as Religion 布局是一种信仰，好的布局使得逻辑准确，代码一致、易读、可维护。

**Keywords**

Objectives of Good Layout

- 准确表示代码的逻辑结构
- 始终如一的表现代码的逻辑结构
- 改善可读性
- 经得起修改：**修改某行时不必连带修改其他行的代码**

#### Layout Styles

- Pure blocks
- Emulating Pure Blocks: 用`{``}`模仿纯块风格，`{`放在第一行末尾
- Begin-End pairs (braces) as boundaries: `{``}`单独占一行，但是都有缩进
- Endline Layout: 让`switch`看起来对更齐，但是可维护性差

#### Laying Out Control Structures

if 必须加大括号，多个条件放到不同的行上

`goto`使得代码的布局很不好办

段落之间空行

#### Laying Out Individual Statements

多用空格

Formatting Continuation Lines 断行

- 让续行更明显，比如，把运算符、逗号等放到行的末尾。如果把运算符放在前面可以凸显逻辑结构。
- 把紧密关联的元素放在一起
- **将子程序调用的后续航按照标准量缩进**，尽管对齐党觉得应该美观，可是就维护性来讲，标准缩进好些。
- 让续行的结尾易于发现

```
    CallMethod(
        LongNameA,
        LongNameB,
        C,
        D
    );
```

- 不要讲赋值语句按等号对齐，要按照标准量缩进
- 每行只写一条语句，只能有一个操作

```
    strcpy(char *t, char *s) {
        while (*++t = *++s) // 不容易发现错误
        ;
    }

    strcpy(char *t, char *s) {
        do {
            ++t; // 很容易发现错误
            ++s;
            *t = *s;
        } while (*t != '\0');
    }
```

#### Laying Out Data Declarations

- 每行只声明一个数据
- 声明尽量接近首次使用的位置
- 合理组织声明的顺序
- C++指针声明把**`*`靠近变量名**

#### Laying Out Comments

- 注释缩进与代码一致
- **每行注释用至少一个空行分开**

```
    // Comment0
    Statement0

    // Comment1
    Statement1
```

#### Laying Out Routines

- 用空行分割子程序的各部分
- 将子程序按照标准缩进

```
    public bool ReadEmployeeData(
        int maxEmployees,
        EmployeeList *employees,
        EmployeeFile *inputFile,
        int *employeeCount,
        bool *isInputError
    )
```

#### Laying Out Classes

Interfaces

1. Ctors and destructors
2. Public routines
3. Protected routines
4. Private routines and member data

Class implementations

1. Class data
2. Public routines
3. Protected routines
4. Private routines

如果文件包含多个类，要（用注释）清楚标出每个类

#### Laying Out Files and Programs

最好一个文件只有一个类，文件命名与类名一致或者相关

### 32 Self-Documenting Code

**Abstract** 好的代码是Self-documenting的，但是因此就不需要注释了吗？非也非也。

**Keywords**

注释有那些种类？

- 重复代码（bad）
- 解释代码：用于解释复杂、巧妙、敏感的代码
- Maker in the Code: like `TODO`
- Summary of the code
- **Description of the Code's Intent: 指出需要解决的问题why，而非解决的方法how**
- 传达代码无法表述的信息

如何写高质量的注释呢？

- 不写难以维护的（很麻烦的）注释
- 用PPP伪代码编程法减少注释时间
- 将注释集成到你的开发风格中 （写注释也是写代码）

最佳注释频率？

- 十步一杀

Commenting Techniques

- Commenting Individual Lines
- Endline Comments and Their Problems
    - 不好之处
        - 难看难维护
        - 不要对单行代码做尾注释
        - 不要对多行代码做尾注释
    - 那么什么时候可以用呢？
        - 数据声明时解释数据
        - 标记block的尾部, end while, end if, etc.
- Commenting Paragraphs of Code
    - 注释应该表明代码的意图，为什么这么做而不是具体怎么做
        - e.g. find the command-word terminator ($)
        - 想象将这段代码换成同样功能的sub routine应该如何命名
        - 额外的变量或者函数本身具有说明作用
        - 说明非常规的嘴阀
        - 别用缩略语
        - 错误或者语言环境的独特点要注释
        - 给出违背良好编程风格的理由
        - 投机取巧的代码要重写，而不是加注释
- Commenting Data Declarations
    - 注释数值单位
    - 对数值允许的范围给出解释
    - 如果没有枚举类型的支持，或者对于bit flags，注释编码含义
    - 注释对输入数据的限制
    - 注释要与变量名关联起来，e.g. `cref=""`
    - 注释全局数据
- Commenting Control Structures
    - while上一行，if/case下一行，都是不错的位置
- Commenting Routines
    - 无视子程序的大小和复杂，在开头放一堆信息：这是荒谬的做法
    - 记得注释接口的assumption
- Commenting Classes, Files, and Programs
    - 可以考虑说明类的设计方法、局限性等等
    - 如果不看实现就知道接口的用法吗？如果不知道，注释，但是不说实现细节
