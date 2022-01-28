---
layout: page
title: CSE 230 - Prin. Programming Languages
permalink: /education/grad/cse230/
keywords: principles, languages, programming, compilers, CSE, 230, UCSD, UC, San Diego, haskell, OCaml
description: Notes for CSE CSE 230 (Principles of Programming Languages) at UC San Diego
mathjax: true
---

## Lecture 0

- Goals of programs: readable, checkable, modifiable
- Functional programming:
  - no assignment, no mutation, no loops

- What is the **smallest universal language**?
  - lambda calculus
    - define functions
    - call functions


### Syntax of Lambda Calculus

```
e ::= x // variable
    | λx -> e // (anonymous) function definition
    | e1 e2 // call a function <function> <argument>
```

## Lecture 1

More lambda calculus

```
\x y -> y // A function that takes two arguments and returns the 2nd
```

- Can reduce expressions in lambda calculus -- just like algebra
  - apply arguments to functions until no more arguments can be applied


Scope:

```
\x -> e y x --> (\x -> e) y x
```

- `e` is the **scope** of x
- any occurrence of `x` in `\x -> e` is **bound**


- The beta reduction (function call)
  - apply the argument to a function

## Lecture 2

- implementing booleans in lambda calculus
- given a true false, implement an if-then-else (`ITE`)
  - `ITE {cond} {e1} {e2}`
  - `ITE TRUE e1 e2 =b> e1`
  - `ITE FALSE e1 e2 =b> e2`

- implementing `NOT` --> same as `ITE` returning the opposite of its input
  - e.g. `\cond -> ITE cond FALSE TRUE`
- `AND` and `OR` operations can be short-circuited based on the first input
  - `AND := \b1 b2 -> ITE b1 (ITE b2 TRUE FALSE) FALSE`
  - `OR := \b1 b2 -> ITE b1 TRUE (ITE b2 TRUE FALSE)`

- structs and records


## Lecture 3

- see lecture slides from class website

## Lecture 4

Intro to haskell

basic types and expressions

```haskell
ex1 :: Int
ex1 = 1 + 1
ex7 :: Int
ex7 = ex1 * 2
```

functions with one or more arguments

```haskell
f :: Int -> Bool
f x = x > 0
-- >>> f 1
-- >>> True

f2 :: Int -> Int > Bool
f2 x y = x > y
-- >>> f 1 2
-- >>> False
```

Lists/arrays

```haskell
f :: a -> [a] -> [a]
f x y = x ++ y
-- >>> f "cat" ["dog", "lizard"]
-- >>> ["cat", "dog", "lizard"]

-- length
-- >>> length [1, 2, 3]
-- >>> 3
```

creating and using new datatypes

- `type` names existing types
- `data` types: create new types

`type` example:

```haskell
type Circle = (Double, Double, Double)
ex1 :: Circle
ex1 = (1.0, 2.0, 3.0)
```

`data` types

```haskell
data Circle = 
```


