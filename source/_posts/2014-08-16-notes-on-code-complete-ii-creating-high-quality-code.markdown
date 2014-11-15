---
layout: post
title: "Notes on Code Complete II: Creating High Quality Code"
date: 2014-08-16 12:00
comments: true
categories: software_craftsmanship
---

## II Creating High Quality Code

### 5 Design in Construction

**Abstract** 软件开发管理的实质是管理复杂性，怎么简单怎么来，设计和抽象有助于降低复杂性。一种方式是层层向下，另一种方式是启发式地从下向上。分层向下时，模块之间应该尽量少交互、尽量不要出现循环依赖。各个模块要把“我该隐藏些什么？”牢记在心。自下向上时，不要固执于单一一种方法，（这会损害创新），反复迭代与重新设计以达到最优方案。在设计的过程中要记录或者画下设计方案。

**Keywords** KISS, hiding secrets, loose coupling, design patterns, principle of one right place

Software's primary technical imperative: managing complexity. Dijkstra说，没有人的头脑能大到装得下一个复杂程序的全部细节。

好的设计的特征:

- 简单（当看到这片代码的时候，不去参考其他代码，也能很好地理解）。
- minimal connectedness （strong cohesion, loose coupling, encapsulation）
- 可拓展性（可以在enhance代码的同时不更改已有的代码）
- 可重用性
- high fan-in, low-to-medium fan-out 尽量让别人引用自己，自己少引用别人
- 可移植性
- 精简，一本书的完成并不止于没有内容可以增加，而是没有内容可以除去
- 分层(stratification)

#### 自上而下：设计是有层次的，不是一蹴而就的

设计的层次:

1. software system
2. division into subsystems/packages
    - 任何需要花费几周才能完成的项目
    - 尽量精简子系统之间的交互关系，这样一个子系统的改变，对其他所有子系统的影响就会小很多
    - 依赖关系应该是无环的
    - 常用的子系统
        - business logic
        - UI
        - DB access
        - OS dependencies
3. division into classes within packages
4. division into data and routines within classes
5. internal routine design

#### 自下而上：因为软件开发是非确定性的，启发式(heuristics)的，所以需要“试错”

启发式设计的原则：你无需卡在单一的问题上，将问题留在未解决的状态，等拥有了更多信息后再去做。

- 从现实世界寻找对象
- 抽象
- 封装
- Inherit when inheritance simplifies the design. 方便之处在于，
    1. 传递同一类对象
    2. **对同一类对象执行同样的操作 e.g. foreach loop**
- **隐藏信息**: 本质是隐藏复杂性
    - 大量使用自我说明性质的简短函数有助于减少复杂性
    - 信息隐藏的秘密可以分为两大类：隐藏复杂度，不需要仔细研究的时候不用细看；隐藏**变化源(sources of change)**，当代码被改动时，其影响就能被限制在局部。
    - 信息隐藏失败的例子
        - 信息过度分散，循环依赖，类太大导致类内变量也像全局变量，性能上可能的损耗导致过早优化（明晰的设计比过早优化要好很多）
    - 养成问“我该隐藏什么？”的习惯
- 找出容易改变的区域，隔离开来，设计好固定的接口，把变化都限制在区域内部。那么，哪些区域容易发生变化呢？
    - 业务规则、硬件依赖、输入和输出、非标准的语言特性、困难部分（因为困难所以很可能被重新改写）、状态变量、data-size constraints
- 松散耦合
    - 小规模：模块之间的连接数要小，少的调用参数，少的public method
    - 高可见性：不要偷偷摸摸地通过全局变量传数据
    - 高灵活性：`LookupVacationBenefit(Employee.Date, Employee.Classification)`取代`LookupVacationBenefit(Employee)`因为有的地方需要临时拼凑`Employee`的其他字段，但是只有`Date`和`Classification`用到了
- 耦合的种类
    - 简单数据参数耦合(simple-data-parameter coupling) primitive data type parameters coupling 正常可接受
    - 简单对象耦合 a module instantiates an object 也很不错
    - 对象参数耦合(object-parameter coupling) 这种耦合比第一中更紧密
    - 语义上的耦合(semantic coupling) 使用某个的模块的时候需要知道其内部的细节，如果想当然就很容易出错，这种耦合很糟糕，因为松散耦合的关键就在于抽象的简单性——意思是，一旦你写好了，使用者就应当可以想当然地用它，不用知道额外信息。
- Look for common design patterns
- 其他不大常用的方法
    - 分层、接口是模块与其他部分的**契约**、为了测试而设计、**the principle of one right place 对于每一段有用的代码应该只有一个地方看到他，并且只在一个正确的位置去做维护修改**、拿不准时就用蛮力、画图。。。

### 6 Working classes

**Abstact** 本章介绍如何使用类，作为数据和操作的集合，是管理复杂度的首选工具。数据成员最好不要超过七个，函数成员也是越少越好。函数的抽象层级要保持一致。类与类之间的联系应该尽可能少，尽量让被人用自己，而自己少用别人。用别人的时候，一大串的调用链条是很危险的。inheritance会增加复杂性，如果这个子类不是真的有其特殊性，那就不要用，即便用，层次超过3层就已经很麻烦了。完成类的最后一步，是检查是不是已经消除了所有不必要的信息。

**Keywords** 接口抽象一致性, 七正负二, Liskov Substitution Principle, high fan-in and low fan-out, Law of Demeter

为什么需要类?

- encapsulation: 集中一系列的数据与操作。隐藏自己，减少自己对别人、别人对自己的影响，减少外界感知自己的复杂度。
- inheritance: 重用代码
- polymorphism: 对一系列的obj做同样的操作

#### Good Class Interfaces

- **接口抽象一致性**: 应该展现一致的抽象层次，是对`Employees` class 的逻辑就不要用计算机逻辑`NextListItem()`而是`NextEmployee()`
- 提供成对的服务，有开就有关
- 不要对使用者做出任何假设，使用者不应该知道这些个接口的实现细节，比如尽量不要让接口之间相互依赖：method1()必须在method2()之前调用，可以用assert但是不要仅仅靠注释去解释
- 采取保守的accessibility
- 不要暴露data member
- C++中不要把private data member 暴露给到头文件类的接口中
- 避免使用friend class，因为它tight coupling

#### Design and Implementation Issues

- Containment ("has a" Relationship)
    - 警惕有超过7个数据成员的类，人们在做事时能记住的离散项目个数是5~9个，平均7个
- public inheritance 是"is a" relationship
    - 继承会给程序增加复杂度（尽管带来了重用的便利性），使用时要进行详细说明，要么就不要用它
    - Liskov Substitution Principle: 除非派生类真的是一个更特殊的基类，否则不应该从基类继承
    - 只有一个实例的类（singleton除外）、只有一个派生类的基类都值得怀疑
    - 派生后覆盖了某个子程序，但是其中没有做任何操作，也值得怀疑
    - **尽量使用多态，避免大量的类型检查** 频繁重复出现的`case`意味着或许应该用继承了
- 多重继承
    - 多重继承的主要用途是定义**mixins**，很复杂，会极大增加复杂性

什么时候用inheritance 什么时候用containment?

- 重用数据用包含，一旦重用操作则用继承（继承的方便之处在于对同一类对象执行同样的操作）。由基类控制接口用继承，自己控制接口用包含。

#### Member Functions and Data

- keep the number of routines in a class as small as possible
- 禁止隐式自动生成的，但是自己并不需要的methods or operator
- high fan-in, low fan-out: minimize direct routine calls to other classes
- Law of Demeter: 间接调用也要尽量避免, e.g. `A.CreateB().CreateC()`
- 尽量减少类和类之间相互合作的范围

#### Constructors

- **initialize all member data in all constructors, if possible**, in the order in which they are declared.
- Enforce singleton property by using a private constructor. 这时候当然要用static member了
- 优先使用deep copy，论证可行后才用shallow copy

#### Classes to Avoid

避免god class，消除无关紧要的类，避免用动词命名类


### 7 High Quality routines

**Abstract** 如何写高质量的routine( = function + procedure)? 高内聚（只干一件事），清楚命名（动宾结构、与返回值有关的是function，与返回值无关的是procedure），长度尽量短，参数有序讲抽象层次。除非特定的理由，不要用macro and inline routines.

**Keywords** cohesion

除了常见的好处，还有，隐藏顺序，自我注解

#### Design at the Routine Level

Cohesion

- For routines, cohesion refers to how closely the operations in a routine are related.
- Good cohesion
    - 最强的内聚, Functional cohesion : 一个函数只做一件事情
    - sequential cohesion: 操作们顺序确定、共享数据 e.g.算年龄，再根据年龄算退休时间。
        - 解决方法：两个函数分别算年龄和算退休时间
    - communicational cohesion: 操作们仅仅共享数据
    - temporal cohesion: e.g. `Startup()`, `Configuration()` 这种内聚应该作为一系列事件的组织者，具体操作应该由里面的其他子函数完成。
- Bad cohesion
    - procedural cohesion, 操作们顺序确定，**不共享数据**
    - logical cohesion, 一大堆`if ... else ...`/`case` 混杂着不同层次的逻辑
        - 解决方案：`Event handler` 唯一的功能是发布各种命令，本身不做任何具体的操作。
    - coincidental cohesion: 一堆操作碰巧聚在了一起

#### Good Routine Names

- 完整描述所做的所有事情
- 不能含糊、通用
- 不要通过数字区别不同的routine
- 对返回值要有所描述
- 使用语气强烈的“动宾”结构
- 使用对仗词
    - add/remove, increment/decrement, insert/delete, show/hide, create/destroy, start/stop, get/put, old/new
- 为常用操作确定coding style

#### How Long Can a Routine Be

越短越好，绝对不要超过200行

#### How to Use Routine Parameters

- 按照输入-修改-输出的顺序排列参数，考虑自建`in`和`out`关键字，甚至把参数加上前缀`i_`, `m_`, `o_`.
- 类似的函数参数保持一致
- 使用所有的参数
- 把状态或者出错变量放在最后
- 不要把子程序的参数用作工作变量，应该自己新建局部变量
- 刚开始写函数的时候就document interface assumptions about parameters
- 参数数量约7个以内
- pass variables vs. pass objects
    - 答案是都可以，主要看函数的抽象层次。比如，Generally code that “sets up” for a call to a routine or “takes down” after a call to a routine is an indication that the routine is not well designed.

C is weakly typed. C++ and Java are strongly typed. TODO: see [Strong and Weak Typing](http://en.wikipedia.org/wiki/Strong_and_weak_typing)

#### Special Considerations

##### Function vs. Procedure?

Function有返回值，Procedure没有返回值

如果routine的主要用途是**返回由其名字命名的返回值**，就应该使用函数，否则，如果命名根本没有提到返回值的命名，用过程。

##### Return values

检查所有可能的路径，预设默认返回值是不错的做法。不要返回指向局部数据的引用或指针。

#### Macro Routines

    #define Cube(a) a*a*a   --> (a)*(a)*(a) --> ((a)*(a)*(a))
                    -----       -----------     -------------
            如果a是 x+1 就悲剧了 仍然可能被更高级的拆开    不错

还有就是比如 a 是 x++ 就会很隐晦 TODO see [Macro and Security](http://en.wikipedia.org/wiki/Macro_and_security)

Macros are useful for supporting conditional compilation (see Section 8.6), but careful programmers generally use a macro as an alternative to a routine only as a last resort. 不到万不得已不要用macro

#### Inline Routines

C++要求inline子程序写在头文件里面，违反了封装原则，**除非profile代码觉得真的能够提高性能**，否则不要用。

### 8 Defensive Programming

**Abstract** 承认程序总会出问题，严于律己宽以待人，所以我们需要Defensive Programming. 具体来说，有很多不同的技术，常用的有Assertion, Exception，前者面向自己，后者面向别人。有很多debugging aids可以在开发时候采纳，使得错误显现得更加明显；在Production code中要清理他们，使得错误不影响程序的运行。处处检查当然也会产生不必要的开销，因此要有主次之分。

**Keywords** Error-handling, Assertion, Exception, Barricade, Offensive Programming

总是检查输入

#### Assertion

`assert(bool, msg)`

在private里用assert, 在public里throw exception

用error-handling处理预期会发生的情况，用断言处理绝不该发生的情况

把断言看做是可执行的注解，如果检查的目标变量来自系统外部，用error-handling，如果来自可信的系统内部，用assert。

Design by Contract, 用断言来注解并验证precoditions and postconditions

#### Error-Handling Techniques

可以考虑的做法：换用下一个正确的数据，用最接近的合法值，把警告信息记录到日志，返回错误码，调用集中处理错误的对象或者routine，显示出错消息，局部最优地处理错误，关闭程序

Robustness vs. Correctness

- 两者的选择很可能是冲突的，要根据具体情况trade-off. (注：开发的时候求Correctness, production求Robustness)

保持异常处理的一致性

#### Exceptions

什么时候抛出异常？遇到了意料之外的情况，举起双手说，“我不知道怎么处理，谁来告诉我该怎么办！”异常机制的优越性在于，提供了一种**无法被忽略**的错误通知机制，其他的机制可能会导致错误在不知不觉中扩散。

C++ vs. Java on Exception Handling

- C++不支持`try-finally`，但是Java支持。
- C++可以抛出各种数据类型，Java只能抛出Exception的派生类
- 如果异常未捕获，C++ call `std::unexpected()` - `std::terminate()` - `abort()`, Java: if it is a "checked exception" terminate the thread. If it is a `RuntimeException`, 不产生任何影响 TODO checked vs runtime exceptions in Java
- Java必须在类的接口中定义可能会抛出/捕获的异常，而C++不用


避免在构造函数和析构函数中抛出异常，除非你在同一地方把他们捕获

在恰当的抽象层次抛出异常

```java Coding Horror
class Employee {
    //                             EmployeeDataNotAvailable
    public TaxId GetTaxId() throws EOFException {
}
}
```

避免使用空的catch语句

考虑创建一个集中的异常报告机制，`void ReportException(className, Exception)`

想清楚为什么要使用异常，除了异常还有其他的error handling techniques, 不能仅仅因为编程语言提供了这种机制而使用他，这是一种典型的"Programming in a Language" 而非我们提倡的 "Programming into a Language"

#### Barricade Your Program to Contain the Damage Caused by Errors

在外部的不可信的脏数据和内部的可信的类之间搭起“防火墙”，统一负责数据的清理工作。这种清理甚至可以有多层。Barricades的外部用error-handling techniques, 内部用assertion. 如果内部assertion检测到了错误，说明错误来自于程序内部，而非外部数据的错误。

#### Debugging Aides

Don't Automatically Apply Production Constraints to the Development Version. 为了开发便利，可以采取开发版本不允许的一些做法。

Introduce Debugging Aids Early

Use Offensive Programming: 在开发阶段让异常显现得更猛烈些吧，在产品运行阶段让它能够自我恢复。

Plan to Remove Debugging Aids

- Use version control and build tools like make
- Use a built-in preprocessor
- Write your own preprocessor
- Use debugging stubs (写routine检查，完结后去掉routine里面的东西)

#### Determine How Much Defensive Programming to Leave in Production Code

- Leave in code that checks for important errors. Remove (with verison control, precompiler switches, etc.) code that checks for trivial errors.
- Remove code that results in hard crashes. Leave in code that helps the program crash gracefully (显示debug信息)
- Log errors for your technical support personnel

#### Being Defensive About Defensive Programming

### The Pseudocode Programming Process

**Abstract** 本章讲述了如何利用伪代码简化routine的编码流程。伪代码让你更明确方向，更容易修改。

**Keywords** PPP

如何写routine，作者最喜欢 Pseudocode

- 好的伪代码应该忽略编程语言的细节，在intent层面上编码
- 伪代码可以作为注释，在写代码的时候逐渐被删除掉

**在通过编译后，在debugger中逐行执行代码，以检查错误**

一件事情完结的时候，不要忘记清理工作！不清理就是没做完！

Peer review! 绝大部分的问题出在自身
