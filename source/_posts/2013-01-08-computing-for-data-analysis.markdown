---
layout: post
title: "Computing for Data Analysis"
date: 2013-01-08 23:52
comments: true
categories: machine_learning
published: false
---

[Coursera](https://class.coursera.org/compdata-002)
[An Introduction to R](http://cran.r-project.org/doc/manuals/R-intro.html)

## Week 1

[Video Lectures](https://class.coursera.org/compdata-002/lecture/index)

### Background & Overview

- R is a dialect of the S language.
- **Free software** and runs on almost any standard computing platform/OS (even on the PlayStation 3)
	0. 使用自由
	1. 学习&修改自由
	2. 分发自由
	3. 改进自由

### Data Types

#### Objects

- 5 atomic classes of objects
	1. character
	2. numeric (real numbers)
	3. integer
	4. complex
	5. logical (True/False)
- The most basic object is a **vector**
	- can only contain objects of **the same class**
	- **BUT for *list***, represented as vector, can hold objects of different classes.
	- created with `vector()` and `list()`

#### Numbers

- numbers *by default* treated as `numeric` objects
- integer specified with `L` suffix
- infinity represented by `Inf`, `1/Inf` is 0
- undefined value represented by `NaN`

#### Attributes

- R objects have attributes
	- names, dimnames
	- dimensions
	- class
	- length
	- user-defined attributes/metadata
- Attributes of an object can be accessed with `attribute()`

#### Entering Input

- `<-` assignment operator
	- `x <- 1:20`
- `#` comment
- `print()`
- `c()` _can_ create vectors of objects
	- `> x <- c(0.5, 0.6) ## numeric`
	- `> x <- c(TRUE, FALSE) ## logical`
	- `> x <- c(T, F) ## logical`
	- `> x <- c("a", "b", "c") ## character`
	- `> x <- 9:29 ## integer`
	- `> x <- c(1+0i, 2+4i) ## complex`
- `vector()` creates a vector
	- `x <- vector("numeric", length = 10)`
- `as.*()` casts class type 
	- nonsensical coercion results in NAs
- `class()` shows the class type
- `matrix(nrow = 2, ncol = 3)` matrices are vectors with a dimension attribute. The dimension attribute is itself an integer vector of length 2(nrow, ncol)
	- `m<-matrix(1:6, nrow = 2, ncol = 3)`
	- `dim(m) <- c(2, 5)`
- `dim()` shows the length of each dimension 
- matrices can be created by column-binding or row-binding with `cbind()` and `rbind()`, which bind the rows or columns
- `factor()` creates factor class(integer vector with labels). 类似enum

#### Missing Values

- Missing values are denoted by NA or NaN for undened mathematical operations.
	- `is.na()` is used to test objects if they are NA
	- `is.nan()` is used to test for NaN
	- NA values have a class also, so there are integer NA, character NA, etc.
	- A NaN value is also NA but the converse is not true

#### Data frames

- Data frames are used to store tabular data
	- They are represented as a special type of list where every element of the list has to have the same length
	- Each element of the list can be thought of as a column and the length of each element of the list is the number of rows
	- Unlike matrices, data frames can store different classes of objects in each column (just like lists); matrices must have every element be the same class
	- Data frames also have a special attribute called row.names
	- Data frames are usually created by calling read.table() or read.csv()
	- Can be converted to a matrix by calling data.matrix()

#### Names

`dimnames()` dimension names

#### Summary

- Data Types
	- atomic classes: numeric, logical, character, integer, complex
	- vectors, lists
	- factors
	- missing values
	- data frames
	- names
 
### Subsetting
### Vectorized Operations
### Reading/Writing Data
### Set Your Working Dir & Editing R Code
### str function