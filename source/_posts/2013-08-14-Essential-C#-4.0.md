77;10200;0c---
layout: post
title: "Essential C# 4.0"
date: 2013-08-14 10:23
comments: true
categories:  programming_language
published: false
---


# Essential C# 4.0

## 1 Introducing C# 1

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

``` cs
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
4. **Categories of Types**
  - Value Types
    - data copied by value
	- for < 16 bytes
	- stack
  - Reference Types: `string`, any custom classes, most classes
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

	  
``` C#
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

``` C#
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


``` C#
// Null Coalescing Operator
string fileName;
// ...
string fullName = fileName??"default.txt";
// ...
```

``` C#
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
``` C#
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

``` C#
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

``` C#
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

``` C#
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
5. Properties
  - Declaring
  - Naming Conventions
  - Using Properties with Validation
  - Read-Only and Write-Only
  - Access Modifiers on Getters and Setters
  - Properties as Virtual Fields
  - Properties and Method Calls Not Allowed as ref or out Parameter Values
6. Constructors & Finalizers
  - Declaring
  - Default constructors
  - Overloading Constructors
  - Calling one Constructor Using this
  - Finalizers
7. Static
  - Static Fields
  - Static Methods
  - Static Constructors
  - Static Classes
8. Extension Methods
9. Special Classes
  - Partial Classes
  - Nested Classes

## 6 Inheritance 269

## 7 Interfaces 305

## 8 Value Types 331

## 9 Well-Formed Types 357

## 10 Exception Handling 405

## 11 Generics 421

## 12 Delegates and Lambda Expressions 469

## 13 Events 507

## 14 Collection Interfaces with Standard Query Operators 535

## 15 LINQ with Query Expressions 589

## 16 Building Custom Collections 611

## 17 Reflection, Attributes, and Dynamic Programming 651

## 18 Multithreading 701

## 19 Synchronization and More Multithreading Patterns 749

## 20 Platform Interoperability and Unsafe Code 815

## 21 The Common Language Infrastructure 843

## Appendix

A Downloading and Installing the C# Compiler and the CLI Platform 865
B Full Source Code Listings 869
C Concurrent Classes from System. Collections. Concurrent 895
D C# 2.0 Topics 899
E C# 3.0 Topics 903
F C# 4.0 Topics 905
