---
layout: post
title: "Essential C# 4.0: Intermediate"
date: 2013-09-05 14:00
ccomments: true
categories:  programming_language
published: true
---

[Essential C# 4.0: Basics](http://www.puncsky.com/blog/2013/08/14/essential-c-sharp-basics/)
Essential C# 4.0: Intermediate
[Essential C# 4.0: Advanced]()

This is the second part of my serials of notes on learning C#. 

A productive-focused language like C# has so much *syntax sugar* that I feel not so comfortable with, because I used to be a C++ programmer and everything in the C++ kingdom appears to be straight forward, although, at the same time, tend to be fallible.

The most serious problem I find about Microsoft's tools is that up there exist so many auto-generated codes that I cannot know them all in details. I mean, that is good, you can ship as much stuff as possible within limited time. The problem is, it is not good for curious people like me, nor for the beginner. Back in school, when we build something, we build it *from scratch*. Perhaps, *reinvent the wheel* is the best way to learn the wheel. IEDs such as Visual Studio and Eclipse make people so lazy and forget to remember and to think.

Consequently, in my spare time, I would do write every code with VIM.

The following is my notes from Chapter 9 through Chapter 13.

<!--more-->

## 9 Well-Formed Types 357

1. Overriding Object Members
	  - Guidelines for overriding `System.Object` members. Reference on request.
	  - `GetHashCode()`
	  - `ReferenceEquals()` Object Identity vs. `Equals()` Equal Object Values
	  - Calling `ReferenceEquals()` on *value types* will always return false since
2. Operator Overloading
	  - `public static`
		    - avoid recursive loop `(leftHandSide == null)` when check equality
			- One of the parameters of a operator must be the containing type
3. Referencing other Assemblies
	  - Assembly Target: `csc /target:library /out:Coordinates.dll Coordinate.cs IAngle.cs`
		    - console executable
			- class library
			- windows executable
			- module
	  - Reference an Assembly `csc /R:Coordinates.dll Program.cs`
	  - By default, a class without any access modifier is defined as `internal` (accessible from within the assembly only).
4. Defining Namespaces
	  - ***namespace alias qualifier***
		    - `csc /R:CoordPlus=CoordinatesPlus.dll /R:Coordinates.dll Program.cs`
			- `extern alias CoordPlus;` before all `using` statements
			- `using CoordPlus:
			Blah.Blah;` equally or `using CoordPlus.Blah.Blah;`
			- How about global scope? `using global::Blah.Blah` (different from `using global.Blah.Blah` which means the real namespace of `global`)
5. XML Comments
	  - `///`, `<summary>`, `<remarks>`, `<param name="blah"> <param>`, `<returns>`, `<date>`
	  - Generate an XML doc file `csc /doc:Comments.xml DataStorage.cs`
	  - tools to generate docs: GhostDoc, NDoc
6. GC
	  - **Weak reference** save the reference for future reuse (memory cache) `private WeakReference Data;`
	  - Finalizer: `~ClassName()` like [Java's `finalize()`](http://www.puncsky.com/blog/2013/01/14/gc-garbage-collection-in-java/)
	  - Deterministic finalization with the `using` statement
		    - The `IDisposable` interface defines
			the details of the pattern with a single method called `Dispose()`, which
			developers call on a resource class to “dispose” of the consumed
			- HOWEVER, there is a chance that an exception will occur before the dispose call
			resources. If this
			happens, Dispose() will not be invoked and the resource cleanup will
			have to rely on the finalizer.
			- SO 2 ways:
			  1. try / finally
			  2. `using` statement and **all variables are of the same type and they implement `IDisposable`**
	  - The ***f-reachable queue*** is a list of all the objects that are ready for
	  garbage collection and that also have finalization implementations. `System.GC.SuppressFinalize(reference)` can remove reference instance from f-reachable queue.
	  - Resource Utilization and Finalization Guidelines. Refer the book page 400.
	  - Generally, `~Destructor()` calls `Dispose()`
	  - **Lazy Initialization**: Defer the init of an object until it is required.
		    - `System.Lazy<T>`

```
// Lazy Initialization
using System.IO;

class DataCache
{
  // ...
  
  public DataCache(string FileStreamName)
  {
    _FileStream = new Lazy<TemporaryFileStream>(
	    () => new TemporaryFileStream(FileStreamName));
	// the lambda expression provides a means of passing the
	//	instructions for what will happen, but not actually performing those
	//	instructions until explicitly requested.
  }
  
  public TemporaryFileStream FileStream
  {
    get
	{
	  return _FileStream.Value;
	}
  }
  private Lazy<TemporaryFileStream> _FileStream;

  // ...
}
```

## 10 Exception Handling 405

1. Multiple Exception Types
2. Catching Exception
3. General Catch Block
4. Guidelines
5. Custom Exceptions

## 11 Generics 421

1. Generics
	- name rule as `EntityCollection<TEntity>`
	- `public class Tuple<T1, T2, T3, T4, T5, T6, T7, TRest>: ... {...}`, `TRest` can be used to store another tuple.
	- `Tuple.Create("555-55-5555", new Contact("Tim Pan"));` == `new Tuple<string, Contact>("555-55-5555", new Contact("Tim Pan"));`
4. ***Constraints***: To avoid a runtime exception and instead provide a compile-time error, C# enables you to supply an optional list of **constraints** for each type parameter declared in the generic class by using `where`.
	- Interface Constraints
	- Base Class Constraints 
	- `struct`/`class` Constraints
	- Multiple Constraints
	- **Constructor Constraints**: ensure default ctor like `new ctor()`
5. Generic Methods
	- explicit cast is preferred at most of the times.
6. Variance `Type1<Type2>` and `Type3<Type2>` are not *covariant*.
	- Covariance TODO: Read Again
		- with `out` for getters
		- with `in` for setters
	- Contra variance
7. Generic Internals
	- *Language Contrast*: Sun's implementation of generics for Java occurs within the compiler entirely, not within the JVM. JVM cannot support generics for value types.
	
	
```
// Needing the type parameter to support an interface or exception thrown
public class BinaryTree<T>
{
	// ...
	public Pair<BinaryTree<T>> SubItems
	{
		get { return _SubItems; }
		set {
			IComparable<T> first;
			first = (IComparable<T>)value.First.Item;
				
			if (first.CompareTo(value.Second.Item) < 0)
			{
				// first is less than second.
			}
			else
			{
				// second is less than first.
			}
			_SubItems = value;
		}
	}
	private Pair<BinaryTree<T>> _SubItems;
}

// Declaring the interface constrant
public class BinaryTree<T>
	where T: System.IComparable<T>
{
	// ...
	public Pair<BinaryTree<T>> SubItems
	{
		get{ return _SubItems; }
		set
		{
			IComparable<T> first;
			// Notice that the cast can now be eliminated.
			first = value.First.Item;
			
			if (first.CompareTo(value.Second.Item) < 0)
			{
				// first is less than second
			}
			else
			{
				// second is less than or equal to first
			}
			_SubItems = value;
		}
	}
	private Pair<BinaryTree<T>> _SubItems;
}
```

```
// multiple constraints
// an AND relationship is assumed
public class EntityDictionary<TKey, TValue>
	: Dictionary<TKey, TValue>
	where TKey : IComparable<TKey>, IFormattable
	where TValue : EntityBase
{
	// ...
} 
```

```
// constructor constraints ensure default ctors only
// Ctors with parameters are NOT supported
public class EntityDictionary<TKey, TValue> :
	Dictionary<TKey, TValue>
	where TKey: IComparable<TKey>, IFormattable
	where TValue : EntityBase<TKey>, new()
{
	//...
	TValue newEntity = new TValue();
	//...
}
```

## 12 Delegates and Lambda Expressions 469

1. Introducing Delegates
	- Why Delegates
		- C/C++ method pointer == ***delegate***, which encapsulates methods as objects, enabling an indirect method call bound at runtime.
	- Delegate As Data Types
		- Although all delegate data types derive indirectly from `System.Delegate`, the C# compiler does not allow you to define a class that derives directly or indirectly (via `System.MulticastDelegate`) from `System.Delegate`. 
	- Delegate Internals
		- TODO
	- Instantiating Delegates
2. Anonymous Methods
	- `BubbleSort(items, delegate(int first, int second){return first<second})`
	- can be *parameterless*: Parameterless Anonymous Methods
	- System-Defined Delegates: `Func<>`
		- `Func<>` can also be used for generic delegate.
		- `Func<int, int, bool>` the last type is the return type.
		- `System.Action` should be used for delegates that have no return.
		- However, the delegate's name provides a more explicit indication of what it is for, whereas Func<> provides nothing more than an understanding of the method signature.
		- ***Generic `Func` delegate and explicitely defined delegate are not compatible***. e.g. `Func<int, int, bool>` and `ComparisonHandler` are not compatible. ***But some degree of casting is allowed.***
	- All anonymous delegates are immutable.
3. Lambda Expressions
	- anonymous functions = lambda expressions (expression lambda + statement lambda) + anonymous methods
	- Statement Lambdas
		- `BubbleSort(items, (int first, int second) => {return first < second;})`
			- *go/goes to*, *becomes*, *such that*: "integers `first` and `second` *go* to returning the result of `first` less than `second`"
		- `BubbleSort(items, (first, second) => {return first < second;})`
			- statement lambdas can omit parameter types as long as the compiler can infer the types
		- typically a statement lambda uses only two or three statements in its statement block.  
	- Expression Lambdas
		- no statement block, only an expression
		- `names.Where(name => name.Contains(" "))`, "names where names dot contains a space"
	- you cannot use the typeof() operator (see Chapter 17) on an anonymous method, and calling GetType() is possible only after assigning or casting the anonymous method to a delegate variable.
	- TODO: internals
		- Closure—a data structure (class in C#) that contains an expression and the variables (public fields in C#) necessary to evaluate the expression. 
5. Expression Trees
	- Used to store lambda expressions as data for passing or translating.
	- Expression trees are object graphs
	- composed of read-only collection of parameters, a return type, and a body-- which is another expression.
	- Lambda Expressions vs. Expression Trees
		- compiled into a delegate in CIL
		- compiled into a data structure of type "System.Linq.Expressions.Expression"

```
using System;
class DelegateSample {
    public static void BubbleSort(int[] items, ComparisonHandler comparisonMethod) {
        int i;
        int j;
        int temp;

        if (items == null) return;
        if (comparisonMethod == null) throw new ArgumentNullException("comparisonMethod");

        for (i = items.Length - 1; i >= 0; i--) {
            for (j = 1; j <= i; j++) {
                if (comparisonMethod(items[j - 1], items[j])) {
                    temp = items[j - 1];
                    items[j - 1] = items[j];
                    items[j] = temp;
                }
            }
        }
    }

    public delegate bool ComparisonHandler(int first, int second);
    
    public static bool GreaterThan(int first, int second) {
        return first > second;
    }
	
	public static bool AlphabeticalGreaterThan(int first, int second) {
		int comparison;
		comparison = (first.ToString().CompareTo(second.ToString()));
		
		return comparison > 0;
	}

    static void Main() {
        int[] items = new int[100];
        Random random = new Random();
        for (int i = 0; i < items.Length; i++) {
            items[i] = random.Next(0, 100);
        }

        BubbleSort(items, GreaterThan);
		// In C# 2.0, we can also use BubbleSort(items, new ComparisonHandler(GreaterThan))

        for (int i = 0; i < items.Length; i++) {
            Console.WriteLine(items[i]);
        }

        Console.ReadLine();
    }
}
```

## 13 Events 507

publish-subscribe design pattern

1. Coding the Observer Pattern with Multicast Delegates (with operator+, +=, ...)
	- See the examples below
	- TODO: P518 Multicast Delegate Internals
	- How to handle exceptions from subscribers? 
		- `foreach(TDelegate handler in delegates.GetInvocationList())`See examples below
	- How to handle multiple returns from multicast delegates?
		- Also `foreach(TDelegate handler in delegates.GetInvocationList())`
2. Events
	- introduced to overcome 2 delegate shortages
		1. Encapsulating the Subscription
			- User may use `=` instead of `+=` by mistake
		2. Encapsulating the publication
			- Delegates may be invoked outside the containing class
		3. easy to forget to check for null before invoking the delegate
	- `event` keyword before the delegate type and follows a empty delegate assignment
		- e.g. `public event TemperatureChangeHandler OnTemperatureChange = delegate { };`
		- `event` ensures that any reassignment of the delegate could occur only from within the class.
		- an empty delegate `delegate {}` represents a collection of zero listeners.
	- customize with `add` and `remove`

```
// delegate's implementation of subscriber-publisher
using System;

// Heater and Cooler Event Subscriber Implementations
class Cooler {
    public Cooler(float temperature) {
        Temperature = temperature;
    } 

    public float Temperature {
        get { return _Temperature; }
        set { _Temperature = value; }
    }

    private float _Temperature;

    public void OnTemperatureChanged(float newTemperature) {
        if (newTemperature > Temperature) {
            System.Console.WriteLine("Cooler: On");
        }
        else {
            System.Console.WriteLine("Cooler: Off");
        }
    }
}

class Heater {
    public Heater(float temperature) {
        Temperature = temperature;
    }

    public float Temperature {
        get { return _Temperature; }
        set { _Temperature = value; }
    }
    private float _Temperature;

    public void OnTemperatureChanged(float newTemperature) {
        if (newTemperature < Temperature) {
            System.Console.WriteLine("Heater: On");
        }
        else {
            System.Console.WriteLine("Heater: Off");
        }
    }
}

// Defining the Event Publisher, `Thermostat`
public class Thermostat {
    // Define the delegate data type
    public delegate void TemperatureChangeHandler(float newTemperature);

    // Define the event Publisher
    public TemperatureChangeHandler OnTemperatureChange {
        get { return _OnTemperatureChange; }
        set { _OnTemperatureChange = value; }
    }
    private TemperatureChangeHandler _OnTemperatureChange;

    public float CurrentTemperature {
        get { return _CurrentTemperature; }
        set {
            if (value != CurrentTemperature) {
                _CurrentTemperature = value;

                TemperatureChangeHandler localOnChange = OnTemperatureChange;
                
                if (localOnChange != null) {
                    // call subscribers
                    OnTemperatureChange(value); // multicast delegates
                }
            }
        }
    }
    public float CurrentTemperatureHandlingError {
        get { return _CurrentTemperature; }
        set {
            if (value != CurrentTemperature) {
                _CurrentTemperature = value;

                if (OnTemperatureChange != null) {
                    foreach (TemperatureChangeHandler handler in OnTemperatureChange.GetInvocationList()) {
                        try {
                            handler(value);
                        }
                        catch (Exception e) {
                            Console.WriteLine(e.Message);
                        }
                    }
                }
            }
        }
    }
    private float _CurrentTemperature;
}

class Program {
    public static void Main() {
        Thermostat thermostat = new Thermostat();
        Heater heater = new Heater(60);
        Cooler cooler = new Cooler(80);
        string temperature;

        // Register subscribers
        thermostat.OnTemperatureChange += heater.OnTemperatureChanged;
        thermostat.OnTemperatureChange += cooler.OnTemperatureChanged;

        Console.Write("Enter temperature: ");
        temperature = Console.ReadLine();
        thermostat.CurrentTemperature = int.Parse(temperature);

        // Delegate Operators +=, -=, +, -
        Thermostat.TemperatureChangeHandler delegate1;
        Thermostat.TemperatureChangeHandler delegate2;
        Thermostat.TemperatureChangeHandler delegate3;

        delegate1 = heater.OnTemperatureChanged;
        delegate2 = cooler.OnTemperatureChanged;

        Console.WriteLine("Invoke both delegates:");
        delegate3 = delegate1;
        delegate3 += delegate2;
        delegate3(90);

        Console.WriteLine("Invoke only delegate2");
        delegate3 -= delegate1;
        delegate3(30);

        // Error handling
        thermostat.OnTemperatureChange += (newTemperature) => { throw new ApplicationException(); };
        thermostat.CurrentTemperatureHandlingError = int.Parse(temperature) + 1;
    }
}
```

```
// event implementation of subscriber-publisher
using System;

// Heater and Cooler Event Subscriber Implementations
class Cooler {
    public Cooler(float temperature) {
        Temperature = temperature;
    } 

    public float Temperature {
        get { return _Temperature; }
        set { _Temperature = value; }
    }

    private float _Temperature;

    public void OnTemperatureChanged(object sender, Thermostat.TemperatureArgs args) {
        if (args.NewTemperature > Temperature) {
            System.Console.WriteLine("Cooler: On");
        }
        else {
            System.Console.WriteLine("Cooler: Off");
        }
    }
}

class Heater {
    public Heater(float temperature) {
        Temperature = temperature;
    }

    public float Temperature {
        get { return _Temperature; }
        set { _Temperature = value; }
    }
    private float _Temperature;

    public void OnTemperatureChanged(object sender, Thermostat.TemperatureArgs args) {
        if (args.NewTemperature < Temperature) {
            System.Console.WriteLine("Heater: On");
        }
        else {
            System.Console.WriteLine("Heater: Off");
        }
    }
}

// Defining the Event Publisher, `Thermostat`
public class Thermostat {
    public class TemperatureArgs: System.EventArgs { // conventions
        public TemperatureArgs(float newTemperature) {
            NewTemperature = newTemperature;
        }

        public float NewTemperature {
            get { return _newTemperature; }
            set { _newTemperature = value; }
        }
        private float _newTemperature;
    }

    // Define the delegate data type
    // It is a norm:
    //      sender: reference of the to the object that invoke the delegate 
    //      args:   if of type `System.EventArgs` or derives from `System.EventArgs` but contains additional data
    public delegate void TemperatureChangeHandler(object sender, TemperatureArgs newTemperature);

    // Define the event publisher
    // delegate类型之前加event，赋一个空的delegate
    // an empty delegate represents a collection of zero listeners
    public event TemperatureChangeHandler OnTemperatureChange = delegate {};

    public float CurrentTemperature {
        get { return _CurrentTemperature; }
        set {
            if (value != CurrentTemperature) {
                _CurrentTemperature = value;
                // If there are any subscribers
                // then notify them of changes in 
                // temperature
                if (OnTemperatureChange != null) {
                    // Call subscribers
                    OnTemperatureChange(this, new TemperatureArgs(value));
                }
            }
        }
    }
    private float _CurrentTemperature;
}

class Program {
    public static void Main() {
        Thermostat thermostat = new Thermostat();
        Heater heater = new Heater(60);
        Cooler cooler = new Cooler(80);
        string temperature;

        // Register subscribers
        thermostat.OnTemperatureChange += heater.OnTemperatureChanged;
        thermostat.OnTemperatureChange += cooler.OnTemperatureChanged;

        Console.Write("Enter temperature: ");
        temperature = Console.ReadLine();
        thermostat.CurrentTemperature = int.Parse(temperature);
    }
}
```

```
// Define the event publisher
public event TemperatureChangeHandler OnTemperatureChange {
	add
	{
		System.Delegate.Remove(_OnTemperatureChange, value);
	}
	remove
	{
		System.Delegate.Combine(value, _OnTemperatureChange);
		
	} 
}
protected TemperatureChangeHandler _OnTemperatureChange;
```
