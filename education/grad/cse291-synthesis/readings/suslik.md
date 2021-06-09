---
layout: page
---

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
- other "macros"
    - Vars(A) --> all variables occurring in A
    - $$GV(\Gamma, P Q) = Vars(P) \ \Gamma$$ --> Ghosts
    - $$EV(\Gamma, P Q) = Vars(Q) \ (\Gamma \cup Vars(P))$$ --> Ghosts
- synthesis goal is a triple: $$\Gamma; P \rightarrow Q$$.
    - $$\Gamma$$ is the environment, P precondition, Q postcondition
    - solution amounts to finding a program c and the derivation for the
      SSL assertion
- includes recursive predicate logic
- dynamic memory


## Questions

### Summary

This paper introduces a method for synthesizing programs based on heap (or
pointer) manipulating programs that extends the specifications of separation
logic. The synthesis method uses a set of pre- and post- conditions on variables
within the program and then uses a proof search to synthesize the program. The
main contribution of the paper is the Synthetic Separation Logic which helps
generalizes the problem of moving from an initial heap state P to a final state
Q. The logic  incorporates many of the most common operations on heaps which
gives more options for the synthesizer to build a program from. The result
is a synthesizer which is capable of generating small heap-manipulating programs
along with externally verifiable correctness proofs for the generated code.


### Strengths

- formal verification of synthesized programs that can be independently verified
- performance..of course
- small code sizes compared to input specifications

### Weaknesses

- writing constraints is non-trivial and requires knowledge of the program behavior
- limited to structurally recursive programs, or else the synthesizer will not terminate
- cannot synthesize auxiliary functions (to replace loops)

### Q's...


- What does SuSLik use as behavioral constraints? Structural constraint? Search strategy?

- behavioral
    - SL Rules --> {P} c {Q}, precondition/postconditions
- structural
    - SSL grammar and assertion syntax
- search strategy
    - goal-directed backtracking proof search

- Consider Frame rules in Fig. 1. This rule in general would not be sound if our programming language had mutable variables and assignment statements (such as x := 1). Why not? Give an example of a specification where it would be incorrect to apply the Frame rule in such an extended language.

If there were mutable variables or assignment statements within the logic,
then programs could feasibly modify or change pointers throughout the program
using that syntax.

With the frame rule, it specifies that if a pre and post-conditions are the same
it can be assumed to be removed from the heap. However, if mutability is
introduced then variables could be mutated later in a program which violates
the assumption that the sub-programs can't modify the pointer.

{ P } c { Q }
{ x -> z * y -> 5} c {x -> z * y -> z}


- Sec 5.1 introduces the invertible rule optimization. Why is the Frame rule not invertible?

According to the paper, invertible rules have the following properties:

> no rule that is applicable to the original goal can become inapplicable as a result of the modification"
> the effect of invertible rules is to either change a ghost into a program-level variable or strengthen the precondition.

For one, the frame rule is not like the other mentioned invertible rules in that it is not strengthening the pre or post conditions. Additionally, the frame rule does not modify them in a meaningful way. You also can't derive or work backwards easily from
the frame rule because it's hard to determine what the removed sub-heap conditions are from the original heaps.
