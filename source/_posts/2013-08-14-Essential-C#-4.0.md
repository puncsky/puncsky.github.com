---
layout: post
title: "Essential C# 4.0: Basics"
date: 2013-08-14 10:23
comments: true
categories:  programming_language
published: true 
---

No one can ever master a programming language (PL) by studying it only without looking into and comparing it with other ones. In addition, modern application software often requires a variety of components written in different PLs. Most importantly, language itself is not important at all; at least not important when compared to the fundamental ideas on architectures, frameworks, the design of the PL. A competing programmer can always get the hang of any PLs quickly.

Consequently, I make a list of PLs I would study seriously in the future.

<!-- more -->

1. C# or Java
  - A serious pure OOP and enterprise level PL is always necessary for the reason of productivity.
2. C/C++
  - The most powerful PL, which can be used to create almost everything.
  - Help to understand things under the hood.
  - C++: semi-OOP version of C, a horrible language which requires a life long time to study.
3. Bash/Python
  - Scripts make your life easier.
4. Javascript
  - Everything could be written in Javascript will be eventually written in Javascript.
5. Lisp
  - Help to understand the beauty of functional PL.


The following part is my notes on the book *Essential C#*, which is redeemed as the best book for C# learners.


# Essential C# 4.0

## 1 Introducing C# 1

![CLR converts CIL to native code](http://upload.wikimedia.org/wikipedia/commons/6/6f/CLR_diag.svg)

HelloWorld.exe is an *assembly*.

Dynamic Link Library (DLL) is also an *assembly*.

*Language Contrast*: Java—Filename Must Match Class Name

keywords, identifiers. keywords may be used as identifiers if they include “@” as a prefix.

- **Pascal Casing**: Type's name should begin with a capital letter and a *noun*...
- **Camel Casing**: same except that the first letter is lowercase.

method, statement

*Language Contrast*: C++/Java -- `main()` is all lowercase

**Strings are immutable.**

composite formatting.

``` 
System.Console.WriteLine("Your full name is {0} {1}.", firstName, lastName);
//                        -------format string-------  ----format item-----

```

- Comment Types
  - Delimited comments
  - Single-line comments
  - XML delimited comments `/**comment**/`
  - XML single-line comments `///comment`

C# src --(C# compiler)--> common intermediate language(CIL) --(justintime compiling)--> machine code

Virtual Execution System(VES)

- whether require runtime to execution?
  - managed code and managed execution
  - unmanaged code and unmanaged execution



## 2 Data Types 31

1. Numeric Types
  - Integer Types
    - Base Class Library (BCL)
  - Float-Point Types
  - Decimal
    - greater precision than the floating-point types, but a smaller range.
	- conversions from floating-point types to the decimal type may result in overflow errors.
	- calculations with decimal are slightly slower.
  - Literal Values
    - a representation of a constant value within source code.
	- **hardcoding**: place a value directly into src
	  - numbers with a decimal point will default to `double`
	  - `decimal`? append an m (or M): `1.32525456874526m`
	    - no suffix -> `int` `uint`, `long`, `ulong`.
		- suffix `U`/`L`/`UL`/`LU`
		- infix `E`
2. More Types
  - Boolean
  - Character
    - Escape Sequence: `\uxxxx` unicode char in hex, e.g. `\u0029`
  - Strings
    - verbatim string (with prefix `@`)
	  - signify that a backslash should not be interpreted as the beginning of an escape sequence.
	  - a newline cannot be placed directly within a string that is not prefaced with the @ symbol.
	  - The only escape sequence the verbatim string does support is "", which signifies double quotes and does not terminate the string.
	- **immutable**
	  - How to modify? `System.Text.StringBuilder`
	- New Line
	  - Win : `\r\n`
	  - *nix: `\n`
	  - `Console.WriteLine()` or `Console.Write(Environment.NewLine);`
3. null and void
  - In C++, void is a data type commonly used as void**. In C#, `void` *is not
  considered a data type* in the same way. Rather, it is used to identify that a
  method does not return a value.
  - `var` introduced to support anonymous types, e.g. `var patent1 = new { Title = "Bifocals", YearOfPublication = "1884" };`. Avoid to use in other cases.
    - avoid anonymous types until working with lambda and query expressions that associate data from different types or you you are horizontally projecting the data so that for a particular type, there is less data overall.
4. **Categories of Types**
  - Value Types: all the C# primitive types except `string ` and `object`, all derive from `System.ValueType`
    - data copied by value
	- for < 16 bytes
	- stack
  - Reference Types: `string`, `object`, any custom classes, most classes
    - ***IMPORTANT*** string is passed by value and cloned when pass it to a method, BUT 
    - data copied by reference
	- heap
  - **Nullable Modifier**, value types can be null, like `int? count = null;`
5. Conversions
  - Explicit Cast
    - ***checked and unchecked block*** whether to throw overflow exceptions
	- No numbers to booleans conversion
  - Implicit Cast
  - Conversion Without Casting
    - `Parse(T1 to, T2 from)`
	- `System.Convert`
	- `bool TryParse(T1 to,T2 from, T1 number)`
6. Arrays
  - Declaring
    - `string[] languages;` square brackets identify the **rank** (# of dimensions)
	- before the variable. *Language Contrast*: different from C++/JAVA, `int numbers[]` is not allowed.
  - Instantiating
    - **IMPORTANT the same applied to other similar cases**
      - If only one statement: `string[] languages = { "C#", "COBOL", "Java" };`
	  - If multiple lines: `string[] languages; languages = new string[]{"C#", "COBOL", "JAVA" };`
	- `new string[size];` 
  - Assigning
    - multi-dimensional
	  1. ***consistently sized array*** `int[,,,]`
	  2. ***jagged array*** `int[][][]`

  - Using
    - `Sort()`, `BinarySearch()`, `Reverse()`, `Clear()`, ...
	- Redimension? `System.Array.Resize()`
  - String as Arrays
	- `.ToCharArray()`

	  
``` 
// instantiating
int[,] cells = {
{1, 0, 2},
{0, 2, 0},
{1, 2, 1}
};

int[][] cells = {
new int[]{1, 0, 2},
new int[]{0, 2, 0},
new int[]{1, 2, 1}
};
```

```
// handle conversion overflow with checked and unchecked
checked {
}

unchecked {
}

```

## 3 Operators and Control Flow 83

1. Operators
  - Thread-Safe Incrementing and Decrementing, use thread-safe `Increment()` and `Decrement()` in `System.Threading.Interlocked` class
2. Boolean Expressionn
  - XOR for eXclusive OR
  - Conditional Operator (?)
    - Shortcut to the conditional operator: **Null Coalescing Operator (`??`)** evaluates an expression for null and returns a second expression if the value is null.
3. Bitwise Operators
4. Control Flow Statements
  - `foreach (type variable in collection) { /* do something */ }`
    - `variable` is read-only, and its scope is limited to the `foreach loop`
5. Jump Statements
  - *Language Contrast*: Unlike C++, C# does not allow a `switch` statement to fall through from one `case` block to the next if the `case` includes a statement. A jump statement is always required following the statement within a case.
  - *Language Contrast*: C# prevents using goto into something, and allows its use only within or out of something.
6. ***Preprocessor Directives***
  - *Language Contrast*: C/C++ contain a preprocessor. Preprocessor directives generally tell the compiler how to
  compile the code in a file and do not participate in the compilation process
  itself. In contrast, the C# compiler handles preprocessor directives as part
  of the regular lexical analysis of the source code. As a result, C# does not
  support preprocessor macros beyond defining a constant. In fact, the term
  preprocessor is generally a misnomer for C#.


``` 
// Null Coalescing Operator
string fileName;
// ...
string fullName = fileName??"default.txt";
// ...
```

``` 
// Preprocessor Directives

#if CSHARP2
System.Console.Clear();
#endif

#if LINUX
...
#elif WINDOWS
...
#endif

// you can define a preprocessor symbol in two way
// first,
#define CSHARP2
// second, in CLI
// >csc.exe /define:CSHARP2 TicTacToe.cs

#warning "Some move allowed multiple times."
// Performing main compilation...
// ...\tictactoe.cs(471,16): warning CS1030: #warning: ’"Same move allowed
// multiple times."’
// Build complete -- 0 errors, 1 warnings


// Note that warning numbers are prefixed with the letters CS in the compiler output.
// to disable warnings, first,
#pragma warning disable 1030
// second,
// > csc /doc:generate.xml /nowarn:1591 /out:generate.exe Program.cs

#pragma warning restore 1030
// one of the most common warnings to disable is CS1591, as this appears when you elect to
// generate XML documentation using the /doc compiler option, but you neglect to document
// all of the public items within your program.

// specify line numbers
#line 113 "TicTacToe.cs"
#warning "Same move allowed multiple times."
#line defualt
// display line 113 and then restore

// for visual editors to open/collapse
#region Display Tic-tac-toe Board
...
#endregion Display Tic-tac-toe Board

```

## 4 Methods and Parameters 149

1. Calling a Method
  - Namespace
    - Everything should appear within a class definition.
  - Type Name
  - Scope
  - Method Name
  - Parameters
  - Method Return
    - *Language Contrast*:Unlike C++, C# classes never separate the implementation from the declaration. Cleaner and more readable.
2. Declaring a Method
3. The Using Directive
  - *Language Contrast* Java uses wildcards in `import` directive, while C# requires each namespace to be imported explicitly.
  - `using` could be nested in other namespaces but seldom used in this way.
  - ***Aliasing*** a namespace or type: `using CountDownTimer = System.Timers.Timer;`
4. Parameters
  - Value Parameters
    - `static int Main(string[] args)` By convention, a return other than zero indicates an error.
	  - *Language Contrast*, CLI arguments start from `args[0]`. The name of the program is omitted. 
	  - access the CLI arguments through `args` or `System.Environment.GetCommandLineArgs()`
	  - ***Multiple `Main()` Methods in a program***? use `>csc.exe /m main.cs`
	- call stack, *stack unwinding*, *call site*调用地点
	- `string` as parameters are passed by value
	- By default, parameters are passed by value
  - ***Reference Parameters (`ref`)***
    - declare `ref Type variable` in the function's list of args and call it with `ref variable` 
  - ***Output Parameters (`out`)***
  - ***Parameter Arrays (`params`)***
  - Optional Parameters
    - optional parameters
	must appear after all required parameters (those that don’t have default
	values). Also, the fact that the default value needs to be a constant, compile-
	time-resolved value
	- **named parameters**
5. Method Overloading
6. Exception Handling
  - ***generic catch***: `try { } catch { }` *Language Contrast* C++: `try { } catch (...) { }`. JAVA: `Exception` is the base class for all exceptions, so `try { } catch (Exception e) { }`.
  - Avoid using exception handling to deal with expected situations
  - `bool int.TryParse(textVariable, out number)`
``` 
// Aliasing a namespace or type
using Timer = System.Timers.Timer;

class HelloWorld
{
  static void Main()
  {
    Timer timer;
	// ...
  }
}

```

``` 
// Refrence Parameters
class Program
{
  static void Main() {
    string first = "first";
	string second = "second";
	Swap(ref first, ref second);

    System.Console.WriteLine(@"first = ""{0}"", second = ""{1}""", first, second);
  }

  static void Swap(ref string first, ref string second) {
    string tmp = first;
	first = second;
	second = tmp;
  }
}
```

``` 
// Parameter Arrays
class PathEx
{
  static void Main()
  {
    string fullName;
	fullName = Combine(
	    Directory.GetCurrentDirectory();
		"bin", "config", "index.html");
    Console.WriteLine(fullName);
	fullName = Combine(
	    Environment.SystemDirectory,
		"Temp", "index.html");
    Console.WriteLine(fullName);
	fullName = Combine(
	    new string[] {
		    "C:\", "Data",
			"HomeDir", "index.html"} );
	Console.WriteLine(fullName);
  }

    static string Combine(params string[] paths)
	{
	    string result = string.Empty;
		foreach (string path in paths) {
		    result = System.IO.Path.Combine(result, path);
	    }
		return result;
	}
}
```

``` 
// named arguments
class Program
{
  static void Main()
  {
    DisplayGreeting(firstName: "Tim", lastName: "Pan");
  }
  public void DisplayGreeting(
    string firstName,
	string middleName = default(string),
	string lastName = default(string))
  {
    // ...
  }
}
```

## 5 Classes 201

1. Declaring and Instantiating a Class
2. Instance Fields
  - Declaring
  - Accessing
  - Const and readonly modifiers
3. Instance Methods
4. Access Modifiers
  - `public`, `private`, `protected`, `internal`, `protected internal`
5. Properties
  - Declaring
    - *Language Contrast* Programmers should view `new` as a call to instantiate an object, *not as a call to allocate memory*. It just retrieves memory from the memory manager.
  - Naming Conventions
    - property name `FirstName`, then field name `_FirstName`(preferred), `_firstName`, or `m_FirstName`(C++ style).
  - Using Properties with Validation
  - Read-Only and Write-Only
  - Access Modifiers on Getters and Setters
    - e.g. `private set()`
  - Properties as Virtual Fields
    - They can even do not exist physically
  - Properties and Method Calls **Not Allowed** as `ref` or `out` Parameter Values
6. Constructors & Finalizers
  - Declaring
  - Default constructors
    - Once there is a explicitely defined constructor, the default one (without parameters) is no longer provided.
  - Object Initializers `Employee emp1 = new Employee("Inigo", "Montoya") { Title = "Computer Nerd", Salary = "Not Enough"};`
    - Collection Initializers
  - Overloading Constructors
  - Constractor Chaining: Calling another Constructor Using `this`
    - `public Employee(int id, string fristName, string lastName): this(firstName, lastName) { Id = id ; }`
  - Generalize initialization: Refactor init process in the ctor into a private init method
  - Finalizers
    - Like JAVA, unlike C++
7. Static
  - *Language Contrast* The equivalent of a global field or function within the realm of C# is a static field or function
  - Static Fields
  - Static Methods
  - Static Constructors
  - Static Classes
    1. it prevents a programmer from writing code that
	instantiates the SimpleMath class.
	2.  it prevents the declaration of any
	instance fields or methods within the class. Since the class cannot be
	instantiated, instance members would be pointless.
  - `const` fields are `static` automatically, and declaring a `const` field as `static` explicitly will cause a compile error
  - ***`readonly` modifier is available only for fields (not for local variables)*** it is modifiable only from inside the constructor or directly during declaration. 似乎是把C++中`const`可以ctor初始化的功能拆成`readonly`了.
8. ***Extension Methods*** requirements
  - The first parameter corresponds to the type on which the method
  extends or operates.
  - To designate the extension method, prefix the extended type with the
  this modifier.
  - To access the method as an extension method, import the extending
  type’s namespace via a using directive (or place the extending class in
  the same namespace as the calling code).
9. Special Classes
  - `partial` Classes
    - the general purpose of a partial class is to allow the splitting of a class definition across multiple files
    - `partial` methods allow for a declaration of a method without requiring
	  an implementation. However, when the optional implementation is
	  included, it can be located in one of the sister partial class definitions,
	  likely in a separate file.
  - Nested Classes
    - One unique characteristic of nested classes is the ability to specify private
	as an access modifier for the class itself
	- Another interesting characteristic of nested classes is that they can
	access any member on the containing class, including private members.
    - treat public nested classes suspiciously;
	they indicate potentially poor code that is likely to be confusing
	and hard to discover.

## 6 Inheritance 269

1. Derivation
  - Extension methods are also inherited.
  - *Language Contrast:* Different from C++, C# is a **single-inheritance** programming language, as is the CIL. Derive from only one class a time.
    - avoid using a multiple-inheritance class
	- `sealed` class cannot be derived. *Language Contrast:* C# sealed class = Java final class. In java, `final` can be applied to
	  1. classes. = C# sealed class
	  2. **methods, cannot be overridden in a derived class. This is default in C#, unless you declare a method as `virtual`**, and in a derived class this can be prevented for further classes with `sealed` again.
	  3. fields and variables, can be initialized only once. = C# readonly
2. Overriding
  - *Language Contrast:* Java -- Virtual Methods by Default. Java and C++ -- Implicit Overriding. However, in C#, in order to override a method, both the base class and the derived class members must match and have corresponding `virtual` and `override` keywords.
  - ctor: *Language Contrast:* Dispatch Method Calls during Construction
    - C++: the type is associated with the base type rather than the derived type, and virtual methods call the base implementation.
	- C#: dispatches virtual method calls to the most derived type.
  - only instance members can be virtual. The CLR uses the concrete
  type, specified at instantiation time, to determine where to dispatch a
  virtual method call, so static virtual methods are meaningless and the
  compiler prohibits them.
  - `new` modifier for methods. ***If neither `override` nor `new` is specified, then `new` will be assumed, thereby maintaining the desired version safety.***
  - *upcasting*: please see the example codes below. *downcasting*: ?dangerous?
  - *sealed* modifier for methods. prevent overriding
  - *base* member
    - ctor `public Contact(string name) : base(name) { Name = name; }`
3. Abstract Classes
  - *Language Contrast*
    - C++ pure virtual function with `=0`. It does not require the class itself to have any special declaration.
    - C# and Java require `abstract` if the class has `abstract` member
  - polymorphism.
    - base.foo() to derived1.foo(), derived2.foo(), derived3.foo() overriding
4. `System.Object`
  - Every class is derived from `System.Object`
5. ***`is` operator***
  - verify the underlying type with `is` operator, e.g. `if (data is string) data = Encrypt((string) data);`
6. ***`as` operator***
  - conversion to a data type, and assign null if the source type is not inherently (within the inheritance chain). Avoid additional try/catch handling code. e.g. `Print(data as Document);`

``` cpp
// C++ Dispatch method calls during construction
// It will call method in the same class although it is virtual
#include <iostream>
using namespace std;

class A {
public:
    A() {
        cout << "A ctor()" <<endl;
        Foo();
    }

    virtual void Foo() {
        cout << "A Foo()" <<endl;
    }
};

class B: public A {
public:
    B() {
        cout << "B ctor()" << endl;
        Foo();
    }
    void Foo() {
        cout << "B Foo()" << endl;
    }
};

class C: public B {
public:
    C() {
        cout << "C ctor" << endl;
        Foo();
    }
    void Foo() {
        cout << "C Foo()" << endl;
    }
};

int main(int argc, char** args) {
    A* a = new C();

    delete a;

    return 0;
}
// output>
// A ctor()
// A Foo()
// B ctor()
// B Foo()
// C ctor
// C Foo()


```
``` 
// C#
using System;
class Tmp {
    static int Main(string[] args) {
        A a = new C();

        return 0;
    }
}

class A {
    public A() {
        Console.WriteLine("A ctor"); 
        Foo();
    }

    public virtual void Foo() {
        Console.WriteLine("A Foo()");
    }
}

class B: A {
    public B() {
        Console.WriteLine("B ctor"); 
        Foo();
    }
    public override void Foo() {
        Console.WriteLine("B Foo()");
    }
}

class C: B {
    public C() {
        Console.WriteLine("C ctor"); 
        Foo();
    }
    public override void Foo() {
        Console.WriteLine("C Foo()");
    }
}
// output>
// A ctor
// C Foo()
// B ctor
// C Foo()
// C ctor
// C Foo()

```

```
// upcasting in C#
// `new` modifier for methods 
using System;
class Tmp {
    static int Main(string[] args) {
        D1 d1 = new D1();
        D2 d2 = new D2();
        C c = d1;
        B b = c;
        A a = b;

        d1.Foo();
        d2.Foo();
        c.Foo();
        b.Foo();
        a.Foo();

        return 0;
    }
}

class A {
    public void Foo() {
        Console.WriteLine("A Foo()");
    }
}

class B: A {
    public new virtual void Foo() { // warning if without new
        Console.WriteLine("B Foo()");
    }
}

class C: B {
    public override void Foo() {
        Console.WriteLine("C Foo()");
    }
}

class D1: C {
    public new void Foo() {
        Console.WriteLine("D1 Foo()");
    }
}

class D2: C {
    public void Foo() {  // warning to add `new` by default
        Console.WriteLine("D2 Foo()");
    }
}
// D1 Foo()
// D2 Foo()
// C Foo()
// C Foo()
// A Foo()

```

``` cpp
// upcasting in C++
// A -> B -> C -> D
// A::Foo()
// virtual B::Foo()
// virtual C::Foo()
// D:Foo()
// ...
int main(int argc, char** argv) {
     D* d = new D();
     C* c = d;
     B* b = c;
     A* a = b;

     d->Foo();
     c->Foo();
     b->Foo();
     a->Foo();

     delete d;

     return 0;
 }
//
// D Foo()
// D Foo()
// D Foo()
// A Foo()

```

## 7 Interfaces 305

- `IPascalCase`, no implementation, no data (no fields, but properties)
- Polymorphism
- Interface Implementation
  - Declaring a class to implement an interface is similar to deriving from a
  base class in that the implemented interfaces appear in a comma-separated
  list along with the base class (order is not significant between interfaces).
  ***The only difference is that classes can implement multiple interfaces.***
  - The base class specifier (if there is one) must come first: `public class Contact : PdaItem, IListable, IComparable {...`
  - ***Explicit(more often) vs Implicit*** [Stackoverflow](http://stackoverflow.com/questions/143405/c-sharp-interfaces-implicit-implementation-versus-explicit-implementation) ??
    - Explicit: mechanism code, or avoid overriding,
	  - `ITrace.Dump()` to save info to files in `Person`
	- Implicit: semantic/model/core code
	  - Including an implicit `Compress()` implementation on a `ZipCompression`
	  class is a perfectly reasonable choice, since `Compress()` is a core
	  part of the `ZipCompression` class’s behavior.
- Interface Inheritance
  - upcasting is always successful (`Base b = new Derived()`)
  - downcasting is not, so requires an explict cast
  - Explicit Implementation should match the exact corresponding level in the hierachy. 
- Versioning
- Extension Methods on Interfaces

```
// Explicit interface implementation
public class Contact : PdaItem, IListable, IComprarable
{
  // ...
  string[] IListable.ColumnValues
  {
    // ...
  }
  // ...
}

// ...
    values = ((IListable)contact2).ColumnValues;
// ...
```

## 8 Value Types 331

All the C# primitive types are value types except `string` and `object`. How to define user's own value types? `struct`

1. Structs (derive from `System.Object` -> `System.ValueType`)
  - Recommend: Once you have instantiated a value type, you should not be able to modify the same instance.
  - *C# prevents explict definition of default constructors (parameterless)
  - `struct` fields can not be initialized at declaration time.
  - Unlike `class`, `struct` does not support finalizers. There is no need for the GC.
  - *Language Contrast*: In C++, the difference between structs and classes is simply that by
  default, a struct’s members are public. C# doesn’t include this subtle distinction.
  The contrast is far greater in C#, where struct significantly
  changes the memory behavior from that of a class.
  - `T default(T)` to get the default value
2. ***Boxing***: upcasting `ValueType` to `Object`
  1. First, memory is allocated on the heap that will contain the value
  type’s data and a little overhead (a SyncBlockIndex and method
  table pointer).
  2. Next, a memory copy occurs from the value type’s data on the stack,
  into the allocated location on the heap.
  3. Finally, the object or interface reference is updated to point at the
  location on the heap.
  - ***unboxing*** TODO start from page 339
  - ***`lock` statement***
3. Enums
  - An enum always has an underlying type, which may be `int`(default), `uint`, `long`,
  or `ulong`, but not `char`.
  - Cast between enums via `System.Array`
  - Cast between enums and strings via
    - *enum -> string*.
	- *string -> enum*. `Enum.Parse()`
  - Enums as bit flags with `[Flags]` FlagsAttribute to perform bit operations.


