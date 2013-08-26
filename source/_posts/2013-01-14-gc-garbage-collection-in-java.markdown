---
layout: post
title: "GC(Garbage Collection) in Java"
date: 2013-01-14 00:18
comments: true
categories: programming_languages
---

本文摘录自[Thinking in Java](http://www.amazon.com/Thinking-Java-4th-Bruce-Eckel/dp/0131872486)

## 1. The method  `finalize()`

`finalize()`使用时，会在GC释放空间之前被调用到，目的是在GC的时候做一些特殊的清理工作；但**finalize()绝不是C++里面的destructor，因为**

1.对象**不一定会**被GC (而C++里析构函数一定会被调用到)

2.GC不等于析构

既然有GC，Java就没有必要用到析构函数，所以如果有需要在GC时候额外做些事情，就使用finalize()做最后的清理工作，**尽管它不一定会被调用到，除非JVM没有内存空间了**。

3.GC仅和内存有关

既然Java本身内存回收问题就被GC解决了，为什么还要finalize()呢？**因为，Java可能内嵌其他语言的代码，如果有类似C的内存处理函数malloc()，就必须要手动写free()。**因此，finalize()极少会被用到。

C++创建对象的位置有两种，一是创建在本地的stack上，自动销毁于本段代码的回花括号处，另一个是创建在heap上，必须用**delete**手动销毁。Java不允许创建本地对象，必须使用**new**，但是既然有了GC就不用delete.

总之，要记住的是，GC和finalize()并不总会被调用到。

finalize()可以用到debug，代码如下

``` java TerminationCondition.java

	class Book {
	  boolean checkedOut = false;
	  Book(boolean checkOut) {
	    checkedOut = checkOut;
	  }
	  void checkIn() {
	    checkedOut = false;
	  }
	  public void finalize() {
	    if(checkedOut)
	      System.out.println("Error: checked out.");
		  // Normally, you’ll also do this:
		  // super.finalize(); // Call the base-class version
	  }
	}

	public class TerminationCondition {
	  //static Test monitor = new Test();
	  public static void main(String[] args) {
	    Book novel = new Book(true);
	    // Proper cleanup:
	    novel.checkIn();
	    // Drop the reference, forget to clean up:
	    new Book(true);
	    // Force garbage collection & finalization:
	    System.gc();
	  }
	}
	/* output:
	Error: checked out.
	*/
```
此处，`System.gc()` 强制`finalize()`必须执行。**如果有finalize(),一般情况下，父类的finalize()也必须被执行到**，所以`Book.finalize()`中有`super.finalize()`。

## 2. GC的工作机制

Java中在heap上new东西并不像其他语言那么expensive，几乎和其他语言在stack上创建对象一样快。

非Java的GC机制：_reference counting_ 数对象被引用了多少次，当等于零时就被清理掉。

Java的机制：**_adaptive generational stop-and-copy mark-and-sweep._** 所有未死的对象都可以追溯到stack或者static storage上的一个引用，形成一个个web。这时候，也有不同的处理方式：

1. _stop-and-copy_. 如果程序停下来了，将活的对象拷贝到新的heap上，垃圾就被留在了原来的heap。新heap上的对象也可以被排列得更加紧凑(compact)了。有两个问题：
	- 保持两个heap
	- 周期性拷贝，即便没有new新的东西，或者产生的垃圾很少，会导致浪费，所以需要另一个机制处理这种特殊的情况
2. _mark-and-sweep_. 比较慢，但是在产生少量垃圾或者没有垃圾时候非常快。做法是，标记活对象，所有的标记完成后，扫除没有标记的对象。

如何做到adaptive地进行转化呢？JVM的内存分布在很多大的block当中。大的对象，可以独占block，不拷贝，而是增加其_generation count_，内含小对象的block则会被拷贝并整理。JVM会进行监视,如果所有对象都很稳定,GC效率降低的话,就切换到_mark-and-sweep_;同样, JVM会注意其效果,要是heap出现很多碎片,就会切换回_stop-and-copy_。

EOF