---
layout: page
---

## Notes

- sketching - high level insight about a probably, along with computer's
  ability to manage details
- use a partial program to describe desired implementation, leaves
  low-level details to automated synthesis
- put "generators" in place for the program to let the synthesizer know
  where its work needs to be done
- user also needs to define the expected behavior of the program (e.g.
  PBE)
- CEGIS Loop with SAT-based inductive synthesis

### Sketch Language

- Allow leaving _holes_ as code fragments
- core concept - `??`, an integer
- higher-level generators (e.g., repeat)
- support procedure/function calls - abstract parts out and input could be a
  known grammar with a particular set of variable inputs

### Sketch Semantics

- synthesizer must assign different values to same hole depending on calling context
    -to model: a control function $$\phi: H \times T \rightarrow Z$$
- generators: powerful, because they can generate sets of functions
    - sometimes too powerful! Can generate any function in the set of real
    numbers


### Solving Sketches with CEGIS

- implementation must work correctly for common cases, and then for all the different corner cases.
- Bounded observation hypothesis
    - find a small set of inputs that represents the entire domain of inputs
- uses SAT for validation

-

## Questions

### Summary

In this paper the author presents "Sketch", a system for program synthesis which
allows programmers to write a rough "sketch" of a program, or a program with
holes, and then uses a CEGIS loop to fill in the holes left by the programmer.
Sketch achieves this by first noting that in the program, the core type of hole
that can be left is an integer represented by ??, and everything else can be
built off of those integers, such as generators. The sketch language itself
contains common features found in most languages such as conditionals and loops
that are not typically included in grammars for SyGuS solvers such as euphony or
EUSolver. The author also makes the observation, that the sketches don't need
thousands of examples to get a program right, but rather only a select few
examples which test the common case, and then corner cases of the program in
order to represent the entire input space, which can be quite large. Benchmarks
show that sketch is faster than a naive CEGIS solver which is good, and performs
well on many benchmarks.


### Strengths

- case for sketch is strong, and builds upon a simple foundation that the
  program only needs to be filled with integer constants from `??`
- technically not restricted to very specific domain (e.g., bitvector, strings)
  because the programmer can sketch many traditional functions a programmer may
  need due to including conditionals, loops, and generators (functions). This
  allows sketch to be applied to more abstract problems, like implementing
  linked lists.

### Weaknesses

- author doesn't really make a case for specific "low-level details that
programmers should be concerned with
- need to have a really good idea of the program beforehand. Too many holes =>
  long search time


### Q's

- What does Sketch use as behavioral constraints? Structural constraints? Search strategy?

- behavioral
    - reference implementation/set of test cases
- structural
    - sketch program language grammar - a program including holes (??).
- search strategy
    - CEGIS loop w/ SAT solver


- Compare structural constraints of Sketch with those in SyGuS (i.e. CFGs) and in Brahma; which of those are more expressive?

Sketch seems far more expressive because not only does it include loop (Brahma restricts itself to loop-free programs), but it contains many of the most common
features of programming languages such as conditionals, vector indexing, statements
and generators. A grammar for the Sketch language was provided in the paper,
so it is similar in a sense to the SyGuS CFGs, but far more expressive than
the previous synthesizers we've studied due to the addition of constructs like
loops and conditionals.

- Consider extending the Sketch language with a new command, abort, that makes the program fail unconditionally. What should the denotational semantics of this command be, i.e. [\mathcal{C}[[abort]]^{\tau}\langle \sigma, \Phi\rangle = ?]

the semantics of abort:

$$ \mathcal{C}[[abort]]^{\tau}\langle \sigma, \Phi\rangle = \langle\sigma,{\phi \in \Phi : \sigma\phi = 0} \rangle $$

This is similar to the semantics of the `assert` directive given in the paper
but instead of failing only when the assertion is false, we fail all the time,
hence we refrain from using A[[]] like in other denotations, and then assign 0
to the value of $$\sigma\phi$$ to denote the program failure (since nothing succeeds)

- Give an example of a satisfiable synthesis constraint that violates the Bounded Observation Hypothesis, and hence cannot be efficiently solved by CEGIS. More precisely, give an example of a formula Q(c, x), where c, x are bit-vectors of size n, such that solving the constraint [\exists c.\forall x. Q(c,x)] in the worst case would require [2^n] CEGIS iterations.

To violate the BOH, in the worst case, the constraint could be c >= x. The CEGIS loop
could start by making `c = 0`, then, the verifier could come up with the counterexample x = 1
then, in the worst case, the solver could come up with c = 1, and then the counterexample
be x = 2, etc.. We could make 2^n iterations of the CEGIS loop in the worst case if
c is increased by 1 each time in the loop


To violate the BOH, in the worst case, the constraint could be c >= x. The CEGIS
loop could start by making `c = 0`, then, the verifier could come up with the
counterexample x = 1 then, in the worst case, the solver could come up with c =
1, and then the counterexample be x = 2, etc.. adding to the set of counter
examples one by one. This can result in 2^n iterations of the CEGIS loop in the
worst case if c is increased by 1 each time in the loop
