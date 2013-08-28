---
layout: post
title: "Essential C# 4.0: Intermediate"
date: 2013-08-26 11:03
77;10200;0ccomments: true
categories:  programming_language
published: false
---

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
  - Refrence an Assembly `csc /R:Coordinates.dll Program.cs`
  - By default, a class without any access modifier is defined as `internal` (accessible from within the assembly only).
4. Defining Namespaces
  - ***namespace alias qualifier***
    - `csc /R:CoordPlus=CoordinatesPlus.dll /R:Coordinates.dll Program.cs`
	- `extern alias CoordPlus;` before all `using` statements
	- `using CoordPlus.Blah.Blah;` equally or `using CoordPlus.Blah.Blah`
	- How about global scope? `using global::Blah.Blah` (different from `using global.Blah.Blah` which means the real namespace of `global`)
5. XML Comments
  - `///`, `<summary>`, `<remarks>`, `<param name="blah"> <param>`, `<returns>`, `<date>`
  - Generate an XML doc file `csc /doc:Comments.xml DataStorage.cs`
  - tools to generate docs: GhostDoc, NDoc
6. GC
  - **Weak reference** save the reference for future reuse (memory cache) `private WeakReference Data;`
  - Finalizer: `~ClassName()` like [Java's `finalize()`](http://www.puncsky.com/blog/2013/01/14/gc-garbage-collection-in-java/)
  - Deterministic finaliztion with the `using` statement
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
7. Resources Cleanup

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
