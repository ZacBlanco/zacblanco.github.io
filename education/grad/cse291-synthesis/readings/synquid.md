---
layout: page
---

## Introduction

- synthesize recursive functions to satisfy a specification
- introduce "refinement types": types decorated with predicates from a
  decidable logic
- similar to liquid type inference(?)
- :construct requirements on functions and inputs that are necessary,
  but necessarily sufficient for the correctness of f(x), but are strong
  enough to filter out sub-terms
- MUSFix fixpoint solver


## Overview

- Types in form $${B | \psi} $$
- $$\psi$$ is the refinement predicate
- recursive refinement, either pick a component in the set, or
  recursively decompose into smaller sub-problems
- "shapes of two types" must have a unifier
- refinements of subtypes must subsume supertype and assumptions encoded
  in the environment
- abductive reasoning to guess the best branch condition
    - restrict conditions to liquid formulas
- synquid expects user to provide high-level insight about an algorithm
  in the form of auxiliary function signatures


## Synquid Language

- built on top of liquid types framework
- refinement terms $$\psi$$ with sorts $$\Delta$$
- language of programs consists of program terms, $$t$$ that are split into
  **elimination** and **introduction** terms (Eterms/Iterms)
- synquid type is either sclar base type refine with formula, or
    - ITerms further separated into branching or function terms. dependent
      function type
- bidirectional type checking - interleaves top-down and bottom-up propagation
  of type information
    - inference judgement, states that a term generates a type
    - checking judgment, states that term check against a known type
- bi-di type checking analyzes program top-down until encountering a term which
  no type-checking rule applies
- key insight - type information in top-down doesn't have to be thrown away, but
  can be used to make a stronger judgement about bottom-up inference
-


## Questions


### Summary

This paper presents a new method for synthesizing recursive programs using the
structure of the grammar (refinement types) which helps type check the program
output, and using liquid types can help refine the search space for the
synthesis problem.

The contributions are twofold. First, a new mechanism for liquid type checking
is introduced which uses a combination of top-down and bottom-up approach of
type propagation. Second, a new "liquid abduction" rule is applied which also
enables checking of branching terms within a program.

Additionally, the Synquid language specifies a fixed set of terms and types
which can be used in its synthesis, which is relatively expressive that includes (non-exhaustively) refinement, branching, functions, scalars, and more.

The results show that that Synquid is able to effectively synthesize recursive
programs that are arguably more complex than other papers we have studied
recently, showing that it can synthesize programs for binary tree or list
insertions.


### Strengths

- synquid is a relatively expressive language
- complex programs are able to be synthesizes (sorting, BST/AVL, list
  operations)
- able to solve all benchmarks when all bounds applied

### Weaknesses

- restrict variable range to scalar quantities
- requires complete specification of refinement types for program to be
  synthesized
- couldn't understand how programs actually become synthesized based on type checking/abduction.


- What does Synquid use as behavioral constraints? Structural constraint? Search strategy?

- behavioral constraint
    - refinement types, logical qualifiers
- structural constraint
    - synquid language
- search strategy
    - liquid abduction?



- Find a typo in the Example in Section 3.2

Well, arguably The word "judgment" should be "judgement" in the final paragraph of the example subsection of 3.2, but it depends on who you ask. According to the following link "judgment" may be more common in british english:
https://www.thesaurus.com/e/grammar/judgement-vs-judgment/



- Can Synquid's Round-Trip Type Checking discard the following incomplete terms? Explain how or why not.


    - `inc ?? :: {Int | ν = 5}`, where `inc :: x:Int -> {Int | ν = x + 1}`
        - synquid could discard this term because it may not always produce a value
        which matches the constraint of v = x+1 since for the incomplete term, it specifies v = 5

    - `duplicate ?? :: {List Int | len ν = 5}`, where `duplicate :: xs:List a -> {List a | len ν = 2*(len xs)}`
        - synquid could discard this term because the `len v = 5` is not true for a list
        of any size.

    - `nats ?? :: List Pos`, where `nats :: n:Nat -> {List Nat| len ν = n}, Nat = {Int | ν >= 0}, Pos = {Int | ν > 0}`
        - synquid could not discard this term because `List Pos` is valid because Nat implies v >= 0, which includes all numbers which are valid as Pos (v > 0)
        and so this incomplete term may not be discarded



- Compare Synquid's condition abduction mechanism to the one from EUSolver.

EUSolver uses conditional abduction to solve subproblems, but then continues to enuerate programs on the unsolved subproblems. This approach needs to synthesize the condition which needs to be met in order to properly solve the subproblems. On the other hand, synquid's liquid abduction tries to infer the type of one of the branches first, and then deduce what the condition may be based on the types. In both EUSolver and synquid the papers rely on fast mechanisms to determine the condition.
