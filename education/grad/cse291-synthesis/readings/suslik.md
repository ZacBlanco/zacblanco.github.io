## Introduction

- program (e.g. `swap`) can be effectively captured by providing logical
  pre/post conditions with **Separation Logic**
- declarative -- says what heap should look like before an after
  operation with specifying the actual operation
- synthesis of an operation can be governed by a given specification in
  the same way a proof search is guided by logics

- paper goal: improve synthesizing provably correct heap-manipulating programs from a declarative functional spec. (functional-spec ==> behavioral specification)
- DSLs impose strong structural constraints on space of programs via
  restricted syntax or strong type system.
- C programs lack inherent structure --> hard to eliminate options

- key ideas
    - structural constraints can be recovered from program logic
    - synthesis of heap-manipulating programs formulated as proof search
- usefulness...
    - system must: expressive enough to verify programs with non-trivial
      heap manipulation
    - restrictive enough to make synthesis problem tractable
    - must ensure termination of possibly recursive programs
- contribution: SSL (Synthetic Separation Logic)
- deductive set of rules which prescribe to how decompose specifications
  for complex programs

- SuSLik algorithm is backtracking search in space of SSL derivations

### Deductive Synthesis from Separation Logic Specifications

- separation logic; assertions capture program state represented by a
  symbolic heap.
- SL assetion is customarily represented as `{\phi, P}` (pure part phi,
  spatial part P)
- "pure" part is a quantifier-free boolean formula describing
  constraints over symbolic values
- spatial part represented as primitive heap assertions describing
  disjoint symbolic heaps
- the logic doesn't matter as long as there is a "validity oracle" and
  "synthesis oracle"
- 


## Questions

### Summary

### Strengths

### Weaknesses

### Q's...


- What does SuSLik use as behavioral constraints? Structural constraint? Search strategy?

- Consider Frame rules in Fig. 1. This rule in general would not be sound if our programming language had mutable variables and assignment statements (such as x := 1). Why not? Give an example of a specification where it would be incorrect to apply the Frame rule in such an extended language.

- Sec 5.1 introduces the invertible rule optimization. Why is the Frame rule not invertible?
