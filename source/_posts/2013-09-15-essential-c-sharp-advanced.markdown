---
layout: post
title: "Essential C# 4.0: Advanced"
date: 2013-09-15 13:58
comments: true
categories: programming_language
published: true
---

This is the third and final part of my series of notes on learning C#.

1. [Essential C# 4.0: Basics](http://www.puncsky.com/blog/2013/08/14/essential-c-sharp-basics/)
2. [Essential C# 4.0: Intermediate](http://www.puncsky.com/blog/2013/09/05/essential-c-sharp-intermediate/)
3. Essential C# 4.0: Advanced(this post)

In general, C# leaves me an unpleasant impression of redundancy and tyranny. The designers of the language seem to be always mocking "oh my stupid boy, we have to spare no effort to prevent you from making stupid mistakes~" However, this idea of design is *good* to some extend.

My passion on coding is mostly inspired by the colorful vim on Ubuntu. *It is amazing fun. I love it!!* So the first app I install on my new Windows workstation is, of course, cygwin. Now I have to use Visual Studio, so the first plugin I install on it is surely vsvim. Moreover, maybe I am not a guy of Eclipse. I can see no beauty and usability in it, although it is much more extensible than Visual Studio. The experience of configuring the build environment of Android and later Hadoop is awful and drives me crazy.

As to the last chapters of this book, I am not interested in muti-processing/multi-threading in Windows OS, which is much slower than \*nix OS and thus somewhat boring. Windows was not designed to be a multiprocessing and multi-user platform in the first place; instead, it was originally intended for *personal* usage. So I just skimmed this chapter. Maybe I will revisit it someday when I have to exhaust the computing power on Windows. May that not happen.

At last, C# is not designed for manipulating pointers and addresses directly. Why do we bother to study it? We have C/C++ already. So I also just skimmed the last two chapters. :D

The following is my notes.

<!--more-->

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

- Thread-safe: Code or dada synchronized for simultaneous access by multiple threads.

- [torn read](http://msdn.microsoft.com/en-us/library/ff963559.aspx). When reading a variable requires more than one machine instruction, and another task writes to the variable between the read instructions.
- Local variables are loaded onto the stack and each thread has its own logical stack. (But they can still be accessed by other threads.)

1. Synchronization
	- Monitor
		- use a monitor to block the second thread from entering a protected code section before the first thread has exited that section. See the example.
		- `_Sync` object, `try`/`finally`
	- Lock
		- Monitor is fallible with `try`/`finally` (forgettable)
		- use a per-synchronization context instance of type object for the lock target.
		- avoid locking `this`, `typeof(type)`, and `string`  TODO:??我的理解是不要从instance内部锁，一律从外部锁，防止死锁。
	- Volatile *Language Contrast*:
		- C#: ensures that code accessing the field is not subject to some thread unsafe optimizations that may be performed by the compiler, the CLR, or by hardware. (forces all reads and writes to the volatile field to occur at the exact location the code identifies instead of at some other location that the) optimization produces. 
		- Java: read its current value before continuing, instead of (potentially) using a cached value. Volatile reads and writes establish a happens-before relationship, much like acquiring and releasing a mutex.
		- C: volatile exists for specifying special treatment for such locations, specifically: (1) the content of a volatile variable is "unstable" (can change by means unknown to the compiler), (2) all writes to volatile data are "observable" so they must be executed religiously, and (3) all operations on volatile data are executed in the sequence in which they appear in the source code.
			- usage of volatile keyword as a portable synchronization mechanism **is discouraged** by many C/C++ groups.
	
	- `System.Threading.Interlocked`
		- synchronization with System.Threading.Monitor is a relatively expensive operation
		- If `_Data` is `null` then set it to `newValue`
			- `Interlocked.CompareExchange(ref _data, newValue, null);` 
	- Synchronization Best Practices
		- avoid deadlocks. Deadlock's 4 conditions:
			1. Mutual exclusion
			2. Hold and wait
			3. No preemption
			4. Circular wait condition
		- All static data should be thread-safe.
			- programmers should declare private static variables and then provide public methods for modifying the data. Such methods should internally handle the synchronization.
	- `Mutex`
	- WaitHandle
	- Reset Events
		-  reset events have nothing to do with C# delegates and events. Instead, reset events are a way to **force code to wait for the execution of another thread until the other thread signals**
		- Favor `ManualResetEvent` and `Semaphores` over `AutoResetEvent`
	- Concurrent Collection Classes
		- designed to include built-in synchronization code so that they can support simultaneous access by multiple threads without concern for race conditions. 
		- `BlockingCollection<T>`
		- `ConcurrentBag<T>`
		- `ConcurrentDictionary<TKey, TValue>`
		- `ConcurrentQueue<T>`
		- `ConcurrentStack<T>`
2. Thread Local Storage
	- `ThreadLocal<T>`
	- `ThreadStaticAttribute`
3. Timers
4. Asynchronous Programming Model (APM)
	- the key aspect of the APM is the pair of `IAsyncResult BeginX()` and `EndX()` methods with well-established signatures.
	- `WaitHandle` to determine when the asynchronous method completes. 
	- `EndX()`
		1. block further execution until the successful complete
		2. get the return data
		3. receive the exception
		4. clean up resources
5. Background Worker Pattern
	- TODO
6. Windows UI Programming
	- TODO


``` cpp
// monitor
using System;
using System.Threading;
using System.Threading.Tasks;

class Program {
  // Note that calls to Monitor.Enter() and Monitor.Exit() are associated 
  // with each other by sharing the same object reference passed as the 
  // parameter (in this case _Sync).
  readonly static object _Sync = new object();
  const int _Total = int.MaxValue;
  static long _Count = 0;

  public static void Main() {
    Task task = Task.Factory.StartNew(Decrement);

    // Increment
    for (int i = 0; i < _Total; i++) {
      bool lockTaken = false;
      Monitor.Enter(_Sync, ref lockTaken);
      try {
        _Count++;
      } finally {
        if (lockTaken) {
          Monitor.Exit(_Sync);
        }
      }
    }

    task.Wait();
    Console.WriteLine("Count = {0}", _Count);
  }

  static void Decrement() {
    for (int i = 0; i < _Total; i++) {
      bool lockTaken = false;
      Monitor.Enter(_Sync, ref lockTaken);
      try {
        _Count--;
      } finally {
        if (lockTaken) {
          Monitor.Exit(_Sync);
        }
      }
    }
  }
}
```

``` cpp
// lock
using System;
using System.Threading;
using System.Threading.Tasks;

class Program {
  readonly static object _Sync = new object();
  const int _Total = int.MaxValue;
  static long _Count = 0;

  public static void Main() {
    Task task = Task.Factory.StartNew(Decrement);

    // Increment
    for (int i = 0; i < _Total; i++) {
      lock (_Sync) {
        _Count++;
      }
    }

    task.Wait();
    Console.WriteLine("Count = {0}", _Count);
  }

  static void Decrement() {
    for (int i = 0; i < _Total; i++) {
      lock (_Sync) {
        _Count--;
      }
    }
  }
}

```

``` cpp
// mutex
// It cannot run correctly on Ubuntu with mono.
using System;
using System.Threading;
using System.Reflection;

class Program {
  public static void Main() {
    // Indicates whether this is the first
    // application instance
    bool firstApplicationInstance;

    // Obtain the mutex name from the full
    // assembly name.
    string mutexName = Assembly.GetEntryAssembly().FullName;

    using (Mutex mutex = new Mutex(false, mutexName, out firstApplicationInstance)) {
      if (!firstApplicationInstance) {
        Console.Write("This application is already running.");
      } else {
        Console.WriteLine("ENTER to shut down");
        Console.ReadLine();
      }   
    }   
  }
}

```

``` cpp
// APM 
using System;
using System.IO;
using System.Net;
using System.Linq;

public class Program {
  public static void Main(string[] args) {
    string url = "http://www.puncsky.com";
    if (args.Length > 0) {
      url = args[0];
    }   

    Console.WriteLine(url);
    WebRequest webRequest = WebRequest.Create(url);

    // puncsky: `BeginX` method call
    IAsyncResult asyncResult = webRequest.BeginGetResponse(null, null);

    while (!asyncResult.AsyncWaitHandle.WaitOne(100)) {
      Console.Write('.');
    }   

    // puncsky: `EndX` method call
    // Retrieve the results when finished
    // downloading
    WebResponse response = webRequest.EndGetResponse(asyncResult);
    using (StreamReader reader = new StreamReader(response.GetResponseStream())) {
      int length = reader.ReadToEnd().Length;
      Console.WriteLine(FormatBytes(length));
    }   
  }

  static public string FormatBytes(long bytes) {
    string[] magnitudes = new string[] { "GB", "MB", "KB", "Bytes" };
    long max = (long)Math.Pow(1024, magnitudes.Length);

    return string.Format("{1:##.##} {0}", magnitudes.FirstOrDefault(
        magnitude => bytes > (max /= 1024))?? "0 Bytes", (decimal)bytes / (decimal)max).Trim();
  }
}
```

## 20 Platform Interoperability and Unsafe Code 815

- What if the memory addresses and pointers have be used?
- 3 ways
	- Platform Invoke (P/Invoke)
	- **unsafe code**
	- **COM**

## 21 The Common Language Infrastructure 843

- TODO