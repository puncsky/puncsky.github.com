---
layout: post
title: "Fast and Parallel Webpage Layout (WWW '10)"
date: 2012-11-16 15:56
comments: true
categories: Paper Review
---
<iframe src="http://www.slideshare.net/slideshow/embed_code/15270792" width="427" height="356" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC;border-width:1px 1px 0;margin-bottom:5px" allowfullscreen webkitallowfullscreen mozallowfullscreen> </iframe> 

[UCB](http://www.eecs.berkeley.edu/~lmeyerov/), [paper](http://www.eecs.berkeley.edu/~lmeyerov/projects/pbrowser/pubfiles/playout.pdf)

1. Problem
-

The browsing of webpages is slow on smartphones for their limited CPU computational resources. The *power wall* forces hardware architects to apply increases in transistor counts towards improving parallel performance, not sequential performance. So the authors introduce the **parallel** mobile browser.

2. Challenges
-

<!--more-->

In the analysis,  three core limitations of the rendering speed are:

1. CSS selector matching
2. Box and text layout
3. Glyph rendering

3. Solution
-

Overall Input and Output

- Input: an HTML tree of content, CSS style rules, font files.
- Output: absolute element positions.

### 3.1 Algo1: CSS Selector Matching

*Rule matcher* associates CSS rule set with HTML node tree.

Two assumptions:

1. In general, selector language is an exact subset of regular expression.
2. Disjunctions are split into separate selectors

Algorithm paraphrase:

1. sequentially read rules and correspondingly build hash maps
2. parallelly **map** nodes to different kinds of rules
3. parallelly **reduce** several rules to each node

Optimizations from WebKit:

1. Hashtables. [×] check CSS for every node [√] read once, build hashmap, and check hash
2. Right-to-left matching.

New Optimization:

1. Redundant selector elimination. 
2. Hash Tiling. partition the hashtable. reduce cache misses.
3. Tokenization. store attributes as int instead of string to save cache.
4. Parallel document traversal.
5. Random load balancing. If in sequence, neighboring nodes will cause load imbalance.
6. Result pre-allocation.
7. Delayed set insertion.
8. Non-STL sets.preallocate a vector with a size of potential matches.

Overall Speedup = 60x: 204ms->3.5ms, 3s->50ms


### 3.2 Algo2:

- Input: HTML tree nodes with symbolic constraint attributes
- Output: layout actual details (size, shape, position)

Because CSS is confusing and informally-writtened, we create a new simple, concise, uniform, and intermediate language, Berkeley Style Sheets (BBS), which is transformed from CSS and will be specified with an attribute grammar (which shows potential for parallelization).

Three contributions:

1. Increase performance. decompse the tasks.
2. Uniform a correct, concise specification.
3. Prove it is at most linear in the size of HTML tree. 

**PARALLELIZATION**

Two steps recursively for every node in the DOM tree

1. calculate inherited attributes (top-down). Every level of childs in the tree enjoyes the parallelization.
2. calculate synthesized attributes (node's own attributes) (bottom-up). Every level of parents in the tree enjoys the parallelization.

2 is dependent on 1.

Complexity: O(log)

Speedup of box + text layout = 2-3x 

Advanced layouts: floats

### 3.3 Algo3: Font Handling

1. create a pool of necessary font library request
2. group the requests
3. make parallel calls to process each group

## 4. Conclusions

Address three bottlenecks of loading a page

1. CSS selector matching
    - Pre-built hash tables, map-reduce
2. Box and text layout solving
    - Specify layout as attribute grammars
3. Glyph rendering
    - Combine requests to groups and render in parallel

Milestone in building a parallel and mobile browser


