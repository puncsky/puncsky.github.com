---
layout: post
title: "Expert C Programming: Deep Secrets"
date: 2013-09-03 08:29
comments: true
categories: 
published: false
---

## Introduction

second book on C. ANSI C

- What does `typedef struct bar {int bar;} bar;` actually mean?
- How can I pass different-sized multidimensional arrays to on function?
- Why doesn't `extern char *p;` matches `char p[100];` in another file?
- What is a bus error? What is a segmentation violation?
- What's the difference between `char *foo[]` and `char (*foo)[]`?

hammock 吊床

`ctime` has nothing to do with language C. It just means "convert time".

`gmtime()` `asctime()`

## 1.  C History

- UNIX comes before C. Everything starts at January 1, 1970.
- Golden Rule of Compiler - Writers: Performance is almost everything.
- C is weak-typed. Assignment between objects of different types.
- Arrays and pointers are *not always* the same.
- Plenty of C features invented for convenience of C compiler-writer.
- C preprocessor
	- string replacement
	- source file inclusion
	- Expansion of general code templates
		- space ` ` makes a difference

### 1. C History: Sample Code

``` c
// space ` ` makes a difference
#define a (y) a_expanded (y)
a(x); // it is transformed to (y) a_expanded(y)
```

## 2. Paradoxical Feature

## 3. Declarations

## 4. C Arrays and Pointers are NOT the Same

## 5. Linking

## 6. Runtime Structures

## 7. Memory

## 8. ??

## 9. Arrays

## 10. Pointers

## 11. C++

## Appendix: Programmer Job Interviews

node[i>>3] += ~(0x01 << (i&0x7))