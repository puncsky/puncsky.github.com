---
layout: post
title: "Essential C# 4.0: Advanced"
date: 2013-09-05 13:58
comments: true
categories: programming_language
published: false
---

[Essential C# 4.0: Basics](http://www.puncsky.com/blog/2013/08/14/essential-c-sharp-basics/)
Essential C# 4.0: Intermediate
[Essential C# 4.0: Advanced]()

Standard Query Operators vs LINQ query expression

## 14 Collection Interfaces with Standard Query Operators 535

1. Anonymous Types
	- `var patent = new {Title = "Bifocals", YearOfPublication = "1784"}`
2. Implicit Typed Local Variables (`var`)
	- There is no difference in the resultant CIL code for implic- itly typed variables whose assignment is not an anonymous type (such as string) and those that are declared as type string.
	- ***still strongly typed as well by the compiler.***
		- *Language Contrast*: JavaScript's var is dynamically typed. [See here](http://stackoverflow.com/questions/8457813/difference-between-the-implementation-of-var-in-javascript-and-c-sharp). C# is (usually) a statically typed language.
 	- You should use implicitly typed variable declarations sparingly
		- To accomplish this with implicitly typed local variables, use them only when the type assigned to the implicitly typed variable is entirely obvious and makes themselves more readable.
		- e.g. `var items = new Dictionary<string, List<Account>>();`
		- e.g. `Dictionary<string, List<Account>> dictionary = GetAccounts();`
		- the requirements for two anonymous types to be type-compatible within the same assembly are a match in property names, data types, and order of properties.
3. Collection Initializers
4. Collections
	- Arrays(length fixed, index operator supported)
		- `foreach(TItem item in array) {}`
	- IEnumerable<T> (such as `Stack<T>`, `Queue<T>`, `Dictionary<Tkey, Tvalue>`)
		- `while (stack.MoveNext()) {number = stack.Current; Console.WriteLine(number)}`
		- **The state of moving to the next is shared.** So we need `Enumerator` and `GetEnumerator()`
		- `Dispose()` the enumerator's state. A more simplified version in the example.
	- `foreach` without `IEnumerable`? 
		- "duck typing": if no IEnumerable/IEnumerable<T> method is found, it looks for the `GetEnumerator()` method to return a type with Current() and MoveNext() methods. Duck typing involves searching for a method by name rather than relying on an interface or explicit method call to the method.
	- ***compiler prevents assignment of the foreach variable*** Do not modify the collection during a foreach loop.
5. **Standard Query Operators**
	- Each method on `IEnumerable<T>` is a standard query operator
	- Filtering with `Where()`
		- A delegate expression that takes an argument and returns a Boolean is called a **predicate**
	- Projecting with `Select()`
		- `IEnumerable<FileInfo> files = fileList.Select(file => new FileInfo(file))`
		- PLINQ(Parallel LINQ): `.AsParallel().` See CH18, CH19
	- Counting with `Count()`
	- ***Deferring Execution***: One of the most important concepts to remember when using LINQ
		- At the time of declaration(".Where()"), lambda expressions do not execute. It isn’t until the lambda expressions are invoked (`foreach`) that the code within them begins to execute. 
		- **To avoid such repeated execution, it is necessary to cache the data that the executed query retrieves.** To do this, you assign the data to a local collection using one of the “To” method’s collection methods. 
	- Sorting with `OrderBy()` and `OrderBy().ThenBy()`
		- return a `IOrderedEnumerable<T>` instead of `IEnumerable<T>`
	- `IEnumerable<IGrouping<int, Employee>> groupedEmployees = employees.GroupBy((employee) => employee.DepartmentId);`
	- `GroupJoin()`, `SelectMany()`...


``` cpp
using(System.Collections.Generic.Stack<int>.Enumerator<int> 
	  enumerator = stack.GetEnumerator()) {
    while (enumerator.MoveNext()) {
		number = enumerator.Current;
		Console.WriteLine(number);
	}
}
```


## 15 LINQ with Query Expressions 589

- **More readable and SQL-like query expressions**, when compared to Standard Query Operators

## 16 Building Custom Collections 611

1. More Collection Interfaces
	- `IList<T>`
	- `IDictionary<TKey, TValue>`
	- `IComparable<T>`
	- `ICollection<T>`
2. Primary Collection Classes
	- `List<T>`
	- `Dictionary<TKey, TValue>`
	- `SortedDictionary<TKey, TValue>` and `SortedList<T>`
	- `Stack<T>`
	- `Queue<T>`
	- `LinkedList<T>`
3. Providing an Index Operator `[]`
	- `public T1 this[T2 index] {}` and `public T this[params PairItem[] branches] {}`
4. Returning null or an Empty Collection
	- The better choice in general is to return a collection instance with no items.
	- One of the few times to deviate from this guideline is when `null` is intentionally indicating something different from zero items. e.g. telephone number, `null` for not set, empty for no phone number
5. Iterators, `IEnumerator<T>` and `IEnumerator`
	- If classes want to support iteration using the foreach loop construct, they must implement the enumerator pattern. 
	- Defining
	- Syntax
		1. the class should derive from `IEnumerable<T>`
		2. `public IEnumerator<T> GetEnumerator() {}` method
	- ***yield***
		- Iterators are like functions, but instead of returning values, they yield them. 生成一个个即将被iterate 的结果？
		- `yield return SomeObject;` you can use yield only in `GetEnumerator()` methods that return `IEnumerator<T>`, or in methods that return `IEnumerable<T>` but are not called GetEnumerator().
			- C# compiler generates the code to maintain the state machine for the iterator. 
		- `yield break;`
		- some other restrictions: P649

## 17 Reflection, Attributes, and Dynamic Programming 651

**Reflection** is the process of examining the metadata within an assembly.

1. Accessing Metadata Using `System.Type`
	- `GetType()`
	- `typeof()`
2. Member Invocation
	- `type.GetProperty()` and `property.SetValue()` see examples
3. Reflection on Generics
	- `type.IsGenericType`
	- `foreach(Type type in t.GetGenericArguments()) {}`
4. Custom Attributes
	- attributes are a means of associating additional data with a property (and other constructs).
	- *Language Contrast*: Attributes are to C# what annotations are to Java
	- 2 ways to combine
		1. `[Attr1] [Attr2]`
		2. `[Attr1, Attr2]`
	- custom attributes
		- `public class CLISwitchRequiredAttribute : Attribute {}`
	- get custom attributes
		- `Attribute[] a = (Attribute[])property.GetCustomAttributes(...)`
5. Attribute Constructors
	- `public class CLISwitchRequiredAttribute : Attribute { CLISwitchAliasAttribute(string alias) {Alias = alias;}}`
6. Named Parameters
7. Predefined Attributes
	- `AttributeUsageAttribute` before the attribute class to set target properties
	- `ConditionalAttribute("CONDITION_BLAH")` before the methods to call on condition of `#define CONDITION_BLAH`
	- `ObsoleteAttribute` causes a compile-time warning, optionally an error.
	- `Serializable` before class for storage
		- `NonSerializable`
		- version problem? `System.Runtime.Serialization.OptionalFieldAttribute`
8. Dynamic Objects
	- Dynamic object support provides a common solution for talking to runtime environments that don’t necessarily have a compile-time-defined structure.
	-  The key difference when using a dynamic object is that it is necessary to identify the signature at compile time, rather than determine things such as the member name at runtime (like we did when parsing the command-line arguments).
	- TODO

```
// Using Type.GetProperties() to Obtain an Object's Public Properties
DateTime dateTime = new DateTime();

Type type = dateTime.GetType();
foreach(System.Reflection.PropertyInfo property in type.GetProperties()) {
	Console.WriteLine(property.Name);
}
```
	
## 18 Multithreading 701

thread, time slicing.

In .NET Framework 4, the task requests a thread from the thread pool.
 
- `Task` in **Task Parallel Library**
	- `class Task(Delegate Func);` and `class Task<TResult>(Delegate Func)`
	- `task.Result`, `task.IsCompleted`, `task.Id`, `task.AsyncState`, static `Task.CurrentId`, 
	- ``
	
1. Parallel Loops
	- `Parallel.For()`
	- `Parallel.ForEach<T>()`
2. Unhanded Exceptions
3. **Parallel LINQ**
4. Multithreaded Programming with Tasks
	- Task Basiscs
	- `ContinueWith()`
	- Unhandled Exceptions
5. TPL Cancellation Requests
	- Canceling a Task
	- Canceling Parallel Loops
	- Canceling a Task
6. <del>Multithreaded Programming before TPL (Before .NET 4)</del>
	
``` cpp
// Task
using System;
using System.Threading.Tasks;

public class Program {
  public static void Main() {
    const int repetitions = 10000;
    Task task = new Task( () => {
        for (int count = 0; count < repetitions; count++) {
          Console.Write('-');
        }   
    }); 

    task.Start();
    for (int count = 0; count < repetitions; count++) {
      Console.Write('.');
    }   

    // Wait until the Task completes
    task.Wait();
  }
}
```
## 19 Multithreading Patterns 749

1. Synchronization
2. Thread Local Storage
3. Timers
4. Asynchronous Programming Model
5. Background Worker Pattern
6. Windows UI Programming


## 20 Platform Interoperability and Unsafe Code 815

## 21 The Common Language Infrastructure 843

## Appendix

A Downloading and Installing the C# Compiler and the CLI Platform 865
B Full Source Code Listings 869
C Concurrent Classes from System. Collections. Concurrent 895
D C# 2.0 Topics 899
E C# 3.0 Topics 903
F C# 4.0 Topics 905
