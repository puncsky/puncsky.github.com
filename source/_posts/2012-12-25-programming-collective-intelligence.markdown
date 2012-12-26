---
layout: post
title: "Programming Collective Intelligence"
date: 2012-12-25 03:40
comments: true
categories: programming_languages
published: false
---

## 1. Introduction

### Python tips

list: an ordered list of any type of value. constructed with [] 

	number_list = [1,2,3,4]
	string_list = ['a','b','c','d']
	mixed_list = ['a', 3, 'c', 8]
	
dictionary: unordered set of key/value pairs, like hash map in other languages. constructed with {}

	ages = {'John':24, 'Sarah':28, 'Mike':31}
	// access
	string_list[2] 	# returns 'b'
	ages['Sarah'] 	# returns 28
	
indentation defines code blocks
	
	if x==1:
		print 'x is 1'
		print 'Still in if block'
	print 'outside if block'
	
list comprehensions 

	[expression for variable in list]
	[expression for variable in list if condition]
	
	l1 = [1,2,3,4,5,6,7,8,9]
	print [v*10 for v in l1 if v1>4]
	timesten = dict([(v, v*10) for v in l1])
	