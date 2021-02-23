## Introduction

- Flashfill already exists
- programming by example techniques for learning data transformations
- semi-supervised learning to reduce ambiguity
- InputDataGraph to represent large set of logical patterns shared across input
  data


- challenges with flash fill:
    - needs to store lots of possible programs
    - slow to operate on larger strings
- BlinkFill learns a graph structure which can "encode the set of all logical
  structured shared across a set of column values"
    - sub-paths in the graph to learn and disambiguate between a large number of
      DSL expressions.
- DSL of BlinkFill does not support conditionals/loops but supports more
  substring extraction tasks


## DSL

- two token types:
    - regex tokens
        - ProperCase, CAPS, lowercase, Digits, Alphabets, Alphanumeric, Whitespace, StartT, endT, ProperCaseWithSpaces, CAPSWSpaces, lowercaseWSpaces, AlphabetsWSpaces
    - constant string tokens
- version space algebra (VSA) - idea to succinctly represent an exponential number of programs in polynomial space
    - viewed as a DAG
        - leaf nodes programs
        - intermediate union nodes as set-union of children VSAs
        - intermediate join nodes as VSAs with k-ary function F  with k inputs

## Approach and InputDataGraphs

- goal is to find a program which satisfies all input output examples AND
    - the subexpresions of the program P are contained within a data structure
      which holds a set of subexpressions derived from the input dataset and the
      DSL
- nodes in IDG correspond to different indices within a set of strings
    - indices represented with a labeling function
    - algorithm computes IDG for each example, then unions examples into a
      single graph containing the common sub expressions.
- 

## Questions

### Summary

The authors of BlinkFill present an improvement to the FlashFill predecessor
introduced in Microsoft Excel. The authors devise an interesting approach to
compressing the total program space utilized during the learning phase. As the
amount of input data grows, they are able to contain the graph size to a much
smaller size. This helps them improve the speed of learning the synthesis program and enables an efficient graph search with dijkstra's to pick the most
likely program candidate once the initial set of programs is generated. 

### Strengths

- This work shows that novel methods and techniques can be used to further
  improve the performance of particular synthesis problems
- The authors develop a strong mathematical model for their DAG representation


### Weaknesses

The main weaknesses of BlinkFill is that is it constrained to string
manipulation programs, and even then, maintains a limited set of possible regex
operations. It does not support conditionals or loops like its predecessor
FlashFill which potentially limits its use cases depending on how complicated
the data transformations may be


#### What does BlinkFill use as behavioral constraints? Structural constraints? Search strategy?

- Behavioral
    - input/output examples
- Structural
    - Regex subset + string constant DSL
- Search Strategy
    - bottom up approach guided by the InputDataGraph, then highest ranked
      program chosen (searches DAG for shortest path with a modified Dijkstra's)


### What is the main technical insight of BlinkFill wrt FlashFill?

The main technical insight with BlinkFill is the ability to represent the input
space in a compressed format as the `InputDataGaph` and once the possible
programs are generated from the `InputDataGraph` a most likely candidate is
chosen by ranking the possible programs which reduces the number of examples
required to synthesize a correct program.

- 


### Write a program in the BlinkFill DSL (L_s) that extracts conference acronyms and years; the program should satisfy the following examples:
    
    > "Programming Language Design and Implementation (PLDI), 2019, Phoenix AZ" -> "PLDI 2019"
    
    > "Principles of Programming Languages (POPL), 2020, New Orleans LA" -> "POPL 2020",


```
Concat(SubStr(Position("(", 1, Start), Position(")", 1, Start)), SubStr(Position(", ", 1, Start), Position(",", 2, Start)))
```


### As described in the paper, the position expressions of the BlinkFill DSL only support matching a single token from Table 1, e.g. C. Could we extend the algorithm to support sequences of tokens, e.g. C ws $ to match caps followed by whitespace follows by end of string? How would this change the construction and intersection of input data graphs (use Fig 6 as an example).

I think it is possible, but would be difficult and likely affect the performance benefits of BlinkFill. If the DSL supported compound regex formulas I presume that the IDG would probably be much larger in size as you would likely need to express exponentially more edges since the combination of operators increases the search space of the synthesis program
