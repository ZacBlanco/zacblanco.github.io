## Introduction

- problem of synthesizing loop-free programs which implement a particular functionality given a set of library components
- component-based synthesis problem, by generating a constraint and then solving the constraint.
- solving the "resource-bounded component-based synthesis problem"
- focus only on bitvector manipulation programs
- synthesis is constraint based
    - logical specification given in the form of an "unoptimized program"
- size of constraint is quadratic in the number of components.
- technique encodes all possible straight line programs using two particular operations using a logical formula.

> One of the key technical contributions of the paper is an algorithm to find satisfying assignments to such synthesis constraints by using an off-the-shelf SMT solver

- problem statement
    - given a spec with a tuple of input variables, an expression over the inputs and outputs which generates the output
    - a specification of library components to build an optimized form
- set of specifications is a _library_
- discover a program conforming to s spec using only components in the library

- synthesis constraint
    - reduce problem of straight-line program synthesis to that of
      satisfying an assignment to a first-order logic constraint
    - two constraints needed to guarantee well-formed programs
        - consistency constraint - every line in the program has at most
          one component
        - acylicity constraint - every variable is initialized before being used

- synthesis constraint solving
    - uses counterexample-guided iterative refinement
    - SAT solver for synthesis


## Questions

### Summary

In this paper the authors present Brahma, which is a program synthesizer built
to synthesize programs using component-based synthesis. The authors thoroughly
formulate the component-based synthesis problem. They state that solving the
component-based synthesis problem requires a formal specification which is a
tuple of input variables and output variable, and the expression over the
variables which specifies an input/output relationship. It also requires a set
of specification (the library) which should be used to build the final solution.
They show it's possible to iteratively construct and verify the solution to the
component-based synthesis problem using SAT solvers. The general approach to
finding the solution seems quite naive as it is simply a top-down search through
the problem space. The authors' implementation focuses on solutions to problems
in the bitwise operator space, but assert that it could be applied elsewhere
with different library components. The implementation shows marked improvements
over previous synthesizers which don't formulate the problem as a
component-based synthesis problem.

### Strengths

- Concrete problem formulation (I don't think the component-based synthesis was formulated like this before?)
- Clearly demonstrates the ability to generate solutions with SAT solvers
- performance gain over sketch/AHA
- even though bitvector manipulations may be of limited use, they do give
a number of particular places it could be useful, particularly in finding
potentially overflowing arithmetic operations and replacing it with bitvector
manipulation which doesn't suffer from overflow problems 


### Weaknesses

- The paper only focuses on solving bitwise-manipulation which the authors make
  an argument for, but seems of limited use aside from making static
  optimizations at runtime.
- The solver requires the ability to generate input/output solutions by having
  some kind of expression which already maps input to output - synthesis is
  arguably less useful in this case since that a "program" is already expressed.


### Questions

- What does Brahma use as behavioral constraints / user interaction mode? Structural constraints? Search strategy?

- Behavioral
    - formal specification
- Structural
    - component libraries
- Search Strategy
    - enumerative SAT solver

- Is it possible to specify Brahma's structural constraints in the SyGuS format (as a grammar)? Is yes, show a small example. If no, explain why.

Yes, it is possible to specify the structural constraints as a grammar in the
SyGuS format. Take the bitvector example from the paper:
If our two possible operations on a bitvector V are `&` and `-` then a grammar
could be written

```
S ::= V - O | V - V
A ::= V & V
O ::= 1
V ::= x
```


- Consider the following modification of Brahma's synthesis problem: you are given N components, but your program is only allowed to use any K of them (for a fixed K <= N). How would you modify the SMT encoding in section 5 to enforce this restriction?

Given the original constraint below:
∃L∀ ~I,O,T: ψwfp(L) ∧ (φlib(T) ∧ ψconn(~I,O,T,L) ⇒ φspec(~I,O))

The ψwfp(L) does not have to change, since the well-formed programs specification should be the same is the same.

This leaves the component of the specification: 
- (φlib(T) ∧ ψconn(~I,O,T,L) ⇒ φspec(~I,O))

since φlib(T) represents the the specifications of the base components ψconn(~I,O,T,L) represents the interconnections between those particular components, we would need to reformulate
these two pieces such that they represent only a subset of the particular library. so φlib and φconn would need to have a modified representation, possible with an argument for the value K to limit the available components.